# NOTE

**`Torch`** has undergone some changes in it's backend since I last updated this repository. Hence, the installation now need to be performed inside `nnx`, instead of `nn` directly.
I have updated the installation instructions accordingly.

# Torch-Vol

In this repository I will be maintaining a list of modules that I had to come up with to ease my work. While starting out to work with ***Volumetric*** (**4D** or *batchmode* **5D**) data I found a lot of implementations missing, even though their ***Spatial*** versions existed. This repository is the collection of the codes that I wrote then. 

*Hopefully it will be of help to someone, someday, somewhere*.

## Note
***
Most of these are simple extensions of their ***Spatial*** counter-parts. So there's a high chance that even though it wasn't there when I wrote it, it may have been incorporated in the official library now. So it is highly recommended that you cross-check with [Torch's main repository](https://github.com/torch/nn) to see if it already exists. I will notify the same, below each module, if I happen to come across one.

## Installation Instruction
***
To use this modules along with the exisiting modules of the `nn` and `cunn` package requires installing them in a bit *hackish* (read *dirty*) way. 

It is explained over [here](INSTALL.md) in detail.

**It is important to note that there should not be any name conflicts with any of the existing modules**. For example, suppose you want to use a module `abc.lua`. Then it is important to make sure that there is no other module called `abc.lua` present in the `nn` (or `cunn` package).

## List of Modules
***

### VolumetricConvolution
___
Performs `Volumetric Convolution` that supports ***padding***

```lua
nn.VolumetricConvolution(nInputPlane, nOutputPlane, kT, kW, kH, [dT], [dW], [dH], [padT], [padW], [padH])
```

This module is already *[merged](https://github.com/torch/nn/pull/481)* into the main **Torch** repository.

*Associated files*

1. [VolumetricConvolution.lua](https://github.com/kmul00/torch-vol/blob/master/VolumetricConvolution.lua)
2. [VolumetricConvolutionMM.c](https://github.com/kmul00/torch-vol/blob/master/generic/VolumetricConvolutionMM.c)
3. [VolumetricConvolution.cu](https://github.com/kmul00/torch-vol/blob/master/cuda/VolumetricConvolution.cu)

### VolumetricBatchNormalization
___
Performs `Batch Normalization` over ***5D*** (batch mode) data

```lua
nn.VolumetricBatchNormalization(N [,eps] [, momentum] [,affine])
```

where `N = Number of input features`. Details regarding the other parameters could be found over 
[here](https://github.com/torch/nn/blob/master/doc/convolution.md#nn.SpatialBatchNormalization)

This module is already exists in the main **Torch** repository.

*Associated files*

1. [VolumetricBatchNormalization.lua](https://github.com/kmul00/torch-vol/blob/master/VolumetricBatchNormalization.lua)

### VolumetricUpSamplingNearest
___
Performs `3D upsampling` on input ***videos*** containing any number of `input planes`.

```lua
nn.VolumetricUpSamplingNearest(scale_t, scale_xy)
```

where `scale_t = upsample ratio along time domain. scale_xy = upsample ratio along height, width dimension. Must be positive integers.` I have personally used this to perform ***unpooling***. Use case in ***Spatial*** domain could be found in the [dc-ign](http://arxiv.org/pdf/1503.03167v4.pdf) paper.

*Associated files*

1. [VolumetricUpSamplingNearest.lua](https://github.com/kmul00/torch-vol/blob/master/VolumetricUpSamplingNearest.lua)
2. [VolumetricUpSamplingNearest.c](https://github.com/kmul00/torch-vol/blob/master/generic/VolumetricUpSamplingNearest.c)
3. [VolumetricUpSamplingNearest.cu](https://github.com/kmul00/torch-vol/blob/master/cuda/VolumetricUpSamplingNearest.cu)

### VolumetricMaxUnpooling
___
Performs 3D `Volumetric Max Unpooling` using indices previously computed with `Volumetric Max Pooling`.

```lua
nn.VolumetricMaxUnpooling(max_module)
```

*Example usage*
```lua
max_module = nn.VoulmetricMaxPooling(kT, kW, kH)
model = nn.Sequential()
model:add(max_module)
model:add(nn.VolumetricMaxUnpooling(max_module))
```
Currently this is only the `CPU` version. I will update the `CUDA` version soon.

**Note:** It requires `kT==dT`, `kW==dW`, `kH==dH`.

This module is already *[merged](https://github.com/torch/nn/pull/569)* into the main **Torch** repository.

*Associated files*

1. [VolumetricMaxUnpooling.lua](https://github.com/kmul00/torch-vol/blob/master/VolumetricMaxUnpooling.lua)
2. [VolumetricMaxUnpooling.c](https://github.com/kmul00/torch-vol/blob/master/generic/VolumetricMaxUnpooling.c)

### VolumetricMaxPooling
___
Performs `Volumetric Max Pooling` that supports ***padding***

```lua
nn.VolumetricMaxPooling(nInputPlane, nOutputPlane, kT, kW, kH, [dT], [dW], [dH], [padT], [padW], [padH])
```

This module is already *[merged](https://github.com/torch/nn/pull/589)* into the main **Torch** repository.

*Associated files*

1. [VolumetricMaxPooling.lua](https://github.com/kmul00/torch-vol/blob/master/VolumetricMaxPooling.lua)
2. [VolumetricMaxPooling.c](https://github.com/kmul00/torch-vol/blob/master/generic/VolumetricMaxPooling.c)
3. [VolumetricMaxPooling.cu](https://github.com/kmul00/torch-vol/blob/master/cuda/VolumetricMaxPooling.cu)

## Ending Note
***

I will keep on adding stuffs as and when required. If you need anything in particular, you are most welcome to ask about it. Also feel free to give suggestions, comments. It will be much appreciated.

And finally I would like to thank all the wonderful [Torch's contributors](https://github.com/torch/nn/graphs/contributors) for actively maintaining such a wonderful and easy-to-use library. Really appreciate their efforts and hard work.
