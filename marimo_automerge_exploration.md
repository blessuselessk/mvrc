# marimo + automerge: An Exploration for MVRC

## The Gap in the Current Design

The inception document converges on a clear architecture:

1. **Canonical Layer** — a typed claim graph (the "truth")
2. **Validation Layer** — MECE checks, dependency propagation, structural integrity
3. **Projection Layer** — linearized prose (Markdown, Typst, PDF)

marimo was chosen for layers 2 and 3 because its reactive DAG gives you automatic recomputation when structure changes. That's the right call.

But layer 1 — the canonical claim graph — is currently a Python dictionary in a marimo cell. That works for a single-user prototype. It does not answer several questions that matter as soon as the tool becomes real:

- **Who changed what, and when?** A Python dict has no history.
- **What if the human and the LLM propose conflicting structural edits?** A dict can't merge.
- **What if you want to explore two argument structures and compare them?** You can't branch a dict.
- **What if you want to work offline and sync later?** A dict lives in memory.

This is where automerge enters.

---

## What automerge Actually Is

Automerge is a CRDT (Conflict-free Replicated Data Type) library. Its data model is JSON-like: maps, lists, text, counters. The core is written in Rust, with bindings for JavaScript, Python (`automerge-py`), Swift, and others.

The key properties:

- **Every mutation is an operation**, not an overwrite. Insert key, delete key, set value — each is a discrete, replayable event.
- **Concurrent edits merge automatically.** Two actors can modify the same document independently, and the merge is deterministic and lossless.
- **Full version history is retained.** You can inspect or replay any prior state.
- **Transport-agnostic.** Sync over WebSocket, HTTP, files, or even email attachments — if you can move bytes, you can sync documents.
- **Offline-first by default.** The document works locally. Sync happens when connectivity exists.

---

## The Core Insight: The Claim Graph IS an Automerge Document

Instead of:

```python
# A mutable Python dict — no history, no merging, no branching
claims = {
    "profitability_decline": {
        "type": "claim",
        "partition": ["revenue", "cost", "capital_allocation"]
    },
    ...
}
```

You get:

```python
import automerge

# An automerge document — every change tracked, mergeable, branchable
doc = automerge.init()
with automerge.transaction(doc) as tx:
    tx.put(automerge.ROOT, "profitability_decline", automerge.MAP)
    node = tx.get(automerge.ROOT, "profitability_decline")
    tx.put(node, "type", "claim")
    partition = tx.put_object(node, "partition", automerge.LIST)
    tx.insert(partition, 0, "revenue")
    tx.insert(partition, 1, "cost")
    tx.insert(partition, 2, "capital_allocation")
```

The claim graph gains:

| Property | Python dict | Automerge document |
|---|---|---|
| History | None | Full operation log |
| Concurrent edits | Last write wins (data loss) | Deterministic merge (no data loss) |
| Branching | Manual copy | Native fork/merge |
| Offline editing | N/A | Built-in |
| Audit trail | None | Every operation attributed to an actor |
| Persistence | Serialize/deserialize manually | Binary format, save/load natively |

---

## How the Layers Map

### Layer 1: Canonical Graph (automerge)

The automerge document stores:

- **Nodes**: Each claim/argument/evidence is a map entry with `type`, `scope`, `content`, `parent`, `depends_on`
- **Partitions**: Each parent's partition is an automerge list — ordered, appendable, with conflict-free concurrent edits
- **Edges**: Relationships (`supports`, `depends_on`) stored as references between nodes

Mutations happen through **automerge transactions**. This enforces the inception document's core rule:

> "Never edit prose. Only edit structure."

You literally cannot bypass this — the only way to change the graph is through structured operations.

### Layer 2: Validation (marimo reactive cells)

marimo cells read from the automerge document and react to changes:

```python
# Cell: Load current graph state from automerge
graph = automerge_doc_to_dict(doc)

# Cell: MECE validator (automatically reruns when graph changes)
errors = validate_mece(graph)

# Cell: Dependency integrity check
orphans = find_orphaned_nodes(graph)

# Cell: Structural integrity score
score = 1 - (len(errors) + len(orphans)) / max_possible_errors
```

The reactive DAG in marimo maps onto the validation pipeline. When the automerge document changes, dependent marimo cells recompute.

### Layer 3: Projection (marimo output cells)

```python
# Cell: Render to Markdown
markdown_output = render_markdown(graph)

# Cell: Export to Typst
typst_output = render_typst(graph)
```

These are derived artifacts. They recompute when the validated graph changes.

---

## What This Enables That a Dict Cannot

### 1. Structural Branching ("Git for Arguments")

Want to explore whether "regulatory risk" belongs under "cost" or deserves its own partition element?

```python
# Fork the document
branch_a = automerge.fork(doc)
branch_b = automerge.fork(doc)

# In branch A: add regulatory_risk under cost
add_evidence(branch_a, parent="cost", scope="regulatory_risk", ...)

# In branch B: expand the partition
expand_partition(branch_b, parent="profitability_decline",
                 new_element="regulatory_risk")
add_argument(branch_b, parent="profitability_decline",
             scope="regulatory_risk", ...)

# Compare validation results
errors_a = validate_mece(to_dict(branch_a))
errors_b = validate_mece(to_dict(branch_b))

# Merge the winner back
automerge.merge(doc, branch_b)
```

You can explore alternative argument structures as parallel branches, validate each, and merge the one that holds up. This is version control for reasoning.

### 2. The LLM as a Collaborative Peer

The LLM doesn't edit text. It proposes automerge operations:

```python
# Human works in the marimo notebook
# LLM works on a fork
llm_fork = automerge.fork(doc)

# LLM proposes structural additions via transactions
with automerge.transaction(llm_fork, message="LLM: expand revenue analysis") as tx:
    add_evidence(tx, parent="revenue", scope="churn",
                 content="Customer churn increased 15% YoY")
    add_evidence(tx, parent="revenue", scope="pricing",
                 content="Average deal size decreased 8%")

# Validate LLM's proposed changes before merging
proposed_errors = validate_mece(to_dict(llm_fork))
if not proposed_errors:
    automerge.merge(doc, llm_fork)
else:
    flag_for_human_review(proposed_errors)
```

The LLM becomes a peer author on the claim graph, not a black-box text generator. Its contributions are:
- Structured (operations on typed nodes, not string edits)
- Reviewable (each transaction has a message and attribution)
- Reversible (roll back any transaction)
- Validated (MECE checks run before merge)

### 3. Full Audit Trail of Argument Evolution

Every change to the claim graph is an automerge operation with an actor ID and timestamp. You can answer:

- "When was this claim added?"
- "Who added this evidence — the human or the LLM?"
- "What did the argument structure look like before we added regulatory risk?"
- "Show me every mutation that touched the revenue branch."

This is crucial for the inception document's stated goal of preventing **silent scope expansion** and **untracked thematic mutation**.

### 4. Resilient Persistence

The automerge document serializes to a compact binary format. It can be:
- Saved to disk as a single file alongside the marimo notebook
- Synced between devices
- Embedded in the marimo `.py` file as base64 (if small)
- Stored in a git-tracked binary file (automerge's format is designed for efficient storage)

The claim graph survives across sessions without manual serialization/deserialization.

### 5. Multi-Agent Collaboration

Multiple LLMs (or human collaborators) can work on different branches of the pyramid simultaneously:

```
Agent A: Working on revenue subtree
Agent B: Working on cost subtree
Agent C: Validating overall MECE integrity
Human:   Reviewing and approving merges
```

Automerge handles the merge. marimo handles the validation. The human approves the result.

---

## The Bridge: Connecting automerge Changes to marimo Reactivity

This is the key engineering challenge. marimo's reactivity is triggered by cell re-execution. automerge's changes happen at the document level. The bridge needs to:

1. Expose the automerge document as a marimo-reactive value
2. Trigger cell recomputation when the document changes

**Approach A: Polling wrapper**

```python
import marimo as mo

# A marimo state variable backed by automerge
graph_state, set_graph_state = mo.state(automerge_doc_to_dict(doc))

# When mutations happen (via UI or LLM), update state
def mutate_graph(mutation_fn):
    mutation_fn(doc)  # automerge transaction
    set_graph_state(automerge_doc_to_dict(doc))  # trigger marimo reactivity
```

This is the simplest approach: mutations go through a wrapper function that updates both the automerge document and the marimo state.

**Approach B: Custom marimo plugin**

A more elegant solution would be a custom marimo UI element that wraps the automerge document and emits reactive updates on change. This is feasible given marimo's plugin architecture.

**Approach C: File-based sync**

The automerge document is saved to disk on every transaction. A marimo cell watches the file and reloads on change. Crude but effective, and decouples the two systems.

---

## What About `automerge-py`'s Maturity?

Honest assessment:

- **`automerge-py`** is a low-level wrapper around the Rust core. The API is verbose (as shown above). A more ergonomic layer is "in the works" per the README.
- The last PyPI release was `1.0.0rc1` (September 2022), but the GitHub repo was updated February 2026.
- For MVRC's use case, you don't need the full automerge feature set immediately. You need: create documents, make transactions, fork, merge, save/load. The low-level API covers this.

**Mitigation**: Build a thin Pythonic wrapper around `automerge.core` specifically for the MVRC claim graph schema. This isolates the rest of the codebase from the low-level API and can be swapped if a better wrapper emerges.

---

## Critical Context: marimo Already Uses Loro (a CRDT)

Research reveals that **marimo has already implemented real-time collaboration using Loro**, a high-performance CRDT library. Loro is a newer CRDT that explicitly draws from both Automerge (columnar encoding) and Yjs (merge algorithms), plus the Fugue algorithm for text editing. marimo's existing implementation includes:

- Server-side `DOC_MANAGER` maintaining Loro documents
- WebSocket-based `ws_rtc_handler` for sync
- Frontend `loro-codemirror` integration for collaborative editing
- Room-based architecture supporting editors and viewers

This means the CRDT plumbing for collaborative notebook editing already exists in marimo. The question becomes: **should MVRC use a CRDT at the application data layer (the claim graph) in addition to what marimo already provides at the notebook editing layer?**

The answer is **yes, and the reasons are distinct**:

- **Loro in marimo** = collaborative editing of the *notebook cells themselves* (code, markdown)
- **Automerge for MVRC** = collaborative editing of the *claim graph data structure* (the semantic content)

These are different layers. You could have two people editing the same marimo notebook (via Loro) while also having the claim graph inside that notebook be a CRDT document (via Automerge) that supports forking, branching, and multi-agent collaboration on the *argument structure itself*.

## Alternative CRDTs: Landscape Comparison

| | Automerge | Yjs | Loro (used by marimo) |
|---|---|---|---|
| Core language | Rust | JavaScript | Rust |
| Algorithm | RGA | YATA | Fugue |
| Data model | JSON documents | Modular shared types | JSON documents |
| History | Full DAG built-in | Requires extra overhead | Full DAG built-in |
| Branching | Native fork/merge | Not native | Native fork/merge |
| Python bindings | automerge-py (pre-release) | ypy (more stable) | No Python bindings yet |
| Maturity | Established | Most mature | Newest |

For MVRC, both **Automerge** and **Loro** have the key features needed (history, branching). Given that marimo already uses Loro, there's an argument for using Loro for the claim graph too — but Loro lacks Python bindings currently. Automerge's Python bindings, while low-level, exist and work. A pragmatic path would be to start with automerge-py and monitor Loro's Python story.

---

## Proposed Architecture Summary

```
┌─────────────────────────────────────────────────┐
│                  marimo notebook                 │
│                                                  │
│  ┌──────────┐  ┌──────────┐  ┌───────────────┐  │
│  │ UI cells │  │Validation│  │  Projection    │  │
│  │ (mutate) │  │  cells   │  │  cells         │  │
│  │          │  │ (MECE,   │  │ (Markdown,     │  │
│  │ add_node │  │  deps,   │  │  Typst, PDF)   │  │
│  │ rm_node  │  │  score)  │  │                │  │
│  │ edit     │  │          │  │                │  │
│  └────┬─────┘  └────▲─────┘  └──────▲────────┘  │
│       │             │               │            │
│       ▼             │               │            │
│  ┌──────────────────┴───────────────┘──────────┐ │
│  │        marimo reactive state bridge         │ │
│  └─────────────────────┬───────────────────────┘ │
│                        │                         │
└────────────────────────┼─────────────────────────┘
                         │
                         ▼
              ┌─────────────────────┐
              │  automerge document  │
              │                     │
              │  - claim nodes      │
              │  - partitions       │
              │  - edges            │
              │  - full history     │
              │  - actor tracking   │
              │                     │
              │  Persisted to disk  │
              │  as .automerge file │
              └──────────┬──────────┘
                         │
            ┌────────────┼────────────┐
            ▼            ▼            ▼
       ┌─────────┐ ┌─────────┐ ┌──────────┐
       │  Human  │ │   LLM   │ │  Other   │
       │  edits  │ │ proposed │ │  agents  │
       │         │ │ changes  │ │          │
       └─────────┘ └─────────┘ └──────────┘
```

---

## Concrete Next Steps (If Pursued)

1. **Spike: automerge-py viability** — Install `automerge-py`, implement the claim graph schema, verify fork/merge/save/load work in practice.

2. **Spike: marimo-automerge bridge** — Build the reactive state bridge (Approach A above). Confirm that automerge mutations trigger marimo cell recomputation.

3. **Define the claim graph schema** — Formalize node types, edge types, and partition rules as an automerge document structure.

4. **Build mutation functions** — `add_claim()`, `add_argument()`, `expand_partition()`, `remove_node()` — all as automerge transactions.

5. **Build validation cells** — MECE checker, dependency checker, structural integrity scorer — all as marimo reactive cells.

6. **Build projection cells** — Markdown and Typst renderers — as marimo output cells.

7. **LLM integration** — Wire an LLM to propose structural mutations via automerge transactions, with human-in-the-loop approval.

---

## The Philosophical Alignment

The inception document's deepest insight is:

> "Never edit prose. Only edit structure."

Automerge enforces this at the data layer. You cannot "edit text" in an automerge document the way you edit a string. You make structured operations on typed data. Every change is explicit, attributed, and reversible.

marimo enforces this at the computation layer. Prose is a derived artifact — a reactive cell output that recomputes from structure.

Together, they create a system where:

- **Structure is the source of truth** (automerge document)
- **Validation is automatic** (marimo reactive cells)
- **Prose is always derived** (marimo projection cells)
- **History is complete** (automerge operation log)
- **Collaboration is native** (automerge CRDT merge)
- **Branching is first-class** (automerge fork/merge)

That's a rhetorical compiler with version control, collaboration, and audit trails built into the foundation — not bolted on later.
