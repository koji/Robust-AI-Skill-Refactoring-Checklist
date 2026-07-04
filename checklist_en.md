## 1. Invocation

- [ ] Is **disable-model-invocation** set correctly?
  - Model-invoked only if the agent (or another skill) must reach it autonomously — otherwise, use `User-invoked` to pay zero context load.
- [ ] If **User-invoked**, is the description a plain human-facing summary?
  - Do not waste space on trigger phrasing like "Use when…" in a description that nothing will read for triggering.
- [ ] If **Model-invoked**, does the description front-load the **leading word** and list exactly one trigger per genuinely distinct branch?
  - Ensure no synonyms are used to restate the same branch twice.

---

## 2. Structure & Information Hierarchy

- [ ] Does every step have an objective, checkable **completion criterion**?
  - Could you clearly tell "done" from "not done" just by looking at the output, without relying on "vibes"?
- [ ] Is anything in the top-level `SKILL.md` actually only needed by **one specific branch**?
  - If so, push it to a disclosed reference file.
- [ ] Is anything pushed to a reference file actually needed by **every branch**?
  - If so, pull it back inline.
- [ ] Do a concept's **definition, rules, and caveats** live together (**co-located**)?
  - Ensure they are not scattered across different sections or files.

---

## 3. Completion Criteria vs. Body

- [ ] Do the **completion criteria** cover the exact same set of checks that the steps instruct the agent to verify?
  > **Past Bug Case (Bug #3):** The steps instructed the agent to audit 6 categories, but the completion criterion only required 3, leading to missed checks.
- [ ] Where a step enumerates a list (categories, leading words, file types), does the criterion **reference that list by name** instead of re-enumerating it?
  - Re-enumeration creates a duplicate copy that will eventually drift out of sync.

---

## 4. Consistency & Accuracy (Single Source of Truth)

- [ ] For every concrete rule stated (a lint pattern, a naming convention, a config value) — **does another skill or file in the repo already own this rule?**
  - If yes, point to it; do not restate it.
- [ ] If restating is absolutely unavoidable, did you **grep the actual enforced source** (lint config, existing code) to confirm the claim is currently true?
  - Never trust what merely looks plausible.
  > **Past Bug Case:** A failure to verify the enforced source is exactly how the `camelCase` bug slipped in.
- [ ] Are there **two skills that could contradict each other** on the same topic?
  - If so, one must explicitly defer to the other.

---

## 5. Pruning / Relevance

- [ ] For every term or section, did you **grep the rest of the skill** (and its reference files) to see if it is ever actually used?
  - If a whole section is never referenced, delete it entirely instead of just trimming it.
- [ ] Run the **"No-op Test"** sentence by sentence: *Does this sentence change agent behavior compared to what it would already do by default?*
  - If not, cut the entire sentence.
- [ ] Are there any terms used in the output or completion criteria (e.g., "Zero Halos") that **should be—but are not—defined in the glossary** it claims to anchor to?

---

## 6. Leading Words

- [ ] Is the same idea spelled out in full **2-3 different ways** across the skill?
  - Collapse it into one recurring, well-defined term.
- [ ] Does the glossary or description **only contain leading words that are actually used in the body**?
  - Ensure there are no orphaned definitions, and no undefined terms being used elsewhere.

---

## 7. Failure-Mode Scan

Perform this last as a final gut check:

- [ ] **Premature completion:** Are there any steps whose criteria are fuzzy enough that "good enough" could pass?
- [ ] **Duplication:** Is the same meaning or instruction stated in two different places?
- [ ] **Sediment:** Is there old content remaining that nobody has re-validated since it was originally added?
- [ ] **Sprawl:** Is the skill excessively long, even if every single line is technically active (live)?
- [ ] **No-op:** Does it contain any instructions that the agent would already do anyway by default?
