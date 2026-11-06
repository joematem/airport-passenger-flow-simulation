# AnyLogic model

Place the real Airport AnyLogic project file in this directory.

Recommended filename:

```text
AirportPassengerFlow.alp
```

Do not add a fabricated or reconstructed model file. The portfolio should contain the actual model that runs in AnyLogic.

## Before committing the model

Confirm that:

- the model opens in AnyLogic 8.9.9;
- `pedSource` creates `Passenger` agents;
- flight assignment works;
- `BoardingEvent` and `DepartureEvent` execute without compilation errors;
- Gate 1 and Gate 2 routing works;
- charts use numeric expressions rather than block objects;
- temporary debugging traces are removed or clearly labelled.

If the project depends on external assets, keep them in the same relative folder structure required by the model.
