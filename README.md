# MotoScout DMS: Drone-Assisted Distributed Routing for Motorcyclists

## Project Description
**MotoScout DMS** is a conceptual Distributed Messaging System designed to make motorcycling both safer and more enjoyable. Traditional navigation systems prioritize the fastest route, often neglecting a rider's unique preferences—such as a desire for scenic backroads, curvier paths, or group-ride compatibility. 

This project solves that problem by leveraging an autonomous drone to scan the immediate local area (a 5 to 10-mile radius). The drone transmits raw environmental and road data to a cloud-based AI, which processes the information according to the rider's specific preferences. The optimized route is then funneled through a centralized Message Broker directly back to the rider's mobile application and on-bike navigation hardware, allowing motorcyclists to make informed, real-time routing decisions while safely stationary.

To demonstrate the architecture and validate the data pipeline, the system's core communication network is simulated using the **Message Passing Interface (MPI)** in C.

---

## System Architecture & Data Flow
The architecture simulates a highly decoupled, reactive network of independent nodes partitioned using localized MPI communicators (`MPI_Comm_split`):

1. **Mobile App (Rank 0):** The entry point where the user inputs custom routing preferences (e.g., *"backroads only"*). It passes the request to the Message Broker and handles peer-to-peer broadcasting to sync routes with fellow group riders.
2. **Message Broker (Rank 1):** The central nervous system. It uses non-blocking communication (`MPI_Isend`) to dispatch tasks to the drone while concurrently managing incoming optimized routes from the AI platform.
3. **Drone System (Rank 2):** Simulates aerial data acquisition. It accepts requests from the broker, simulates internal camera/sensor data generation, and handles autonomous data transmission to the AI layer.
4. **AI Processing (Rank 3):** Consumes the heavy raw data payloads from the drone, applies simulated route-optimization logic, downsamples the data size into an optimized path, and routes it back to the broker.
5. **Navigation System (Rank 4):** A reactive edge-node embedded on the motorcycle that receives the final computed route from the broker and displays it instantly to the rider.

---

## Core Technologies
* **Language:** C
* **Parallel/Distributed Framework:** MPI (Message Passing Interface)
* **Key MPI Concepts Used:** Point-to-point communication (`MPI_Send`/`MPI_Recv`), Non-blocking I/O (`MPI_Isend`), Collective operations (`MPI_Bcast`), and Custom Communicator Management (`MPI_Comm_split`).
