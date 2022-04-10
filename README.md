# Micrososft Challenge!

# Team QQT

### Part 1: Warmup tasks
Solutions to Part 1 of this challenge can be found in the folder [Part1](./Part1).
* [Task 1. Implementing a quantum oracle (Q#)](./Part1/Task1_QuantumOracleQsharp.ipynb) – 1 point
* [Task 2. Submitting Azure Quantum jobs from Q# Jupyter Notebooks (Q# + IonQ simulator)](./Part1/Task2_DeutschAlgorithmQsharpIonQ.ipynb) – 1 point
* [Task 3. Submitting Azure Quantum jobs from Python Jupyter Notebooks (Qiskit + Quantinuum emulator)](./Part1/Task3_QrngQiskitQuantinuum.ipynb) – 1 point

### Part 2: Free-form project
For part 2, we have implemented the Variational Quantum Thermalizer(VQT) and noise-induced VQT for 3 qubit Sherrington Kirkpatrick (SK) model of spin glass.
 
VQT for 3 qubit SK model can be found in  the folder [Part2](./Part2) in the file 3qubitSK_Model_VQT.ipynb

A noise-induced VQT for 3 qubit SK model can be found in  the folder [Part2](./Part2) in the file Noisy3qubitSK_Model_VQT.ipynb



### Blog post for the free form project
#The Variational Quantum Thermalizer Algorithm


## Theory
Variational Quantum Thermalizer(VQT) is a natural generalization of the ubiquitous Variational Quantum Eigensolver Alogorithm(VQE).

VQT prepares the thermal state of a given hamiltionian, defined as:
$$
\rho_\text{thermal} \ = \ \frac{e^{- H \beta / k_B}}{\text{Tr}(e^{- H \beta / k_B})}
$$

where $H$ is the is the Hamiltonian of the system, $\beta \ = \ 1/T$ , where T 
is the temperature of our system.


##Simulation

For our case, we run the 3 qubit Sherrington Kirkpatrick (SK) model of spin glass. This is a classical
spin model with all-to-all couplings between the n spins.

The SK Hamiltonian over n spins is given as: 

$$
H(x)=-1/\sqrt{n} \sum_{i<j} w_{i,j}x_ix_j
$$
 where $x_i\in\{\pm 1\}$ is the configuration of spins and $w_{i,j}\in\{\pm 1\}$ is a disorder chosen independently and uniformly at random.


```
## Noise modeling

Then, we added a depolarizing noise to each qubit in order to have a finer control of the noise across the circuit. We did this by creating a noise error for the custom instruction used in the ansatz.

```python
from qiskit.providers.aer.noise import NoiseModel
from qiskit.providers.aer.noise import QuantumError, ReadoutError

from qiskit.providers.aer.noise import depolarizing_error

# Create an empty noise model
noise_model = NoiseModel()

# Add depolarizing error to all single qubit unitary gate
error = depolarizing_error(0.1, 1)
noise_model.add_all_qubit_quantum_error(error, ['add_u_gate'])

# Print noise model info
print(noise_model)
```

## Results
Target state
Expectation Value: (-0.5033432861349194+0j)

Entropy: (1.898337128267965-0j)

Final Cost: (-2.4016804144028843+0j)

![target.png](figures/target.png)

VQT approximation

Final Entropy: 1.950094553660756

Final Expectation Value: -0.3376905843085512

Final Cost: -2.287785137969307

![vqt.png](figures/vqt.png)

Noise induced VQT model results

Final Entropy: 1.950102534136878

Final Expectation Value: -0.33768274199267745

Final Cost: -2.2877852761295556

![noise.png](figures/noise.png)
