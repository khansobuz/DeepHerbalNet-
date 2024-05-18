Algorithm Documentation: DeepHerbalNet
Overview
DeepHerbalNet is an advanced neural network architecture based on the MobileNetV2 framework, enhanced with Channel Attention modules for targeted feature recalibration. This model is specifically optimized for the identification of Chinese herbal medicine, leveraging depth-wise separable convolutions to ensure computational efficiency while maintaining high accuracy.
Components
1. Utility Functions
•	_make_divisible(v, divisor, min_value=None):
o	Purpose: Ensures the number of channels is divisible by a specific divisor, aiding in the compatibility of network layers.
o	Parameters:
	v: Desired number of channels.
	divisor: Divisor for alignment.
	min_value: Minimum value to be considered if specified.
o	Returns: Adjusted number compatible with specified divisor.
2. ConvBNReLU (Class)
•	Purpose: Combines a convolutional layer, a batch normalization, and a ReLU activation in a sequential module.
•	Configuration:
o	Convolutional layer with specified number of input planes, output planes, kernel size, stride, and groups.
o	Batch normalization for the output planes.
o	ReLU6 activation for non-linearity without destroying gradients.
3. ChannelAttention (Class)
•	Purpose: Implements a channel-wise attention mechanism to enhance important features and suppress less relevant ones dynamically.
•	Mechanism:
o	Applies global average pooling to squeeze spatial dimensions, resulting in a channel descriptor.
o	Processes the descriptor through two fully connected layers with a reduction ratio, introducing non-linearity between them.
o	Outputs attention scores using sigmoid activation, scaled to affect the original feature maps through multiplication.
4. InvertedResidual (Class)
•	Purpose: Defines the building block of the network utilizing depthwise separable convolutions for efficient feature transformation and expansion.
•	Components:
o	Expands the input channels if the expansion ratio is greater than 1.
o	Applies depthwise convolution for spatial filtering within channels.
o	Uses pointwise convolution to combine features across channels.
o	Optionally integrates Channel Attention to recalibrate features post convolution.
o	Supports residual connections if applicable based on stride and channel dimensions.
5. DeepHerbalNet (Class)
•	Architecture:
o	Initialization:
	Sets up initial and final channels, scaling them as per the width multiplier.
	Constructs the network using a series of Inverted Residual blocks with varying configurations.
o	Features:
	Starts with a ConvBNReLU layer for initial feature extraction.
	Sequentially stacks Inverted Residual blocks, each configured with specific expansion ratios and strides.
	Concludes feature extraction with a ConvBNReLU layer.
o	Classifier:
	Applies dropout for regularization.
	Ends with a fully connected layer mapping the features to class probabilities.
o	Forward Pass:
	Processes input through the feature extractor.
	Pools the final feature maps to a fixed size.
	Classifies the result using the linear layer.
