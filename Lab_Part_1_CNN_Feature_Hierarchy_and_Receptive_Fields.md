# Lab Part 1: CNN Feature Hierarchy and Receptive Fields

**Prompt:** Explain how CNNs build a hierarchy of features through their layer structure.

**Answer:** Convolutional networks build a feature hierarchy by stacking layers so each layer detects patterns in the activations of the layer below it. For AutoParts inspection, that lets one model catch both minor surface flaws and larger structural defects without separate hand-coded detectors.

**Prompt:** Describe what types of features are typically detected at early, middle, and deep layers of a CNN.

**Answer:** Early convolutional layers sit on the pixels. Small filters, often 3×3, respond to oriented edges, sudden intensity changes, fine texture, and local color or finish differences. A scratch or hairline crack first appears here as a thin, high-contrast discontinuity.

Middle layers combine those detections. Aligned edges become contours; repeated local responses become textures such as machining marks, pitting, or coating variation; nearby edge fragments become part-like shapes such as a bracket hole or a flange edge. At this stage the network sees components of a defect, not the whole part.

Deep convolutional layers assemble those mid-level parts into high-level structures: a curved discontinuity on metal texture classified as a crack on a bracket, or a warped contour relative to the expected outline classified as a structural deformation.

**Prompt:** Explain the concept of a receptive field and how it changes as we move deeper into the network.

**Answer:** The receptive field is the region of the input image that can influence a unit. With 3×3 kernels and stride 1, a unit in layer 1 sees 3×3 pixels. Each additional stride-1 3×3 convolution expands that field by 2 pixels on each side, so layer 2 sees 5×5 and layer 3 sees 7×7. Stride-2 convolution or 2×2 pooling roughly doubles the field. After several conv–pool stages, a deep unit may cover most of a part. That growth is why depth matters: a dark line is a crack only if it lies on the component rather than the background, and a bend is a deformation only if it violates the part’s geometry. Wider context is what separates a true defect from glare, a shadow, or a normal edge.

**Prompt:** Illustrate with a specific example of how this hierarchical feature extraction might work in developing a Convolutional Neural Network (CNN) approach, as CNNs excel at extracting meaningful visual features from images.

**Answer:** On the 50,000-image set of defective and non-defective parts, a compact stack such as conv 3×3 (32) → ReLU → pool, conv 3×3 (64) → ReLU → pool, then deeper 3×3 blocks (128) learns this progression across brackets and engine parts instead of relying on fixed rules. A hairline crack starts as a low-level edge in the first 3×3 block. Middle 64-filter layers group those edges into a candidate crack rather than machining noise. Deep 128-filter layers, with a receptive field large enough to see the surrounding metal and the part outline, classify that discontinuity as a crack on a bracket, or a warped contour as a structural deformation, only when it sits on the component and violates expected geometry.
