# Description
Provides simple and lightweight implementations of model validation and hyperparameter optimization for Julia. 

# Installation
Run the following code to install the package:
```julia
] add https://github.com/triepels/CrossValidation.jl
```

# Get Started
You have to define functions `fit!` and `loss` for your own model type. Function `fit!` takes a model and fits it on some data based on some optional fitting arguments:

```julia
julia> function fit!(model::MyModel, data; args)
           // Code to fit model...
       end
```

Function `loss` estimates how well the model performs on (out-of-sample) data:

```julia
julia> function loss(model::MyModel, data)
           // Code to evalute loss of the model...
       end
```

# Features
Model validation based on various resample methods:
```julia
julia> validate(MyModel(2.0, 2.0), KFold(data), epochs = 100)
```

Hyperparameter optimization using various optimizers.
```julia
julia> space = Space(a = -6.0:0.5:6.0, b = -6.0:0.5:6.0)
julia> sha(MyModel, space, FixedSplit(data), GeometricBudget(epochs = 100), 0.5, false)
```

Model validation with hyperparameter optimization:
```julia
julia> validate(KFold(data)) do train
           parms = brute(MyModel, space, FixedSplit(train), false, epochs = 100)
           return fit!(MyModel(parms...), train)
       end
```

# Resample methods
The following resample methods are available:
* Fixed split
* Random split (holdout)
* K-fold
* Forward chaining
* Sliding window

# Optimizers
The following optimizers are available:
* Grid search
* Random search
* Hill-Climbing (HC)
* Successive Halving (SHA)
* Simulated Annealing and Successive Halving (SASHA)

# Note
This package is not yet stable. Future releases might be subject to breaking changes.
