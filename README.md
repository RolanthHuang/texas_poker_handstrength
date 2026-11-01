# Texas Hold'em Probabilistic Decision Engine

An offline, browser-based demonstration of a probabilistic Texas Hold'em decision engine. The decision core was originally designed for machine-player agents; the interactive interface also lets human players explore how the model responds to different cards and table stages.

## Live demo

**[Open the interactive demo](https://rolanthhuang.github.io/texas_poker_handstrength/)**

The project is a self-contained static site. You can also download and open `index.html` directly.

## What it shows

- Editable bot hole cards and community cards
- Flop, turn, and river progression
- Current Hand Strength (HS)
- Positive and Negative Hand Potential (PP and NP)
- Effective Hand Strength (EHS) using the Python model
- Fold, call, raise, and all-in action probabilities
- Python PT1 and PT2 action-policy variants
- Reproducible fast-potential sampling with a configurable random seed
- Approximate adjustment for one to five opponents

## Decision pipeline

```text
Visible cards
  -> hand evaluation
  -> HS / PP / NP
  -> Python EHS
  -> PT1 or PT2 probability triple
  -> fold / call / raise / all-in distribution
```

The selected EHS definition is:

```text
EHS = HS^Opp * (1 - NP) + (1 - HS^Opp) * PP
```

When `HS == 1`, the implementation follows the original Python function and returns `EHS = 1`.

## Implementation notes

- Everything runs locally in one `index.html`; there are no external libraries or API requests.
- HS enumerates all possible two-card holdings for one unknown opponent.
- Flop and turn potential use a fast sampled future board for every possible opponent holding.
- River has no future community cards, so PP and NP are zero.
- Multi-opponent EHS uses the original model's `HS^Opp` approximation rather than exact joint multi-player equity.
- A raise is split into ordinary raise and all-in probabilities so the four displayed actions always total 100%.

## Scope

This repository demonstrates a decision model. It is not a poker solver, does not read a live poker client, and does not automate actions on any gaming platform.
