# Lab Part 3: Comparing CNNs to Traditional Neural Networks

**Prompt:** Compare parameter efficiency between CNNs and fully-connected networks using a specific image size example.

**Answer:** Take one inspection frame at 224×224 with 3 color channels: 224 × 224 × 3 = 150,528 inputs. A fully connected first layer with 512 units needs a weight from every input to every unit, so 150,528 × 512 ≈ 77 million weights, before biases or later layers. A 3×3 convolution with 32 filters on those 3 channels needs 3 × 3 × 3 × 32 = 864 weights, because the same filter is reused at every position. That gap is memory, training time, and generalization. Seventy-seven million weights will not fit reliably on 50,000 part images, and they will not run at line speed on an edge device.

**Prompt:** Explain how parameter sharing and local connectivity in CNNs address the limitations of fully-connected networks.

**Answer:** Local connectivity means a unit sees only a small neighborhood. A crack is a local pattern, so a pixel on the far side of the bracket should not have its own weight into that detector. Parameter sharing means that same 3×3 filter is applied everywhere: one scratch detector is learned and slid across the frame, instead of a separate detector for every location. Those two choices cut the parameter count and match vision, where the same edge matters in the corner and in the center. They also ease the curse of dimensionality. A flattened image is a point in a 150,000-dimensional space. A shared local filter estimates far fewer numbers from the same 50,000 labels.

**Prompt:** Describe how CNNs maintain spatial information that would be lost in traditional neural networks.

**Answer:** A fully connected network flattens the image before the first multiply, so neighboring pixels are no longer neighbors. A ten-pixel shift of the part becomes a different input vector. A convolutional layer keeps a feature map: each activation still has a position, so later layers can tell that two edge responses are adjacent, aligned, or on opposite sides of a hole. Pooling shrinks that map but does not discard the layout. That layout is how the model separates a crack on the bracket from a dark line in the background, and it is what would be required to mark where the defect is, not only that the part fails.

**Prompt:** Illustrate a specific problem where a fully-connected network would struggle but a CNN would excel, with reasoning.

**Answer:** A hairline crack can appear anywhere on a bracket, at a slight angle, and the bracket is not always centered. A fully connected net treats each position as a new pattern, so 50,000 images are not enough and a small shift at inference can flip the call. A CNN reuses the early edge filter, groups aligned edges in the middle layers, and uses the wider receptive field deeper in the network to check that the discontinuity sits on metal. The same weights cover every location, which is why the convolutional model is the right choice for real-time inspection.
