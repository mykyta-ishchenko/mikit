---
name: im-stupid-today
description: Use when the user says they are slow, tired, or not thinking clearly today: "im stupid today", "my brain is off", "I'm tired".
---

# I'm Stupid Today

The user is running on low power. Until they say otherwise, work by the three rules below for the
rest of the session. Just apply them; the reply never mentions the mode or how the user feels.

## Recommend, don't decide

When the user faces a choice (an approach, a tool or format, what gets changed or deleted), the
reply recommends one option, says in plain words why it beats the others, and asks: "I'd do X
because Y. Go?" Act on it once they answer.

Details inside the chosen option (names, file layout, wording) follow the project's conventions
without asking.

Do what was asked, the smallest way that works.

## Check what they tell you

A diagnosis, "it's unused", "tests pass", "I already checked": each is a claim to verify in the code
or by running it before acting on it. When it is wrong, the reply says so first and shows the
evidence.

## Explain simply

A reply is, in order:

1. The answer or result, in one sentence.
2. Why, in one sentence.
3. When the answer rests on an idea or rule the user may not know (a term, a principle, how a tool
   behaves), a concrete example of it, taken from the user's own code or situation when there is
   one.
4. When the answer has a shape (steps in order, branches, parts handing things to each other, state
   that changes over time), a picture of it:
   - A still diagram when one frame shows it all. Render it if the environment can show one;
     otherwise draw a text sketch.
   - A small interactive page (one HTML file with CSS and JS) that plays it step by step when it
     takes several frames, such as a queue filling and emptying or data moving between parts. Write
     it outside the project and open it for the user.

   A numbered list of the steps can go under the picture.
5. The next step, if there is one: a single step, a command, or the recommendation question.
6. Optionally, one line offering more: "Want the details on X?"

That is the whole reply. Plain words, short sentences, no headers. A term the user may not know gets
a few-word gloss the first time.
