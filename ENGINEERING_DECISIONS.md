This document records significant engineering and architectural decisions made
during the development of Project SIKLAB.

Each entry documents the decision, the reasoning behind it, relevant trade-offs,
and its impact on the system. These records are preserved to provide context
for future revisions and to prevent important design rationale from being lost
as the project evolves.

ED-001 - Local-First AI

  Decision: Prioritize local AI inference over cloud-dependent inference.

  Reasoning
    - reduced dependence on internet connectivity
    - lower latency for certain robot interactions
    - increased privacy
    - greater system autonomy
    - exploration of resource-efficient edge AI

  Trade-offs
    - limited compute resources
    - smaller language models
    - lower inference speed
    - reduced reasoning capability compared with large cloud models

ED-002 - Raspberry Pi 4 as Initial Compute Platform

  Decision: Use Raspberry Pi 4 Model B as the primary onboard computer.
  
  Reasoning
    - already available
    - low power consumption
    - compact form factor
    - GPIO and camera support
    - Linux and ROS 2 compatibility
    
  Limitations discovered
    - limited AI inference performance
    - memory constraints
    - thermal considerations
