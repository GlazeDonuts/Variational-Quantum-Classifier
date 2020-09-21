# Variational Quantum Classifier

Parameterized quantum circuits play an essential role in the performance of many variational hybrid quantum-classical (HQC) algorithms. One challenge in implementing such algorithms is to choose an effective circuit that well represents the solution space while maintaining a low circuit depth and number of parameters[1]. This repository contains a set of experiments and comparisons between different feature maps used in an elementary implementation of a Quantum Variational Classifier circuit, developed from scratch, using Qiskit.

## Experiments

The circuits were trained using two repetitions of a feature map, followed by a standard (fixed) variational circuit, in order to set a standard to compare and validate performance across different choices of feature maps. All the circuits were trained to classify the digits `4` and `9` from a subset of the [MNIST](http://yann.lecun.com/exdb/mnist/) Dataset. The data used for training the models in this repository is available [here](https://gitlab.com/GlazeDonuts/vqc-datasets/-/blob/master/datasets/dataset_4_9.csv). Further, 5% of data was initially segregated to use as a validation/testing dataset during the process of training.

More than 20 feature maps were tested by coupling them with an `EfficientSU2`, linearly entangled, variational circuit, repeated twice. All the circuits were trained using 4% of the training data using the `COBYLA` optimizer for 100 iterations. Below is table summarising the scores.


|   Experiment No.    | Score (%)|
|:-------------------:|:--------:|
|          1          |    64    |
|          2          |   63.5   |
|          3          |   72.9   |
|          4          |   71.2   |
|          5          |   75.6   |
|          6          |   75.7   |
|          7          |   76.8   |
|          8          |   75.2   |
|          9          |   77.6   |
|         10          |   77.8   |
|         11          |   65.4   |
|         12          |   76.4   |
|         13          |   70.1   |
|         14          |   75.2   |
|         15          |   77.3   |
|         16          |   73.1   |
|         17          |    77    |
|         18          |   73.8   |
|         19          |   78.7   |
|         20          |   77.5   |
|         21          |   78.5   |
|  Sandwich (9-10-9)  |   77.8   |
| Sandwich (10-9-10)  |   73.3   |
| Sandwich (10-20-10) |   76.2   |

**<u>Note:</u>** All the scores are classification accuracies that were obtained on a *hidden* datset used by Qiskit for evaluation in their recent [QML Challenge](https://medium.com/qiskit/introducing-the-qiskit-india-challenge-a-taste-of-quantum-machine-learning-for-qiskitters-in-india-4780ddbb03ab).

As it is evident from the above table, circuit in experiment 19 has a better score than that in experiment 10. However, it was observed that the score of the former dropped drastically with increase in number of repetitions of the variational circuit, whereas the latter consistently improved in performance when the number of repetitions were increased. Hence, the circuit from experiment 10 was chosen as the final feature map.

## Best Circuit
The hyperparameters and other specifications of the best circuit are tabulated below.

| Parameter        | Value               |
| ---------------- | :-----------------: |
| Score            | 81.1%               |
| Training Samples | (10%) 270 per class |
| Optimizer        | COBYLA              |
| Number of epochs | 250                 |
| Tolerance        | $10^{-6}$           |

The feature map from experiment 10 was deployed with three repetitions as shown below.
<p align="center">
<img src="https://github.com/GlazeDonuts/Variational-Quantum-Classifier/blob/master/resources/final_fmap.jpg?raw=True"/>
</p>

The above feature map was coupled with a varitaional circuit implemented using the `TwoLocal` variational quantum circuit module from Qiskit. The parameters and their values are listed below:
* `num_qubits = 3`
* `rotation_blocks = ['ry', 'rz']`
* `entanglement_blocks = ['cx']`
* `entanglement = 'linear'`
* `reps = 4`
* `insert_barriers = True`
<p align="center">
<img src="https://github.com/GlazeDonuts/Variational-Quantum-Classifier/blob/master/resources/final_var_circ.jpg?raw=True"/>
</p>

### References:
[1] [Expressibility and entangling capability of parameterized quantum circuits for hybrid quantum-classical algorithms](https://arxiv.org/abs/1905.10876)
[2] [Qiskit Documentation](https://qiskit.org/documentation/index.html)
