# truename

Find the name for an artificial entity — a company, product, GitHub repo, plugin, CLI,
variable or module handle, skill name, or skill trigger phrase — from a search space that is
effectively infinite. truename is a [Claude Code](https://claude.com/claude-code) plugin
containing one agent, **namesmith**.

*Status: 0.0.2. The round-by-round process is new and has not been run yet.*

## What it does

namesmith works one round at a time, and you steer between rounds:

1. **Sources.** It reads 7 new contemporary, accessible sources on the entity's subject
   (current docs, popular articles, widely read recent books, forum threads, neighboring
   products' pages) and ranks the 50 words and phrases they share most. Common words such as
   "of" and "the" stay inside phrases. It drops buzzwords and keeps plain, concrete terms.
2. **Generation.** It produces about 20 candidates by taking words from the sources and from
   everyday areas (kitchen, workshop, sport, travel, weather, body, idiom) and combining them
   with moves: swap, verbify, twist, compress, pair, and sound. At most about 1 candidate in
   50 is a deep cut from myth, word roots, or old trades. Names may be any length.

   Every candidate arrives through an extra step, as `source or move → nearby idea → name`,
   so the first name any model would suggest is filtered out.
3. **Filtering.** Cheap filters cut the list to about 8. Each survivor gets at most two quick
   searches: a GitHub repository search sorted by stars, and one search of the entity's
   ecosystem. The search stops at two notable hits. Only projects with 200 or more stars, or
   products with real users, can knock a name out.
4. **Scoring.** Each name is rated 1–5 on fit, surprise, resonance, distinctiveness, and
   availability. These scores are never added into a total.
5. **Round report.** It returns its top 5, each with its path, a one-line story, its scores,
   and its availability evidence, and then stops.

The calling session then asks you, with AskUserQuestion, to pick favorites, steer, or stop. If
you keep going, it resumes the same agent with your picks and steering, and the next round
builds on them. There is no round limit.

### Taken names are signal

If a name is blocked by a notable project in the same space, namesmith keeps it on a
**taken list** instead of discarding it. It records how that name relates to the entity in
abstract terms, and in later rounds it uses that description to imagine a new candidate. A
name whose only notable conflict is in an unrelated space or another region is kept, with a
flag.

## Install

Not yet published to the marketplace. Load it from a local checkout:

```bash
claude --plugin-dir ~/Projects/truename
```

## Use

Describe the thing you need to name and ask for names. Claude dispatches namesmith when the
request matches. A useful brief covers:

- what the thing does
- how it should feel
- the principle or community it serves
- hard constraints (length, characters, case)
- the region it operates in

namesmith only generates and checks names. To try a name on Claude Code display surfaces, use
`iterate-potential-project-names`. To rename something everywhere it appears, use
`rebrand-propagate`.

## Limits

- The availability search looks for conflicts. It is not legal or trademark clearance.
- Each round fetches 7 sources and runs up to two searches per surviving name, so a run costs
  more than a quick brainstorm. It also pauses for your steering after every round.

## Layout

```
.claude-plugin/plugin.json   plugin manifest
agents/namesmith.md          the agent
```

## License

MIT
