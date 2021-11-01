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
