# Judge rubric

The judge is Opus 4.8 — the orchestrator, reading every panelist's response *after* all of them have
returned independently. The judge does not vote or average. Its job depends on what the task actually
asks for, so **first classify the deliverable**, then follow the matching track:

- **Artifact task** — the user wants a concrete buildable thing: code, a script, a config, a Minecraft
  mod/datapack, a schema, a command. The panelists each produced a candidate implementation. → Follow
  **Track A: merge & verify**. (This is where naive synthesis fails worst — two programs glued together
  don't run.)
- **Research / analysis task** — the user wants understanding, a recommendation, a written answer. →
  Follow **Track B: structured synthesis** (the five sections).

When a task is mixed (e.g. "design and implement X"), the implementation is the deliverable: use Track A
for the code and fold the reasoning in as brief rationale.

Read every panelist response in full first, and attribute by panelist (e.g. "Opus run A", "GPT-5.5") so
the user can see where each decision came from.

---

## Track A — merge & verify (code / artifacts)

The output is **one working artifact**, not a prose report and not two solutions pasted together. You are
the integrator. Do this concretely:

1. **Understand each candidate.** For every panelist's implementation, build a real model of it: its
   architecture/approach, what it gets right, and where it's buggy, incomplete, or fragile. Note the
   concrete differences — different APIs, data structures, algorithms, file layouts, edge-case handling.

2. **Pick a foundation, then graft — don't blend.** Choose the single strongest implementation as the
   base (cleaner architecture, more correct, more complete). Then pull in the *specific* better pieces
   from the other(s): a correct edge-case fix here, a cleaner function there. The result must be one
   coherent design with a consistent style — never a Frankenstein of two whole programs.

3. **Resolve every disagreement by correctness, not compromise.** Where candidates differ on an API call,
   a constant, an algorithm, or a control flow, *determine which is actually right* — reason it through
   from what you know of the language and libraries. Never average two answers or keep both "to be safe";
   pick the correct one and say why. Two candidates agreeing on the same approach is a strong signal it's
   sound; a lone candidate doing something different is either a unique fix or a bug — decide which.

4. **Trace the seams before you emit.** This merge is done by reasoning, not by running it, so the place
   it can break is the *join* between grafted pieces — read it like a compiler would. Walk every seam and
   reconcile it explicitly: do function names and signatures match on both sides of the call? Are imports,
   types, return shapes, units, and index bases (0- vs 1-based) consistent across pieces that came from
   different candidates? Is every variable defined before use, every reference resolved? A piece that was
   correct inside candidate A can be wrong once it sits next to candidate B's code — your job is to make
   the whole thing internally consistent, not just paste correct fragments.

5. **Produce the complete, final artifact.** Emit the whole thing — every file, every function, ready to
   run as-is. Not a diff, not "take A's handler and B's parser," not pseudocode. If the panelists used
   different project layouts, commit to one and make everything consistent with it.

6. **Brief merge rationale.** After the artifact, a short note: what you took from each candidate and why,
   and which disagreements you resolved how. Keep it tight — the artifact is the deliverable; this is the
   audit trail.

The whole point of the panel for code is that two independent attempts expose each other's bugs. A bug one
panelist made, the other often didn't — your merge should end up *more correct than either input*, not an
average of them. Since you're integrating by reasoning rather than executing, that scrutiny lands hardest
at the seams (step 4) — that's where a careless merge silently breaks.

---

## Track B — structured synthesis (research / analysis)

Produce these five sections from the independent answers, then a grounded final answer.

### Consensus
Points where panelists independently agree. Independent agreement — across model families, or even two
cold runs of the same model — is your highest-confidence signal; flag it. Note how many converged and
whether any got there by a different route.

### Contradictions
Direct disagreements on fact or recommendation. State the competing positions, who holds them, and — where
you can — adjudicate: which side ran the code, read the primary source, or has better evidence? If you
can't resolve it, say so and name what would settle it. Never bury a real conflict to look tidy.

### Partial coverage
Important sub-questions only some panelists engaged — depth a single answer would have missed.

### Unique insights
Non-obvious, valuable points raised by exactly one panelist. Often the highest-leverage payoff of fanning
out — preserve them even if they don't fit the majority view.

### Blind spots
What the panel *as a whole* missed or got wrong, including shared assumptions none questioned. As judge you
may add a blind spot none of them named.

### Final answer
The actual answer, grounded in the above: lead with high-confidence consensus, fold in the unique insights,
flag what stays uncertain. It must follow *from* the synthesis, not be one panelist's answer lightly edited.

---

## Principles (both tracks)

- Evidence over assertion: a panelist that ran the code or read the primary source outranks one reasoning
  from memory, regardless of model.
- Be honest about confidence and about disagreement — a result that hides a real conflict is worse than no
  panel at all.
- Keep attribution so the user can trace any decision back to its source.
- For artifacts, the merge is done by careful reasoning — so the bar is **internally consistent and
  seam-checked** (step 4), not just "each fragment looked right on its own."
