# Hierarchical Federated Learning for Vehicular Networks

## Baseline Implementation and Evaluation

This repository provides a Python/TensorFlow implementation of Hierarchical Federated Learning (HFL) for vehicular networks. The purpose of this project is to validate the hierarchical federated learning workflow in a realistic vehicular setting, rather than to achieve state-of-the-art image classification accuracy.

The system models a three-layer V2X learning architecture:

Vehicles (Clients) → Roadside Base Stations (Edge) → Macro Cloud Server


This architecture reflects real vehicular networks where vehicles communicate with nearby roadside units (RSUs), which then synchronize with a central cloud server.

### Motivation

Centralized Federated Learning (CFL) is inefficient in vehicular networks due to:

High communication latency

Cloud bottlenecks

Single points of failure

Highly dynamic vehicle mobility

Hierarchical Federated Learning (HFL) introduces an edge layer between vehicles and the cloud. Instead of all vehicles sending updates directly to the cloud, vehicles communicate with nearby base stations, which perform local aggregation before forwarding results to the cloud. This reduces communication overhead, improves scalability, and better supports mobility.

### System Architecture

The implemented HFL system contains three layers:

Vehicles (Clients): Train local neural networks using private data

Roadside Base Stations (Edge Servers): Aggregate vehicle models

Macro Cloud Server: Aggregates base-station models into a global model

The experimental setup uses:

50 vehicles

5 base stations

1 macro-level cloud server

Each base station serves 10 vehicles.

### Dataset

The CIFAR-10 dataset is used as the learning task.

60,000 images (32×32, 10 classes)

50,000 training samples

10,000 test samples

The training data is split IID across 50 vehicles (≈1000 samples per vehicle).
The test set remains centralized at the cloud and is only used for evaluation.

The dataset is not stored in this repository.
Download CIFAR-10 from:

https://www.cs.toronto.edu/~kriz/cifar.html


Extract it into the data/ directory.

### Learning Model

A lightweight CNN is used to reflect realistic on-vehicle computation:
- Two convolutional layers with ReLU
- Max-pooling
- Fully connected layer
- Softmax output

Training settings:
- Optimizer: Stochastic Gradient Descent (SGD)
- Learning rate: 0.01
- Loss: Sparse categorical cross-entropy

This simple model is chosen to emphasize federated learning behavior, not raw classification performance.

### Hierarchical Training Process

Each HFL round follows four steps:

**Step 1 — Local Training (Vehicles)**
Each vehicle trains its model for one local epoch using its private data.

**Step 2 — Edge Aggregation (Base Stations)**
Each base station aggregates the models from its 10 vehicles using data-size-weighted averaging, producing an edge-level model.

**Step 3 — Cloud Aggregation**
The cloud aggregates the 5 base-station models into a global model using weighted averaging.

**Step 4 — Model Distribution**
The global model is sent back to all base stations and then to all vehicles for the next round.
This two-stage aggregation implements the full Hierarchical Federated Learning pipeline.

### Experimental Results

The system is run for multiple HFL rounds. After each round, the global model is evaluated on the CIFAR-10 test set.
The results show:
- Test accuracy increases steadily
- Test loss decreases monotonically
- Training remains stable
This confirms that hierarchical aggregation correctly propagates local learning progress and enables convergence.

### Scope of This Implementation

This repository implements a baseline HFL system:
- All vehicles participate every round
- Data is IID
- No mobility, scheduling, or resource optimization
- No partial participation
The goal is to provide a clean reference implementation for validating hierarchical federated learning.

### Future Extensions

- This codebase can be extended to support:
- Non-IID data distributions
- Partial and dynamic vehicle participation
- Mobility-aware base station association
- Communication-aware scheduling
- Reinforcement-learning-based resource allocation
- Comparison with fully decentralized FL



Download CIFAR-10 from: https://www.cs.toronto.edu/~kriz/cifar.html
Extract to: data/
