---
title: "Deep Learning Concepts Pt. 1: One Neuron Steepest Descent"
date: 2018-02-21
draft: false
---

## One Neuron Steepest Descent in Python
Code is available [here](https://github.com/aowens-21/basic-learning/blob/master/one-neuron-learning.py).

This is going to be a two part series on some very basic introductory concepts of (deep learning)[https://en.wikipedia.org/wiki/Deep_learning], using Python to make a program
that learns the [boolean functions](https://en.wikipedia.org/wiki/Boolean_algebra#Basic_operations) AND and OR. We will be using a
specific case of the [steepest descent](https://en.wikipedia.org/wiki/Gradient_descent)(also known as gradient descent) to "learn the functions",
using [backpropagation](https://en.wikipedia.org/wiki/Backpropagation) on one layer to compute the weight gradients for our gradient descent.

## What is a Neuron (in the Deep Learning Sense)?
In the sense of our deep learning example, the basic structure of a neuron can be broken down into four parts:

* Inputs
* Weights
* Bias
* Output

In this case, we have two inputs because we're trying to learn a boolean function (which operates on two binary values). Let's call our inputs `x1` and `x2`.
These two inputs are passed into the neuron and multiplied by corresponding weights: `w1` and `w2`. We then add together these two products and add the bias,
which is just another value coming into the neuron. This computation of: `x1*w1 + x2*w2 + bias` is called the neuron's excitation, which we'll call `u`. After
`u` is computed, we pass it into the [sigmoid function](https://en.wikipedia.org/wiki/Sigmoid_function), which gives us the actual output of the neuron. These
computations are done in two functions in my example:

```python
          def compute_output(weight_vec, input_vec, bias_vec):
              return sigmoid(compute_excitation(weight_vec, input_vec, bias_vec))

          def compute_excitation(weight_vec, input_vec, bias_vec):
              return weight_vec@input_vec + bias_vec
```

Just a note, that '@' symbol means do the dot product of the input and weight vectors (because I'm treating all these as vectors). If you're unfamiliar with
the concept of dot products, it basically means multiply the corresponding elements in the two vectors and then sum the products.

We can then compare that output to the output we were expecting, and that's where the learning happens.

## Steepest Descent, Backpropagation, and other confusing words
So what exactly is Steepest Descent? Very simply put, steepest descent is an algorithm used in deep learning to find the correct path to
take to reach a given point. We have some goal we want to reach, so we slowly take steps in the direction that most points towards our goal. There's
a really good [analogy on the Wikipedia page](https://en.wikipedia.org/wiki/Gradient_descent#An_analogy_for_understanding_gradient_descent) that can
explain the algorithm in high-level terms.

The core, foundational operation for steepest descent is something called the gradient step. To understand this, we first have to understand the concept
of a [gradient](https://en.wikipedia.org/wiki/Gradient). That page is pretty confusing, so I think a more understandable way to think about the gradient is
that it's the way the weights and bias are changed, and in turn how that affects the output of the neuron. This is the **most** important thing to understand
about deep learning. We change the weights and bias based on a computed gradient, and that's exactly how learning happens. We calculate the correct gradient to
apply and eventually we'll get the correct weights to multiply our inputs, thus learning how to interpret the given input set. For those of you (like me) who need
to see code to picture this, here's how the gradient step works in my example:

*First the gradient computation*:
```python
          def compute_gradient(output, expected_output, input_vec=1):
              return (output - expected_output) * output * (1 - output) * input_vec
```

Don't get too caught up in the actual formula here. It's some semi-complicated calculus derivations that to give us this formula for the correct gradients.
It takes an optional input vector because I wanted to be able to use it to compute the weight gradients and bias gradient with the same formula. The bias
gradient is calculated if we don't provide an input vector.

*Then later when we apply the gradient step*:
```python
          weight_gradients = compute_gradient(output, expected_output, input_vec)
          bias_gradient = compute_gradient(output, expected_output)

          # Apply our gradients to weights and bias, with learning rate
          weight_vec -= learning_rate * weight_gradients
          bias_vec -= learning_rate * bias_gradient
```

We compute gradients for each part of the neuron and then apply them to our current weights. This code brings up an important concept: the learning rate. As I said
earlier, steepest descent is just taking lots of steps in the direction of our goal value. We use the learning rate, which is some small constant less than 1, to make sure
we don't take too large of a step and end up "less learned" than we had been. It can be somewhat hard to visualize, but if you think back to the mountain analogy, we don't want
to commit too much to a step downhill because we might get off the quickest path down and ending up more lost than we were in the first place. And since we're computer people, we want
to be off that mountain and at our desk as quickly as possible. The learning rate helps us do that.

## Loss
Perhaps another one of the most important parts of learning is the concept of loss. Loss tells us how far off our mark we were based on the expected output. Generally,
we want to keep learning until our loss is smaller than some goal. This is my python function for computing loss:

```python
          def compute_loss(output, expected_output):
              return ((output - expected_output) ** 2) / 2
```

This is another formula you don't really need to get hung up on unless you want to go deeper into deep learning. Just understand that it essentially tells us
how well we did this time.

## Did it Learn?
Let's think back to the original goal of this one neuron example. We want to make the neuron compute AND and OR given two binary inputs. To break this down further,
the program should be able to show the following outputs for the corresponding input pairs.

X1 | X2 | AND | OR
--- | --- | --- | ---
0 | 0 | 0 | 0
0 | 1 | 0 | 1
1 | 0 | 0 | 1
1 | 1 | 1 | 1

In the code, these pairs of inputs and expected outputs are represented like so:

```python
          BOOLEAN_AND = ((np.array([0, 0]), 0), (np.array([0, 1]), 0), (np.array([1, 0]), 0), (np.array([1, 1]), 1))
          BOOLEAN_OR = ((np.array([0, 0]), 0), (np.array([0, 1]), 1), (np.array([1, 0]), 1), (np.array([1, 1]), 1))
```

The `np.array` function just constructs a [numpy array](https://docs.scipy.org/doc/numpy/reference/generated/numpy.array.html) (used for dot product and they're more efficient
than Python lists) to hold the input pair. The second item in the tuple is the expected output.

If we run these two lists through my test function:

```python
          def output_learning_results(func_learned, weight_vec, bias_vec, iterations):
              for func_pair in func_learned:
                  (input_vec, expected_output) = func_pair
                  output = compute_output(weight_vec, input_vec, bias_vec)
                  print("Expected: " + str(expected_output) + " Actual: " + str(output))    
              print("Number of Iterations: " + str(iterations))
```

We get the following output:

```
          // OUTPUT FOR AND
          Expected: 0 Actual: [[  1.41328111e-05]]
          Expected: 0 Actual: [[ 0.02230234]]
          Expected: 0 Actual: [[ 0.02237487]]
          Expected: 1 Actual: [[ 0.97364273]]
          Number of Iterations: 44842

          // OUTPUT FOR OR
          Expected: 0 Actual: [[ 0.03211036]]
          Expected: 1 Actual: [[ 0.97974366]]
          Expected: 1 Actual: [[ 0.97981671]]
          Expected: 1 Actual: [[ 0.99998587]]
          Number of Iterations: 21555
```

Looking at this output we can see that while it's not perfectly precise, it clearly worked. It took quite a few iterations because my loss goal was
very, very small, but we can say that to an extent we found the correct weights and bias for the AND and OR functions. I encourage you to go play around with
some of these values, maybe try a different boolean function or higher loss goal. The code is aggressively commented, so you should be able to figure out what
does what.

## Conclusion
This is the first part in my deep learning series. In the next post we'll look at a very small network that actually uses two layers to learn the boolean XOR
function, which is impossible for one neuron, or even one layer of neurons to learn. Hopefully this wasn't too dense and you learned something about deep learning, and if
you want to learn more about the ideas behind deep learning, I can recommend [this book](http://www.deeplearningbook.org/). That book is pretty large (it's the textbook for
my college course), however, so if you want something lighter I can also recommend [this one](https://www.amazon.com/Fundamentals-Deep-Learning-Next-Generation-Intelligence/dp/1491925612/ref=sr_1_1?ie=UTF8&qid=1519274871&sr=8-1&keywords=fundamentals+of+deep+learning).
