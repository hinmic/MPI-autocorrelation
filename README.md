# High-Performance Distributed Autocorrelation with C++ & MPI
### Overview
This repository implements a high-performance distributed system to compute the Autocorrelation of massive datasets. Autocorrelation is a critical statistical tool used to measure the correlation of a signal with a delayed version of itself.

In this project, I utilised MPI (Message Passing Interface) to distribute the computational load across multiple processors, processing a dataset of 8 million elements with 1024 shifts. This implementation achieved a linear speed-up curve, demonstrating efficient load balancing and minimal communication overhead.

### System Architecture: Master-Worker Pattern
The program utilises a robust Commander-Worker (Master-Slave) architecture to ensure data integrity and parallel efficiency:

#### 1. Commander Processor:

Reads the primary dataset from the source.

Partitions the data array based on the number of available worker nodes.

Dispatches sub-arrays and shift-parameters via MPI_Send.

#### 2. Worker Processors:

Receive data segments via MPI_Recv.

Perform independent partial autocorrelation computations.

Return local results back to the Commander.

#### 3. Aggregation:

The Commander collects all local results and performs the final sum/normalisation to produce the global output.

### Technical Specifications
Dataset Size: 8,000,000+ elements.

Shift Depth: 1024 shifts.

Communication Protocol: Point-to-point blocking communication for guaranteed data consistency.

Performance Metric: Optimised for Linear Speed-up (Execution time decreases proportionally as the number of processors increases).

### Mathematical Definition
The program calculates the autocorrelation coefficient $`R(k)`$ for a lag $`k`$:

$`R(k) = \sum_{i=1}^{n-k} (X_i)(X_{i+k})`$

By parallelising this summation across $`P`$ processors, the complexity is reduced from $`O(N \cdot K)`$ to $`O(\frac{N \cdot K}{P})`$.

### Getting Started
#### Prerequisites
A C++ compiler (e.g. g++).

An MPI implementation (e.g. OpenMPI).

#### Compilation
Build the project using mpic++:
mpic++ -o autocorrelation_mpi main.cpp

#### Execution
Run the simulation across _N_ processors:

mpirun -np _N_ ./autocorrelation_mpi

### Future Roadmap
Non-blocking Communication: Implement MPI_Isend and MPI_Irecv to overlap computation with communication.

Hybrid Parallelism: Integrate CUDA kernels within each MPI node to create a multi-GPU distributed simulator.

Real-time Streaming: Adapt the architecture to process live telemetry data from ion-trap sensors.

#### Quantum Relevance
In ion-trap quantum computing, autocorrelation might be helpful for:

Noise Analysis: Analysing $`1/f`$ noise and decoherence patterns in qubits.

Signal Processing: Processing high-frequency microwave control signals.

System Scaling: Developing communication protocols for modular multi-chip architectures.
