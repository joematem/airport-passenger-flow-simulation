# Modelling notes

## 1. Purpose

The airport model represents passenger movement and processing through an airport terminal using AnyLogic's Pedestrian Library together with custom Java logic.

The current model is a learning model being extended into a portfolio-quality analytical case study.

## 2. Main passenger flow

Current structure:

```text
pedSource
   ↓
pedSelectOutput
   ├── checkInAtCounter
   └── alternate route
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

## 3. Passenger agent

The custom `Passenger` agent is used so passenger-specific variables can be stored on each pedestrian.

Examples include:

```java
ped.business
ped.flight
```

A generic `Agent` does not automatically contain these fields.

Therefore `pedSource` must create `Passenger`, not generic `Agent`, when downstream logic calls:

```java
setupPassenger(ped);
```

## 4. Flight assignment

A temporary local variable can hold a chosen flight:

```java
Flight f = flights.random();
```

This means: select one Flight from the `flights` population and temporarily call it `f`.

The relationship can then be stored in both directions:

```java
ped.flight = f;
f.passengers.add(ped);
```

The passenger knows its flight, and the flight keeps a collection of related passengers.

## 5. Passenger setup

The `setupPassenger` AnyLogic Function receives:

```text
ped : Passenger
```

The function body contains only the instructions, not a full Java method declaration.

Example:

```java
ped.business = randomTrue(0.15);
```

## 6. Boarding

A boarding function can operate on one Flight and release its passengers:

```java
for (Passenger p : flight.passengers) {
    pedWait.free(p);
}
```

Read in plain English:

> For every Passenger p belonging to this flight, release that passenger from pedWait.

## 7. Events

`BoardingEvent` controls the start of boarding.

`DepartureEvent` handles departure-related logic.

When removing one Flight from the population, pass the individual Flight:

```java
remove_flights(flight);
```

not the entire population:

```java
remove_flights(flights);
```

## 8. Routing

Example condition:

```java
ped.flight.gate == 1 && ped.business
```

Meaning:

> the passenger's flight is assigned to Gate 1 AND the passenger is a business passenger.

## 9. Gate measurement

Create numeric counters on `Main`:

```java
int gate1Count = 0;
int gate2Count = 0;
```

Increment them as passengers complete the corresponding gate movement:

```java
gate1Count++;
```

```java
gate2Count++;
```

Use these numeric variables in charts.

Do not use:

```java
gotogate1
```

as a Time Plot Y-value. `gotogate1` is a `PedGoTo<Passenger>` object, while a chart Y-value must evaluate to a number.

## 10. Debugging principles learned

### Method signature

```java
hold1.unblock();
```

works when the API signature is `void unblock()`.

```java
hold1.unblock(1);
```

does not satisfy that method contract.

### Empty Wait

Before:

```java
wait.get(0)
```

check whether an agent exists:

```java
if (wait.size() > 0) {
    wait.get(0);
}
```

### Agent type

If a function expects:

```text
Passenger
```

but receives:

```text
Agent
```

the source or block agent type must be corrected rather than forcing a cast blindly.

### Null

`null` means no object reference exists. It is different from zero, false or an empty collection.

### Tracing

Temporary diagnostic output can provide evidence:

```java
traceln("wait=" + wait.size());
```

Remove or reduce excessive traces after debugging.

## 11. Portfolio measurement plan

The first four analytical visuals should be:

1. Gate 1 versus Gate 2 passenger counts.
2. Queue length over time.
3. Passenger-time distribution.
4. Service utilisation.

After those are working, add controlled scenario comparisons.

## 12. Validation notes

Before presenting results:

- use repeatable experiment settings;
- record the simulation time unit;
- state the warm-up period if one is used;
- use multiple replications for stochastic comparisons;
- keep scenario changes isolated where possible;
- report measured outputs separately from illustrative examples.
