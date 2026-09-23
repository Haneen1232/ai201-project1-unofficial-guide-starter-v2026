# Acceptance criteria — The Unofficial Guide

Five criteria that say what "working" means for this system, written in unit 1
**before** any results existed.

An acceptance criterion names a target: a number, a count, a rate, or something
a person could plainly observe. *"Retrieval works"* is an opinion. *"For at
least 4 of my 5 test questions, the top results include a chunk containing the
answer"* is a criterion.

Under each one, write a sentence or two on **why that target** and not a
stricter or looser one. A reason that says something about your corpus or your
pipeline earns credit; *"80% seemed reasonable"* does not.

> Missing your own targets next unit costs you nothing. Setting a target so
> easy you can't miss it does.

---

## 1. Retrieved chunks contain the answer

For at least 4 of my 5 test questions, the retrieved chunks include one that
contains the answer.

**Why this target:**
This is the most basic proof retrieval works at all. If the right chunk isn't
even coming back, nothing downstream matters. 4 of 5 leaves room for one
edge case question without treating retrieval as broken, since one of my
questions (about noise policy) scored a higher distance (0.402) than the
others and could plausibly miss.

---

## 2. Every answer names a source

Every answer the system produces names at least one source document.

**Why this target:**
This is all five and not four of five on purpose. The source line is
generated automatically by the pipeline for every answer that passes the
relevance gate, so it does not depend on how hard the question is. If this
ever fails, it means something is broken in the code, not that a question
was simply difficult.

---

## 3. The relevance gate stops out-of-corpus questions

When I ask a question my documents clearly don't cover, the relevance gate
stops it and the system returns "I don't have enough information about that" —
in at least 4 of 5 tries.

**Why this target:**
My in-scope test questions all had distances between 0.248 and 0.402, while
my out-of-scope questions are on completely unrelated topics (a diesel
engine, the 1994 World Cup, Rust syntax) that should score far above my
cutoff of 0.6. I expect a clean gap between the two groups, so 4 of 5 leaves
room for one borderline case without treating the gate as broken.

---

## 4. Something about your chunks

At least 4 of my 5 sample chunks (shown in Milestone 3) read as a complete
thought, meaning someone could answer a question using only that chunk's
text, without needing the sentence before or after it.

**Why this target:**
Chunk quality is invisible until you actually read the chunks. This forces
me to check that my chunking is not cutting sentences in half or burying the
answer next to unrelated text. My corpus averages 317 characters per chunk
(range 178 to 549), which should be enough to hold a full thought given how
short the source documents already are.

---

## 5. Your choice

For at least 4 of my 5 test questions, the source the system names is
actually the correct file that contains the answer, not just any retrieved
document.

**Why this target:**
Naming a source isn't enough on its own (criterion 2). It has to be the
right source. A confident sounding answer citing the wrong file is arguably
worse than no citation at all, because it looks trustworthy and isn't. This
is the criterion I care about most since it is the one that would let a
wrong answer slip through undetected.

---

<!-- UNIT 2 — read this before you change anything above.

     If a criterion turns out to be BROKEN rather than merely unmet, you can
     revise it, and that earns credit. But never delete or edit the original
     line. Add the revision underneath it, like this:

         ## 1. Retrieved chunks contain the answer

         For at least 4 of my 5 test questions, the retrieved chunks include
         one that contains the answer.

         **Why this target:** ...

         > **Revised in unit 2:** For at least 4 of 5 questions, the top three
         > results contain the answer.
         >
         > **Why revised:** I couldn't judge "the chunks include one that
         > contains the answer" the same way twice — I scored two questions
         > differently on Monday than on Wednesday. The new version is
         > something I can actually check.

     That's a revision because the criterion couldn't be MEASURED.

     Lowering a target because you missed it is not a revision, and it costs
     you the point:

         X "I said 4 of 5 but got 2 of 5, so 2 of 5 is more realistic."

     A number you missed stays where it is, gets diagnosed, and gets a fix
     attempted. That's where the points are.

     The whole reason the originals stay visible is so someone can see what you
     said before you knew the answer.
     -->