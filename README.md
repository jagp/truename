# truename

A [Claude Code](https://claude.com/claude-code) plugin for naming artificial things: companies,
products, repos, plugins, CLIs, handles, skills, and trigger phrases. It is being rebuilt as **the
forge**, and the first stage is in place.

## In place

- **`/forge-a-name temper "<name>"`** is the launcher. It takes a name you already have (an
  heirloom), asks for anything missing from the brief (what is being named, its constraints, its
  region), and hands the name to the smith.
- **namesmith** is the smith agent. It forges and tempers one candidate:
  - names its archetype (coined, compound, descriptive, …) and works out 3–6 variants;
  - checks domains through RDAP, including domain hacks such as `bit.ly`, with a DNS fallback
    for TLDs that publish no RDAP;
  - runs a web collision search, with GitHub as a note only;
  - scores fit, surprise, resonance, distinctiveness, and availability;
  - returns a blade report: **forged**, **THE ONE TRUE BLADE** (5 on every axis, at most one 4),
    or **broken**, with salvage for a later melt-down.

  You can steer a hardening pass on the same blade.

## Roadmap

Not built yet. The design is in [`docs/ideas/the-forge.md`](docs/ideas/the-forge.md).

- **name-smelter**: turns sources ("ores") into a pool of candidates with a wild / boring / plain
  spread.
- **forgemaster**: coordinates the smelters and smiths, hides their intermediate steps, and stops
  early on THE ONE TRUE BLADE.
- **Ore and archetype libraries**, and a skill that keeps the forge's memory between runs.

## Install

```bash
claude plugin marketplace add jagp/truename
claude plugin install truename@truename
```

This installs the latest release from `main`, which is still 0.0.2's single-agent namesmith. To try
the forge before its release, check out `feature/forge-smith` and run
`claude --plugin-dir <path-to-checkout>`.

## Limits

- The checks look for conflicts. They are not legal or trademark clearance.
- One smith run takes about three minutes and makes many lookups.

## Layout

```
.claude-plugin/plugin.json        plugin manifest
.claude-plugin/marketplace.json   self-hosted marketplace entry
agents/namesmith.md               the smith
skills/forge-a-name/SKILL.md      the launcher
docs/ideas/the-forge.md           the forge's design one-pager
```

## License

MIT
