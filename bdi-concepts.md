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

```text
Belief update
   ↓
Event generated
   ↓
Applicable plan found
   ↓
Intention created
   ↓
Plan body executed
```

The Intention links the plan to the concrete situation.

## BDI Flow

```text
Perception or knowledge update
   ↓
Belief update
   ↓
Event generated
   ↓
Applicable plan found
   ↓
Intention created
   ↓
Plan steps executed
   ↓
Actions or new beliefs
```

## Practical Notes

- Belief is not only sensor data. It may also represent internal knowledge or communication.
- Goal is about desire or objective, not about knowing the current state.
- Plan is the "how".
- Intention is the executable commitment.
- An intention may execute actions, send messages, update beliefs, or perform no physical action at all.