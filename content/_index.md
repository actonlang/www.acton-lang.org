+++
title = "Acton Programming Language"
sort_by = "weight"
template = "index.html"
[extra]

[[extra.list]]
title = "Persistent Environment"
content = "No need to use a database again. Acton automatically persists and resumes the state of your application"

[[extra.list]]
title = "Fault Tolerant"
content = "Built-in redundancy; Acton's quorum based distributed RTS can seamlessly resume applications after hardware or software crashes"

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
content = "Acton is a compiled language"

+++

# Overview

Acton is a general purpose programming language, designed to be useful for a wide range of applications, from desktop applications to embedded and distributed systems. In a first approximation Acton can be described as a seamless addition of a powerful new construct to an existing language: Acton adds actors to Python.

Acton is a compiled language, offering the speed of C but with a considerably simpler programming model. There is no explicit memory management, instead relying on garbage collection.

Acton is statically typed with an expressive type language and type inference. Type inferrence means you don't have to explicitly declare types of every variable but that the compiler will infer the type and performs its checks accordingly. We can have the benefits of type safety without the extra overhead involved in declaring types.

The Acton Run Time System (RTS) offers a distributed mode of operation allowing multiple computers to participate in running one logical Acton system. Actors can migrate between compute nodes for load sharing purposes and similar. The RTS offers exactly once delivery guarantees. Through checkpointing of actor states to a distributed database, the failure of individual compute nodes can be recovered by restoring actor state. Your system can run forever!


# Getting started

## Install
Check out the [installation section](install) for how to get Acton!

## Learn

Read an introduction to using Acton in the [learn Acton](learn) section.


# State of Acton & Road Map


All of the features and qualities listed above aren't implemented in Acton yet. We could have kept it more to "what is currently there" but we want you to get as excited about Acton as we are and we couldn't do that without selling you on the vision.

Acton doesn't scale to thousand of compute nodes just yet. It can only run on a single compute node. Actually, using the database backend is broken right now but we'll have that fixed shortly. And the distributed RTS, which enables two or more compute nodes to divide up actors between them, is also being worked on! Once we can do it across 2 compute nodes, a thousand is not far away!

It's easy to think *Why go about this whole trouble of writing a new language if all we want is some persistence/actors/X? Why not just add actors to Go? Why not add persistence to Python?* We didn't set out with the starting point of *let's write a language, what should it do?*. No, we started from the other end, looking at what we wanted to achieve and came to the conclusion that writing a new language was our only choice. With all these features coming together and to implement them well, it turns out you want support for things in the compiler and maybe we even want to shape the language itself to make it fit for this purpose. That means writing a new language!

What's important is that Acton was designed from the beginning for the features and qualities listed. Even though some things are not completed yet, or implementation may not even have begun, Acton is still in a better position to deliver on these than say Java or Go is or ever will be. It is in the nature of the language. You could never support a persistent programming environment, snapshotting program state, when you are using locks and threads and have a big shared heap of memory like Java. People have tried with various read and write barriers but it leaves a sour taste of poor performance, making it unsuitable for actual industrial use. Acton uses actors and there is no shared memory, making actor snapshotting not just a tractable proposition but also a performant one.

We have worked on Acton for years, first through a proof of concept which used an earlier version of the current actonc compiler, implemented in Haskell, but which would output Python code. Since Acton is very similar to Python, this meant we could do "pass-through" on some code. It used a heavily modified Cassandra database for safely storing state. The foundational concepts were proven with this prototype.

Since then we have implemented a new run time, where we compile to C and then to machine code and there's a new lockless distributed database, written from scratch for Acton, with quorum reads and writes. It is not like an SQL database or anything, it only offers a low level C API for simple but very fast and low latency queries; suitable for building a language run time system on top of! The new run time system in turn is designed to work well together with the database. It works well standalone too, if you just want to write a "normal" program and with native code, it is easy to make small independent programs - there's no interpreter to install. The fundamental run time paradigm in Acton is the execution of continuations based on receiving messages, which is our take on the actor model. An actor method is split up into one or more parts at compile time by actonc in the CPS (Continuation Passing Style) transformation phase. Each part is called a continuation and this is what the run time system operates on. State snapshotting happens on a continuation basis, like a message + continuation waiting to be executed is one atomic state, the RTS picks up the work and runs it, then commits the result to the database.

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
- actor snapshotting has been demonstrated in an earlier POC version of Acton (written in Python and using a modified Cassandra database)
  - this also included load sharing of actors across multiple compute nodes
  - we "just" have to implement the same thing in the current incarnation of Acton
    - now written in C for speed
    - this is under way
- the current Acton version does support actor snapshotting and resumption to/from the distributed database
  - though a regression is currently making it not work
- distributed backend does not support persisting data to disk
  - in-memory only for now
  - but its distributed, so hey, dont turn off all your machines at once
- not possible to live upgrade data in database
  - we will leverage the Acton type system to detect when live upgrades are possible or when data migration needs to happen
  - assist user in writing data migration code
  - need to migrate actor state & actor messages in flight!
- Acton does not have a garbage collector
  - you effectively can't run long lived programs today
