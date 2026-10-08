---
name: forge-a-name
description: This skill should be used when the hero runs /forge-a-name, or asks to "temper this name", "forge this name", "is X a good name?", or "check my name idea" about a name they already have. It launches truename's forge. For now it supports only the heirloom path, in which one existing candidate goes to the truename:namesmith agent and comes back as a checked blade.
argument-hint: temper "<name>"
---

# Forge a Name

Launch the forge from the hero's side. This launcher is the only part of the forge that talks to the hero: it gathers the brief, sends the work to the smith, and presents what comes back. Only the heirloom path exists so far. The smelter and the forgemaster are planned but not built, so do not simulate them here.

## Parse the request

Read the arguments:
- **`temper "<name>"`** (or `temper <name>`): the heirloom path. The quoted text is the candidate.
- **A bare name** with no subcommand: treat it as `temper`.
- **Anything else**, including no arguments or a request to come up with names from scratch: explain that only the heirloom path exists so far, show `/forge-a-name temper "<name>"`, and ask for the heirloom. Do not generate a pool of names.

## Gather the brief

The smith needs three things: what is being named, its constraints, and its region. Take whatever the arguments and the conversation already provide. Ask only for what is missing, in one AskUserQuestion call with at most three questions:
- **What is being named**: the kind of thing (product, repo, plugin, company, CLI, …) and what it does, in a line.
- **Constraints**: character set, case convention, any length limit, and where the name will be displayed. Offer common choices such as "lowercase kebab-case handle" and "Title Case brand name".
- **Region**: where the entity operates, for example "global developers, GitHub" or "Utah LLC, US market".

Do not ask how many smiths to use, or about ores. The heirloom path uses one smith. Archetypes are the smith's call; pass one along only if the hero names it.

## Send it to the smith

Launch the `truename:namesmith` agent with one message containing:
- the candidate, labeled as a family heirloom;
- the brief: what is being named, constraints, region;
- an archetype, only if the hero named one.

Ask it to return its blade report. Do the forging only through the smith, never in the main conversation.

## Present the blade report

Retell the report in plain words: the verdict, the blade with its story, its scores, its availability (domains, domain hacks, closest collision), and the alternates. Summarize the checks run in a line or two, and show the full list only if the hero asks.

Then act on the verdict:
- **THE ONE TRUE BLADE**: lead with it, plainly.
- **Forged**: offer a hardening pass with AskUserQuestion: keep the blade, steer a hardening pass (the hero types the steering in "Other"), or stop.
- **Broken**: say what blocked it and list the salvage. Note that the salvage is kept for the smelter, which is not built yet, and offer to temper a different heirloom.

## Hardening pass

When the hero steers, resume the same smith with SendMessage and pass the steering on. Do not launch a new smith; the existing one keeps its earlier checks and skips forms that have not changed.
