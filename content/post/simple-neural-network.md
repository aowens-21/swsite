---
title: "Deep Learning Concepts Pt. 2: A Basic Neural Network"
date: 2018-03-03
draft: false
---

## Network Steepest Descent in Python ##
Code is available [here](https://github.com/aowens-21/basic-learning/blob/master/simple-network-learning.py).

Welcome to part two of my series on Deep Learning concepts, if you haven't read through [part one](http://www.aowens.me/post/one-neuron-steepest-descent/) then I'd suggest doing
that. This will be a much shorter post that builds on all of the ideas I covered there.

## What makes a network? ##
In the last post we talked about how artificial neurons are represented. While a neuron is just a single learning entity, a network
is a collection of layers of neurons. In my example we have the first layer, which we can call the input layer. It consists of two neurons, which
both get the same input pair. Then we have the second layer, called the output layer, which takes the previous layer's output and learns based
on those. This second layer is the final layer in this particular example, but neural networks, specifically deep neural networks, usually consist
of many layers with a lot of neurons in each layer.

## Learning the exclusive-or function ##
I ended part one of the series by saying it's impossible for one neuron to learn exclusive-or, lets try to understand why that is. A simple neuron works
on the idea of [linear separation](https://en.wikipedia.org/wiki/Linear_separability#Linear_separability_of_Boolean_functions_in_n_variables). One way to understand it is
by looking at a graph for the boolean OR function:

<p align="center">
  <img src="/img/or_graph.png">
</p>

Linear separation is essentially being able to draw one straight line to separate these two classifications. By classifications I mean when we want 1 and when we
want 0. In this picture, we could easily draw a line to separate the 0 at the origin from the 1s. However, this becomes impossible if we look at the graph for
XOR:

<p align="center">
  <img src="/img/xor_graph.png">
</p>

We can say that this graph is not linearly separable, because no matter how we draw the straight line, we can never have all the 1s on one side of it and all the
0s on the other.

Using more than one layer, we can essentially push the input from the first layer to be the input for the next layer, which allows us to do more specific classification.
If you want to think about it visually, one layer can separate the 0 at the origin, and the other layer can separate the 0 at point (1, 1). This gives us the
region we were hoping for. If you want to learn more about how this works, I recommend [this post](http://home.agh.edu.pl/~vlsi/AI/xor_t/en/main.htm).

## Computing gradients for both layers ##
While we're still using backpropagation and gradient descent, the process of getting our gradients has changed a bit:

```python
                l1_w_grads = compute_l1_weight_gradients(output, expected, inputs, l1_outputs, layer2_weights)
                l1_b_grads = compute_l1_bias_gradients(output, expected, l1_outputs, layer2_weights)

                l2_w_grads = compute_l2_weight_gradients(output, expected, l1_outputs)
                l2_b_grad = compute_l2_bias_gradient(output, expected)
```

The important thing to note here is that we have to compute layer two's gradients based on the output of layer one. The second layer is dependent on the first,
if we were just computing using the same formula as the single neuron, we'd still have the problem of linear separation.

## Conclusion ##
There are a lot of details here about neural networks that I didn't cover. Specifically there is a lot of calculus that goes into
computing gradients for all the layers of your neural network. If you want to know more, I'd suggest reading about [backpropagation](https://en.wikipedia.org/wiki/Backpropagation). It can be hard to grasp but it's actually a really amazing and clean algorithm.

I hope this series taught you something about deep learning, and I hope that maybe you'll go try out some machine learning/deep learning on your own. You
definitely don't have to do all of this low level setup on your own, and there are amazing libraries out there like [scikit-learn](http://scikit-learn.org/stable/)
and [Tensorflow](https://www.tensorflow.org/). I'm going to be diving into Tensorflow in the coming weeks, so maybe I'll make a post about using it.

Thanks a lot for reading and I'll see you next time!
