# Models

## Task-specific models

### ResNet

Original paper: [**Deep Residual Learning for Image Recognition**](https://openaccess.thecvf.com/content_cvpr_2016/html/He_Deep_Residual_Learning_CVPR_2016_paper.html), Kaiming He, Xiangyu Zhang, Shaoqing Ren, Jian Sun (2016)

#### Performance

ResNet won the **ILSVRC 2015** competition (ImageNet dataset), taking 1st place in image classification, detection, and localization, as well as 1st place at **COCO 2015** in detection and segmentation. The 152-layer model achieved **a top-5 error** (the correct class appears in the model's 5 best guesses) **of 3.57% on ImageNet** - surpassing human-level performance on this benchmark. Its greatest achievement was proving that networks of extreme depth (152 layers, 8 times deeper than VGG, yet with lower computational complexity) can be trained successfully without the degradation problem.

#### Novelty

Before ResNet, a common assumption was that more layers should provide better representations. In practice, there were two problems. **Vanishing/exploding gradients** - they were partially solved by careful initialization and batch normalization. Still, the **degradation problem** remained: beyond a certain depth, training accuracy dropped sharply - not due to overfitting, but because the error increased on the *training set*. Current optimizers simply failed to find good parameters for very deep networks in reasonable time.

ResNet's solution is the **residual (skip) connection**. Instead of asking a block of layers to learn a full mapping `x -> H(x)` from scratch, the block learns only the residual `x -> F(x) = H(x) − x`, and the original input `x` is added back at the end:

```
output = F(x) + x
```

The intuition (supported by examples from other fields, such as solving Partial Differential Equations) is that optimizing a residual is easier than learning a full transformation. In the extreme case where the identity transformation is the optimal option, the block just needs to drive its weights to zero - the shortcut moves the signal unchanged. Without this trick, nonlinear layers would need to approximate the linear function `f(x) = x` through activations, which is much harder to train.

Key implementation details:

- The shortcut adds the block's input directly to its output - no extra parameters (previous ideas included parameters, which could close the shortcut and block gradient flow in backpropagation), no added computational cost, fully compatible with backpropagation;
- When input and output dimensions differ, a 1×1 convolution can project the input to the required dimensions;
- Each residual block contains at least two layers for the effect to be meaningful (for one layer it would just result in a regular layer behavior);
- Works for both fully connected and convolutional layers;
- Skip connections are placed every few layers throughout the network;
- Deeper ResNets (50+ layers) use a bottleneck block: three consecutive convolutions (1×1, 3×3, 1×1), where the first reduces the channel count, the 3×3 processes the compressed representation, and the last restores it - keeping computational cost low at greater depths.

Experiments on both ImageNet and CIFAR-10 confirm that deep residual networks achieve lower training error than plain networks (without residual connections) of the same depth. At publication, ResNet-152 was the deepest successfully trained network ever.

#### Related Literature

- **He et al. (2016)** - [*Identity Mappings in Deep Residual Networks*](https://link.springer.com/chapter/10.1007/978-3-319-46493-0_38) - continuation of original paper, analysis of signal propagation in residual blocks. Authors propose a redesigned block layout (pre-activation ResNet v2) that simplifies optimization and improves generalization. Experiments include a 1001-layer network on CIFAR-10;
- **Geirhos et al. (2019)** - [*ImageNet-trained CNNs are biased towards texture; increasing shape bias improves accuracy and robustness*](https://openreview.net/forum?id=Bygh9j09KX&trk=public_post_comment-text) - demonstrates that ResNet classifies objects based on local texture rather than shape, e.g. a cat silhouette covered in elephant skin texture is confidently classified as an elephant;
- **Bello et al. (2021)** - [*Revisiting ResNets: Improved Training and Scaling Strategies*](https://proceedings.neurips.cc/paper_files/paper/2021/hash/bef4d169d8bddd17d68303877a3ea945-Abstract.html) - a systematic analysis of how training and scaling strategies (depth, width, image resolution, regularization) affect ResNet's accuracy on ImageNet. Authors demonstrate that a properly optimized ResNet (ResNet-RS) achieves accuracy comparable to EfficientNet while using less memory and training faster.

#### **Adoption of residual connections in other architectures:**

Residual connections from ResNet were adopted by many subsequent architectures - most notably the **Transformer** ([*Attention Is All You Need*](https://proceedings.neurips.cc/paper/2017/hash/3f5ee243547dee91fbd053c1c4a845aa-Abstract.html), Vaswani et al., 2017), where skip connections wrap every attention block and feed-forward layer, enabling effective stacking of dozens of layers. Today they are present in virtually every deep learning architecture.

#### Trained Instances

- [**microsoft/resnet-50**](https://huggingface.co/microsoft/resnet-50) - 50-layer ResNet v1.5 architecture trained with the original training recipe (short training, basic data augmentation, SGD optimizer). Default configuration in HuggingFace Transformers library;
- [**microsoft/resnet-152**](https://huggingface.co/microsoft/resnet-152) - 152-layer ResNet v1.5 architecture, trained just like the resnet-50 (above);
- [**timm/resnet50.a1_in1k**](https://huggingface.co/timm/resnet50.a1_in1k) - the same 50-layer ResNet v1.5 architecture, but with different training based on [*ResNet Strikes Back*](https://openreview.net/forum?id=NG6MJnVl6M5), Wightman et al., 2021 (more epochs, more advanced data augmentation and other improvements). Has higher accuracy on ImageNet than basic version.

### ConvNext

### EfficientNet

## Self-supervised foundational models

### CLIP

### DINO family

### SigLIP

### BLIP

## Fine-tuning techniques

### Freezing

### LoRA

### Linear probing
