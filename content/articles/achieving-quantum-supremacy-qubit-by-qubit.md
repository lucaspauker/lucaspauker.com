---
title: "Achieving Quantum Supremacy, Qubit by Qubit"
date: 2021-03-11
draft: false
tags: ["science", "CS"]
---

## Faster than a Supercomputer?
In the 1980s, American physicist Richard Feynman proposed the idea of quantum computers to model complex quantum systems.
In October 2019, around 40 years later, Google AI and NASA scientists unveiled a quantum computer which ran an experiment in a few minutes that would take the fastest supercomputer 10,000 years.
The quantum computer sped up the computation by a factor of 1 billion! This was one of the first major successes in the nascent field of quantum computing.

To understand why quantum computers are faster than non-quantum computers, we have to understand how they work.
The computers that we use every day encode data such as pictures and text with sequences of bits.
Individual bits can be either be off or on, a zero or a one.
The equivalent of a bit in a quantum computer is a qubit (quantum bit) that can be in the state zero, one, or a combination of zeros and ones.
However, when a qubit is measured, it must be either a zero or one.
A qubit is like a coin spinning on a table.
A spinning coin isn’t either heads or tails, instead it’s a combination of both, yet when it stops spinning and lies flat on the table, it must be either heads or tails.
This idea that a qubit or a coin can be in multiple states (i.e., both heads and tails) at the same time is known as superposition.

The property of superposition in qubits allows for fast computations on quantum computers that would take ages for typical computers.
One problem that can be solved quickly using quantum computing is factoring numbers with an algorithm called Shor’s algorithm.
Shor’s algorithm is significantly faster than any known algorithm not on a quantum computer.
Practically, Shor’s algorithm would allow a quantum computer to break encryption schemes that rely on the difficulty of factoring large numbers.
Such a cyber-attack, however, would require factoring numbers that are hundreds of digits long.
To date, Shor’s algorithm has only been used to factor small numbers such as 15 and 21 on quantum computers, so we do not need to worry about quantum computers hacking our bank accounts!

Now that we understand some of the basics of quantum computing, let’s go back to the experiment.
For the experiment, the Google team built a quantum processor called _Sycamore_.
_Sycamore_ is the most advanced quantum computer to date.
It contains 54 qubits, more than other quantum computer, and can therefore run previously undoable experiments.
The scientists ran an experiment showing that _Sycamore_ could do a task significantly faster than a non-quantum computer.
This idea of running an algorithm faster on a quantum computer than a regular computer is known as quantum supremacy.
Quantum supremacy is important since it shows that quantum computers are better than regular computers for certain tasks, essentially proving that they are useful.
Thus, achieving quantum supremacy, as Google claims, is a major advancement in the quantum computing field and paves the way for new experiments and computers.

## The Experiment: Gates and Random Circuits
Building _Sycamore_, a big quantum computer that runs with low error, was possible due to improvements in quantum hardware, specifically quantum gates.
Quantum computers (as well as regular computers) are composed of many gates.
A gate takes an input of qubits (or bits for regular computers) and outputs some qubits (or bits).
A gate is like a factory: it takes in some materials (qubits), transforms and processes them, and outputs different materials (qubits).
Gates allow computers to run complicated algorithms using simple building blocks; gates are the DNA of computers.

Since gates are implemented in hardware, they always have some error.
Building anything perfectly in the real world is impossible! For regular computers, gates are very accurate.
However, for quantum computers, gates tend to be inaccurate since qubits are inherently fragile.
Bits in a regular computer can be stored in a magnetic hard drive, which can last for years without much error.
Qubits, however, are more complicated.
One way to build qubits is by trapping electrons in small chunks of silicon.
Such qubits are sensitive to electromagnetic fields, heat, and other neighboring atoms.
That is a lot of sources of error! These errors can cause decoherence, which means that the information encoded in the qubit is lost.
As time goes on, the qubit is more likely to decohere, so it is difficult to build a computer that can run programs with long execution times.

Errors in qubits causes qubits to change their state, which is bad for reproducible science.
A single bit flip over the course of a trial of an algorithm will lead to an incorrect result.
Before the quantum supremacy experiment was performed, the Google lab worked on building low-error quantum gates.
On average, the error for an individual gate used in the experiment is around half a percent, meaning that for every 200 trials, there is expected to be a single error.
Clearly, there is still more work to be done to reduce the error of quantum gates, but this error was good enough to perform the experiment and achieve accurate results.

Using their accurate gates, the Google experiment was designed to show that quantum computers can run certain computations faster than a classical computer.
From a small set of quantum gates, the scientists created random configurations of around one thousand gates.
This is similar to a monkey writing random code on the quantum computer for each trial.
The scientists then ran the randomly configured quantum computer and recorded the results.
They then simulated the results of the quantum circuit on a (non-quantum) supercomputer, verified them, and compared runtimes between the quantum circuit and the simulated circuit.
It is very difficult for a non-quantum computer to simulate these random quantum circuits since there is not any structure to exploit, so the non-quantum computer ran much slower than the quantum computer.
In fact, as the number of gates grew, the simulation problem became exponentially more time consuming for a non-quantum computer.

## Implications: Why is this Experiment Important?
This experiment was important for the field of quantum computing since it showed that a 54-qubit quantum computer could work with reasonably low error.
More qubits in the computer means that more complex systems can be modeled.
In the case of the Google computer, there are around 10 quadrillion possible configurations of qubits! Also, the fact that such a large computer works shows the robustness of quantum mechanics.
The Google team ran into no unexpected physics that prohibited the project from working.
Therefore, in the future, it should be possible to build even bigger and more complex quantum computers.
The computer used in this experiment can be used in other fields where simulations often help advance the science, such as physics and quantum chemistry.
For example, this quantum computer could be used to model the structure of molecules, which is difficult and computationally expensive on regular computers.
There are also proposed applications of quantum computing in finance, healthcare, and computer science, where processors similar to _Sycamore_ could be used.
For example, quantum computing could help speed up calculations to optimize the selection of a portfolio of financial assets.
Also, the quantum computer developed by Google can be used to create certifiably random numbers.
This contrasts with most random number generators that are pseudorandom, meaning they could be predicted given certain information.
True randomness is important for computer science applications.
For example, in cryptography, randomness is used to generate encryption keys.
The more random the numbers, the more secure the system.

## Controversy: IBM and Quantum Supremacy
Although nobody can deny the difficulty of the Google experiment and the scientific value of the results, there is some controversy about how much of a speedup the quantum computer has over a normal supercomputer.
Google claimed that their experiment would take a supercomputer 10,000 years to run.
However, IBM, another leader in the quantum computing space wrote that they would be able to perform the experiment in two or three days using their Oak Ridge Summit supercomputer.
IBM stated that the experiment did not achieve quantum supremacy because it would still be possible to simulate it on a supercomputer in a few days.
At the end of the day, lots of the controversy around the experiment revolves around how to define “quantum supremacy.” However, as one adds more qubits, the computations would get exponentially more difficult and would largely be impossible to simulate on a non-quantum supercomputer, meaning that even if IBM can simulate the 54-qubit processor, it would be almost impossible for them to simulate a 75-qubit or 100-qubit processor.

## Looking to the Future
In conclusion, we can see that this experiment is a first step in building quantum computers that are better at certain tasks than regular supercomputers.
The field of quantum computing is still young, but this experiment shows promise for the future.
The field is growing at a fast rate and there will be more exciting advancements in the coming years.

## Sources
-	[https://ai.googleblog.com/2019/10/quantum-supremacy-using-programmable.html](https://ai.googleblog.com/2019/10/quantum-supremacy-using-programmable.html)
-	[https://www.nature.com/articles/s41586-019-1666-5](https://www.nature.com/articles/s41586-019-1666-5)

