---
name: forge-a-name
description: This skill should be used when the hero runs /forge-a-name, or asks to "temper this name", "forge this name", or "is X a good name?" about a name they already have. It sends one heirloom candidate to the truename:namesmith agent and presents the blade report.
argument-hint: temper "<name>"
---

# Forge a Name

Only the heirloom path exists; do not simulate the smelter or forgemaster.

1. **Parse**: `temper "<name>"` or a bare name is the candidate. For anything else, show `/forge-a-name temper "<name>"` and ask for an heirloom.
2. **Brief**: the smith needs what is named, constraints (charset, case, length, display), and region. Ask only for what is missing, in one AskUserQuestion. Pass an archetype only if the hero names one.
3. **Dispatch** `truename:namesmith` with the heirloom and brief. Never forge in the main conversation.
4. **Present** the verdict, lead form, story, scores, availability, and alternates; summarize checks in a line. THE ONE TRUE BLADE: lead with it. Forged: offer keep / harden (steer via "Other") / stop. Broken: show the block and salvage, note it is kept for the smelter, and offer another heirloom.
5. **Harden**: resume the same smith with SendMessage and the steering.
