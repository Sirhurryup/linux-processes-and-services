# Linux Processes and Services

## Purpose

This repository documents my effort to move beyond executing Linux commands and begin understanding the system behavior those commands reveal.

The work is based on four hands-on labs covering:

1. Performance and Resource Monitoring
2. systemd Services
3. Diagnosing Increasing System Load
4. Building a Resilient systemd Service

The labs provide the technical environment, but they are not the complete learning experience. Each exercise will be examined through conversation, observation, evidence, and reflection so that I can understand not only what command to run, but why that command is appropriate and what its output reveals about the system.

## Learning Goal

My goal is to develop a stronger Linux mental framework around processes, services, system resources, and operational troubleshooting.

Instead of guessing when a server becomes slow or unstable, I want to learn how to ask disciplined questions:

* What changed?
* Which resource is under pressure?
* Which process is responsible?
* Who or what started that process?
* Is the process connected to a managed service?
* What evidence supports the diagnosis?
* What action can be taken without making the incident worse?
* How can the system be made more resilient afterward?

These questions turn Linux commands into investigative tools.

## Working Method

Each lab will follow the same reasoning process:

```text
Observe
   ↓
Question
   ↓
Inspect
   ↓
Interpret
   ↓
Form a Theory
   ↓
Test
   ↓
Act
   ↓
Verify
   ↓
Extract a Principle
```

The command itself is not the final answer. It is an instrument used to gather evidence.

## Lab Documentation

Each lab record will include:

* The operational situation
* The administrator’s first question
* Commands executed
* Important output
* Interpretation of the evidence
* Incorrect assumptions avoided
* Decisions made
* Verification of the result
* Lessons the lab did not explicitly teach
* Engineering principles earned
* Personal reflection

## Original Story

This repository will not reproduce the training platform’s scenarios as a tutorial.

Instead, it will document my own learning journey: the questions I asked, the mistakes I corrected, the evidence I interpreted, and the changes that occurred in the way I think about Linux systems.

The final story will focus on how learning processes and services helped me stop seeing Linux as a collection of unrelated commands.

I am beginning to see Linux as a connected system:

```text
Users and Applications
          ↓
       Services
          ↓
       Processes
          ↓
        Kernel
          ↓
CPU | Memory | Storage | Network
```

A performance problem may appear at the application layer, but the evidence may lead through a service, into one or more processes, and finally toward pressure on a particular system resource.

## Final Deliverables

This repository will conclude with:

* Four completed lab investigations
* A collection of Linux administration principles
* A multiple-choice and scenario-based assessment
* A reflection on how my system thinking changed
* A Medium article designed to help other learners develop a stronger Linux mental model

## Central Principle

> A slow system should not trigger random commands or immediate reactions. It should trigger better questions, deliberate observation, and evidence-based decisions.

