# Quantum Palindrome Solver

![QuantumPalindromes](https://github.com/JMartinArocha/QuantumPalindromes/blob/986bced6f2f67308f2e13330dfd55ff4af60fb86/QuantumPalindromes.png)

Github: https://github.com/JMartinArocha/QuantumPalindromes

## Overview

This repository hosts a Quantum Palindrome Solver, a project leveraging the power of quantum computing to identify palindromic numbers efficiently. Palindromic numbers are numbers that read the same forwards and backwards, resembling the concept of textual palindromes but in a numerical context.

## Badges

![GitHub license](https://img.shields.io/github/license/JMartinArocha/QuantumPalindromes.svg)
![Python version](https://img.shields.io/badge/python-3.x-blue.svg)
![last-commit](https://img.shields.io/github/last-commit/JMartinArocha/QuantumPalindromes)
![issues](https://img.shields.io/github/issues/JMartinArocha/QuantumPalindromes)
![commit-activity](https://img.shields.io/github/commit-activity/m/JMartinArocha/QuantumPalindromes)
![repo-size](https://img.shields.io/github/repo-size/JMartinArocha/QuantumPalindromes)

## Project Objectives:

- Demonstrate Quantum Superposition and Entanglement: Utilize quantum gates to create states that represent all possible number combinations simultaneously and exploit quantum mechanics to identify palindromes.
- Educate on Quantum Computing Concepts: Provide a clear and approachable example of how quantum computing can be applied to solve specific problems that can be easily visualized and understood.
- Promote Quantum Experimentation: Offer a platform for users to experiment with quantum circuits, potentially adjusting the code to explore other quantum computing capabilities.

## Features:

- Quantum Circuit Implementation: A Qiskit-based quantum circuit that applies Hadamard gates for superposition, uses X gates for state flipping, and CNOT gates for conditional operations.

- Simulator Runs: Detailed examples of running the quantum circuit on a simulator, demonstrating both the setup and the analysis of results.

- Visualization: Utilization of Matplotlib to visualize quantum circuits and results, aiding in understanding and analysis.

## Circuit Initialization:

Here, a quantum circuit is initialized with 4 qubits and 4 classical bits. The qubits are used for quantum operations, and the classical bits are used to store the results of measurements.

```python
circuit = QuantumCircuit(4, 4)
```


## Applying Hadamard Gates:

Hadamard gates are applied to the first three qubits. A Hadamard gate creates a superposition of 0 and 1, meaning after this operation, each of these three qubits represents both values simultaneously. This step is crucial as it allows the exploration of all possible 3-bit combinations in parallel due to quantum superposition.

```python
circuit.h(0)
circuit.h(1)
circuit.h(2)
```

## Setting the Fourth Qubit:

An X gate (Pauli-X, quantum NOT gate) is applied to the fourth qubit. This flips the qubit from its default 0 state to 1. This qubit will act as an ancilla or helper qubit in determining if the number is a palindrome.


```python
circuit.x(3)
```

## Applying CNOT Gates:


Controlled-NOT (CNOT) gates are used here between the first and the last qubits, and the third and the last qubits. The fourth qubit changes state only if the controlling qubit (first or third) is in state 1. This setup ensures that the ancilla qubit (fourth qubit) flips its state based on the parity of the outer qubits of the binary number represented by the first three qubits.

```python
circuit.cx(0, 3)
circuit.cx(2, 3)
```

## Adding a Barrier:

A barrier is added to prevent quantum operations from being reordered by the compiler during optimization. It ensures that all operations defined before the barrier are completed before those defined after the barrier.

```python
circuit.barrier()
```

## Measurement:

Each qubit is measured and the result is stored in the corresponding classical bit. The measurement collapses the superposed qubits into definite states (either 0 or 1).

```python
circuit.measure(0, 0)
circuit.measure(1, 1)
circuit.measure(2, 2)
circuit.measure(3, 3)
```

## Circuit Drawing:

This command visualizes the quantum circuit using Matplotlib. It's a useful tool for debugging and presentations to see the arrangement and flow of gates in the circuit.

```python
circuit.draw('mpl')
```

![Qcircuit](https://github.com/JMartinArocha/QuantumPalindromes/blob/986bced6f2f67308f2e13330dfd55ff4af60fb86/Qcircuit.png)



## How It Identifies Palindromes:

In a 3-bit binary palindrome, the first and last bits must be the same. The Hadamard gates create a superposition of all possible 3-bit numbers. The ancilla qubit flips based on the equality of the first and third qubits due to the CNOT gates. If they are the same, the ancilla qubit remains 1 (as it starts at 1 and would flip twice if the bits are different — once for each differing bit, thus returning to 1). When measured, if the ancilla bit is 1, it indicates a palindrome.

By simulating this circuit on AerSimulator and observing the measurement outcomes where the ancilla bit is 1, you can identify which of the 3-bit numbers are palindromes. This is a basic demonstration of using quantum superposition and entanglement to solve a problem in parallel, leveraging the quantum properties for computational advantage over classical methods.

## Set Up the Simulator
```python
simulator = AerSimulator()  # Updated instantiation
```
- **AerSimulator**: This is part of Qiskit Aer, which is designed for high-performance simulation of quantum circuits. It can simulate noisy quantum operations, quantum error corrections, and can be used to emulate how a quantum circuit would run on an actual quantum machine.

## Transpile the Circuit
```python
transpiled_circuit = transpile(circuit, simulator)
```
- **Transpiling**: This step adapts your quantum circuit to the specific syntax and gate set supported by the simulator. Transpilation may also optimize the circuit for execution, potentially reducing the number of gates or the computational complexity of the simulation.

## Run the Transpiled Circuit
```python
job = simulator.run(transpiled_circuit)
result = job.result()
```
- **Running the Circuit**: After transpiling, the circuit is executed on the simulator. The `run` method submits the circuit to the simulator and returns a `job` object, which tracks the execution status.
- **Getting the Result**: `job.result()` fetches the results of your quantum circuit execution. This contains the outcomes of the quantum measurements across multiple shots (executions).

## Print the Results
```python
print(result.get_counts())
```
- **Result Interpretation**: The `get_counts()` method returns a dictionary where the keys are possible outcomes of the quantum circuit (e.g., '0001', '1010') and the values are the number of times each outcome was measured. For instance, if '0001' appears 500 times out of 1000 shots, the output would be `{'0001': 500}`.

## Explanation of the Process:
In this simulation:
- The circuit is first **transpiled** to ensure it is compatible with the backend's requirements (in this case, the simulator).
- It is then **executed** on the simulator to simulate how it would behave if run on a quantum computer.
- Finally, the **results** are printed to show how often each possible state of the qubits was observed.


## About virtual envs

Using a virtual environment in Python is a best practice for managing project dependencies. It allows you to create a self-contained directory that contains all the Python executable files and packages you need for your project. This way, you can avoid conflicts between package versions across different projects.

Why Use a Virtual Environment?
Dependency Management: Each project can have its dependencies without affecting other projects or the global Python installation.
Reproducibility: Makes it easier to share and collaborate with others, ensuring that everyone has the same setup.
Isolation: Prevents inadvertent changes to system files and configurations.
Setting Up a Virtual Environment
Here’s how to set up a virtual environment for your clustering project:

1. Install Virtual Environment
First, you need to install the virtualenv package if it's not already installed. This package allows you to create virtual environments in Python. You can install it using pip:

```shell
pip install virtualenv
```

2. Create a Virtual Environment
Navigate to your project directory, or where you want to set up your project:

```shell
cd path/to/your/project
```

Now, create a virtual environment within this directory:

Here, venv is the name of the virtual environment directory. You can name it anything, but venv or .venv is typical.

``` shell
virtualenv venv
```

3. Activate the Virtual Environment
Before using the environment, you need to activate it:

```shell
venv\Scripts\activate # Windows
source venv/bin/activate # Linux/macOS
```

Deactivate the Environment
To stop using the virtual environment and go back to your global Python, you simply type:

```shell
deactivate
```

Keeping Track of Dependencies
To make it easier for others to set up the same environment, it’s good practice to save your dependencies in a requirements.txt file:

```shell
pip freeze > requirements.txt
```

This command writes a list of all installed packages and their versions to the requirements.txt file. Others can install all required packages using:

```shell
pip install -r requirements.txt
```

## Prerequisites

Before running the scripts, it's essential to install the necessary Python packages. The project has a `requirements.txt` file listing all the dependencies. You can install these using `pip`. 

Note: The commands below are intended to be run in a Jupyter notebook environment, where the `!` prefix executes shell commands. If you're setting up the project in a different environment, you may omit the `!` and run the commands directly in your terminal.

```bash
!pip3 install --upgrade pip
!pip3 install -r requirements.txt
```
