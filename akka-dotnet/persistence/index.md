# Akka.Persistence

These notes are specifically for the module Akka.Persistence for the akka dotnet project.

**Table of Contents**
- [Akka.Persistence](#akkapersistence)
  - [What is Akka.Persistence?](#what-is-akkapersistence)
    - [What is EventSourced?](#what-is-eventsourced)
  - [Persistent Actor Requirements](#persistent-actor-requirements)
    - [PersistenceId](#persistenceid)
    - [Deterministic Guid](#deterministic-guid)
  - [Commands vs Events](#commands-vs-events)
    - [Persisting Events](#persisting-events)
    - [Upcasting Events](#upcasting-events)
    - [Subscribing to Events](#subscribing-to-events)
    - [Projecting Events](#projecting-events)
  - [Database Tables](#database-tables)
    - [EventStore](#eventstore)
    - [SnapshotStore](#snapshotstore)
    - [Serialization](#serialization)



## What is Akka.Persistence?

It is a module to be added on top of an actor system that helps implement event sourced architecture. It's goal is to save an actor's state so whenever it is created/hydrated the state will be the same as it was before. SQL Server is the common database technology that is used with Akka.Persistence, but any datastore can be used to save data. Adding Akka.Persistence and event sourced architecture can add more work/headaches, but it's very useful and great for some use cases. 

### What is EventSourced?

An event sourced system is a type of architecture/design for a system where an object's data or state is represented by events that have happened to that object. For example, if you have a shopping cart actor, these might be some events that would happen and affect the state of the shopping cart:

  - ProductAdded
  - ProductRemoved
  - QuantityUpdated
  - ClearedCart
  - CheckedOut

This example will allow you to have a specific view of the shopping cart at a specific instance of time. If you wanted to view what the shopping cart was last month vs this month, then you would build the shopping cart's state with all of the events, except exclude this month's events. With event sourced, you're able to see a specific object's instance at a specific time, vs traditionally it's only the object's instance at the current time.

Pros:
  - Build in audit logs
  - Historical view for reporting or specific use cases
  - Easier to debug the state

Cons:
  - Saving more data
  - More difficult to implement
  - Changing an actor's structure becomes more difficult

## Persistent Actor Requirements

A persistent actor builds on top of a regular actor, so all of the base workflows and features of a regular actor are found within a persistent actor. To make an actor persistent, the class will need to inherit from the `ReceivePersistentActor` or `UntypedPersistentActor`. The difference between the 2 are the same differences between `ReceiveActor` and `UntypedActor`.

### PersistenceId

An actor's persistence id is a unique identifier that will distinguish 1 actor from another. This is the same concept of a primary key for a database table. Whenever the actor system wants to save anything for a persistent actor, the persistence id is attached. It is the only required property for a persistent actor. The PersistenceId is a string, so it can derive from any type, as long as it can be converted to a string. A deterministic guid is a great persistence id, or a string that is guaranteed to be unique. 

### Deterministic Guid

A deterministic guid is a great way to ensure that you have a unique persistence id that is also able to be recreated repeatably. This is a combination of a guid namespace and a string of properties. If a request comes in that doesn't have the actor system specific id but it does have the specific data points, then the unqiue identifier can be recreated. I've used this with a string that is built on multiple unique properties that define a specific actor, i.e. "{dateOfBirth}-{lastName}-{firstName}". As long as you have those data points, the guid can be recreated.

The projects I've used are built on something similar to this: (NGuid Github)[https://github.com/bgrainger/NGuid]

## Commands vs Events

A persistent actor is sent commands, and turns those commands into events. Commands are named in present/future tense and events are named in past tense. A command is something that will happen to an actor, an event is something that has happened to an actor. The distinction is important because only events are stored for persistent actors, so a persistent should not receive a message of type event. The opposite is true, where a persistence actor will only receive command messages and not store commands as events. 




### Persisting Events

### Upcasting Events

### Subscribing to Events

### Projecting Events


## Database Tables

### EventStore

### SnapshotStore

### Serialization