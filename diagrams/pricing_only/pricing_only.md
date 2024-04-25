# Pricing Only

## Sequence diagram

```mermaid
sequenceDiagram
  actor User
  User->>ClientSystem: Access
  ClientSystem->>Belong: ListManufacturers
  Belong-->>ClientSystem: List of manufacturers such as Apple, Google, Samsung etc
  User->>ClientSystem: Select manufacturer
  ClientSystem->>Belong: ListSeries with manufacturer
  Belong-->>ClientSystem: List of series such as iPhone, iPad etc
  User->>ClientSystem: Select series
  ClientSystem->>Belong: ListModels with series
  Belong-->>ClientSystem: List of models and storages
  User->>ClientSystem: Select model, storage and grade
  ClientSystem->>Belong: GetPrice with program_id, model, storage and grade
  Belong-->>ClientSystem: Price
  ClientSystem-->>User: Show trade-in price
```
