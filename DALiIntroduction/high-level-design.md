# High Level Design

## Architecture

 + **DALi Core:** Event handling, Scene graph, Rendering, Animation framework, Resource management
 + **DALi Adaptor:** Threading Model, integration with the main loop.
 + **DALi Platform Abstraction:** Resource loading & decoding in multiple threads (part of dali-adaptor)
 + **DALi Toolkit:** Reusable UI controls, Effects & Scripting support

![ ](./Images/architecture.png)

## Main, Update & Render Threads

DALi uses a multithreaded architecture in order to provide the best performance and scalability.

 + **Event Thread:** The main thread in which application code and event handling runs.
 + **Update Thread:** Updates the nodes on the scene as well as running animations & constraints
 + **Render Thread:** OpenGL drawing, texture and geometry uploading etc.
 + **Resource Threads:** Loads and decodes images into bitmaps etc.

![ ](./Images/dali-threads.png)

## Implementation details

DALi uses the Pimpl design pattern.

The Pimpl pattern hides implementation details in the header file and provides improved encapsulation.
Implementation details are hidden, only the required API is visible for the application developer.
API/ABI 'breaks' are reduced since the implementation of a class can be changed without modifying the public API.

