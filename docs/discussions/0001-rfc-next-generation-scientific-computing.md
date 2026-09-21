<!--
Plain-language version, written for a general audience.
The earlier, more technical draft is kept as
0001-rfc-next-generation-scientific-computing-v1.md.

To post: repo Settings -> General -> Features -> enable Discussions, add an "RFC"
category (open-ended format), then paste everything below the line into a new
discussion in that category. Put the title in the title field.

This file is left uncommitted. Track it on a docs/ branch if the team wants the
RFC text versioned next to docs/decisions/.
-->

---

# What a new report on AI and scientific computing means for this project

*RFC means "request for comments": a proposal put up for discussion before anything is decided.*

## The short version

A new report has come out on how scientific software should be built now that AI
writes a large part of it. It was produced by a 2025 workshop that brought together
more than 40 people from national research laboratories, universities, and industry.
You can read it here: [*Reimagining Scientific Computing in the Age of AI*](https://arxiv.org/abs/2510.03413)
(also as a [full PDF](https://arxiv.org/pdf/2510.03413)).

That report is written for large research centres. This project is the opposite in
size. It is a small tool that maps where wildlife can still move across a landscape,
built to run on an ordinary laptop, for conservation and rewilding groups that have
little money and no software department. Much of its code is written by an AI
assistant (Claude) and then checked by people.

So the question for this thread is simple. Which of the report's ideas are worth
adopting in a project this small, and which ones do not fit?

## What the report says, in plain terms

The report's main message is that the people side of a software project and the
technical side should be designed together, not treated as separate problems.

Its three main recommendations:

- (i) Build software in small, well-defined parts, and make it possible to trust
  the result even when an AI helped write it.
- (ii) Let teams use AI in their day-to-day work without losing human judgement,
  honesty about limitations, or accountability for the result.
- (iii) Keep teaching people as the tools keep changing.

It also names four things to try soon: sharing large computing resources for AI
work, mixing disciplines and teaching across them, writing clear guidelines for
responsible AI use, and testing partnerships between public bodies and private
companies.

## What this project already does that fits

- The work is split into small parts so different people can work on them in
  parallel.
- Nobody, including the project lead, can change the main version of the code on
  their own. Every change is reviewed by someone else first.
- Every change that used AI has to say so in writing: which files it touched, what
  the person asked the AI to do, and what a human then checked by hand.
- No datasets are stored inside the code. Each one is downloaded fresh from a named,
  credited source, so anyone can trace where a number came from.
- Locations of sensitive species, currently the European wildcat, are blurred to a
  5 km square before they appear on any map or file that leaves the project.
- Each released version is archived in public repositories so it can be cited like a
  paper.

## What we could do next

Nine ideas, each drawn from the report. For every one: what the report asks, what
we could do here, and the question I would like your view on.

1. **Small parts that can be trusted.** *Report:* modular, trustworthy software.
   *Here:* write down what each part promises to the others, and add an automatic
   test that runs the whole example from start to finish on every change.
   *Question:* are the parts split up the right way, while they are still small and
   easy to change?

2. **A clear record of how each result was made.** *Report:* trust comes from being
   able to retrace a result. *Here:* keep a standard log for every run: which data
   went in, which version of the code produced it, and what came out. *Question:*
   what is the smallest such record a small team can realistically keep every time?

3. **Make AI disclosure easy to check later.** *Report:* preserve trust and rigour
   while using AI. *Here:* store the AI-use notes in a fixed format, and keep the
   conversation transcripts, so someone can audit them a year from now. *Question:*
   is a free-text note enough, or does it need structure?

4. **A short, plain AI policy.** *Report:* write responsible-AI guidelines. *Here:*
   one page in the project docs saying when AI use has to be disclosed and who is
   accountable. *Question:* beyond writing code, what else should it cover:
   choosing settings, cleaning data, wording of figure captions?

5. **Turn onboarding into a teaching tool.** *Report:* teach across disciplines.
   *Here:* rework the getting-started guide into a worked lesson, with an ecologist
   and a software person learning it together. *Question:* how do we bring in an
   ecologist who does not use code tools, without weakening the safeguards above?

6. **Keep a living "how we use AI here" note.** *Report:* keep training current as
   tools change. *Here:* a short document that is expected to change often, with its
   own change history. *Question:* who keeps it up to date?

7. **Be honest about when a laptop is not enough.** *Report:* plan for larger
   computing when needed. *Here:* state plainly the size of area, or level of
   detail, at which the tool outgrows a laptop, and what the fallback is.
   *Question:* where is that limit, and what runs out first?

8. **Share the partnership recipe.** *Report:* test public and private partnerships.
   *Here:* write up the mix already in use, a conservation charity plus a cloud data
   provider plus public archives, as a template other groups can copy. *Question:*
   which parts of it would carry over to a different project, and which are specific
   to ours?

9. **Does our way of deciding things work for outside users?** *Report:* design the
   social side deliberately. *Here:* check whether our light decision-making rules
   still work when an outside team wants to *use* the tool on their own landscape
   rather than help build it. *Question:* do those users need a simpler role than a
   contributor, and what would it let them do?

## How to take part

Reply on the points that matter to you. If a point has support and no strong
objection within two working days, I will turn it into a task on the issue tracker.
Anything larger, a new dependency or a change of method, gets its own discussion
first.

If you run a conservation or restoration project that cannot take on heavy software,
points 7 and 9 are the ones I most want your read on. Which of these should come
first, and is any of it fixing a problem we do not actually have?

Linda Angulo Lopez, September 2026
