# MTC Student Developer Programme 2023

![alt Quantum](https://github.com/MaalavikaS/Maals.github.io/blob/main/Q.png)
# Quantum Computing: Applications in Chemistry and Material Sciences<br>
Quantum Computing is a newly emerging technology that harnesses the physics of quantum mechanics to solve complex problems that would otherwise be computationally expensive using classical theory (binary states). The fundamental unit in quantum computation that gives it an edge over classical methods is the qubit which can exist in multiple states as opposed to just two.<br>

The key purpose in the quantum computing world is to identify the problems that would benefit from a quantum approach. Classical techniques offer simpler solutions in most cases but fail when the size or complexity of problems to be solved increases. Quantum Computing finds applications in increasing the speed of search algorithms like Grover’s Algorithm and Shor’s Algorithm where polynomial and exponential speedup have been obtained, respectively. Quantum Cryptography is also a popular application where security is enhanced due to the inability to determine the state of a qubit in superposition.<br>

Recently, Chemical and Material Sciences have seen an increase in quantum computing applications. Due to the molecular systems being Quantum systems in nature, the ability to interpret their interactions becomes easier. Molecules are quantum systems and properties such as superposition, entanglement and interference affect the molecular state and its configurations. Simulating such interactions using classical methods are too vast and provide only estimates. Quantum methods are more accurate since they simulate the actual behavior of the molecules.
Quantum approach can prove beneficial in pharmaceutical industries where drug discovry process can be accelerated as it relies predominantly on molecular level simulations and interactions. Classical methods lack the ability to view reactions from an electronic point of view and hence their dynamics with each other can't be accurately predicted.<br>

Quantum Computing has the capability to simulate molecular interactions, and these simulations can be effectively demonstrated using Azure Quantum Elements. The computation of ground state energy for molecules can be accomplished using various quantum algorithms, such as Variational Quantum Eigensolvers (VQEs) and Quantum Adiabatic Optimization Algorithm (QAOA). Through these quantum algorithms, it becomes possible to explore and understand molecular reactions, identify their by-products, and gain insights into the behavior of chemical systems. Additionally, quantum simulations can aid in the discovery of novel materials with potential applications in various emerging fields.

## Azure Quantum & Chemistry<br>
The Azure Quantum workspace provides hardware and software to run quantum computations on actual Quantum Hardware or Simulators. The Quantum Development Kit with its Chemistry Tools provides many built-in functions to estimate ground state energies such as QPEs, perform Jordan Wigner transformations, loading Broombridge schematics etc.
In this blog we shall look into the implementation of a Variational Quantum Eigensolver using Qiskit and run it on an Azure backend for three molecules: 
  - H2 (Inter-Atomic distance of 0.76 Armstrong)
  - HF (Inter-Atomic Distance of 0.92 Armstrong)
  - LiH (Inter-Atomic distance of 1.6 Armstrong)<br>
  
(The Inter-Atomic distances are approximations and not the exact values)<br>
The compounds/molecules are chosen based on their size and inter-molecular distances as larger molecules require a greater number of qubits which is limited by the availability of qubits by the Quantum resource providers. 

## Introduction to Variational Quantum Eigensolvers (VQEs) and its Implementation<br>
VQEs are a class of hybrid algorithms in Quantum Computing that aims to obtain the ground state energy of a molecule (usually in a configuration). A quantum circuit (An trial wave function that represents the problem to be solved) is measured for the energy and the classical computer evaluates on how the parameters assumed initially can be improved. Once the classical computer fine tunes the quantum circuit parameters using an optimizer and passes it back to the quantum circuit, the procedure is repeated till the ground state energy is obtained. The mathematics behind the VQEs depends on the fact that the expectation of a wave function’s energy is always greater than or equal to its exact ground state energy level. Thus as the parameters are optimized the expectation value of the enrgy gets closer to the actual ground state energy.<br>

In simple terms, the quantum part comes into effect in simulating different electronic configurations in the molecule and their interactions. These are then measured at each configuration and sent to the classical algorithm. Classical optimization techniques like gradient descent, SPSA or SLSQP are performed for each configuration to finally obtain the lowest energy (the ground state) for that molecule. It is variational as the expectation of the trial wavefunction is at least equal to the ground state energy.<br>

  ### Keywords and their meaning<br>
- **Hamiltonian**: An operator defined for a system, it encompasses both the kinetic and potential energy of a system. The Hamiltonian is a mathematical function that encompasses the energy of a system of atoms or molecules<br>
- **Ground State**: Lowest energy level for an atom or a molecule<br>
- **Quantum Phase Estimation**: Determines the eigen values using Quantum techniques meant for quantum computation tasks<br>
- **Eigensolver**: A classical algorithm to find the eigen values given a matrix<br>
- **Simultaneous Perturbation Stochastic Approximation (SPSA)**:  SPSA is a gradient-free optimization algorithm that excels in noisy or black-box environments<br>
- **Sequential Least Squares Quadratic Programming (SLSQP)**: SLSQP is a gradient-based optimization algorithm that is well-suited for smooth optimization problems with constraints


## Tech Stack

| Software/Service/Language | Description | 
| :-------------- | --------------- | 
| [Azure Quantum](https://learn.microsoft.com/en-us/azure/quantum/) | Workspace to run quantum computation tasks with the help of providers   | 
| [IonQ Provider](https://learn.microsoft.com/en-us/azure/quantum/provider-ionq#ionq-harmony-quantum-computer) | Dynamically reconfigurable trapped-ion quantum computer for up to 11 fully connected qubits that lets you run a two-qubit gate between any pair. IonQ Simulators offer upto 29 qubits   | 
| [Quantum Development Kit](https://learn.microsoft.com/en-us/training/modules/qsharp-create-first-quantum-development-kit/2-install-quantum-development-kit-code?ns-enrollment-type=learningpath&ns-enrollment-id=learn.quantum-computing-fundamentals) | An SDK used to interface with Azure Quantum service. Supports languages such as Q#, Qiskit and Cirq |
| [Q#](https://learn.microsoft.com/en-us/training/modules/get-started-azure-quantum/get-started-jupyter-notebook?ns-enrollment-type=learningpath&ns-enrollment-id=learn.quantum-computing-fundamentals&tabs=tabid-ionq) | Q# is a domain-specific language optimized for quantum programming in the Microsoft Quantum ecosystem |
| Python | Programming language |
| [Quantum Chemistry Library](https://learn.microsoft.com/en-us/azure/quantum/user-guide/libraries/chemistry/installation) | The quantum chemistry library included in the Quantum Development Kit is specifically designed to synergize effectively with different computational chemistry packages |
| [Qiskit](https://qiskit.org/) | Qiskit is based on Python, widely used, and focuses on a variety of quantum applications - Developed by IBM |
| [Broombridge](https://learn.microsoft.com/en-us/azure/quantum/user-guide/libraries/chemistry/schema/broombridge) | It is a data format that represents various aspects of a quantum circuit |
| [PYSCF Driver](https://qiskit.org/documentation/stable/0.31/apidoc/qiskit.chemistry.drivers.pyscfd.html) | Python-based Simulations of Chemistry Framework, a platform for quantum chemistry calculations and methodology |
| [NWChem (Alternate to PySCF Driver)](https://arrows.emsl.pnnl.gov/api/) | NWChem is an open-source computational chemistry software package widely used for performing quantum chemical calculations and simulations. The fermionic Hamiltonian data for various compounds can be downloaded to be used in simulations|

## Procedure

1) Realize a Hamiltonian to compute the ground state of the required molecule – this would be the problem you want to solve for ( Molecular Hamiltonian in this case).
2) Map the Fermionic (molecular) Hamiltonian to a qubit Hamiltonian. – The qubit Hamiltonian mimics the electronic interactions within the molecule. This is where the limitation arises. As the size of the molecule increases, so does the number of electrons in each orbital and hence the number of qubits needed to encode them.
 BeH2 remains the largest molecule to be simulated as of 2022.<br>
There are a few methods to transform a Fermionic Hamiltonian to a qubit 	Hamiltonian.
    - The Jordan Wigner representation aids in converting a Hamiltonian which consists of molecular arrangements (Fermions) into qubit states – this enables the simulation of molecular level computation (as molecules are quantum level systems in itself)
    - The Bravyi Kitaev transformation is a specific approach for mapping fermionic Hamiltonians to qubit Hamiltonians. It is particularly useful when dealing with problems in quantum chemistry, where the electronic structure of molecules is described by fermionic operators.
3) Creating an Ansatz- Ansatz is an assumed function which has some adjustable parameters that can be manipulated to obtain an electronic configuration that gives us the ground state (in this case) -It is the set of qubits and quantum gates that represents the assumption with all its operations and variables. (Operations <=> Quantum Gates)<br>
There are different kinds of Ansatz available:
    - UCCSD(As used here)
    - RY
    - RYRZ
4) Compute the total energy of this system by implementing it on quantum hardware.<br>

### Running on Azure Quantum Backend (Ionq provider) <br>

5.	Apply classical optimization techniques such as SPSA or Gradient descent to minimize the cost function. This tunes the ansatz circuit parameters and is then sent back to the quantum circuit for energy re-estimation for that configuration.
6.	Reiterate the procedure till the minimum energy is obtained.

## Block Diagram 
![alt VQE workflow](https://github.com/MaalavikaS/Maals.github.io/blob/main/Fermionic%20Hamiltonian.png)

## Code Snippet
### [Source Code obtained from Microsoft Quantum Development Kit Samples](https://github.com/microsoft/Quantum/blob/main/samples/azure-quantum/variational-quantum-eigensolver/VQE-qiskit-hydrogen-session.ipynb)

### Using Qiskit
#Importing PySCFDrivers to obtain the Hamiltonian for the molecule of choice
```python
from qiskit_nature.second_q.drivers import PySCFDriver
driver = PySCFDriver(atom="Li 0 0 0; H 0 0 1.6", basis="sto-3g")
problem = driver.run()
```
#Calling the Jordan-Wigner Mapper which maps a fermionic Hamiltonian to a qubit Hamiltonian
```python
from qiskit_nature.second_q.mappers import JordanWignerMapper
mapper = JordanWignerMapper()
```
#Creating a UCCSD(Unitary Coupled Cluster Singles and Doubles) Ansatz (which is usually preferred for a VQE problem) to compute the energy for a certain configuration of parameters. In UCCSD, the wavefunction of a molecular system is represented as a unitary transformation applied to a reference state, typically the Hartree-Fock state.
```python
from qiskit_nature.second_q.circuit.library import UCCSD, HartreeFock
ansatz = UCCSD(
    problem.num_spatial_orbitals,
    problem.num_particles,
    mapper,
    initial_state=HartreeFock(
        problem.num_spatial_orbitals,
        problem.num_particles,
        mapper,
    ),
)
```

#Initializing the VQE function in Qiskit and passing the Ansatz and the optimizer(Classical part) to estimate the ground state
```python
import numpy as np
from qiskit.algorithms.optimizers import SPSA
from qiskit.algorithms.minimum_eigensolvers import VQE
from qiskit.primitives import Estimator
vqe = VQE(Estimator(), ansatz, SPSA())
```

#Setting the initial parameters to zero and begin ground state estimation and comparing each configuration to check for the minimum value
```python
vqe.initial_point = np.zeros(ansatz.num_parameters)

from qiskit_nature.second_q.algorithms import GroundStateEigensolver
solver = GroundStateEigensolver(mapper, vqe)
result = solver.solve(problem)

print(f"Total ground state energy = {result.total_energies[0]:.4f}")
```

### Running the algorithm with IonQ Simulators
```python
from azure.quantum.qiskit import AzureQuantumProvider

# Connect to the Azure Quantum workspace via a Qiskit provider
provider = AzureQuantumProvider(
            resource_id = "",
            location = "")
ionq_sim = provider.get_backend('ionq.simulator')

# Set the backend you want to use here.
# WARNING: Quantinuum simulator usage is not unlimited. Running this sample against it could consume a significant amount of your eHQC quota.
backend_to_use = ionq_sim

from qiskit_nature.second_q.algorithms import GroundStateEigensolver
solver = GroundStateEigensolver(mapper, vqe)
# Wrap the call to solve in a session "with" block so that all jobs submitted by it are grouped together.
# Learn more about Interactive Hybrid and the Session API at https://aka.ms/AQ/Hybrid/Sessions/Docs
with backend_to_use.open_session(name="VQE H2") as session:
    result = solver.solve(problem)

print("AzureQuantum " + backend_to_use.name() + " result:\n")
print(result.groundenergy)
```
## Output
**Ansatz** : UCCSD<br>
**Optimizer Used** : SPSA <br>
**Backend** : IonQ Simulator<br>

| Molecule | Ground State Energy using simulator)| Ground State Energy using QPU (IonQ Aria) |
|:---------- | ---------- | ---------- |
| H2 | -1.804 eV | -1.832 eV |
| HF | -102.864 eV | -101.038 eV|
| LiH | -6.836 eV| -6.445 eV |

----------------------------------------------------------------------------------------------

## Visualizing Molecules using Copilot for Azure Quantum
**Di-Hydrogen**
![alt H2](https://github.com/MaalavikaS/Quantum_Team.github.io/blob/main/Copilot-H2.png)

**Hydrogen Fluoride**
![alt HF](https://github.com/MaalavikaS/Quantum_Team.github.io/blob/main/Copilot-HF.png)
---------------------------------------------------------------------
## References and Sources
### VQE Related Articles :electron:
  - [Simulating Ground State of LiH Molecules](https://rodneyosodo.medium.com/simulating-molecules-lih-using-vqe-de40d7456fcc)
  - [A simple guide to VQEs](https://medium.com/geekculture/a-simple-guide-to-vqes-b836d5021b1b)
  - [Ucc based VQE](https://qiskit.org/ecosystem/nature/howtos/vqe_ucc.html#how-to-vqe-ucc)
  - [Quantum Molecular Simulatoins using VQEs](https://medium.com/mit-6-s089-intro-to-quantum-computing/quantum-molecule-simulation-using-vqe-d34b9c651e3d)
  - [The Variational Quantum Eigensolver](https://lzylili.medium.com/the-variational-quantum-eigensolver-c473c6dcd46#:~:text=So%20the%20steps%20are%20of%20a%20general%20VQE,parameters%20Run%20that%20through%20the%20parameterized%20quantum%20circuit)

### Azure Quantum Tools for Chemistry Applications 🧰
  - [Quantum Developer Tools for Chemistry](https://devblogs.microsoft.com/qsharp/quantum-developer-tools-for-chemistry/)
  - [Estimating The Ground State Energy of Hydrogen using VQEs on Azure Quantum](https://learn.microsoft.com/en-us/samples/microsoft/quantum/estimating-the-ground-state-energy-of-hydrogen-using-variational-quantum-eigensolvers-vqe-on-azure-quantum/)
  - [Chemistry Library Installations](https://learn.microsoft.com/en-us/azure/quantum/user-guide/libraries/chemistry/installation)
  - [Chemistry Library Samples](https://learn.microsoft.com/en-us/azure/quantum/user-guide/libraries/chemistry/samples/end-to-end)

### Azure Quantum Learning Resources 📑
  - [Quantum Computing Fundamentals](https://learn.microsoft.com/en-us/training/paths/quantum-computing-fundamentals/)
  - [Q# and Microsoft QDK](https://learn.microsoft.com/en-us/azure/quantum/overview-what-is-qsharp-and-qdk)
  - [Hybrid Computing Concepts](https://learn.microsoft.com/en-us/azure/quantum/hybrid-computing-concepts)
  - [Copilot for Azure Quantum](https://quantum.microsoft.com/en-us/experience/quantum-coding)
