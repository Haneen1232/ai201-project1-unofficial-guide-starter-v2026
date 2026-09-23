# The Unofficial Guide

Haneen, corpus: campus_life

> **This file is your submission.** Fill it in as you go, most sections get
> written during the milestone that produces them, not at the end.
>
> How the starter works, and every command you'll need, is in `RUNNING.md`.
> Leave that file alone.
>
> **Paste everything as text.** No screenshots, no video. A typed table gets
> full credit; a picture of the same table gets none.

---

# Unit 1

## What This Does

This system answers questions about student life using the campus_life corpus, 88 short posts covering housing, dining, courses, and campus administration. Someone can ask a plain question like "is the housing lottery actually random?" or "what is the workload like for CS 210?" and get back a grounded answer that names the exact document it came from. If a question falls outside what the corpus covers, the system says so instead of guessing.

## Chunking Strategy

**Chunk size:** Variable, paragraph based (average 191 characters, range 71 to 397)
**Overlap:** None (paragraph splitting doesn't use a fixed overlap)

The campus_life documents are short posts averaging 317 characters, mostly under 800. The starter's fixed size fallback chunker barely touched them: it produced 88 chunks from 88 documents, meaning every document became exactly one chunk regardless of whether it held one idea or several. Reading the documents in Milestone 1, I noticed several posts cover more than one topic in separate paragraphs, for example a noise post that also mentions library hours as an alternative. I replaced the chunker with one that splits on paragraph breaks and merges any resulting piece under 150 characters into its neighbor, so a fragment never stands alone as its own chunk. This produced 145 chunks instead of 88, averaging 191 characters, with the shortest at 71 and longest at 397. I chose paragraph splitting over a fixed character count because it respects where the writer already separated their thoughts, rather than cutting at an arbitrary character count that might land mid sentence.

## Sample Chunks

**Chunk 1** - source: `admin_add_drop_deadline.txt#0` - produced by: `chunker.py::split_documents`

```
On the add/drop deadline

You can add a course through the end of the second week. Dropping is a longer window, through the end of week six, but a drop after week two shows as a W on your transcript. Nothing anywhere on the registrar's site says this plainly, and students find out from each other.
```

**Chunk 2** - source: `course_cs_340.txt#1` - produced by: `chunker.py::split_documents`

```
Expect 6 hours a week early, 15 in the last three weeks when the project lands.

The one piece of advice: start the term project in week three, not week eight; everyone learns this the hard way.
```

**Chunk 3** - source: `course_stat_150_exams.txt#0` - produced by: `chunker.py::split_documents`

```
STAT 150 Applied Statistics, assessment

Three equally weighted midterms, no final. No curve, but the lowest midterm is dropped.

The dropped midterm makes the first one low stakes; use it to learn the format.
```

**Chunk 4** - source: `dining_verrill_street_grill_followup.txt#1` - produced by: `chunker.py::split_documents`

```
Also worth saying: one register, so the queue is a single line no matter how busy. Nobody tells you this at orientation.
```

**Chunk 5** - source: `housing_morrow_house_noise.txt#0` - produced by: `chunker.py::split_documents`

```
Noise levels in Morrow House

Asked about this a lot so writing it down. Loud until about 1am on weekends, no enforced quiet hours.

If you're someone who needs quiet to work, the library is open until 2am during term and that's what most people in this building end up doing.
```

## Sample Answer

**Question:** Is the housing lottery actually random?

**Answer:**

```
The housing lottery is not entirely random in the way most people assume. While rising sophomores get a number drawn at random, juniors and seniors are ordered by accumulated credit hours first, and ties are broken randomly.

Source: admin_housing_lottery.txt
```

**My relevance cutoff:** 0.6 (the starter's default, kept after measuring my own distances below, since the gap between in corpus and out of corpus questions was wide enough that 0.6 sits cleanly in the middle)

| Question | In corpus? | Best distance |
|---|---|---|
| Is the housing lottery actually random? | Yes | 0.248 |
| What do students say about wait times at Kestrel Commons? | Yes | 0.262 |
| What is the workload like for CS 210? | Yes | 0.276 |
| When do I need to declare a major? | Yes | 0.289 |
| What is the noise like in Aldridge Hall? | Yes | 0.402 |
| What is the capital of Mongolia? | No | 0.825 |
| How do I change the oil in a diesel engine? | No | 0.934 |
| Who won the 1994 World Cup? | No | 0.874 |
| What is the recommended dosage of ibuprofen for a headache? | No | 0.803 |
| How do I write a for loop in Rust? | No | 0.877 |

## How I Used AI

**1.** I got a chroma-hnswlib build error during pip install -r requirements.txt that said Microsoft Visual C++ 14.0 was required. I asked Claude what to do, and it walked me through installing Visual Studio Build Tools with the "Desktop development with C++" workload specifically, rather than the whole Visual Studio IDE, which fixed the install.

**2.** I asked Claude to help me write a chunking strategy for Milestone 3. It suggested paragraph based splitting with a minimum size merge rule after I described that campus_life's default chunker wasn't splitting anything (88 documents, 88 chunks). I ran it, saw the new chunk count (145) and average size, and confirmed the sample chunks still read as complete thoughts before committing it.

---

# Unit 2

<!-- These sections get ADDED to what's already above. Don't delete or rewrite
     unit 1, the point is that someone can see what you said before you knew
     how it went. -->

## Run Log - Before

<!-- Your five criteria, three runs each. `python run_eval.py --label before`
     runs the questions, puts the OUT_OF_SCOPE ones through the gate, and
     writes it all into results/ for you. Targets come from criteria.md; the
     verdict column is your call.

     Criterion 3 is measured in one deterministic pass rather than three, so
     the same number goes in all three run columns. That's correct, not lazy.

     Milestone 1. -->

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 |  |  |  |  |
| 2. Every answer names a source | 5 of 5 |  |  |  |  |
| 3. Gate stops out-of-corpus questions | 4 of 5 |  |  |  |  |
| 4. | | | | | |
| 5. | | | | | |

<!-- Underneath, paste the REAL output for each criterion from one of your
     runs, the actual text your system produced, not a description of it.
     Name the file and function that produced it. -->

## Verdicts

<!-- MET or MISSED for each of the five, against the target you wrote last
     unit, not a new one. Plus a sentence on how you decided. That sentence
     matters most where it was close.

     If your target said 4 of 5 and your runs came out 4, 3, 4, that's a MISS.
     The target has to hold, not show up occasionally.

     Milestone 2. -->

| # | Criterion | Verdict | How I decided |
|---|---|---|---|
| 1 |  |  |  |
| 2 |  |  |  |
| 3 |  |  |  |
| 4 |  |  |  |
| 5 |  |  |  |

## Diagnoses

<!-- For each miss: which stage caused it, and how. The stage alone isn't
     enough, you need the mechanism.

     Not a diagnosis: "Question 3 didn't work."
     A diagnosis:     "Question 3 asks about laundry costs. The answer is in
                       one sentence that got split across two chunks, so
                       neither chunk on its own contains it."

     The five stages: loading, chunking, embedding, retrieval, generation.

     Look for a pattern. If three misses all ask about numbers, that's one
     problem, not three.

     Missed nothing? Say so, then say honestly whether your targets were set
     low, and which one you'd tighten and to what.

     Milestone 3. -->

## The Improvement

**What I changed:**

**Why I picked it:**

<!-- Connect it to a specific diagnosis above in one sentence. If you can't,
     you picked a fix because it sounded impressive. -->

### Run Log - After

<!-- Same format, same five criteria, three runs each.
     `python run_eval.py --label after` -->

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 |  |  |  |  |
| 2. Every answer names a source | 5 of 5 |  |  |  |  |
| 3. Gate stops out-of-corpus questions | 4 of 5 |  |  |  |  |
| 4. | | | | | |
| 5. | | | | | |

**Did it help?**

<!-- Say plainly whether it did, and how you know. If it made things worse,
     say that, a change that backfired, honestly reported, earns full credit
     and is more interesting than one that worked. What matters is that you can
     tell.

     Milestone 4. -->

## What's Still Broken

<!-- For each criterion still missed after your fix: what you'd do about it,
     and why you stopped where you did.

     "I ran out of time" is fine if it's true. Pretending nothing is left is
     not.

     Milestone 5. -->

## What I'd Do Differently

<!-- Knowing what you know now, which of your five criteria would you write
     differently, and why?

     Milestone 5. -->
