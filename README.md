# Maals.github.io
Quantum Computing with Azure Quantum
# Quantum Computing: Applications in Chemistry and Material Sciences<br>
Quantum Computing is a newly emerging technology that harnesses the physics of quantum mechanics to solve complex problems that would otherwise be computationally expensive using classical theory (binary states). The fundamental unit in quantum computation that gives it an edge over classical methods is the qubit which can exist in multiple states as opposed to just two.<br>

The key purpose in the quantum computing world is to identify the problems that would benefit from a quantum approach. Classical techniques offer simpler solutions in most cases but fail when the size or complexity of problems to be solved increases. Quantum Computing finds applications in increasing the speed of search algorithms like Grover’s Algorithm and Shor’s Algorithm where polynomial and exponential speedup have been obtained, respectively. Quantum Cryptography is also a popular application where security is enhanced due to the inability to determine the state of a qubit in superposition.<br>

Recently, Chemical and Material Sciences have seen an increase in quantum computing applications. Due to the molecular systems being Quantum systems in nature, the ability to interpret their interactions becomes easier. Molecules are quantum systems and properties such as superposition, entanglement and interference affect the molecular state and its configurations. Simulating such interactions using classical methods are too vast and provide only estimates. Quantum methods are more accurate since they simulate the actual behavior of the molecules.<br>

Quantum Computing can simulate molecular interactions. Simulations can be demonstrated using Azure Quantum Elements. Computation of ground state energy of molecules using various algorithms like VQEs, Quantum Adiabatic Optimization Algorithm (QAOA) can be done. Understanding molecular reactions, their by-products, etc. can be achieved. It can also be used in the discovery of newly emerging materials.

## Azure Quantum & Chemistry<br>
The Azure Quantum workspace provides hardware and software to run quantum computations on actual Quantum Hardware or Simulators. The Quantum Development Kit with its Chemistry Tools provides many built-in functions to estimate ground state energies such as QPEs, perform Jordan Wigner transformations, loading Broombridge schematics etc.
In this blog we shall look into the implementation of a Variational Quantum Eigensolver using Qiskit and run it on an Azure backend for three molecules: 
  - H2 (Inter-atomic distance of 0.76 Armstrong)
  - LiH (Inter-atomic distance of 1.6 Armstrong)
  - HF (Inter-Atomic Distance of 0.92 Armstrong)
    
The compounds/molecules are chosen based on their size and inter-molecular distances as larger molecules require a greater number of qubits which is limited by the availability of qubits by the Quantum resource providers. 

## Introduction to VQEs<br>
VQEs are a class of hybrid algorithms in Quantum Computing that aims to obtain the ground state energy of a molecule (usually in a configuration). The classical computer is used to define quantum circuits with certain parameters. After the quantum state is measured, the classical computer evaluates how to improve the parameters and then resubmits the circuits. The mathematics behind the VQEs depends on the fact that the expectation of a wave function’s energy is greater than or equal to its exact ground state energy level.<br>

In simple terms, the quantum part comes into effect in simulating different electronic configurations in the molecule. These are then measured at each configuration and sent to the classical algorithm. Classical optimization techniques like gradient descent, SPSA or SLSQP are performed for each configuration to obtain the lowest energy (the ground state) of that molecule. It is variational as the expectation of the trial wavefunction is at least equal to the ground state energy.<br>

  ### Keywords and their meaning<br>
•	Hamiltonian: An operator defined for a system, it encompasses both the kinetic and potential energy of a system. The Hamiltonian is a mathematical function that encompasses the energy of a system of atoms or molecules<br>
•	Ground State: Lowest energy level for an atom or a molecule<br>
•	QPE: Determines the eigen values using Quantum techniques meant for quantum computation tasks<br>
•	Eigensolver: A classical algorithm to find the eigen values given a matrix<br>


![alt VQE workflow](https://github.com/MaalavikaS/Maals.github.io/blob/main/Fermionic%20Hamiltonian.png)
