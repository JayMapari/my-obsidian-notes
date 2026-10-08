# How the first neural networks were trained

> **Short answer:** early neural networks learned by adjusting connection strengths (weights) so their outputs more closely matched examples. The procedure depended on the kind of network: early perceptrons used a simple error-correction rule, while multilayer networks later became practical to train with backpropagation and gradient descent.

## A quick historical distinction

"The first neural networks" does not refer to one single training algorithm. Early artificial-neuron models appeared in the 1940s. In the late 1950s, Frank Rosenblatt introduced the perceptron, a trainable classifier. Its learning rule could train a single-layer classifier, but not every possible pattern: for example, a single-layer perceptron cannot represent XOR. Methods for training multilayer networks by computing gradients with backpropagation and updating weights were developed over time and popularized in the 1980s. So it is misleading to say that all early networks used today's deep-learning recipe.

## The basic idea: learn from examples

Imagine a tiny network that must classify points as belonging to one of two categories. It receives input numbers, combines them using weights, applies an activation/threshold, and produces a prediction.

1. **Initialize weights.** Start with small values (or, in some early demonstrations, chosen values).
2. **Show an example.** Supply its input and known target label.
3. **Make a prediction.** The weighted inputs produce an output.
4. **Measure the error.** Compare output with the target.
5. **Adjust weights.** Change connections to make the correct output more likely next time.
6. **Repeat.** Cycle through examples, often for many passes, and test on examples not used for updating.

The essential learning signal is the difference between what the network produced and what it should have produced.

## The perceptron correction rule

For a basic binary perceptron, let the input be `x`, weights be `w`, bias be `b`, target be `y`, and prediction be `y_hat`. A simple update is:

```text
w     = w + learning_rate * (y - y_hat) * x
bias  = bias + learning_rate * (y - y_hat)
```

Here `learning_rate` sets the update size. If the prediction is correct, the correction is zero; if it is wrong, the weights move in a direction that helps classify that example correctly. This rule works for linearly separable training data, not arbitrary patterns.

## How multilayer networks learn

A network with hidden layers can represent richer patterns, but an output error alone does not immediately say how each hidden-layer weight contributed. Backpropagation addresses this by applying the chain rule to calculate how changing each weight would change the loss. Gradient descent then nudges each weight in the direction that reduces that loss:

```text
weight = weight - learning_rate * gradient_of_loss
```

In practice: forward pass -> loss -> backward pass (gradients) -> optimizer update. Repeat over batches of examples. This is the same broad training loop used in modern neural networks, though today's models, hardware, datasets, and optimizers are vastly more sophisticated.
## Why training was difficult

Early computers had limited memory and processing power, and training multi-layer networks was mathematically and computationally challenging. The perceptron also had clear representational limits. Progress depended on better algorithms, faster hardware, more data, and improved ways to initialize and optimize networks. Training examples also had to be encoded numerically in forms the network could accept.

---
## Connection to language models

A language model uses the same general idea of comparing predictions with targets and updating weights, but the task is usually next-token prediction over sequences. See [[Pretraining]] for the broader data-and-training pipeline, [[Tokenization]] for turning text into token IDs, and [[Neural Network]] for the model components and training loop.

![[initial-neural-network-training.excalidraw]]

## Takeaway

Early networks did not "understand" examples or receive hand-written rules for every answer. They adjusted numeric weights from examples. The perceptron used a direct error-correction update; multilayer networks use gradients propagated backward through the network to improve a loss function.

---
## Step-by-step: training a neural network

A supervised neural network learns by making predictions on training examples, measuring its error, and adjusting its parameters. The cycle below repeats for each mini-batch and across multiple epochs.

1. **Prepare and split the data.** Convert each example into numeric inputs and pair it with its target (the expected answer). Separate examples into training, validation, and test sets. The model learns from the training set; validation helps monitor progress; the test set is held back for a final check.
2. **Choose the training setup.** Select the network structure, a loss function suited to the task, an optimizer, and a learning rate. These specify what the model predicts, how error is measured, and how parameters will be changed.
3. **Initialize weights and biases.** Start the network parameters at suitable small values, typically using a random initialization method. They are the numbers the training process will learn.
4. **Choose a mini-batch.** Take a small group of input-target examples from the training set. Training in mini-batches makes updates computationally practical.
5. **Run a forward pass.** Feed the batch inputs through the network. Each layer combines values using weights and biases, applies its activation function, and passes the result forward. The final layer produces a prediction for each example.
6. **Calculate the loss.** Compare the predictions with the corresponding targets using the chosen loss function. This produces a value (or batch aggregate) that measures prediction error.
7. **Compute gradients with backpropagation.** Propagate the loss information backward through the layers using the chain rule. The resulting gradients indicate how each weight and bias affects the loss. A simple single-layer perceptron instead can use its direct error-correction rule.
8. **Update the parameters.** The optimizer uses the gradients to adjust weights and biases, generally in a direction that reduces loss. A basic gradient-descent update is `parameter = parameter - learning_rate * gradient`.
9. **Repeat over the training set.** Select the next mini-batch and repeat steps 4 to 8. Once every training example has been processed, one **epoch** is complete. Training commonly runs for multiple epochs.
10. **Validate during training.** At intervals, measure performance on the validation set without using those examples to update the weights. Use validation results to spot poor generalization and help decide whether to continue or stop training.
11. **Evaluate and save.** When training is finished, evaluate once on the held-out test set, if available, for an estimate of performance on unseen data. Save the resulting model parameters for use later.

**The core mini-batch cycle:**

```text
input and target -> forward pass -> prediction -> loss
loss -> backpropagation -> gradients -> parameter update
parameter update -> next mini-batch -> repeat
```

The arrows show the learning loop: predictions are compared with targets to calculate loss; gradients from that loss guide parameter updates. Then the next mini-batch is processed. Validation and final testing assess performance but do not themselves update the weights.

The perceptron rule and backpropagation are related by the goal of learning from prediction error, but they are different methods: a perceptron uses a direct correction for a single-layer classifier, while backpropagation computes gradients through multiple layers.

![[Neural Network.excalidraw.md]]
## Summary

Early neural networks learned by adjusting their weights from labeled examples. A perceptron compared its prediction with the correct answer and applied a direct correction; this worked only for linearly separable patterns. For networks with hidden layers, backpropagation calculates how each weight contributes to the error, and gradient descent updates the weights to reduce it. The general loop is: **input -> prediction -> error/loss -> weight update -> repeat**. Modern language models use the same broad learning principle, typically predicting the next token across large datasets.





