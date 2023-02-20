+++
title = "Acton Programming Language"
sort_by = "weight"
template = "index.html"
[extra]

[[extra.list]]
title = "Distributed Computing built in"
content = "Write programs that seamlessly run as a distributed system over an entire data center or region. All without a single line of RPC code."

[[extra.list]]
title = "Durable State"
content = "Acton automatically persists the state of your application (orthogonal persistence) to a built-in distributed backend. No need to use a database or message broker ever again. 0 lines of persistence code."

[[extra.list]]
title = "Fault Tolerant"
content = "Built-in redundancy; Acton's transactional, high performance distributed RTS can seamlessly resume application state after hardware failures"

[[extra.list]]
title = "Non-stop"
content = "Never stop for an upgrade; Live upgrade your running application through compiler-supported code and data migration"

[[extra.list]]
title = "Any scale"
content = "Acton programs, and the actor model, work well from simple script style applications on a single machine up to large distributed systems across a Data Center. Run at your scale."

[[extra.list]]
title = "Safe & Fast"
content = "Static and strongly typed, Acton is safe yet simple to use with low overhead thanks to powerful type inferencing. Being a compiled language, backed by a high performance distributed run time system, Acton is fast."

+++

# Overview
Acton is a general purpose programming language, designed to be useful for a wide range of applications, from desktop applications to embedded and distributed systems. In a first approximation Acton can be described as a seamless addition of a powerful new construct to an existing language: Acton adds actors to Python. Our take on the actor model allows developers to write highly scalable and fault tolerant code, without needing to worry about explicit state checkpointing, synchronization primitives or giving up consistency. 

Acton is a compiled language, offering the speed of C but with a considerably simpler programming model. There is no explicit memory management, instead relying on garbage collection.

Acton is statically typed with an expressive type language and type inference. Type inferrence means you don't have to explicitly declare types of every variable but that the compiler will infer the type and performs its checks accordingly. We can have the benefits of type safety without the extra overhead involved in declaring types.

The Acton Run Time System (RTS) offers a distributed mode of operation allowing multiple computers to participate in running one logical Acton system. Actors can migrate between compute nodes and load balance the application workload. The RTS offers exactly once message delivery guarantees and strong serial state consistency without sacrificing performance. By automatically checkpointing actor states and inter-actor messages to a integrated distributed database and messaging queues, failure of individual actors or compute nodes can be transparently recovered from by the runtime system. There is no need for explicit state checkpointing or use of synchronization primitives between actors. Your system can run forever!


# State of Acton

Acton is a work in progress and still has some way to go to fully deliver on the vision. We want to sell you on the ideas and concepts that we set out to deliver, so that you may be as excited about Acton as we are :)

[Read more](about) about the motivation behind Acton and why we thought a new language was the best avenue to achieve our goals, also covering some of the early history and [learn about](about/future) where we are currently at and what the future might hold.


# Getting started

## Install
Check out the [installation section](install) for how to get Acton!

## Learn

Read an introduction to using Acton in the [learn Acton](learn) section.


