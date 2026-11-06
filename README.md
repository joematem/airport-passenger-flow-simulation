# Airport Passenger Flow Simulation — AnyLogic

A portfolio project modelling passenger movement, processing, flight assignment, boarding and gate routing in an airport terminal using **AnyLogic 8.9.9 Personal Learning Edition**.

## Project purpose

The model is being developed to move beyond a visual airport animation and toward an analytical simulation that can support questions such as:

- Where do passenger queues and bottlenecks form?
- How long do passengers spend in the terminal?
- How does demand split between Gate 1 and Gate 2?
- How do business and economy passengers move through different boarding streams?
- How does service capacity affect waiting, throughput and congestion?
- What happens under higher demand, clustered departures or reduced capacity?

## Model overview

The current passenger process includes:

```text
pedSource
   ↓
pedSelectOutput
   ├── checkInAtCounter
   └── direct route
   ↓
securityCheck
   ↓
pedWait
   ↓
pedSelectOutput1
   ├── businessBoarding1 → gotogate1
   ├── economyBoarding1  → gotogate1
   ├── businessBoarding2 → gotogate2
   └── economyBoarding2  → gotogate2
   ↓
pedSink
```

The model also includes custom flight and boarding logic through:

- `Passenger` agent type
- `Flight` agent type
- `setupPassenger()`
- `startBoarding()`
- `planBoardings()`
- `BoardingEvent`
- `DepartureEvent`
- flight-to-passenger relationships
- business/economy passenger classification
- Gate 1 / Gate 2 routing

## Example logic

Passenger setup uses custom attributes such as:

```java
ped.business = randomTrue(0.15);
```

A passenger can be linked to a flight:

```java
ped.flight = f;
f.passengers.add(ped);
```

Boarding logic can iterate through a flight's passengers:

```java
for (Passenger p : flight.passengers) {
    pedWait.free(p);
}
```

Gate routing can use conditions such as:

```java
ped.flight.gate == 1 && ped.business
```

## Portfolio KPIs

The next analytical layer is being built around the following KPIs:

| KPI | Purpose | Intended visual |
|---|---|---|
| Gate 1 passenger count | Measure demand routed to Gate 1 | Bar chart / time plot |
| Gate 2 passenger count | Measure demand routed to Gate 2 | Bar chart / time plot |
| Queue length | Identify congestion over time | Time plot |
| Passenger time in airport | Measure end-to-end journey time | Histogram + mean |
| Service utilisation | Identify overloaded or under-used services | Bar chart |
| Passenger throughput | Count completed passengers | KPI / time plot |
| Business vs economy mix | Check passenger classification and routing | Bar or pie chart |
| Passengers by flight | Compare demand across departures | Bar chart |

## Gate counters

A simple implementation for Gate 1 and Gate 2 is:

```java
int gate1Count = 0;
int gate2Count = 0;
```

Then increment the appropriate counter when a passenger completes movement toward the gate:

```java
gate1Count++;
```

```java
gate2Count++;
```

These numeric variables can be plotted directly. The `PedGoTo` block itself cannot be used as a Y-value because a chart requires a number, not a block object.

## Planned experiments

The project will be extended with controlled scenarios rather than relying on a single animation run.

| Scenario | Change | Main question |
|---|---|---|
| Baseline | Current configuration | How does the current system perform? |
| Higher demand | Increase passenger arrivals | Where does congestion first appear? |
| Added check-in capacity | Add service capacity | Does waiting time fall? |
| Added security capacity | Add checkpoint capacity | Is security the bottleneck? |
| Departure surge | Cluster flights | How does peak demand affect queues? |
| Priority routing | Change business/economy treatment | What are the distributional effects? |

## Development status

**Working / implemented**

- Passenger pedestrian flow
- Check-in and security stages
- Passenger waiting
- Custom `Passenger` and `Flight` logic
- Flight assignment
- Business/economy classification
- Boarding events
- Departure events
- Gate 1 and Gate 2 routing

**In progress**

- Gate passenger counters
- Queue-length charts
- Passenger-time measurement
- Service-utilisation charts
- Scenario comparison
- Portfolio dashboard

**Not yet claimed**

No numerical performance results are reported in this repository until they have been generated from controlled simulation runs.

## Debugging lessons documented during development

This project has also been used to build practical AnyLogic/Java debugging capability, including:

- `ClassCastException`: generic `Agent` versus custom `Passenger` / `Product`
- method-signature errors such as `unblock(int)` versus `unblock()`
- `IndexOutOfBoundsException` from `get(0)` on an empty collection
- `null` references versus empty collections
- population versus single-agent arguments
- chart type mismatch: block object versus numeric value
- event and function argument typing

## Repository structure

```text
airport-passenger-flow-simulation/
├── README.md
├── model/
│   └── README.md
├── images/
│   └── README.md
├── results/
│   ├── queue-results/
│   │   └── README.md
│   ├── passenger-time-results/
│   │   └── README.md
│   └── scenario-results/
│       └── README.md
├── docs/
│   └── modelling-notes.md
└── LICENSE
```

## Model file

The real AnyLogic `.alp` project will be added to `model/`. It is intentionally not fabricated or represented as uploaded until the actual project file is available.

## Images

Planned portfolio images:

- `model-overview.png`
- `3d-airport.png`
- `passenger-flow.png`
- `dashboard.png`

See `images/README.md` for guidance.

## Tools and methods

- AnyLogic 8.9.9 PLE
- Pedestrian Library
- Discrete-event logic
- Java expressions and functions
- Agent types and populations
- Events
- Collections
- Conditional routing
- AnyLogic Analysis charts

## Next development steps

1. Complete Gate 1 and Gate 2 passenger counters.
2. Add queue-length and throughput plots.
3. Measure passenger time from airport entry to exit.
4. Add service-utilisation metrics.
5. Run baseline and controlled demand/capacity experiments.
6. Export charts and simulation screenshots into `images/` and `results/`.
7. Add the real `.alp` model file.
8. Link the completed case study to a professional portfolio.

## Attribution

This project was developed as part of hands-on AnyLogic learning and extends airport/pedestrian modelling concepts from AnyLogic learning material with additional custom logic, debugging, KPI instrumentation, experiments and portfolio documentation.

Any third-party tutorial content, software, logos or assets remain the property of their respective owners. This repository documents the author's own learning, extensions and analytical development.
