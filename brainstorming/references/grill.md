# GRILL

A question is asked only if it guarantees all of:

1. **One decision.** Resolves exactly one design decision; never bundled.
2. **Not answerable by repo.** Code, tests, config, docs checked first.
3. **Changes the design.** Different answers lead to different designs; otherwise assume and tag `[assumed]`.
4. **Context.** States why it matters or what breaks if wrong.
5. **Trade-offs.** Whatever the answer space (choices, multi-select, free text), the effect, gain, and cost of each possible answer are clear to the user.
6. **Recommendation.** Gives your suggested answer and when another would win.
7. **Default.** States what will be assumed if unanswered.

Format is free: pick the one that fits (single select, multi select, free text). Any question missing one of the above is not asked.

Gate asks (approve, re-approve, skip-design) are GRILL questions too. Their default when unanswered is always "stay blocked", never proceed.