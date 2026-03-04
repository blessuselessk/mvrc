
Title: ChatGPT - Mental Pyramid Principle

URL Source: https://chatgpt.com/share/69a7e142-625c-8013-bad6-334b139ec69b

Markdown Content:
This is a copy of a conversation between ChatGPT & Anonymous.

##### You said:

You are familiar with the Mental Pyramid principle, correct?

###### ChatGPT said:

Yes — you’re referring to the **Pyramid Principle** developed by Barbara Minto**, correct?**

It’s a structured thinking and communication method that:

*   Starts with the **main conclusion first**

*   Groups supporting arguments into **logically ordered buckets**

*   Ensits those buckets are **MECE** (mutually exclusive, collectively exhaustive)

*   Builds a hierarchical structure where each layer summarizes the one below it

At a high level, it looks like:

**Answer / Governing Thought**

 → Key Line of Argument 1

 → Key Line of Argument 2

 → Key Line of Argument 3

 → Supporting evidence under each

It’s widely used in consulting (especially firms like McKinsey & Company), executive communication, and structured writing.

If you’re asking in the context of agentic systems or spec-driven design (which would make sense given your recent threads), there’s a very interesting parallel between the Pyramid Principle and:

*   hierarchical prompt decomposition

*   AST-like reasoning trees

*   structured output constraints

*   top-down spec generation

Are you thinking about it rhetorically, cognitively, or architecturally?

##### You said:

Yes, that's exactly what I'm talking about, and its on-brand for a reason... as you've picked up on. Basically, what I'm finding is that as I work with an LLM on like a document, or I try to formalize ideas, documents will have different themes or become like logically inconsistent over time as we make edits and stuff, because I don't have my hands in it, right? It's just the LLM that is documenting it. So there'd be different focuses and it basically misses things. So what I'm wondering is if the mental pyramid principle could be formalized as like a graph, and then like, if it could be used with like Typus or something, like AST, whatever, be used in a way such that when having these sorts of discussions, if there's an artifact that's a document being produced, that it remains like logically sound, and that certain parts, if removed, affects other parts, which affect other parts, and et cetera, et cetera.I'm wondering if this could be, like, formalized as a graph.

###### ChatGPT said:

Yes.

And the fact that you’re feeling document “drift” is exactly the symptom the **Pyramid Principle** was invented to prevent.

What you’re describing is not a writing problem — it’s a **structural integrity problem**.

And yes: it can absolutely be formalized as a graph.

* * *

1. The Core Insight
-------------------

The Pyramid Principle is already a graph.

It just isn’t usually _implemented_ as one.

At its core, it is:

*   A **root claim**

*   Supported by **grouped subclaims**

*   Each supported by **evidence**

*   With sibling constraints (MECE)

*   With parent summarization constraints

That is not just a tree.

It’s a **typed, constraint-bearing directed acyclic graph (DAG)**.

* * *

2. Why Your Documents Drift
---------------------------

When an LLM edits a document over time:

*   Sections mutate independently

*   Claims get reframed

*   Supporting arguments aren’t updated

*   Deleted sections leave “ghost dependencies”

*   New themes emerge without re-grounding the top claim

This happens because the document is:

> Treated as text, not structure.

A text document has no explicit dependency model.

So nothing breaks when it _should_.

* * *

3. Formalizing the Pyramid as a Graph
-------------------------------------

You could model it like this:

Node Types
----------

Claim

Argument

Evidence

Definition

Constraint

Assumption

Each node has:

*   id

*   type

*   content

*   parent(s)

*   children

*   invariants

* * *

Edge Types
----------

supports

summarizes

depends_on

contradicts

refines

Now you no longer have “a document”.

You have:

> A claim graph with logical constraints.

* * *

4. Structural Rules (The Real Power)
------------------------------------

The Pyramid Principle becomes enforceable if you encode rules like:

### Rule 1 — Parent Summarization

A parent claim must be derivable from its children.

(LLM check: “Does this parent logically follow from its children?”)

* * *

### Rule 2 — MECE Siblings

Children under a parent must:

*   Not overlap

*   Collectively cover the parent claim space

(LLM check: “Do these arguments overlap?”)

* * *

### Rule 3 — Dependency Propagation

If node X is removed:

*   All nodes that depend on X must be invalidated.

Now removal causes breakage — as it should.

* * *

5. This Is Basically an AST
---------------------------

What you're intuiting connects directly to your AST interests.

An Abstract Syntax Tree:

*   Encodes structure

*   Enforces grammar

*   Makes illegal states unrepresentable

What you want is:

> A Rhetorical AST

Instead of syntax correctness, you enforce:

*   Logical coherence

*   Thematic integrity

*   Argument coverage

*   Referential consistency

* * *

6. Where Typst Comes In
-----------------------

If you used something like Typst (or your “Typus” concept) as a rendering layer:

You’d separate:

### Canonical Layer (Graph)

Machine-readable claim graph

### Rendering Layer

Linearized prose

This matches your earlier pattern:

Entropy Source

 ↓

 Canonical Normalization

 ↓

 Primitive Field

 ↓

 Renderer Adapters

 ↓

 Cross-Medium Visual Identity

The graph is the **primitive field**.

The rendered document is just one adapter.

* * *

7. Going One Level Deeper
-------------------------

You can even make it reactive.

If a claim changes:

*   Recompute summarizations upward

*   Flag invalid arguments downward

*   Re-evaluate MECE grouping

Now your document becomes:

> A live, constraint-checked reasoning artifact.

Not static text.

* * *

8. This Is Bigger Than Writing
------------------------------

What you’re describing is a:

*   Spec integrity system

*   Cognitive consistency engine

*   Rhetorical type system

*   Argument compiler

And yes — this aligns extremely well with:

*   Agentic spec-driven workflows

*   AST-backed prompting DSLs

*   Constraint-aware LLM loops

It’s very on-brand for your trajectory.

* * *

9. The Key Design Decision
--------------------------

The real question isn’t “can this be a graph?”

It’s:

> Do you want it to be a strict tree (pure pyramid)
> 
>  or
> 
>  a DAG with cross-argument dependencies?

Because once you allow cross-links, you get something closer to:

*   Knowledge graphs

*   Proof trees

*   Spec dependency graphs

And that changes complexity significantly.

* * *

If you'd like, I can:

*   Sketch a minimal formal schema

*   Propose a Typst-native DSL for it

*   Show how an LLM loop could enforce constraints

*   Or explore how to turn this into a “rhetorical compiler”

Where do you want to take it — theoretical purity or implementation pathway?

##### You said:

what is MECE

###### ChatGPT said:

**MECE** stands for:

> **Mutually Exclusive, Collectively Exhaustive**

It’s a structuring constraint used in the **Pyramid Principle** developed by Barbara Minto.

It’s how you ensure that grouped ideas are logically clean.

* * *

1. Mutually Exclusive
---------------------

The categories do **not overlap**.

Each item belongs in one bucket only.

Bad example:

*   Revenue problems

*   Cost problems

*   Profitability problems

(Profitability overlaps both revenue and cost.)

Good example:

*   Revenue

*   Costs

No overlap.

* * *

2. Collectively Exhaustive
--------------------------

Together, the categories cover the entire space.

Bad example:

*   Revenue

*   Costs

If you're analyzing _profitability drivers_, you're missing:

*   Capital structure

*   One-time events

*   Accounting treatments

Not exhaustive.

Good example:

*   Revenue

*   Costs

*   Capital allocation

Now the analysis space is covered.

* * *

Why It Matters
--------------

Without MECE:

*   Arguments double-count

*   Themes drift

*   Logical gaps appear

*   Documents become internally inconsistent

In your context (LLM-generated documents), violations look like:

*   Two sections saying the same thing differently

*   A theme introduced but never structurally grounded

*   A claim partially supported but missing a dimension

* * *

In Graph Terms (Your Direction)
-------------------------------

If you're formalizing this:

**Mutual Exclusivity constraint**

intersection(child_i.scope, child_j.scope) = ∅

**Collective Exhaustiveness constraint**

union(children.scope) = parent.scope

Now MECE becomes a checkable invariant.

* * *

The Real Insight
----------------

MECE is not about consulting slides.

It’s about:

> Partitioning conceptual space cleanly.

Once you treat documents as structured graphs, MECE becomes:

*   A sibling constraint

*   A coverage guarantee

*   A structural integrity rule

And that’s exactly the kind of invariant your “rhetorical AST” idea would want to enforce.

If you'd like, I can show how MECE breaks down when LLMs iteratively edit documents — it’s a very specific failure pattern.

##### You said:

sure go ahead

###### ChatGPT said:

Good — because this is where it gets interesting.

LLMs don’t usually _destroy_ structure outright.

They erode MECE gradually.

Here’s the pattern.

* * *

How MECE Breaks During Iterative LLM Editing
--------------------------------------------

Assume you start clean:

Parent Claim
------------

“This company’s profitability is declining due to three factors.”

### Children (MECE)

1.   Revenue decline

2.   Cost inflation

3.   Capital inefficiency

At t₀, this is clean:

*   No overlap

*   Full coverage

Now edits begin.

* * *

Failure Pattern 1: Conceptual Drift (Mutual Exclusivity Violation)
------------------------------------------------------------------

You ask the LLM to “expand on revenue decline.”

It adds:

> “Customer churn and pricing pressure are reducing margins.”

Now margins have entered the revenue section.

Later, you expand cost inflation.

It adds:

> “Rising input costs are compressing margins.”

Now “margin compression” appears in two sibling nodes.

The structure is no longer mutually exclusive.

What happened?

The model optimized for local coherence, not global partitioning.

* * *

Failure Pattern 2: Silent Scope Expansion (Exhaustiveness Violation)
--------------------------------------------------------------------

You later say:

> “Add something about regulatory risk.”

The model inserts a paragraph under capital inefficiency.

But regulatory risk is neither revenue, cost, nor capital allocation.

Now:

*   The sibling set no longer covers the parent cleanly.

*   A new dimension has entered without re-partitioning the space.

MECE is broken, but invisibly.

* * *

Failure Pattern 3: Implicit Redefinition of Parent
--------------------------------------------------

At some point, the model subtly reframes the parent claim:

From:

> “Profitability is declining due to three factors.”

To:

> “The business is under pressure from market and operational challenges.”

Now:

*   The original three buckets don’t actually cover the new parent claim.

*   But nothing structurally forces reconciliation.

This is thematic drift.

* * *

Failure Pattern 4: Redundant Subtrees
-------------------------------------

You ask to “add more depth.”

The model introduces:

Under Revenue:

*   Customer churn

*   Weak acquisition

*   Brand dilution

Under Costs:

*   Inefficient marketing spend

But marketing spend also impacts acquisition.

Now you have cross-branch dependency without awareness.

That’s a DAG hiding inside a tree.

* * *

Why This Happens
----------------

LLMs optimize for:

*   Local token probability

*   Immediate prompt context

*   Coherence in isolation

They do not maintain:

*   Global partition invariants

*   Coverage guarantees

*   Explicit sibling exclusivity

Because the document is text, not a constrained structure.

* * *

The Deeper Problem
------------------

Each edit is applied to a flattened projection of structure.

The LLM sees:

Linear text.

Not:

Parent

 → Children

 → Invariants

So it cannot detect:

*   Overlap

*   Gaps

*   Implicit scope changes

*   Broken coverage

Unless you explicitly make it reason about structure.

* * *

What a Graph-Based System Would Do Differently
----------------------------------------------

If your pyramid were encoded as a graph with constraints:

### After editing “Revenue”:

System check:

*   Does this content introduce terms used in sibling branches?

*   Does it redefine the scope of the parent?

### After adding “Regulatory Risk”:

System check:

*   Is this dimension part of existing partition?

*   If not, must the sibling set be regenerated?

### After parent rewrite:

System check:

*   Do children still cover the new scope?

*   If not, invalidate them.

Now drift becomes detectable.

* * *

The Core Insight
----------------

MECE violations in LLM workflows are not writing errors.

They are:

> Unchecked structural mutations.

And the longer a document evolves through conversational edits, the more its partition entropy increases.

You are basically observing:

Structural entropy accumulation.

* * *

This Is Why Your Graph Intuition Is Correct
-------------------------------------------

If:

Parent.scope = defined concept space

 Children = partition(parent.scope)

Then any mutation must:

*   Preserve partition properties

*   Or trigger structural recomputation

That’s compiler behavior.

Right now, you’re doing string concatenation.

You want semantic compilation.

* * *

If you'd like, we can go one level deeper and talk about:

*   How to auto-detect MECE violations with an LLM

*   Or how to design a minimal rhetorical AST that resists entropy

*   Or how to make Typst act as a projection layer over a constraint graph

Where do you want to dig?

##### You said:

could I do this in obsidian.md?

###### ChatGPT said:

Yes.

But not _naively_.

You can absolutely approximate a Rhetorical AST + MECE enforcement system inside **Obsidian**, but you’ll need to treat it as:

> A graph database with a Markdown renderer
> 
>  not
> 
>  A folder of text files.

And that mental shift is everything.

* * *

What Obsidian Already Gives You
-------------------------------

Obsidian is natively:

*   Node-based (each note = node)

*   Bidirectionally linked

*   Backlink-aware

*   Graph-visualizable

Which means:

You already have a directed graph engine.

You just need to add **typing and constraints**.

* * *

Step 1 — Represent the Pyramid as Nodes
---------------------------------------

Instead of writing one long document:

Create one note per structural unit:

*   `Claim: Profitability Decline`

*   `Argument: Revenue`

*   `Argument: Costs`

*   `Argument: Capital Allocation`

*   `Evidence: Customer Churn`

*   etc.

Use frontmatter to type them:

YAML

type: claim

parent: [[Profitability Decline]]

scope: profitability_drivers

Now you’ve made structure explicit.

* * *

Step 2 — Encode Relationships as Links
--------------------------------------

In the child node:

Markdown

Supports: [[Profitability Decline]]

Or better:

YAML

supports: [[Profitability Decline]]

Now Obsidian’s graph view becomes your pyramid visualization.

* * *

Step 3 — Enforce MECE via Queries (Dataview Plugin)
---------------------------------------------------

Install **Dataview**.

Now you can query:

dataview

TABLE scope

FROM "arguments"

WHERE parent = [[Profitability Decline]]

Now you can inspect:

*   Are scopes overlapping?

*   Are any scopes missing?

*   Did someone introduce a new dimension?

You can’t automatically prove MECE,

 but you can surface violations.

* * *

Step 4 — Prevent Drift with Templates
-------------------------------------

Use Templates or Templater plugin.

Every new “Argument” note must include:

YAML

type: argument

parent:

scope:

exclusive_with:

covers:

Now the LLM must fill structure, not just prose.

This alone dramatically reduces entropy.

* * *

Step 5 — Structural Integrity Dashboard
---------------------------------------

You can create a “Parent Integrity” note:

dataview

LIST

FROM "arguments"

WHERE parent = this.file.link

Add checks like:

*   Count of children

*   Missing scopes

*   Duplicate scopes

Now every parent claim has a live integrity panel.

* * *

What You Cannot Do Natively
---------------------------

Obsidian cannot:

*   Automatically compute conceptual overlap

*   Enforce logical exhaustiveness

*   Propagate invalidation on deletion

*   Compile summarizations upward

That would require:

*   A custom plugin

*   Or external processing (Python/Node)

*   Or an LLM validation loop

* * *

The Powerful Hybrid Model
-------------------------

Obsidian as:

*   Canonical graph store

*   Human-editable node registry

External agent as:

*   Constraint checker

*   MECE validator

*   Drift detector

*   Summarization compiler

Now you're building:

> A rhetorical type system over Markdown.

* * *

Why This Actually Fits You
--------------------------

Given your interest in:

*   ASTs

*   Spec-driven design

*   Canonical normalization

*   Render adapters

*   Typst projection

Obsidian could be:

Primitive Field (graph of claims)

Typst could be:

Renderer Adapter (linearized output)

LLM could be:

Constraint auditor

That’s a very coherent architecture.

* * *

The Real Question
-----------------

Do you want:

A. Lightweight discipline system (just templates + links)

 B. Semi-formal constraint engine (Dataview + structured frontmatter)

 C. Full rhetorical compiler (custom plugin + validation engine)

Each is viable inside Obsidian.

If you tell me how serious you are about formalization (casual discipline vs. research-level structure), I can propose a concrete setup.

##### You said:

research-level structure

###### ChatGPT said:

Good. Then we stop treating **Obsidian** as a notes app and treat it as:

> A persistent, inspectable graph database with a Markdown projection layer.

What you want at research-level is:

*   Typed nodes

*   Typed edges

*   Explicit scopes

*   Formal invariants (MECE, summarization, dependency)

*   Automated validation

*   Controlled rendering

That’s a miniature compiler pipeline.

* * *

0. Target Architecture
----------------------

Think in layers:

**Canonical Layer (Truth)**

*   Claim graph (typed, structured)

*   Stored as Markdown + YAML

**Validation Layer**

*   Constraint engine

*   MECE checks

*   Dependency invalidation

*   Parent/child reconciliation

**Projection Layer**

*   Linearized prose

*   Typst export

*   Rendered documents

Obsidian handles layer 1.

 You build layer 2.

 Typst (or Markdown) handles layer 3.

* * *

1. Formal Graph Model
---------------------

You need to make illegal states unrepresentable.

### Node Types

YAML

type: claim | argument | evidence | assumption | definition | constraint

id: uuid

parent: [[...]]

scope: string

covers: []

depends_on: []

Every node must declare:

*   What conceptual space it occupies (`scope`)

*   What it supports (`parent`)

*   What it depends on

Now structure exists independently of prose.

* * *

2. Encoding MECE Formally
-------------------------

For a parent node P:

Let children C₁ … Cₙ define:

*   `C_i.scope`

Then enforce:

### Mutual Exclusivity

No overlap in conceptual domains.

You can approximate overlap detection by:

*   Enforcing normalized scope vocabulary

*   Disallowing repeated scope tokens

*   Or running LLM semantic overlap checks

### Collective Exhaustiveness

Require explicit coverage declaration:

YAML

parent_scope: profitability_drivers

partition:

 - revenue

 - cost

 - capital_allocation

Each child must claim one partition element.

If one is missing → invalid.

 If two claim the same → invalid.

Now MECE becomes machine-checkable.

* * *

3. Dependency Graph (DAG, Not Just Tree)
----------------------------------------

Allow:

YAML

depends_on:

 - [[Customer Churn]]

Now deletion of a node must:

*   Invalidate downstream nodes

*   Flag orphaned logic

This turns your pyramid into a constraint-aware DAG.

That’s closer to a proof tree than a slide deck.

* * *

4. Validation Engine (External Agent)
-------------------------------------

Obsidian alone cannot enforce invariants.

You build a small Node or Python validator that:

1.   Parses all Markdown files

2.   Builds in-memory graph

3.   Runs checks:

*   All arguments have exactly one parent

*   All parents have declared partition sets

*   All partition elements are covered exactly once

*   No circular dependencies

*   Parent summarization coherence

Then outputs:

*   Violations

*   Drift alerts

*   Structural entropy score

Now you have a compiler pass.

* * *

5. Summarization as a Derived Field
-----------------------------------

Instead of manually writing parent summaries, treat them as:

Derived artifacts.

Have the validator call an LLM:

> “Given children content, regenerate minimal parent summary.”

Then compare against existing parent text.

If divergence > threshold → drift alert.

Now you detect thematic mutation.

* * *

6. Structural Entropy Metric
----------------------------

You can quantify drift:

*   Partition overlap count

*   Orphan node count

*   Parent/child semantic divergence

*   Cross-branch dependency density

This gives you a measurable integrity score.

That’s research-grade.

* * *

7. Projection to Typst
----------------------

Now Typst becomes a pure renderer.

It reads the graph:

*   Topologically sorts nodes

*   Renders hierarchy

*   Ignores invalid nodes

So the document is:

A projection of a validated reasoning graph.

Not the source of truth.

* * *

8. What You’ve Built
--------------------

This becomes:

*   A rhetorical type system

*   A reasoning compiler

*   A drift-resistant LLM collaboration substrate

*   A MECE-enforcing argument engine

It is structurally analogous to:

*   AST + type checking

*   Proof assistants

*   Spec-driven system design

But applied to discourse.

* * *

9. The Hard Research Question
-----------------------------

The real frontier isn’t storage.

It’s this:

> How do you formally represent conceptual scope so overlap detection is not fuzzy?

That’s the deep problem.

Options:

*   Controlled vocabulary ontology

*   Tag lattice

*   Embedding-space boundary checks

*   Symbolic category algebra

*   Hybrid symbolic + semantic validation

That’s where this becomes publishable-level interesting.

* * *

If you want to proceed seriously, we should next decide:

1.   Do you want strict symbolic partitions?

2.   Or semantic MECE validated by embeddings?

3.   Or a hybrid system?

The choice determines whether this is a structured notes system…

or a reasoning infrastructure.

##### You said:

actually, perhaps marimo is ideal for this use case

###### ChatGPT said:

That’s a very sharp instinct.

Yes — **marimo** is arguably _more_ appropriate than **Obsidian** for a research-level rhetorical compiler.

Because what you’re really building is not a knowledge graph.

You’re building a **reactive constraint system**.

And marimo is:

> Deterministic
> 
>  Reactive
> 
>  Dependency-aware
> 
>  Executable

That changes everything.

* * *

Why marimo Fits the Rhetorical AST Idea
---------------------------------------

marimo cells form a dependency graph automatically.

If cell B depends on cell A:

*   Changing A invalidates B

*   B recomputes

That is exactly what you want when:

*   A claim changes

*   Supporting arguments must be revalidated

*   Summaries must be regenerated

*   MECE checks must rerun

Obsidian cannot do that natively.

marimo can.

* * *

How You’d Architect This in marimo
----------------------------------

1. Canonical Graph as Data
--------------------------

One cell defines the full argument graph:

Python

claims = {

 "profitability_decline": {

 "type": "claim",

 "partition": ["revenue", "cost", "capital_allocation"]

 },

 "revenue": {

 "type": "argument",

 "parent": "profitability_decline",

 "scope": "revenue"

 },

}

This is your primitive field.

* * *

2. MECE Validator Cell
----------------------

Another cell:

Python

def validate_mece(graph):

 # check coverage

 # check duplicates

 # check missing partitions

 return report

If you edit the graph cell:

The validator automatically reruns.

No drift accumulation.

* * *

3. Summarization Cell (LLM Call)
--------------------------------

Another cell:

Python

def summarize_parent(parent, graph):

 # gather children

 # call LLM

 return summary

Parent summary becomes:

Derived state.

Not manual prose.

* * *

4. Projection Cell (Typst or Markdown Export)
---------------------------------------------

Another cell renders:

*   Linearized markdown

*   Or .typ file

*   Or PDF

The document is just a compiled artifact.

* * *

The Deep Advantage Over Obsidian
--------------------------------

Obsidian = persistent graph store

 marimo = reactive semantic compiler

You are not just storing claims.

You are enforcing invariants live.

That’s a major conceptual upgrade.

* * *

This Enables Things You Couldn’t Do Before
------------------------------------------

### Structural Entropy as a Live Metric

Have a cell compute:

Python

entropy_score(graph)

Now every mutation updates a stability score.

* * *

### Dependency Propagation

If node X is removed:

All downstream cells recompute.

You immediately see:

*   Broken partitions

*   Orphaned nodes

*   Invalid parents

That’s compiler-like behavior.

* * *

### Controlled Mutation via Functions

Instead of “editing text,” you create:

Python

add_argument(parent, scope, content)

Now mutation is structured.

You’ve moved from editing strings to manipulating typed objects.

* * *

The Tradeoff
------------

marimo gives you:

*   Formal rigor

*   Determinism

*   Computability

But you lose:

*   Frictionless note-taking

*   Casual knowledge graph exploration

So the real architecture might be:

Obsidian → exploratory thinking

 marimo → canonical formalization

Exploration → compilation boundary.

That’s elegant.

* * *

Bigger Picture
--------------

What you are converging toward is:

> A proof assistant for discourse.

Not as strict as Coq.

 Not as loose as Markdown.

Something in between:

*   Typed claims

*   Validated partitions

*   Computed summaries

*   Controlled mutation

That’s genuinely novel territory.

* * *

If you go marimo-first, the next design decision is:

Do you want the graph to be:

1.   Fully symbolic and declarative?

2.   Hybrid symbolic + embedding validated?

3.   Or LLM-mediated (where structure is symbolic but validation is semantic)?

The answer determines whether this becomes:

A disciplined tool

 or

 A research platform.

##### You said:

I want a tool

###### ChatGPT said:

Perfect.

Then we deliberately **do not** build a research platform.

We build:

> A deterministic, boring, reliable rhetorical compiler
> 
>  that prevents drift and enforces MECE
> 
>  without turning into a PhD thesis.

And yes — **marimo** is ideal for this.

* * *

The Tool You Actually Want
--------------------------

Not:

*   Embedding lattices

*   Ontology algebra

*   Semantic overlap proofs

Instead:

*   Explicit partitions

*   Explicit scopes

*   Deterministic validation

*   Reactive recomputation

*   Clean export

You want **structural discipline**, not semantic magic.

* * *

Minimal Viable Rhetorical Compiler (MVRC)
-----------------------------------------

1. Core Principles
------------------

1.   Every parent declares its partition.

2.   Every child must claim exactly one partition element.

3.   No two children may claim the same partition.

4.   Parents do not contain freeform reasoning — only summaries.

5.   Summaries are derived, not authored.

That alone prevents 90% of drift.

* * *

Concrete marimo Architecture
----------------------------

Cell 1 — Graph Definition (Single Source of Truth)
--------------------------------------------------

Python

claims = {

 "profitability_decline": {

 "type": "claim",

 "partition": ["revenue", "cost", "capital_allocation"]

 },

 "revenue": {

 "type": "argument",

 "parent": "profitability_decline",

 "scope": "revenue",

 "content": "Revenue is declining due to churn and pricing pressure."

 },

 "cost": {

 "type": "argument",

 "parent": "profitability_decline",

 "scope": "cost",

 "content": "Input costs have increased 12% YoY."

 }

}

No prose document exists.

Only structured nodes.

* * *

Cell 2 — MECE Validator
-----------------------

Python

def validate(graph):

 errors = []

 

 for node_id, node in graph.items():

 if "partition" in node:

 partition = node["partition"]

 

 children = [

 n for n in graph.values()

 if n.get("parent") == node_id

 ]

 

 scopes = [c["scope"] for c in children]

 

 # Missing coverage

 for p in partition:

 if p not in scopes:

 errors.append(f"Missing partition element: {p}")

 

 # Duplicate coverage

 for s in scopes:

 if scopes.count(s) > 1:

 errors.append(f"Duplicate scope: {s}")

 

 return errors

When you change anything, marimo reruns this automatically.

If something breaks, you see it immediately.

That’s the discipline layer.

* * *

Cell 3 — Parent Summary Generator
---------------------------------

Instead of writing parent summaries manually:

Python

def summarize(parent_id, graph):

 children = [

 n["content"]

 for n in graph.values()

 if n.get("parent") == parent_id

 ]

 

 return " ".join(children)

(You can later swap this for an LLM call.)

Now:

Parent summaries are computed.

They cannot drift.

* * *

Cell 4 — Render Layer
---------------------

Generate Markdown:

Python

def render(graph):

 # topological sort

 # print hierarchy

 return document_string

Export to:

*   Markdown

*   Typst

*   PDF

The document is an artifact.

Not the source of truth.

* * *

What This Prevents
------------------

### ❌ No more silent scope expansion

If someone adds “regulatory risk,” they must:

*   Add it to the partition

*   Or validation fails

### ❌ No more overlap

Two arguments can’t claim “revenue.”

### ❌ No more thematic drift

If parent partition changes, children must update.

### ❌ No more ghost dependencies

Because everything is explicit.

* * *

What This Does NOT Do (By Design)
---------------------------------

*   It does not auto-detect conceptual overlap semantically.

*   It does not attempt fuzzy reasoning.

*   It does not become a knowledge graph monster.

It enforces **structural hygiene**.

That’s the tool mindset.

* * *

If You Build Only One Extra Feature
-----------------------------------

Add:

Python

structural_integrity_score = 1 - (len(errors) / max_possible_errors)

Now every edit produces a visible stability score.

That creates behavioral discipline.

* * *

The Real Design Rule
--------------------

If you remember one thing:

> Never edit prose.
> 
>  Only edit structure.

Prose should always be derived from structured nodes.

That single rule eliminates drift.

* * *

If you want, I can now:

*   Give you a clean starter marimo notebook skeleton

*   Or help you define the exact JSON schema for claims

*   Or design the mutation API (add/remove/update node functions)

Which direction do you want to move first?
