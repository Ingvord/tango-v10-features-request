[TOC]

# Tango V10 wish list

## Documentation

* Compose several few A4 pages guides (how to get started)
* Improve in-code documentation: examples, guides. See, for instance, [BUG-810](https://sourceforge.net/p/tango-cs/bugs/810/)
* Mark topics in the forum as SOLVED when solved
* Launch something like askTango

## Code quality and simplicity

* Use maven for Java and CMake for native; Gradle (???) for agregators and language; Python (???)
* Redesign event system:
    * do not perform sync call (!!!) 
    * subscription topics: host; server; device; attribute/command
    * allow user defined events (push(EventType, Data))
    * use dedicated sockets pair for heartbeat
    * distinguish heartbeat error (connectivity errors) from errors returned from server
    * restructure Java kernel part (TBD); restructure and clean cpp kernel part
* redesign threading: easier implementation for multithreaded device servers (State& Status update; send event)  

### Java
* remove slf4j implementation dependency from JTangoServer
* mavenize Java tools (Jive, Pogo, Astor etc)
* replace DevTangoXXX with native java types

### CPP
* rewrite in pure C (TBD)

## Usability
* OS integration: systemd; service (no need in Starter)
* events with 0-efforts (start polling automatically when there is a subscriber; stop polling when there is no subscribers)
* three level of API: low level; high level; extended (for backward compatability)
* redesign API: write_read; write_with_read etc (see [BUG-812](https://sourceforge.net/p/tango-cs/bugs/812/); [BUG-809](https://sourceforge.net/p/tango-cs/bugs/809/) etc)
* Use REST based pathes to define resources (host, device, attributes, attr etc)
* Generate .xmi from .java using pogo from cli

## Replace CORBA
* simplify and standardize Tango protocol (shall be implementation independent)
* remove IDL (no benefit but only brings complexity)

# Removing CORBA issues

1. Define a Tango protocol (in ABNF) with at least: request id obj id (uniq in DS, in CS?) method name request type protocol release data
> Requires more knowledge on current Tango protocol
2. Implement all Tango::DevVarXXXArray type Similar interface - Similar memory management
> Might be true for CPP (but preferably to use stl containers); 
>
> for Java use native structures (double[]; Map; List etc)
3. Implement a CORBA::string_dup/string_free/string_alloc methods
> Use std::string
4. Implement a dummy (?) CORBA::Any object (used in Pogo generated code for commands) in order to provide compatibility
> Pogo must follow kernel not vice versa
5. omnithread library compatibility (C++11 ?) thread class (detached and undetached) oomni_mutex, omni_mutex_lock, omni_condition variable
> Use c++11 (14?) thread features (async; future etc); 
6. Manage TANGO_HOST definition like "orion:10000,orion:11000"
> Aka Binary star pattern (???)
7. Implement something like ORBtraceLevel, ORBtraceFile, ORBtraceTime, ORBtraceThreadId Is Tango logging adequate?
> Use 3rd party well known logging framework (log4cpp ???)
8. Select a serialiation lib: No lib Google protbuf capnproto Messagepack
> Implement in-house
9. Implement a kind of "is_a" method for all devices in order to manage interface changes with time
> Remove inheritance from Tango model (must be managed on a developer level)
10. Define a TOR (Tango Object Reference) stored in DB in order to build object connection How are we going to manage DS on host with several NIC boards?
> Store IP:PORT; DS listens on all NIC using the same port (by default; can configure opposite);
> Implement distributed service (nodb)
11. Select which kind of container used in DS in which we will store the device object pointer (vector, map,...) Choose the object_id coherent with choice
> Each DS - independent process
12. Try to implement things in a way we could easily replace ZMQ by something else (nextcoolprotocol...)
> Abstract Tango transport layer must be developed
13. Can we write it using C++11 features?
> Must
14. Is it possible to remove event heartbeat by using XPUB in server (instead of PUB) and monitor socket API in client?
> Dedicated pair of sockets must be used for heartbeat
15. Possible to replace DevString (char *) with C++ string?
> Must
16. Collocated calls?
> ???
17. Device server with user event loop
> 
18. DS command line option to start DS on a specific port (like ORBendPoint)
> Must
19. A scavanger thread to close unused connections (Think of DB server)
> Must be implemented in the server kernel API
20. Choose string encoding (LATIN-1 / UTF 8). Support several ?
> UTF-8