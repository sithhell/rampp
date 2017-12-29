.. Copyright (c) 2017 Thomas Heller

   Distributed under the Boost Software License, Version 1.0. (See accompanying
   file LICENSE_1_0.txt or copy at http://www.boost.org/LICENSE_1_0.txt)

Remote Asynchronous Message Passing Primitives (RAMPP)
==========================================

RAMP is a C++ library providing an API for:

 * Asynchronous two-sided Message Passing
 * Asynchronous one-sided Remote Procedure Calls (RPC)

The focus is to provide a modern C++ interface for high performance message
passing. Simple message passing is extended by a Remote Procedure Call mechanism
that naturally extends regular (local) C++ function invocations to run on a remote
process.

The objective is to provide a generic solution to be used in Asynchronous Many
Task Runtime Systems (like HPX_) for
Message Passing and Remote Task invocations.

Asynchronity is managed by reusing the Asynchronous Provider and Asynchronous
Return Object concepts as defined by the C++ Standard. That is, all functions
return a `rampp::future<T>` object.

RAMP provides a High Performance solution to message passing by offering the
following features:

 * High performance serialization (marshalling) framework
 * Intrinsic support for RDMA operations for zero copy optimizations
 * Efficient, non-blocking, collective operations
 * Built for massive parallel systems
 * Adaptation to existing code via traits and defined interfaces

Example::

    #include <rampp/rampp.hpp>
    #include <iostream>

    int main()
    {
        // Initialize rampp
        auto world = rampp::init(argc, argv);

        // Get our own location
        auto here = rampp::here(world);

        // Get our neighbor
        auto next = here + 1;

        // Invoke Remote Procedure
        rampp::future f = rampp::async_execute(next,
            []() { std::cout << "Hello World from " << rampp::here() << "\n"; });

        // Block until message has been sent
        f.get();

        // Send/Recv from neighbor
        rampp::future send_future = rampp::send(next, here.rank());
        rampp::future recv_future = rampp::recv<int>(next);

        std::cout << here.rank() << ": received \"" << recv_future.get() << "\"\n";
    }

:ref:`genindex`

.. toctree::
    :maxdepth: 3
    :hidden:


Table of Contents
-----------------

.. toctree::
    :maxdepth: 3

.. _HPX: https://github.com/STEllAR-GROUP/hpx
