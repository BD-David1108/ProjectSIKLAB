History of Project SIKLAB

This document records the origin and technical evolution of Project SIKLAB,
including major architectural decisions, hardware and software revisions,
development milestones, experiments, and lessons learned.

For the current system design, refer to `ARCHITECTURE.md`.

Project Origin - 2026

SIKLAB began in 2026 as a desktop humanoid robotics project created by
Dave Buena.

The original goal was to develop a compact humanoid platform integrating
embedded control, ROS 2, computer vision, local artificial intelligence,
speech recognition, text-to-speech, and physical interaction.

A central design philosophy of SIKLAB is the exploration of local and
edge-based AI. The project investigates how useful embodied AI capabilities
can be deployed directly on resource-constrained robotic hardware while
reducing dependence on cloud-based inference.

The following architecture represents the first functional AI and robotics
configuration of SIKLAB. Components listed here may differ from the project's
current implementation:

  Milana

    Status: Early Development

    Milana was introduced as the embodied AI personality and high-level interaction
    layer of SIKLAB.
    
    Her initial role was to provide conversational interaction by integrating
    speech recognition, language-model inference, computer vision, and synthesized
    speech.
    
    Planned capabilities included:
    
    - contextual memory
    - perception-aware conversation
    - emotional and behavioral state
    - robot tool selection
    - ROS 2 action execution
    - high-level task planning

  Artificial Intelligence Stack

    Platform: Raspberry Pi 4 Model B, 8 GB RAM, Active Cooling
    Description: Served as the primary onboard computer for AI inference, ROS 2 processes, computer vision, speech processing, and high-level robot control.

    Speech Recognition - Whisper.cpp , Model: tiny.en
    Description: Converts recorded speech into text locally without requiring a cloud speech-recognition API.
  
    Computer Vision - YOLO26n, Test Input Size: 320, Observed Inference Time: 307 ms
    Description: Processes camera input for object detection and provides visual information that can be used by the language model and robot-control systems.
  
    Language Model - Qwen2.5:1.5B, Runtime: Ollama
    Description: Handles natural-language interaction, reasoning, response generation, and eventually high-level robot planning and tool selection.
      
    Text-to-speech - Piper, Voice: en_US-amy-medium
    Description: Converts LLM-generated responses into locally synthesized speech.
    
  Hardware Architecture
  
    Raspberry Pi 4 Model B
    Description: Primary onboard computer responsible for running ROS 2, AI inference, computer vision, speech processing, and high-level robot logic.

    Arduino Uno R3
    Description: Low-level microcontroller used for direct hardware control, including servo commands and other time-sensitive actuator operations.
  
    Raspberry Pi Camera V2
    Description: Primary visual sensor used for computer vision, environmental observation, and future perception-based interaction.

    DS3218 Pro Servo Motors
    Description: High-torque actuators used for the robot's joints and mechanical movement.

    Microphone
    Description: Captures voice input for speech recognition, voice commands, and conversational interaction with the robot.

    Speaker
    Description: Provides audio output for synthesized speech, system feedback, alerts, and other robot-generated sounds.

    Power System (Initial Development: Bench Power Supply)
    Description: Provides regulated electrical power during development, testing, and debugging of the robot's computing, control, and actuator systems.

    3D-Printed Structures
    Description: Forms the robot's mechanical body, mounting structures, joint supports, enclosures, and component brackets used to integrate the electronic and mechanical subsystems.


  Software Architecture

    Brain
    - robot-ai/brain/milana.py
    Description: Primary high-level orchestration layer for Milana, the embodied AI personality of Project SIKLAB.  
    Manages speech input, LLM interaction, vision requests, grounded response generation, 
    and future robot tool,memory, emotion, and behavior integrations.

    Speech
    - robot-ai/speech/transcripts
    - robot-ai/speech/user.wav
    - robot-ai/speech/robot.wav
    Description: Handles microphone recording, whisper transcription, and piper outputs.

    Vision
    - robot-ai/vision/live_detect.py
    - robot-ai/vision/vision_system.py
    Description: Handles camera capture, YOLO inference, object detection, and
    perception filtering. The perception layer was introduced to provide grounded
    environmental context to the language model, reducing reliance on generic
    responses when answering questions about the robot's surroundings.

    Emotion (Status: In development)
    - emotion/
    Description: Reserved for future implementation of internal emotional state,
    behavioral modulation, and personality-state integration.
  
    Motion (Status: In development)
    - motion/
    Description: Reserved for robot motion control, gait development, actuator
    coordination, and future integration with high-level AI commands.
  
    AI Models and Inference Components
    
    - robot-ai/models/YOLO/Yolo26n
    Description: Object-detection model used by the visual perception pipeline.
    
    - robot-ai/models/Piper
    Description: Voice model and related resources used by the offline TTS pipeline.
    
    - robot-ai/whisper.cpp
    Description: Local Whisper inference runtime used for speech recognition.
      
    Runtime Logs
    
    - robot-ai/logs
    Description: Stores runtime logs used to record Milana's responses, system
    behavior, debugging information, and interaction history during development

  Mechanical Systems

  The following components formed the initial mechanical configuration of SIKLAB
  Humanoid V1. 
  
  Detailed dimensions, materials, servo orientation, print settings,
  CAD revisions, mass properties, and center-of-mass considerations are documented
  in the Mechanical Design Specification.

   SIKLAB Humanoid V1 - 2026
  
     Head
      - Pi Camera V2 dock
      - Pan Mechanism
    Description: Houses the robot's primary visual sensor and provides horizontal
    camera movement for active visual perception.
    
     Torso
      - Raspberry Pi 4 Model B dock
      - Arduino Uno R3 dock
      - Power distribution 
      - Cable routing
    Description: Serves as the central structural enclosure for the robot's onboard
    computing, embedded-control, and power-distribution hardware.

     Lower Body
      - Left Hip
      - Right Hip
      - Left Leg
      - Right Leg
      - Left Ankle
      - Right Ankle
      - Left Foot
      - Right Foot
    Description: Provides the primary load-bearing and locomotion structure of
    Humanoid V1, integrating the hip, leg, ankle, and foot assemblies required for
    standing, balance, and future walking development.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  
DEVELOPMENT MILESTONES

 Milestone 001 — Local LLM Deployment

  Qwen2.5:1.5B was successfully deployed on the Raspberry Pi 4 using Ollama.

  Significance:
  Established that SIKLAB could perform natural-language inference locally
  without depending on a cloud LLM API.

---

 Milestone 002 — Offline Speech Recognition

  Whisper.cpp with the `tiny.en` model was integrated into the speech pipeline.
  
  Significance:
  Enabled local conversion of microphone input into text.

---

 Milestone 003 — Offline Speech Synthesis

  Piper with the `en_US-amy-medium` voice was integrated.
  
  Significance:
  Completed the initial local conversational pipeline:
  
  Human Speech -> Whisper.cpp -> Qwen2.5 -> Piper -> Robot Speech
  
---

 Milestone 004 — Computer Vision Integration

  YOLO26n was tested on Raspberry Pi 4 using camera input from the Raspberry Pi
  Camera V2.
  
  Initial benchmark:
  
  - Input size: 320 × 320
  - Inference time: ~307 ms
  
  Significance:
  Established the first visual perception capability for SIKLAB.

---

 Milestone 005 — Milana Integration

  The separate speech, language-model, and perception components were integrated
  under `milana.py`.
  
  Significance:
  Milana evolved from individual AI components into an early embodied AI
  orchestration layer.
