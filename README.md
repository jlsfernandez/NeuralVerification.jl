# NeuralVerification.jl

This repo contains implementation of various methods to soundly verify deep neural networks.
We verify whether a neural network satisfies certain input-output constraints.
The verification methods are divided into five categories:
* *reachability methods:*
ExactReach, Ai2, MaxSens
* *primal optimization methods:*
NSVerify, MIPVerify, ILP
* *dual optimization methods:*
Duality, ConvDual, Certify
* *search and reachability methods:*
ReluVal, FastLin, FastLip, DLV
* *search and optimization methods:*
Sherlock, BaB, Planet, Reluplex

Please note that the implementations of the algorithms are pedagogical in nature, and so may not perform optimally.
Derivation and discussion of these algorithms is presented in _link to paper_.

## Example Usage
### Choose a solver
```julia
using NeuralVerification

solver = BaB()
```
### Set up the problem
```julia
small_nnet = read_nnet("examples/networks/small_nnet.txt")
inputSet  = Hyperrectangle(low = [-1.0], high = [1.0])
outputSet = Hyperrectangle(low = [-1.0], high = [70.0])
problem = Problem(small_nnet, inputSet, outputSet)
```
### Solve!
```julia
result = solve(solver, problem) # returns CounterExampleResult(:UNSAT, [1.0])

result.status # returns :UNSAT
```

A result status of `:UNSAT` means that the input-output relationship is "unsatisfied", i.e. that the property being tested for in the network does not hold.
A result status of `:SAT` means that the specified input-output relationship is "satisfied" (note the completeness/soundness properties of the chosen algorithm in interpretting `:SAT` and `:UNSAT`).
A status of `:Undetermined` is also possible.
For a full list of `Solvers` and their properties, requirements, and `Result` types, please refer to the documentation.