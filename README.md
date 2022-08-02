# anomalib_utils
Anomalib Guide

Anomalib is a deep learning library that aims to collect state-of-the-art anomaly detection algorithms for benchmarking.

1. [Install](https://github.com/openvinotoolkit/anomalib)

2. Available Models:

- CFlow
- DFM
- DFKDE
- FastFlow
- PatchCore
- PADIM
- STFPM
- GANomaly

3. Train a model on a custom dataset:
- Modify `dataset` section in `config.yaml` as follows:
```
dataset:
  name: <name-of-the-dataset>
  format: folder
  path: <path/to/folder/dataset>
  normal_dir: normal # name of the folder containing normal images.
  abnormal_dir: abnormal # name of the folder containing abnormal images.
  normal_test_dir: null # name of the folder containing normal test images.
  task: segmentation # classification or segmentation
  mask: <path/to/mask/annotations> #optional (USE null IF DONT NEEDED)
  extensions: null
  split_ratio: 0.2 # ratio of the normal images that will be used to create a test split
  image_size: 256
  train_batch_size: 32
  test_batch_size: 32
  num_workers: 8
  transform_config:
    train: null
    val: null
  create_validation_set: true
  tiling:
    apply: false
    tile_size: null
    stride: null
    remove_border_count: 0
    use_random_tiling: False
    random_tile_count: 16
```

## Example given:
1. Arrange dataset in a directory tree:

```sh
.
├── evaluation
│   ├── fissure
│   └── normal
└── train
    ├── fissure
    └── normal

```

2. Modify the `dataset` section from the `config.yaml` file of the preferred model. This config file can be found [here](models/padim/config_custom.yaml). In this case we use `padim` model. 

```
dataset:
  name: crack_dataset
  format: folder
  path: home/curro/datasets/crack_dataset/
  normal_dir: /home/curro/datasets/crack_dataset/train/normal # name of the folder containing normal images.
  abnormal_dir: /home/curro/datasets/crack_dataset/train/fissure # name of the folder containing abnormal images.
  normal_test_dir: /home/curro/datasets/crack_dataset/evaluation/normal # name of the folder containing normal images.
  task: classification # classification or segmentation
  mask: null # <path/to/mask/annotations> #optional
  extensions: null
  split_ratio: 0.2 # ratio of the normal images that will be used to create a test split
  image_size: 256
  train_batch_size: 8
  test_batch_size: 8
  num_workers: 8
  transform_config:
    train: null
    val: null
  create_validation_set: true
  tiling:
    apply: false
    tile_size: null
    stride: null
    remove_border_count: 0
    use_random_tiling: False
    random_tile_count: 16
```

3. Train the model. Run the following command:
```sh
python tools/train.py --config $CONFIG_PATH$
```

For example:
```sh
python tools/train.py --config anomalib/models/padim/config.yaml
``` 
