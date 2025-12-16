# CrisisSim
### LLM-Powered Agentic AI for Disaster Response Simulation

---

## Overview

CrisisSim is an **LLM-driven agentic AI simulation framework** for disaster response scenarios such as earthquakes. The system models a **grid-based crisis environment** where heterogeneous agents including **medics, drones, trucks, survivors, hospitals, fires, rubble, and depots** operate collaboratively using **Large Language Model reasoning**, rather than rule-based logic.

The project demonstrates how **agentic reasoning frameworks** can be applied to emergency management, coordination, and decision-making under uncertainty.

---

## Key Features

- **LLM-Based Decision Making**
  - Implements three agentic reasoning frameworks:
    - ReAct
    - Reflexion
    - Plan-and-Execute / Chain-of-Thought / Tree-of-Thought
  - No rule-based planning or hard-coded decision logic

- **Structured Action Planning**
  - All planners generate commands using a **strict JSON action schema**
  - Invalid JSON is detected, logged, and re-prompted once

- **Realistic Disaster Environment**
  - Battery constraints with depot recharging
  - Medic slowdown while carrying survivors
  - Rubble clearing and fire extinguishing
  - Hospital triage with capacity limits
  - Optional aftershocks generating new hazards

- **Interactive GUI**
  - Grid-based visualization with unique colors and shapes
  - Live statistics panel
  - Charts for rescues, deaths, and fire control over time

- **Evaluation and Logging**
  - Per-tick LLM prompt and response logs
  - Per-run JSON metrics
  - Aggregated CSV summaries and plots

---

## System Architecture

```
World → Sensors → LLM Planner → JSON Commands → World Update
```

- Sensors provide a bounded JSON state
- LLM planners reason over the state
- Agents execute commands without autonomy
- The world updates dynamics and metrics each tick

---

## Repository Structure

```
crisis-sim/
│
├── main.py                  # Headless simulation runner
├── server.py                # GUI visualization
├── requirements.txt
│
├── configs/                 # YAML maps (small, medium, hard)
│
├── env/
│   ├── agents.py            # Agent executors
│   ├── dynamics.py          # Fires, rubble, aftershocks, triage
│   ├── sensors.py           # World → compact JSON context
│   └── world.py             # Simulation loop & metrics
│
├── reasoning/
│   ├── llm_client.py        # LLM API integration (Groq / Gemini)
│   ├── planner.py           # Strategy dispatcher
│   ├── react.py             # ReAct planner
│   ├── reflexion.py         # Reflexion planner
│   └── plan_execute.py      # Plan-and-Execute / CoT / ToT
│
├── tools/
│   ├── routing.py           # Pathfinding
│   ├── hospital.py          # Triage policies
│   └── resources.py         # Battery & resupply
│
├── eval/
│   ├── harness.py           # Batch experiments
│   └── plots.py             # Result visualization
│
├── logs/                    # LLM prompt/response logs
└── results/
    ├── raw/                 # Per-run JSON metrics
    ├── agg/                 # Aggregated CSV summaries
    └── plots/               # Generated charts
```

---

## Action JSON Schema

All planners must output actions in the following format:

```json
{
  "commands": [
    { "agent_id": "1", "type": "move", "to": [5, 7] },
    { "agent_id": "2", "type": "act", "action_name": "pickup_survivor" },
    { "agent_id": "3", "type": "act", "action_name": "drop_at_hospital" }
  ]
}
```

### Valid Actions

- `pickup_survivor`
- `drop_at_hospital`
- `extinguish_fire`
- `clear_rubble`
- `recharge`
- `resupply`

---

## Installation

```bash
pip install -r requirements.txt
```

### LLM Configuration

Set your API key as an environment variable:

```bash
export GROQ_API_KEY=your_api_key
# or
export GEMINI_API_KEY=your_api_key
```

---

## Running the Simulation

### Headless Mode

```bash
python main.py --map configs/map_small.yaml --strategy react --seed 42
```

### GUI Mode

```bash
python server.py
```

---

## Experiments

- **3 maps**: small, medium, hard
- **3 planning strategies**: ReAct, Reflexion, Plan-and-Execute
- **5 random seeds** per configuration
- **Minimum 45 experimental runs**

All outputs are saved automatically under:
- `logs/`
- `results/raw/`
- `results/agg/summary.csv`
- `results/plots/`

---

## Metrics Collected

- Survivors rescued
- Deaths
- Average rescue time
- Fires extinguished
- Roads cleared
- Energy usage
- Invalid JSON outputs
- Replanning events
- Hospital overflow events

---




## Contributing

This is an academic project. For questions or suggestions, please contact the course instructor or project maintainer.
