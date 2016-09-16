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