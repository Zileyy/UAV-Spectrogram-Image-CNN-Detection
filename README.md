# Radar-Based UAV Classification

This project classifies radar spectrogram images into two UAV classes and one non-UAV class using a convolutional neural network. The model is based on an ImageNet-pretrained EfficientNet-B0 backbone and is trained with PyTorch.

The supported classes are:

- `Corner Reflector`
- `Mavic 2 Pro`
- `Phantom 4 Pro`

The repository includes the training notebooks, a trained model, evaluation graphs, and a small application for testing predictions on individual images.

## Results

The best recorded validation accuracy was **99.11%** after 15 training epochs. This result is specific to the prepared dataset split and should not be treated as a measure of performance on new radar systems or operating conditions.

## Repository layout

```text
.
├── dataset/                         # ImageFolder data used by the training notebook
├── Data/                            # Original class-organized dataset
├── DataSplit/                       # Output created by the split notebook
├── Graphs/                          # Training and evaluation figures
├── training.ipynb                   # Model training and evaluation
├── tranining_split.ipynb            # Dataset splitting notebook
├── Report.pdf                       # Project report
└── requirements.txt
```

## Dataset

The project uses a subset of the **Time-Synchronized Multi-Sensor Drone Dataset (TSMS-Drone)**. The source dataset contains synchronized measurements from CW radar, FMCW radar, and an RF receiver. This project uses FMCW radar spectrogram images from three classes:

- `Corner Reflector`
- `Mavic 2 Pro`
- `Phantom 4 Pro`

Each class contains 500 images. The split used for training and evaluation contains 350 training images, 75 validation images, and 75 test images per class. The split is created with a fixed random seed of 42 in `tranining_split.ipynb`.

The dataset is not owned by this project. Download it from the official source and follow its terms of use:

> Jung, Y.-H., & Han, S.-K. (2026). *Time-Synchronized Multi-Sensor Drone Dataset (TSMS-Drone)* (Version 1). Figshare+.
>
> [https://doi.org/10.25452/figshare.plus.30027313.v1](https://doi.org/10.25452/figshare.plus.30027313.v1)

The related dataset publication is:

> Han, S.-K., & Jung, Y.-H. (2026). A time-synchronized multi-sensor drone dataset acquired from multiple radars and RF receiver. *Scientific Data, 13*, 407.
>
> [https://doi.org/10.1038/s41597-026-06802-6](https://doi.org/10.1038/s41597-026-06802-6)

To use the notebooks, organize the original images as follows:

```text
Data/
├── Corner Reflector/Image File/
├── Mavic 2 Pro/Image File/
└── Phantom 4 Pro/Image File/
```

Run `tranining_split.ipynb` to create the train, validation, and test directories. The split notebook writes to `DataSplit/`. The training notebook expects the same structure under `dataset/`; either copy the generated split to `dataset/` or set `DATA_DIR = "DataSplit"` in `training.ipynb`.

## Installation

Python 3.10 is recommended.

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

For Windows PowerShell:

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

## Training

Open `training.ipynb` after preparing the dataset. The notebook:

1. Resizes images to 224 x 224 pixels and applies ImageNet normalization.
2. Applies random horizontal flips, rotations, and color jitter to training images.
3. Fine-tunes an EfficientNet-B0 backbone.
4. Uses a classification head with dropout, batch normalization, SiLU activation, and three output classes.
5. Saves the best validation checkpoint as `best_efficientnet_radar.pth`.

The default configuration is 15 epochs, a batch size of 32, and a learning rate of `3e-4`. CUDA is used automatically when available.

## Limitations

The model was evaluated on a single prepared subset and a single train/validation/test split. Radar conditions, sensor configurations, ranges, backgrounds, and data collection environments can affect performance. Further testing on independent data is required before using the model outside this project.

## License

No project license is currently specified. The dataset remains subject to the license and usage conditions provided by its original authors.
