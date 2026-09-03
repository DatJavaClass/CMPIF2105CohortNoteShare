# Cliff Jumper Notes

## What these are

A Cliff Jumper Note takes all the separate Cliff Notes from one module and
weaves them into a single, connected lesson. Instead of a stack of short notes
that repeat the same ideas, you get one walkthrough that introduces each idea
once and builds on it. Where it helps, a small amount of extra explanation or
example is added, but only when it is correct and it is labeled as added.

One thing specific to this course: the math is explained for a reader who is
comfortable applying it and less comfortable with the theory behind it. Every
idea arrives attached to a concrete case, and the case is the same one all the
way through a module. The examples are the notes' own, built to teach the same
lesson the lectures teach. The facts, terms, and quiz-keyed statements stay
faithful to the source. Anything that goes beyond what a lecture said is
marked "(added)".

## How they are checked: "Who Watches the Watchmen"

These notes were drafted with AI help, so every one was put through a checking
system to catch mistakes before it went in here. It works in a diamond shape:

1. The note is written.
2. Two separate checkers read it independently and hunt for errors. One checks
   that every claim matches the source Cliff Notes and the lecture transcripts.
   The other checks the technical side and actually runs every code block and
   recomputes every number to confirm it is right.
3. A third checker reviews the first two. This is the "who watches the
   watchmen" step. It catches anything the first two missed and throws out any
   complaint they got wrong.

Anything the checkers flagged was fixed, and the fix was checked again. All
code and all numbers in these notes were run, not guessed.

## Confidence of Accuracy

| Module | Confidence of Accuracy | Basis |
|---|---|---|
| 2 | High (~95%) | Every number and code block run. The nine code blocks re-executed in order under numpy 2.5.2 and sympy 1.14.0, with every printed line matching byte for byte. The substitution chain, all six augmented-matrix states, the inverse, the determinants of the healthy and the doubled-row matrices, the rank-3 rescue, the two-equation alternate solution, the binary carry walk, the 2x2 closed form, and the rotation and toy-lens maps recomputed exactly with sympy. All 42 statements attributed to the lecture traced to transcript phrases. The fidelity auditor caught one sentence that labeled the running example as a stand-in for the lecture's and a hedged float that the lecture states exactly. The execution auditor caught a skipped divide-by-3 in one reduction step and a peril-one sentence that read as a plane where the added text said a line. The meta-auditor overturned a request to correct the professor's description of a NumPy function (the notes report the lecture as given), a false alarm on a markdown table separator, and a flag on a definition the lecture demonstrates. Nine single-sentence edits applied and re-verified |

## A note of caution

This is still a study aid, not a textbook. Use it to understand and review, but
check anything important against the actual course material, especially before
a quiz.

## Contributors

- **DatJavaClass (Victor S)**, author and director. Conceived these notes, established their format and structure, directed their creation, and fact-checked, edited, and quality-controlled every one, with assistance by Claude. Some material may have been derived from assigned material, but has not been copied verbatim. For source materials please contact CMPINF-2105 Faculty and Assistants.
