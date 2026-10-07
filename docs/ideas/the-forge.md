# truename: The Forge

*Idea one-pager, 2026-10-07. Produced with the idea-refine skill. Not yet a spec; nothing here is built.
Lines marked "(Claude's suggestion)" or "(Claude's reading)" were proposed by Claude and have not been
confirmed by Jared.*

## Problem Statement

How might we let the hero set where a name's raw material comes from and how far it strays from obvious,
forge each candidate properly, and melt failures back down for parts, without the run sprawling the way
the first one did?

## Recommended Direction

Rebuild truename around a forge.

**`/forge-a-name` is a launcher skill and the only part that talks to the hero.** It asks the opening
questions (what's being named, the ore blend, the wild / boring / plain spread, how many smiths), shows
the forged blades, takes steering, and resumes the work.

**A new `forgemaster` agent sits behind it as coordinator.** It hides its helpers' intermediate steps,
unless one turns up **THE ONE TRUE BLADE**, which skips straight to the end. It brings in as many
`name-smelter`s as the quest's breadth calls for: one ore for a Frodo's Needle hunt, several lands for a
broad quest. The number of smelters and the number of ores are unrelated. It hands their worked iron to
N smiths, with N set by the hero. On request it takes a family heirloom straight to a smith. It also
recombines failed output with new raw material, or sends it back for a hardening pass.

**Two libraries make the dials concrete.**

- **The smelters own an ore library** (ancient texts, ironic pop culture, 50s Rockwell Americana,
  contemporary, …). Provenance (ore) and purity (wild / boring / plain) are separate dials. A free-text
  phrase either matches an ore or becomes a new one, and ores blend in proportions. *(Claude's reading: a
  60/40 blend means 60% of the material comes from the first ore.)*
- **The smiths own an archetype library** (coined, compound, descriptive, …) modeled on the archetypes in
  cc-toolkit's namesmith plugin. The forgemaster assigns archetypes, the hero can override, and a smith
  may adjust within that. If they conflict, the forgemaster's assignment wins.

**Each smith forges and tempers one candidate.** It works out variants, then checks availability, the
story, the scores, and **domain viability, including domain hacks**: forms where a real top-level domain
completes the word, as in `bit.ly` or `del.icio.us`. It returns the best form plus alternates, or a
broken blade with its usable parts marked for the forge to melt down. GitHub drops from main knockout to
minor signal, and the smiths decide which checks matter based on the blade type.

**A separate hardening skill consolidates the forge's memory.** The scrap heap and favorites are stored
per project in `.truename/`, and the hero's ore and archetype tastes globally in `~/.claude/truename/`.

## Key Assumptions to Validate

- [ ] **A plugin agent can start its sibling plugin agents** by scoped name (`truename:name-smelter`)
  from one level down. *Test:* a throwaway forgemaster that starts one smelter.
- [ ] **The forgemaster can pick up again after the hero steers.** Resuming worked one level down (the
  namesmith run on 2026-10-07), but two levels down is untested. *Test:* steer once, and check whether it
  picks up where it stopped or starts over. This is why the forge's state should live in the forge file,
  not in an agent's memory.
- [ ] **RDAP lookups through `curl` can judge domain viability** for the TLDs that matter, domain hacks
  included. For .com, a 404 means unregistered. Domain-hack TLDs (country codes such as .ly or .us) are
  run by many different registries, and some publish no RDAP service. *Test:* one known-taken domain, one
  nonsense domain, and one domain-hack TLD.
- [ ] **A round costs no more than a 0.0.2 round.** Demoting GitHub helps twice: it removes the knockout
  Jared didn't want *and* the search rate limit behind run 1's long stall. *Test:* compare round 1's time
  and tool-call count against 0.0.2.
- [ ] **Hiding intermediate steps won't make a bad run impossible to diagnose.** *(Claude's suggestion: a
  forge log on disk that the hero only sees on request.)*

## MVP Scope *(Claude's suggestion: build in stages, smallest first)*

1. **Smith first.** Narrow namesmith to forge and temper one candidate, including domain checks and
   domain hacks. Give `/forge-a-name` only the heirloom path: `/forge-a-name temper "<name>"`. This tests
   the smith with no nesting at all.
2. **One smelter.** Add `name-smelter` with a single ore written into the agent itself, with no library
   files yet.
3. **The forgemaster.** Add it, starting the smelter and N smiths, with the hero steering through the
   launcher, plus THE ONE TRUE BLADE early exit. This step is the sibling-spawn test.

## Not Doing (yet), and Why

- **The ore library files.** They'd mean designing a file format before knowing the agents can talk to
  each other. One built-in ore proves the smelter first.
- **The archetype library files.** Same reason. The smith starts with a built-in list of
  cc-toolkit-style archetypes.
- **The hardening skill and both storage locations.** There's nothing worth saving until the chain works.
- **Several smelters per quest.** That needs a working single smelter, plus a way to judge how broad a
  quest is.
- **Trademark searches, voting by several people, non-English markets.** Set aside during refinement.

## Open Questions

- **What does "blade type" mean** in "smiths decide which checks matter based on the blade type"? It
  could mean the archetype: coined names need exact-match domain checks, descriptive ones less so. Or it
  could mean the kind of thing being named (repo, company, variable), which 0.0.2 already uses to decide
  where conflicts live. The two lead to different file layouts.
- **Is a domain hack a check or an archetype?** It can be a check every smith runs on every candidate,
  or an archetype of its own that the forgemaster assigns, or both.
- **Agent names.** `namesmith` is used by two other naming tools, and one of them is installed on this
  machine. The design says "name-smith". `forgemaster` and `name-smelter` haven't been checked for
  collisions.
- **How does the forgemaster judge a quest's breadth?** The hero states it at launch, or it's inferred
  from the brief and the hero can override?
- **What's the bar for THE ONE TRUE BLADE?** 5 on every axis, plus an available domain, plus no notable
  collision? The smith scores, so does the forgemaster decide when to cut to the end?
- **Is a forge log on disk acceptable,** given that the forgemaster doesn't reveal intermediate steps?
