# QCG-Open-Project
Contains the code and readme file for Factorization Using Grover's Algorithm Project

# Background On Grover's Algorithm
Grover's Search Algorithm, developed by Lov Grover, is used for unstructured search. For a given input of size N, it uses a black box function to produce the output with high probability. With a time complexity of O(Sqrt(N)), it provides a quadratic speedup over comparable classical algorithms, which have a time complexity of O(N).

# How Grover's Algorithm is implemented in the code for prime factorization
We make use of the Pennylane SDK to implement our quantum algorithm.
The code asks the user for an input N, the bi-prime to be factorized.
Using the formula n = log2(N), we find the number of qubits to assigned to the z register.
As it turns out, the algorithm is very sensitive to the number of qubits we provide to the x and y registers. There is no general formula for the number of qubits which need to be assigned to the x and y registers, so the code makes use of a switch case to assign the exact right number of qubits required to find the prime factors.
