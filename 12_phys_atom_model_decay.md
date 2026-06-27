# Tool 12 — The Atom: Evolving Model & Radioactive Decay

**Spec:** AQA Combined Science Trilogy · Physics Topic 21 (Atomic structure) **Dual-coding target:** (a) the *historical evolution* of the atomic model as a sequence showing what each experiment changed (the "models evolve with evidence" working-scientifically point); (b) alpha/beta/gamma decay as before→after nuclear diagrams showing exactly what leaves.

---

## Build prompt

Build a single-file interactive HTML tool with **two clearly separated modes** (a simple tab switcher), called **"Inside the Atom."** Keep the two modes fully separate so neither adds load to the other.

### Mode A — "How our model changed"

A horizontal timeline of atomic models the learner steps through:

- **Plum pudding** (positive dough, electrons embedded) →  
- **Nuclear** (Rutherford — dense positive nucleus, mostly empty space) →  
- **Bohr** (electrons in fixed shells). For each step, show the model AND a plain card: *what experiment* prompted it and *what changed*. The dual code: the *visible change in the diagram* is paired with the *evidence that forced it* — teaching that models update with data.  
- Include a tiny optional alpha-scattering mini-visual for the plum-pudding→nuclear jump (most particles pass through, few bounce back) — behind a "see why" button so it's optional depth, not default load.

### Mode B — "Radioactive decay"

A single nucleus (protons \+ neutrons, clearly counted) that the learner makes decay, one type at a time:

- **Alpha:** a 2-proton-2-neutron chunk visibly leaves; mass number −4, atomic number −2. Show the numbers change live.  
- **Beta:** a neutron turns into a proton, a fast electron shoots out; mass number same, atomic number \+1.  
- **Gamma:** energy leaves as a wave; numbers unchanged. The dual code: the *particle/energy you watch leave* is paired with the *change in the mass/atomic numbers* — so the notation describes something seen.

**Interaction (both modes):**

- "Next" / decay-type buttons trigger one change at a time; *Back / Reset* always present.  
- Live atomic-number / mass-number readout updates as decay happens, with one plain sentence each.  
- `aria-live` announces each change.

**Explicit takeaways:** Mode A — "Scientists changed the model when new evidence appeared." Mode B — "In decay, something leaves the nucleus. What leaves decides how the numbers change."

**Boundaries:** the mode tabs are large and unmistakable; within each mode, the diagram is the stage, the explanation in its own strip, controls in a separate rail. Never show both modes at once.

Apply the central design system in full.  
