# Why We Can't Have Exactly-Once Delivery

While designing a notification system, I delved into the challenges of achieving exactly-once delivery in distributed systems.

Of the three common delivery semantics:
- At-most-once – no duplicates, but some messages may be lost
- At-least-once – no message loss, but duplicates may happen
- Exactly-once – no loss, no duplicates (the ideal, but largely impractical)

After reading the 2015 article ["You Cannot Have Exactly-Once Delivery"](https://bravenewgeek.com/you-cannot-have-exactly-once-delivery/) by Brave New Geek, I realized that even after a decade, we still can’t truly achieve exactly-once delivery.

---

## Why Is Exactly-Once Delivery So Difficult?

To guarantee exactly-once delivery, all nodes must agree on:
- Whether a message was delivered
- Whether it was applied or needs to be applied

However:
- If a message is lost, or
- If even one node is unsure,
then the illusion of exactly-once delivery breaks.

This concept is related to several known distributed system problems:

---

### Related Theoretical Limits

- **Two Generals Problem**: Two parties can never be sure of mutual agreement over an unreliable network.
- **Byzantine Generals Problem**: Coordination fails if some nodes are faulty or malicious.
- **FLP Impossibility**: It’s impossible for a consensus algorithm to guarantee agreement in an asynchronous system if even one node might fail.

---

## What Do Real Systems Do Instead?

Real-world systems use practical alternatives, including:
- At-least-once delivery with idempotent operations
- End-to-end retries with deduplication tokens
- Exactly-once processing semantics in controlled environments (e.g., Kafka with idempotent producers and transactional consumers)

---

## A Case Study: Zab Protocol in ZooKeeper

Zab (ZooKeeper Atomic Broadcast) is a consensus protocol used in ZooKeeper to maintain consistency across nodes.

### Leader-Based Replication
- One node is elected as leader.
- All changes go through the leader.
- The leader broadcasts updates to followers.
- Once a quorum of followers acknowledges, the leader commits the change.

---

### Ensuring Total Order
- All nodes apply the same operations in the same sequence.
- Guarantees consistency across replicas.

---

### Durable and Atomic Broadcast
- Each write is committed exactly once.
- No partial state is allowed across replicas.

---

### At-Least-Once Delivery (Under the Hood)
- Messages may be duplicated due to retries.
- Zab does not prevent duplicate delivery to followers.

### Idempotent State Transitions
Zab ensures operations are idempotent to handle duplicates:
- Every write has a unique transaction ID (zxid).
- Each node tracks the last committed zxid.
- When a new update arrives:
  - If the zxid has already been seen, it is ignored.
  - If the zxid is new, it is applied.

This guarantees each write is processed exactly once, even if the message is received multiple times.

---

## Takeaway

Exactly-once delivery is generally impractical in distributed systems. Instead, systems can achieve the effect of exactly-once delivery by using at-least-once delivery with idempotent state management.
