# truename

Find the name for an artificial entity — a company, product, GitHub repo, plugin, CLI,
variable or module handle, skill name, or skill trigger phrase — from a search space that is
effectively infinite. truename is a [Claude Code](https://claude.com/claude-code) plugin
containing one agent, **namesmith**.

*Status: 0.0.1, untested.*

## What it does

namesmith works in rounds. Each round searches more widely than the previous one, and also
more precisely, because it builds on what the previous round found:

1. **Corpus.** It gathers 10 new authoritative sources on the entity's subject (seminal texts,
   landmark analyses) and ranks the 50 words, phrases, and idioms they share most. It then
   removes buzzwords and unrelated common words.
2. **Generation.** It produces 60–100 candidates from three pools:
   - the corpus terms
   - twelve lenses (etymology, myth, old trades, untranslatable words, and more)
   - the stymied list (see below)

   Every candidate must arrive through an extra step, as
   `source → nearby concept → name`, so the first name any model would suggest is filtered
   out.
3. **Filtering.** Cheap filters cut the list to 20–30. A deep web search for conflicts in the
   entity's domain then checks each survivor.
4. **Scoring.** Each name is rated 1–5 on fit, surprise, resonance, distinctiveness, and
   availability. These scores are never added into a total.
5. **Remix.** It breaks the top terms and names into their parts (roots, syllables, images),
   recombines them, and uses the results to search for the next round's sources.

After the final round it returns 7 finalists: 2 wild, 3 middle, 2 plain. Each one comes with
its path, a one-line story, its scores, and its availability evidence.

### Stymied names are signal

If a name is blocked by a direct competitor in the same field, namesmith keeps it on a
**stymied list** instead of discarding it. It records how that name relates to the entity in
abstract terms, and in every later round it uses that description to imagine 2 new
candidates. A name that conflicts only in another region is rejected but kept as a
**near-success**.

### Needle in the haystack

If a candidate scores 5 on every axis, namesmith stops and returns a **NEEDLE FOUND** report.
The calling session asks you, with AskUserQuestion, whether to choose that name and stop. If
you say no, it resumes the same agent, and the agent keeps all of its state.

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
- Each run downloads source texts and does many web searches, so it costs far more than a
  quick brainstorm.

## Layout

```
.claude-plugin/plugin.json   plugin manifest
agents/namesmith.md          the agent
```

## License

MIT
