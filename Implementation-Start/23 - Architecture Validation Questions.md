# Architecture Validation Questions

Before accepting any future architecture change, answer:

1. Does it keep raw DROP transport concerns out of the Silo?
2. Does it preserve canonical MME ordering?
3. Can a crash cause a raw event to be acknowledged but never durably represented downstream?
4. Can replay produce a different final surveillance state?
5. Is account/investor resolution owned by the Silo/projector side?
6. Are declared gaps based on proof rather than sparse-topic assumptions?
7. Are source corruption and identity conflicts durable evidence?
8. Does the change preserve Kafka as the isolation/replay boundary?

Any “yes” to question 3 or 4 blocks the change.
