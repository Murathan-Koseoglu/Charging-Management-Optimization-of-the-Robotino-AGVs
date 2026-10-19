# Robotino AGV Charging Management Optimization

**Python | Manufacturing Automation | Intralogistics | AGV Fleet Management**

An academic group project conducted in the **IoT Factory at Hochschule Bielefeld (HSBI), Germany**. The project investigates battery-aware charging management for Robotino automated guided vehicles (AGVs), progressing from a simulated fleet to manual communication with the factory's Robotino system and an automated charging-management prototype.

## Project overview

Robotino AGVs transport materials between production modules. A charging approach based on fixed stations or idle periods alone can limit flexibility, particularly when robots have different charger requirements and battery levels.

The engineering objective was to explore how **battery condition, robot location, charger compatibility, and station availability** could be combined to support dynamic charging decisions within an intralogistics environment.

## Development stages

The project includes three related implementations:

| Stage | Implementation | Key functionality |
| --- | --- | --- |
| **1. Fleet simulation** | [`Simulation.py`](Project_Submission/Simulation.py) | Simulates battery consumption, recharging, task allocation, charger occupancy, and operational/charging queues for six Robotinos, with a Dash/Plotly dashboard. |
| **2. Manual fleet control** | [`Manual Version.py`](Project_Submission/Manual%20Version.py) | Uses TCP communication to obtain fleet-state messages and issue manually triggered movement and docking commands. |
| **3. Automated charging** | [`Final_version.py`](Project_Submission/Final_version.py) | Estimates battery percentages, detects low-battery robots, selects compatible nearby charging stations, and generates movement and docking commands. |

The **automated charging prototype** is the final submitted implementation. The simulation and manual-control versions document the preceding development stages.

## Charging-management approach

The final prototype implements a rule-based workflow:

1. **Receive fleet data:** Request fleet-state updates via TCP at approximately two-second intervals.
2. **Monitor battery condition:** Convert incoming battery-voltage readings into approximate charge percentages using Robotino model-specific voltage ranges.
3. **Identify charging needs:** Evaluate operational robots against a configured low-battery threshold of **20%**.
4. **Select a charging station:** Check Robotino–charger model compatibility, estimate occupancy from robot proximity, and calculate Euclidean distances.
5. **Issue fleet commands:** Generate movement and docking commands through the Robotino Fleet interface.
6. **Monitor system state:** Maintain operational and charging lists and record diagnostic events in a log.

The station-selection logic prioritizes the **nearest compatible station considered unoccupied**. It is a distance-based heuristic, not a proven globally optimal scheduling algorithm.

## Technologies and methods

| Area | Technologies and methods |
| --- | --- |
| Programming | Python |
| Robotino communication | TCP sockets, `GetFleetState` and `PushJob` commands |
| Concurrent operations | Python `threading` |
| Decision support | Battery thresholds, compatibility filtering, Euclidean distance |
| Fleet coordination | Operational/charging queues and state monitoring |
| Simulation | Python object-oriented programming and simulated battery cycles |
| Visualization | Dash, Plotly, Dash Bootstrap Components |
| Diagnostics | Python logging |

The automated prototype configures six Robotino IDs (`20`–`25`) and six charger IDs (`11`–`14`, `17`–`18`) for the original laboratory environment.

## Running the project

### Fleet simulation

Install the Python packages used by the simulation:

```bash
python -m pip install dash dash-bootstrap-components plotly
```

Run the simulation:

```bash
python Project_Submission/Simulation.py
```

The dashboard is configured to run at **http://127.0.0.1:8080/**. The simulation was developed with an older Dash API (`app.run_server`); compatibility depends on the installed Dash version.

### Robotino fleet control

The manual and automated versions rely only on the Python standard library, but **require access to a compatible Robotino Fleet communication environment**.

```bash
python "Project_Submission/Manual Version.py"
python Project_Submission/Final_version.py
```

Both programs listen for TCP connections on **port 13000**. The commands and robot configuration are specific to the original laboratory setup. The control prototypes are not standalone demonstrations without the corresponding fleet interface.

## Scope and limitations

This repository documents an **academic engineering prototype**, not a production-ready fleet-management application.

- Charger availability in the final version is inferred from proximity rather than protected by an atomic reservation mechanism; simultaneous assignments may conflict.
- Charging completion and operational-queue transitions have limitations and are not fully fault-tolerant.
- The simulation uses simplified battery and task behavior rather than a validated physical-system model.
- The Dash dashboard belongs to the simulation and is not integrated into the final automated-control program.
- Quantified reductions in charging time, fleet downtime, or operating costs have not been established.

These limitations provide directions for further work in resource allocation, charging policies, simulation validation, and industrial system integration.

## Engineering relevance

The project combines **manufacturing operations**, **intralogistics**, **automation**, and **data-driven resource allocation**. It demonstrates a progression from modeling operational behavior to communicating with physical equipment and implementing automated, rule-based charging decisions.

---

**Institution:** Hochschule Bielefeld (HSBI), Germany  
**Environment:** IoT Factory  
**Project type:** Academic group project  
**Field:** Industrial Engineering / Manufacturing Automation
