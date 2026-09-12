# Deep Learning Essentials

Deep learning uses trainable networks to represent complex relationships in data. Learn tensors, automatic differentiation, losses, optimizers, batching, and validation before attempting a large model.

## Choose a framework

| Framework | When to consider it | Important qualification |
|---|---|---|
| PyTorch | Flexible training and the surrounding model ecosystem | Hardware and binary compatibility still matter |
| Keras 3 | A high-level API across TensorFlow, JAX, or PyTorch | Custom backend-specific operations can limit portability |
| TensorFlow | Existing TensorFlow training or deployment systems | Check the supported OS and accelerator installation path |
| JAX | Numerical research using transformations such as gradients and compilation | Its programming model rewards functional, array-oriented code |

Keras is no longer accurately described as TensorFlow-only. Use the migration guide when updating Keras 2 code, especially custom layers and serialization.[^9][^10] TensorFlow's native Windows GPU support ended after 2.10; follow the current supported installation route rather than copying a generic GPU command.[^14] JAX combines array computation with transformations such as `grad`, `jit`, and `vmap`.[^13]

The PyTorch Foundation announced **PyTorch 2.14 on 2 September 2026**. This is a verified release milestone, not a requirement to upgrade every project immediately. Review release notes, framework integrations, and device support before migration.[^11]

## Architecture map

CNNs exploit spatial structure; transformers use attention and are common across language and multimodal tasks. Recurrent networks remain useful in some sequence problems. Autoencoders learn compressed representations. GANs and diffusion-based systems offer different approaches to generation. Architecture choice follows the data, objective, compute budget, and inference constraints.

Training minimizes a loss through gradient-based optimization. Backpropagation computes gradients; the optimizer applies updates. Learning rate, batch size, initialization, regularization, and data quality can all change the result.

## A minimal training step

Requires a compatible PyTorch installation. This uses synthetic data on the CPU and demonstrates mechanics only.

```python
import torch
from torch import nn

torch.manual_seed(42)
inputs = torch.randn(16, 4)
targets = (inputs[:, 0] > 0).long()
network = nn.Sequential(nn.Linear(4, 8), nn.ReLU(), nn.Linear(8, 2))
optimizer = torch.optim.AdamW(network.parameters(), lr=0.01)
loss_fn = nn.CrossEntropyLoss()

network.train()
optimizer.zero_grad(set_to_none=True)
logits = network(inputs)
loss = loss_fn(logits, targets)
loss.backward()
optimizer.step()

network.eval()
with torch.inference_mode():
    predictions = network(inputs).argmax(dim=1)
print("Loss:", loss.item(), "Predictions:", predictions.tolist())
```

Do not apply softmax before `CrossEntropyLoss`; it expects logits. `eval()` changes the behavior of layers such as dropout, while inference mode disables gradient tracking. Training-set predictions here are not a generalization score.

## Adapt before training from scratch

Begin with an appropriate pretrained model and evaluate it. Then compare feature extraction, partial fine-tuning, and full fine-tuning if necessary. LoRA freezes base weights and learns low-rank updates; PEFT provides implementations of parameter-efficient adaptation methods. These can reduce trainable parameters, but the base model still consumes memory.[^53][^21]

Profile before enabling compilation, mixed precision, quantization, or distributed training. Compare both numerical behavior and performance. Save model revision, tokenizer or processor, precision, checkpoint format, and all preprocessing settings.

**Exercise:** Train over several batches with a separate validation set. Plot training and validation loss and explain an overfitting example. Continue to [vision](Computer_Vision.md) or [NLP](NLP.md).

[Back to AI Essentials Hub](README.md)

## Sources

[^9]: Keras team. [Keras 3](https://keras.io/keras_3/). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.

[^10]: Keras team. [Migrating Keras 2 code to multi-backend Keras 3](https://keras.io/guides/migrating_to_keras_3/). Created 2023-10-23; modified 2023-10-30. Reviewed 2026-09-12–2026-09-13.

[^11]: PyTorch Foundation. [PyTorch 2.14 Release Blog](https://pytorch.org/blog/pytorch-2-14-release-blog/). 2026-09-02. Reviewed 2026-09-12–2026-09-13.

[^13]: JAX authors. [Quickstart: How to think in JAX](https://docs.jax.dev/en/latest/notebooks/thinking_in_jax.html). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.

[^14]: TensorFlow team. [Install TensorFlow with pip](https://www.tensorflow.org/install/pip). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.

[^21]: Hugging Face. [PEFT](https://huggingface.co/docs/peft/index). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.

[^53]: Hu et al.. [LoRA: Low-Rank Adaptation of Large Language Models](https://arxiv.org/abs/2106.09685). First submitted 2021-06-17; revised 2021-10-16. Reviewed 2026-09-12–2026-09-13.
