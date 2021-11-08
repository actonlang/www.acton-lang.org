+++
title = "Acton Programming Language"
sort_by = "weight"
template = "index.html"
[extra]

[[extra.list]]
title = "Persistent Environment"
content = "No need to use a database or distributed queue again. Acton automatically persists and resumes the state of your application, while providing strong consistency guarantees"

[[extra.list]]
title = "Fault Tolerant"
content = "Built-in redundancy; Acton's transactional, high performance distributed RTS can seamlessly resume applications after hardware or software crashes"

[[extra.list]]
title = "Non-stop"
content = "Never stop for an upgrade; Live upgrade your running application through code and data migration"

[[extra.list]]
title = "Massively Scalable"
content = "The actor model naturally lends itself to writing scalable applications and the Acton distributed RTS will run your applications across thousands of servers"

[[extra.list]]
title = "Safe"
content = "Static and strongly typed, Acton is safe yet simple to use with low overhead thanks to powerful type inferencing"

[[extra.list]]
title = "Fast"
content = "Acton is a compiled language, backed by a high performance distributed runtime system"

+++

# Overview

Acton is a general purpose programming language, designed to be useful for a wide range of applications, from desktop applications to embedded and distributed systems. In a first approximation Acton can be described as a seamless addition of a powerful new construct to an existing language: Acton adds actors to Python. Our take on the actor model allows developers to write highly scalable and fault tolerant code, without needing to worry about explicit state checkpointing, synchronization primitives or giving up consistency. 

Acton is a compiled language, offering the speed of C but with a considerably simpler programming model. There is no explicit memory management, instead relying on garbage collection.

Acton is statically typed with an expressive type language and type inference. Type inferrence means you don't have to explicitly declare types of every variable but that the compiler will infer the type and performs its checks accordingly. We can have the benefits of type safety without the extra overhead involved in declaring types.

The Acton Run Time System (RTS) offers a distributed mode of operation allowing multiple computers to participate in running one logical Acton system. Actors can migrate between compute nodes and load balance the application workload. The RTS offers exactly once message delivery guarantees and strong serial state consistency without sacrificing performance. By automatically checkpointing actor states and inter-actor messages to a integrated distributed database and messaging queues, failure of individual actors or compute nodes can be transparently recovered from by the runtime system. There is no need for explicit state checkpointing or use of synchronization primitives between actors. Your system can run forever!


# Getting started

## Install
Check out the [installation section](install) for how to get Acton!

## Learn

Read an introduction to using Acton in the [learn Acton](learn) section.


# State of Acton & Road Map


Acton is a work in progress and not all of the features and qualities listed above are implemented in Acton yet. We could have kept it more to "what is currently there" but we want you to get as excited about Acton as we are and we couldn't do that without selling you on the vision.

It's easy to think *Why go about this whole trouble of writing a new language if all we want is some persistence/actors/X? Why not just add actors to Go? Why not add persistence to Python?* We didn't set out with the starting point of *let's write a language, what should it do?*. No, we started from the other end, looking at what we wanted to achieve and came to the conclusion that writing a new language was our only choice. With all these features coming together and to implement them well, it turns out you want support for things in the compiler and maybe we even want to shape the language itself to make it fit for this purpose. That means writing a new language!

We have worked on Acton for years, first through a proof of concept which used an earlier version of the current actonc compiler, implemented in Haskell, but which would output Python code. Since Acton is very similar to Python, this meant we could do "pass-through" on some code. It used a heavily modified Cassandra database that featured transactions and communication queues for safely storing state. The foundational concepts were proven with this prototype.

Since then we have completely rewritten the run time system, compiler and backend for high performance. Acton code is now compiled to C and then to machine code. The runtime system also features a new lock-free distributed database, written from scratch in C for Acton, making use of efficient optimistic concurrency transaction validation, gossip, bulk updates, integrated communication queues, and several other optimizations that make it run fast. The database offers a low level C API for simple but very fast and low latency queries; suitable for building a language run time system on top of! The new run time system in turn is designed to work well together with the database. It works well standalone too, if you just want to write a "normal" program and with native code, it is easy to make small independent programs - there's no interpreter to install. The fundamental run time paradigm in Acton is the execution of continuations based on receiving messages, which is our take on the actor model. An actor method is split up into one or more parts at compile time by actonc in the CPS (Continuation Passing Style) transformation phase. Each part is called a continuation and this is what the run time system operates on. State snapshotting happens on a continuation basis, like a message + continuation waiting to be executed is one atomic state, the RTS picks up the work and runs it, then commits the result to the database.

- the basic Acton language is mostly there
  - normal language features work, you can use Acton to implement various programs etc
  - there may be syntax changes
  - compiler error messages can be improved.. *cough* a lot
- no documentation :/
- no standard library
  - there are essentially no libraries
  - wild ideas if we can do FFI style of another language
    - could fast track us to having a decent standard library
      - or maybe having to manually wrap stuff, but still, that is considerable time saving
    - Acton sort of uses C under the hood, so we can always wrap C libs in Acton
    - however, C means little safety and wrapping code is all manual work
    - Rust is the favorite topic of discussion, much safer & can we automatically FFI wrap it? translate types?
- the current Acton version does support actor snapshotting and resumption of computation to/from the distributed database and queues
  - the implementation of actor migration and placement in the new C RTS is currently under way and will arrive shortly
  - this is a feature that did exist in the old Acton RTS
- the distributed backend does not yet support persisting data to disk
  - the database and distributed queues are all in-memory only for now
  - but its distributed, so hey, dont turn off all your machines at once
- not possible to live upgrade data in database
  - we will leverage the Acton type system to detect when live upgrades are possible or when data migration needs to happen
  - assist user in writing data migration code
  - need to migrate actor state & actor messages in flight!
- Acton does not yet have a garbage collector
  - you effectively can't run long lived programs today
