# QCG-Open-Project
Contains the code and readme file for Factorization Using Grover's Algorithm Project

# Background On Grover's Algorithm
Grover's Search Algorithm, developed by Lov Grover, is used for unstructured search. For a given input of size N, it uses a black box function to produce the output with high probability. With a time complexity of O(Sqrt(N)), it provides a quadratic speedup over comparable classical algorithms, which have a time complexity of O(N).

# How Grover's Algorithm is implemented in the code for prime factorization
We make use of the Pennylane SDK to implement our quantum algorithm.

The code asks the user for an input N, the bi-prime to be factorized.
Using the formula n = log2(N), we find the number of qubits to assigned to the z register.
As it turns out, the algorithm is very sensitive to the number of qubits we provide to the x and y registers. While there is no general formula for the number of qubits to be provided to the x and y registers, for large bi-primes, providing the x and y registers with n-2 and n-1 qubits proves to be sufficient.
For a small bi-prime such as 6, we need at least 2 qubits for each of it's factors.

The code required for "classical" multiplication was taken from https://pennylane.ai/qml/demos/tutorial_qft_arithmetics/, one of the resources provided by OCG. It uses OFT to bring the qubits out of the computational basis (the 0 and 1 basis), into the fourier basis (+ and - basis). It then carries out multiplication and returns to the computational basis, where the results can be iterpreted.

Now, we define the iterations. The value of pi/4 * sqrt(N), can be seen as a result of the geometric interpretation of the algorithm, with each iteration "pushing" our output state to the required state by arcsin(1/sqrt(N)). Best Explanation: https://www.youtube.com/watch?v=c30KrWjHaw4.
The user is also given the choice to set the number of iterations manually. While not recommended, this can reduce the computational load.

Then we have the factorization circuit:
We first convert the all-zeros state the x and y registers were intialized with into a superpostion of states with all possible combinations of 0's and 1's using the given number of qubits. All superposition states have equal amplitude of 1/sqrt(2^n).
Oracle step 1:
The multiplication function takes all possible "classical" products of the states in the x and y registers and stores them into the z register.
Oracle step 2:
The FlipSign function checks which state in the z register matches the input N amd flips the amplitude of that state.
Oracle step 3:
The adjoint operator applied to the multiplication function uncomputes the multiplication and the negative amplitudes are also assigned to the "factor" states in the x and y registers.
Diffusion Operator:
The Diffusion operator is simply referred to as the Grover Operator in pennylane. It "reflects" the states about the mean amplitude. The mean amplitude is brought closer to zero due to negative amplitude of the "factor" states. On reflection, the amplitude of the factor states becomes much greater than the other states. Best Explanation: https://youtu.be/YEI5vYdcoQ4?si=cchwPdaLwXMX0Wmk
The Oracle and the Diffusion Operator are iterated over.
The function finally returns the probability array.

The user is given the option to draw the circuit of the factorization funtion. The option exists primarily to save time if the user is not interested in the circuit. Also, if the cicuit is too large, Jupyter may simply refuse to draw it.
The number of iterations used for drawing the circuit is limited to 1 for the same reason as given above.
For N = 35, 115 and 893, the circuit image is very large, but it can be zoomed into if one wants to verify.

p and q are the factors and are initialized to the value returned by np.argmax(factorization), which returns the postion of the states with maximum probabilty, which are precisely our factors.

Again, this algorithm is very sensitive to the number of qubits assigned to the x and y registers. To generalize the code to most N, the number of qubits assigned to the x and y registers are just enough to make sure correct flipping and amplification takes place in at least ONE of the registers. Thus, as a compromise the if-else statement is included.
The if-else statement identifies the smaller factor and divides N by it to find the other factor.

The factors are then printed.

# Files Uploaded
1. Factorization_code.ipynb
2. Demonstration_on_35.ipynb
3. Demonstration_on_115.ipynb
4. Demonstration_on_893.ipynb (This takes the longest time to process, so to save time, I have chosen to give the x and y registers exactly the right number of qubits)

# Resources 
1. https://youtu.be/tsbCSkvHhMo?si=SVhArKgAEzzLHSV2
2. https://pennylane.ai/qml/demos/tutorial_qft_arithmetics/
3. https://www.youtube.com/watch?v=c30KrWjHaw4
4. https://youtu.be/EoH3JeqA55A?si=J6V2-NduY6iIRocZ
5. https://youtu.be/YEI5vYdcoQ4?si=9yko1OS0-NEGV1_K
6. https://youtu.be/PJorOzsQ7FU?si=57ZHaApP5JYdTsYV
