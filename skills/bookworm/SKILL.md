---
name: bookworm
description: Use when designing architecture or code structure, drawing module, service, or API boundaries, modeling data, or choosing between approaches: "how should we structure this", "should we split this out", "which approach is better".
---

# Bookworm

A design decision stands on a named principle from the literature, not on taste. The reader should
be able to look it up, check it, and disagree with it.

## Every decision point

A decision point is any place the design picks one option over another. Each one carries:

1. **Decision.** What you chose, and over what.
2. **Source.** The principle by its established name, with the author and the work that named it.
3. **Fit.** Why it applies here, said about this code, not in general.

Weave all three into the sentence that states the decision:

> Give each format its own renderer class instead of branching on format in every method. Touching
> five methods for one new format is *Shotgun Surgery*, and the cure is *Replace Conditional with
Polymorphism* (Fowler, *Refactoring*): a new format becomes one new class and nothing else changes.

## Citing

- Author, title, and the idea's name, for a work you know exists. Add a chapter, page, or quote only
  when you are certain of it; never reconstruct one.
- One source per decision: the work that named the idea or the one that is its reference today, not
  a blog post that summarizes it.
- Use the source's own names for smells and patterns (*Strangler Fig*, *Transactional Outbox*,
  *Bulkhead*). They make the reasoning searchable.

## When sources disagree

Name both sides and pick one for this context. Ousterhout's deep modules against Martin's small
functions; Fowler's *MonolithFirst* against Tilkov's *Don't start with a monolith*; mockist
(Freeman & Pryce) against classicist (Khorikov) testing.

The project's own conventions and constraints outrank any book. When the design departs from a
source, say which one and why.

## Canon, by example

The table shows the standard, not the list. For each decision, find the best literature on its topic
as of today: the work practitioners treat as the reference now, in its current edition, whether or
not it is below.

| Topic                            | Sources                                                                                                                             |
|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| Modules, abstraction, complexity | Ousterhout, *A Philosophy of Software Design*; Parnas, "On the Criteria To Be Used in Decomposing Systems into Modules"             |
| Code smells, refactoring         | Fowler, *Refactoring*; Beck, *Tidy First?*                                                                                          |
| Changing legacy code             | Feathers, *Working Effectively with Legacy Code*                                                                                    |
| Object design                    | Metz, *Practical Object-Oriented Design*; Gamma et al., *Design Patterns*                                                           |
| Domain modeling                  | Evans, *Domain-Driven Design*; Wlaschin, *Domain Modeling Made Functional*                                                          |
| Application architecture         | Fowler, *Patterns of Enterprise Application Architecture*; Cockburn, "Hexagonal Architecture"                                       |
| Architecture trade-offs          | Richards & Ford, *Fundamentals of Software Architecture*; Ford et al., *Software Architecture: The Hard Parts*                      |
| Service boundaries, migration    | Newman, *Building Microservices* and *Monolith to Microservices*; Richardson, *Microservices Patterns*                              |
| Data, distributed systems        | Kleppmann, *Designing Data-Intensive Applications*; Joshi, *Patterns of Distributed Systems*                                        |
| Messaging, integration           | Hohpe & Woolf, *Enterprise Integration Patterns*                                                                                    |
| API design                       | Geewax, *API Design Patterns*                                                                                                       |
| Stability, failure handling      | Nygard, *Release It!*; Beyer et al., *Site Reliability Engineering*                                                                 |
| Tests as a design tool           | Freeman & Pryce, *Growing Object-Oriented Software, Guided by Tests*; Khorikov, *Unit Testing: Principles, Practices, and Patterns* |
| Evolution, teams, Conway's law   | Ford et al., *Building Evolutionary Architectures*; Skelton & Pais, *Team Topologies*                                               |
