# Structure Formulas

## Dynamic Three-Act Formula

For projects using a three-act structure:

```text
Act 1 = round(total_episodes * 0.27)
Act 2 = round(total_episodes * 0.46)
Act 3 = total_episodes - Act 1 - Act 2
```

Adjust by drama type:

- Long-form: Act 2 may expand to 48-52% if subplots are important.
- Medium-form: 27% / 46% / 27% is the default reference.
- Short-form: use phases instead of strict acts when paid-point cadence matters more.

## Anchor Episode Formula

```text
first_episode = 1
act1_turn = Act 1 end
midpoint = round(total_episodes * 0.5)
late_turn = Act 1 + Act 2
climax_start = max(late_turn + 1, round(total_episodes * 0.82))
finale = total_episodes
```

## Hook and Payoff Density

### Long-Form

- Small hook: every episode.
- Medium payoff: every 3-5 episodes.
- Major reversal: every 6-8 episodes.
- Large emotional payoff: at act turns, midpoint, climax, finale.

### Medium-Form

- Small hook /爽点: every episode.
- Medium payoff: every 3 episodes.
- Major reversal: about every 6-8 episodes.
- Strong cliffhanger: every episode ending.

### Short-Form

- Opening hook: first 3 seconds.
- Micro payoff or pressure point: every episode.
- Strong cliffhanger: every episode.
- Paid-point / major reversal: every 3-5 episodes, adjusted by platform.

## No Fixed 30-Episode Rule

The 30-episode medium-form example from the source template must be converted into formulas. Never write “第8集 / 第14集 / 第22集” unless the project total episodes make those values correct by calculation.

## Narrative Method Selection Gate

Before applying act / phase formulas to a new creation, expansion, episode plan, structural rewrite, or optimization that changes information order, read `narrative-method-selection.md` and confirm the storytelling mode by popup.

The formula output must include:

- selected narrative method;
- recommendation reason;
- information-release rule;
- whether montage, flashback, parallel narration, or cross-cutting is allowed;
- which core causal scenes must remain linear for clarity.

Do not silently choose montage, flashback, parallel narration, nonlinear structure, or hybrid structure only because it seems suitable. Recommend one, explain why, and let the user select.
