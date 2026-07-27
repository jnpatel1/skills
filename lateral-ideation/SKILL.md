---
name: lateral-ideation
description: A deliberate divergent-thinking protocol for brainstorming, ideation, and breaking out of stuck reasoning. Invoke it when the user wants unconventional angles, novel connections, "out of the box" ideas, ways to attack a problem they feel stuck on, or a wider search than the obvious five answers — including phrasings like "brainstorm," "ideate," "give me weird ideas," "what am I missing," "I'm stuck," "think laterally," or "connect this to something unexpected." This skill widens the search AND then ruthlessly filters it, so the output is signal rather than confetti. Do NOT use it during execution work — implementation, debugging, writing production code, running a math chain, following a fixed spec — where jumping between threads is a defect, not a feature. It is a toggle for the generative phase only.
---
 
# Lateral Ideation
 
A protocol for thinking wide on purpose, then cutting hard.
 
## Why this exists (read this first — it determines whether you use the skill correctly)
 
Left alone, a language model answers by walking toward the most probable next token. That's a feature almost everywhere: it's what makes answers coherent, correct, and reliable. But it means that for an *open* problem — "how could I approach this?" — the default output clusters around the same handful of conventional moves everyone else's default also lands on. Not wrong. Just central. Predictable. The obvious five.
 
This skill does not "add randomness" or "raise entropy" — a prompt can't touch the sampling temperature. What it does is **re-condition the search**: it steers reasoning into regions of the space where the *likely* next move is itself an unusual one, by forcing the problem through distant structural frames before converging. You are trading centrality for reach, deliberately, in a bounded phase.
 
The failure mode you are guarding against is the one that makes most "be creative" prompting worthless: **novelty is cheap, selection is the whole game.** Jumping between frameworks reliably produces *different* ideas. It does not reliably produce *good* ones — most cross-domain connections are noise, and a divergence-only process feels electric while handing back garbage the user has to sift by hand. The cognitive-science version of this is clean: low latent inhibition (letting in "irrelevant" input) predicts creative *achievement* only when paired with high cognitive control; without the control it predicts noise, not insight. Same signal, opposite outcome, and the discriminator is the filter. So this protocol is two engines bolted together — a wide one and a merciless one — and skipping the second is the single way to make it fail.
 
## When to run it, and when absolutely not
 
Run it when the task is generative and the answer space is open: brainstorming, naming, architecture *options* (not implementation), unsticking a stalled line of thought, finding an angle competitors haven't, reframing a problem that feels exhausted.
 
Do **not** run it inside execution. If there is a code path being written, a proof or pricing chain being carried, a spec being followed, or a debug in progress, dropping a thread before it's filtered is a defect. Rapid task-switching mid-build is precisely the impairment this skill is designed to *contain*, not unleash. If a request mixes both ("brainstorm the approach, then build it"), run the protocol for the ideation half, deliver the converged shortlist, and then **exit lateral mode explicitly** before any execution begins. Never let the divergence phase bleed into the build.
 
## The protocol
 
Four phases, in order. Do not collapse them — the value is in the separation, especially the wall between phase 3 (generate) and phase 4 (cut).
 
### Phase 1 — Abstract to structure
 
Strip the problem of its surface. State what it *structurally is*, in domain-neutral terms, before generating anything.
 
The good cross-domain hits are structural, not cosmetic. "An SEO rendering issue" connects to almost nothing; "a routing-under-contention problem where a producer and a consumer disagree about when state is ready" connects to network backpressure, restaurant expediting, air-traffic control, and CRDT convergence. Name the deep structure and the bridges appear on their own.
 
Write one or two lines: *"Stripped down, this is a ___ problem — the core tension is ___."*
 
### Phase 2 — Hop with anchors
 
Do not "think chaotically" — that just produces mush. Instead, **pick 3–5 explicitly distant domains** and force a bridge from each back to the abstracted problem. Concrete anchors beat vague instructions to be creative every time; this is the mechanism behind Oblique Strategies, de Bono's random-entry, biomimicry, and TRIZ, and it works because a fixed foreign reference point gives the association something to grab.
 
Draw anchors from genuinely far fields — biology, geology, music theory, logistics, immunology, game design, ecology, materials science, bureaucracy, mythology. If nothing is jumping out, pull from `references/domain-anchors.md`, which has a seed bank and bridging prompts. Prefer domains the user has *not* mentioned; the whole point is to leave their frame.
 
For each anchor, ask: *how does THIS field solve the abstracted tension, and what does that suggest here?*
 
### Phase 3 — Generate wide (filter deliberately loosened)
 
Now diverge. Produce a lot — aim for 10–15 raw candidates — and while doing it, **suspend the "is this reasonable?" reflex**. Let weird, half-formed, and probably-wrong ideas onto the page. Chase the associations that feel slightly illegal. Do not self-censor here; censoring mid-generation is what collapses the output back to the obvious five.
 
Keep candidates terse — a line each. Volume and range matter more than polish at this stage. Weird is the job.
 
### Phase 4 — Converge ruthlessly (this is what makes it worth reading)
 
Now switch modes hard. Become the skeptic. Run every candidate through a fast filter and *kill most of them*. Do not soften this — the loosened filter from phase 3 was a temporary loan, and phase 4 pays it back with interest.
 
For each surviving candidate worth keeping, sanity-check it against: *Does it actually address the core tension from phase 1? Is it novel, or just dressed-up obvious? Would it survive five minutes of adversarial scrutiny — the `/grill` test? Is it feasible enough to be worth the reader's time, or is it a fun dead end?*
 
Keep **2–4 that survive.** Not more. If everything got killed, say so plainly and offer to re-hop from different anchors rather than promoting weak ideas to fill a quota. A short list of survivors is the deliverable; a long list of maybes is a failure.
 
## Output format
 
Show the work, because the reasoning trail is half the value and lets the user re-run a promising thread.
 
```
**Reframed:** [one-line structural abstraction from Phase 1]
 
**Anchors bridged:** [the 3–5 domains, one line each on what they suggested]
 
**Survivors:**
1. [Idea] — why it survived: [the tension it hits + what makes it non-obvious]
2. ...
 
**Killed (briefly):** [2–3 tempting candidates you cut, and the one-line reason — so the user
sees the filter working and doesn't re-suggest them]
 
**Sharpest bet:** [if you had to pick one to develop, which, and the first concrete step]
```
 
Adapt the scaffold to the request — a quick unstick doesn't need the full ceremony, a deep ideation session earns all of it. But never drop the survivors/killed split: showing what you cut *and why* is the proof that selection happened, and it's what separates this from a random-idea generator wearing a lab coat.
 
## Anti-patterns (the ways this skill degrades)
 
- **Divergence without convergence.** Handing back all 15 raw candidates. This is the default failure and it feels productive while being useless. Cut to 2–4 or the skill did nothing.
- **Surface hops instead of structural ones.** Bridging from the topic's cosmetic features rather than its abstracted structure. Produces cute analogies that don't transfer.
- **Fake weirdness.** Obvious ideas with exotic vocabulary bolted on. The filter in phase 4 exists partly to catch these.
- **Leaking into execution.** The moment real building starts, this mode is a liability. Exit it explicitly.
- **Padding the survivor list.** Promoting a weak idea to hit a count. An honest "only one of these is any good" beats four with two duds.
 
