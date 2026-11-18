# Travel Itinerary Optimizer

A Python decision-support tool that builds complete trip itineraries from a traveler's constraints (budget, preferences, must-dos, and fixed reservations) using mathematical optimization.

> **Status:** V1 in progress. Model design underway; solver code coming next.

## The problem

Planning a trip means making many linked decisions: what to do, what it costs, when things are open, and how long it takes to get between them. Changing one choice affects everything else. Most planning tools leave that juggling to the traveler. This project treats it as an optimization problem: choose and schedule the set of activities that best matches what the traveler wants, without breaking any of their limits.

## Approach

The model runs in two stages:

1. **Selection (PuLP, mixed-integer programming).** Choose which activities to include, maximizing total traveler preference subject to budget (overall and by category), available time, and required must-dos.
2. **Sequencing (Google OR-Tools, routing with time windows).** Order the selected activities across each day, respecting opening hours, fixed reservations, flight layovers, and travel time between locations (from a travel-time matrix).

Design choices:
- **Toggleable constraints.** Only the constraints a user turns on are compiled into the solver, so one engine supports many kinds of trips.
- **Two search modes, one solver.** Mode A searches the full activity dataset. Mode B optimizes over a pool of activities the user picked.
- **Hard time blocks.** Fixed reservations and layovers are locked in, and everything else is scheduled around them.
- **Flexible blocks.** Open-ended time like "beach day" or "explore the city" is modeled as a flexible time-and-cost block instead of a fixed listing.
- **Multiple options, not one answer.** The tool returns several complete candidate itineraries (for example, at different price points) for the traveler to compare.

The full model formulation (decision variables, objective, constraints) will be documented in [`docs/formulation.md`](docs/formulation.md).

## Roadmap

| Version | Scope |
|---|---|
| **V1: Model** | Working selection and sequencing model on a hand-built dataset for a real multi-city trip. No UI. Group travel handled as a simple cost multiplier. |
| **V2: Web app** | Flask/FastAPI web interface and live data from flight, places, and distance APIs. |
| **V3: Group planning** | True multi-person optimization (per-person preferences, split vs. together time), admin overrides, and optional buffer time. |

## Tech stack

Python · PuLP · Google OR-Tools · Pandas

## Project structure (planned)

```
travel-itinerary-optimizer/
├── data/            # sample activity datasets
├── docs/            # model formulation and notes
├── src/
│   ├── schema.py    # data model for activities, budgets, constraints
│   ├── select.py    # PuLP selection model
│   └── sequence.py  # OR-Tools sequencing model
└── README.md
```

## Author

Marc Nordengreen · [LinkedIn](https://www.linkedin.com/in/marcnordengreen) · [GitHub](https://github.com/marcnordengreen)
