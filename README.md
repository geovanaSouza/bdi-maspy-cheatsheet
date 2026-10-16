# BDI Concepts Cheat Sheet

## Belief

A Belief represents knowledge about the world.  
It may come from a sensor, memory, communication, or inference.

```python
from maspy import *
Belief("hello")
```

## Goal

A Goal represents a desire or objective the agent wants to achieve.

```python
from maspy import *
Goal("finish_mission")
```

A Goal says **what** the agent wants. It is not an action and not knowledge.

## Belief vs Goal

In MASPY, a Goal can have a lifecycle:

```text
Goal created
   ↓
Context satisfied?
   ├─ Yes → Plan selected → Intention created → Executed
   └─ No  → Pending
             ↓
        Within retry window?
             ├─ Yes → Reevaluate
             │          ├─ Condition met → Plan selected → Intention created → Executed
             │          └─ Still missing → Stay pending
             └─ No  → Goal fails
```

For comparison, a Belief usually follows a simpler path:

```text
Belief event
   ↓
Applicable plan?
   ├─ Yes → Intention created → Executed
   └─ No  → Event consumed (no automatic retry)
```

Example with a condition:

```python
@pl(gain, Goal("finish_mission"), context=Belief("ready"))
def finish(self, src):
    self.print("Finishing mission")
```

## Plan

A Plan is a rule or strategy used to react to an event or to achieve a goal.

```python
from maspy import *

@pl(gain, Belief("hello"))
def say_hello(self, src):
    self.print("Hello World!")
```

A Plan defines **when** and **how** to act, but does not execute by itself.

## Intention

An Intention is a concrete commitment to execute a Plan for a relevant event or goal.  
It links the plan to the concrete situation and carries the arguments needed to execute it.

## BDI Flow

### Belief Flow

```text
Perception / knowledge update
   ↓
Belief update
   ↓
Event generated
   ↓
Applicable plan found?
   ├─ Yes → Intention created → Plan steps executed → Actions or new beliefs
   └─ No  → Event consumed
```

### Goal Flow

```text
Desire / objective / planning / communication
   ↓
Goal generated
   ↓
Event generated
   ↓
Plan found for Goal?
   ├─ No  → Pending → retry/fail within window
   └─ Yes → Context satisfied?
              ├─ No  → Pending → retry/fail within window
              └─ Yes → Intention created → Plan steps executed → Actions or new beliefs
```

## Practical Notes

- Belief is not only sensor data. It may also represent internal knowledge or communication.
- Goal is about desire or objective, not about knowing the current state.
- Plan is the "how".
- Intention is the executable commitment.
- An intention may execute actions, send messages, update beliefs, or perform no physical action at all.