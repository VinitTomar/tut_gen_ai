## What I did:

- Completed PyTorch learn basics [tutorial](https://docs.pytorch.org/tutorials/beginner/basics/intro.html)

## What clicked:

- To create a Neural network model extend it by `nn.Module`.
- Add layers for which we need parameters to be remembered inside the constructor.
- Define a `forward` method that takes the input and passes it through the layers (defined in the constructor) in order, applying activation functions between them, and returns the model's output. Parameterized transformations go through the layer objects stored in the constructor and stateless operations (activations like `F.relu`, reshaping like `torch.flatten`) can be called functionally.
- We select a `loss function` based on the distribution of our data or the type of neural network layers in out model.
- We initialize an `optimizer` that will update the model parameters based on the `gradient` value of a parameter and a `learning rate`.
- A simple training loop
  - Predict output of a model by passing it the input.
  - Compute the loss i.e. error in our prediction.
  - Compute the gradients of our model parameters by calling `loss.backward()`. (PyTorch _accumulates_ gradients, so they add up across `backward()` calls.)
  - Update parameter values by calling `optimizer.step()`.
  - Reset all gradients to zero by calling `optimizer.zero_grad()`. The critical rule is to zero the gradients before the next `backward()`, otherwise they pile up. The common convention is to call this at the _start_ of each iteration.
- `Tensors` in PyTorch are similar to NumPy's array, except they can run on GPUs. We can initialize a tensor from a NumPy's array also.
