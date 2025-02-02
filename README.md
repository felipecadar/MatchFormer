# MatchFormer: Interleaving Attention in Transformers for Feature Matching

![matchformer](matchformer.png)

### Introduction

In this work, we propose a novel hierarchical extract-and-match transformer, termed as **MatchFormer**. Inside each stage of the hierarchical encoder, we interleave self-attention for feature extraction and cross-attention for feature matching, enabling a human-intuitive **extract-and-match** scheme. 

More detailed can be found in our [arxiv](https://arxiv.org/pdf/2203.09645.pdf) paper.

### Installation

 The requirements are listed in the `requirement.txt` file. To create your own environment, an example is:

```bash
conda create -n matchformer python=3.7
conda activate matchformer
cd /path/to/matchformer
pip install -r requirement.txt
```

### Datasets

You can prepare the test dataset in the same way as [LoFTR](https://github.com/zju3dv/LoFTR/blob/master/docs/TRAINING.md), place the dataset and index in the data directory.

A structure of dataset should be:

 ```
data
├── scannet
│   ├── index
│   │   ├── intrinsics.npz
│   │   ├── scannet_test.txt
│   │   └── test.npz
│   └── test
│   	├── scene0707_00
│    	├── ...
│       └── scene0806_00
└── megadepth
    ├── index
    │	├── trainvaltest_list
    │		└── val_list.txt
    │   └── scene_info_val_1500
    │		 ├── 0015_0.1_0.3.npz
    │		 ├── ...
    │		 └── 0022_0.5_0.7.npz
    └── test
    	├── Undistorted_SfM
        └── phoenix
 ```



### Evaluation

The evaluation configurations can be adjusted at `/config/defaultmf.py`

The weights can be downloaded in [Google Drive](https://drive.google.com/drive/folders/1JSnoQMfr32eoIXwJ1gpwUaPKv4kjdqJ7?usp=sharing).

Put the weight at `model/weights`.

#### Indoor:

```
# adjust large SEA model config:
MATCHFORMER.BACKBONE_TYPE = 'largesea'
MATCHFORMER.SCENS = 'indoor'
MATCHFORMER.RESOLUTION = (8,2)

python test.py /config/data/scannet_test_1500.py --ckpt_path /model/weights/indoor-large-SEA.ckpt --gpus=1 --accelerator="ddp"
```

```
# adjust lite LA model config:
MATCHFORMER.BACKBONE_TYPE = 'litela'
MATCHFORMER.SCENS = 'indoor'
MATCHFORMER.RESOLUTION = (8,4)

python test.py /config/data/scannet_test_1500.py --ckpt_path /model/weights/indoor-lite-LA.ckpt --gpus=1 --accelerator="ddp"
```

#### Outdoor:

```
# adjust large LA model config:
MATCHFORMER.BACKBONE_TYPE = 'largela'
MATCHFORMER.SCENS = 'outdoor'
MATCHFORMER.RESOLUTION = (8,2)

python test.py /config/data/megadepth_test_1500.py --ckpt_path /model/weights/outdoor-large-LA.ckpt --gpus=1 --accelerator="ddp"
```

```
# adjust lite SEA model config:
MATCHFORMER.BACKBONE_TYPE = 'litesea'
MATCHFORMER.SCENS = 'outdoor'
MATCHFORMER.RESOLUTION = (8,4)

python test.py /config/data/megadepth_test_1500.py --ckpt_path /model/weights/indoor-large-SEA.ckpt --gpus=1 --accelerator="ddp"
```

### Training

coming soon...

### Citation

If you are interested in this work, please cite the following work:

```
@article{wang2022matchformer,
  title={MatchFormer: Interleaving Attention in Transformers for Feature Matching},
  author={Wang, Qing and Zhang, Jiaming and Yang, Kailun and Peng, Kunyu and Stiefelhagen, Rainer},
  journal={arXiv preprint arXiv:2203.09645},
  year={2022}
}
```

### Acknowledgments

Our work is based on [LoFTR](https://github.com/zju3dv/LoFTR) and we use their code.  We appreciate the previous open-source repository [LoFTR](https://github.com/zju3dv/LoFTR).
