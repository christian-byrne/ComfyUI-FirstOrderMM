# ComfyUI-FirstOrderMM
A ComfyUI-native node to run First Order Motion Model for Image Animation

https://github.com/AliaksandrSiarohin/first-order-model

Now supports Face Swapping using Motion Supervised co-part Segmentation

https://github.com/AliaksandrSiarohin/motion-cosegmentation

## Workflow:

### FOMM

[FOMM.json](FOMM.json)

![FOMM Workflow](workflow.png)

https://github.com/FuouM/ComfyUI-FirstOrderMM/assets/57849916/36833a94-67f9-4dd9-b79b-181f86e83bfe

### Part Swap

[FOMM_PARTSWAP.json](FOMM_PARTSWAP.json)

![Partswap Workflow](workflow_fomm_partswap.png)

## Arguments

### FOMM

* `relative_movement`: Relative keypoint displacement (Inherit object proporions from the video)
* `relative_jacobian`: Only taken into effect when `relative_movement` is on, must also be on to avoid heavy deformation of the face (in a freaky way)
* `adapt_movement_scale`: If disabled, will heavily distort the source face to match the driving face
* `find_best_frame`: Find driving frame that best match the source. Split the batch into two halves, with the first half reversed. Gives mixed results. Needs to install `face-alignment` library.

### Part Swap

* `blend_scale`: No idea, keeping at default = 1.0 seems to be fine
* `use_source_seg`: Whether to use the source's segmentation or the target's. May help if some of the target's segmentation regions are missing
* `hard_edges`: Whether to make the edges hard, instead of feathering
* `use_face_parser`: For Seg-based models, may help with cleaning up residual background (should only use `15seg` with this). TODO: Additional cleanup face_parser masks. Should definitely be used for FOMM models
* `viz_alpha`: Opacity of the segments in the visualization

## Installation

1. Clone the repo to `ComfyUI/custom_nodes/`
```
git clone https://github.com/FuouM/ComfyUI-FirstOrderMM.git
```

2. Install required dependencies
```
pip install -r requirements.txt
```

**Optional**: Install [face-alignment](https://github.com/1adrianb/face-alignment) to use the `find_best_frame` feature:

```
pip install face-alignment
```

## Model 

FOMM currently supporting `vox` and `vox-adv`. Models can and must be manually downloaded from:
* [AliaksandrSiarohin/first-order-model](https://github.com/AliaksandrSiarohin/first-order-model)
* [graphemecluster/first-order-model-demo](https://github.com/graphemecluster/first-order-model-demo)

Part Swap currently supporting Seg-based models and FOMM (`vox` and `vox-adv`) models.
* `vox-5segments`
* `vox-10segments`
* `vox-15segments`
* `vox-first-order (partswap)` 

These models can be found in the original repository [Motion Supervised co-part Segmentation](https://github.com/AliaksandrSiarohin/motion-cosegmentation) 

Place them in the `checkpoints` folder. It should look like this:
```
place_checkpoints_here.txt
vox-adv-cpk.pth.tar
vox-cpk.pth.tar

vox-5segments.pth.tar
vox-10segments.pth.tar
vox-15segments.pth.tar
vox-first-order.pth.tar
```

For Part Swap, Face-Parsing is also supported **(Optional)** (especially when using the FOMM or `vox-first-order` models)

* resnet18 `resnet18-5c106cde`: https://download.pytorch.org/models/resnet18-5c106cde.pth
* face_parsing `79999_iter.pth`: https://github.com/zllrunning/face-makeup.PyTorch/tree/master/cp

Place them in `face_parsing` folder:
```
face_parsing_model.py
...
resnet18-5c106cde.pth
79999_iter.pth
```