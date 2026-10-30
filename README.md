# Fruit Classification Using a ResNet CNN

CS4287 - Neural Computing, Assignment 1  
University of Limerick | Group 5

**Team:** Colin Walsh, Luke Scanlon and Evelyn Sadlier

This project develops a ResNet-20 convolutional neural network to classify
274 categories of fruits, vegetables, nuts, seeds and mushrooms from the
Fruits-360 dataset.

Images are resized from 100×100 to 32×32 pixels to reduce computational
cost. We will train and evaluate the model, compare hyperparameter and
data augmentation choices, and analyse its classification performance.

## Dataset

We use the [Fruits-360 dataset](https://github.com/fruits-360/fruits-360-100x100),
version **2026.10.18.2** of the 100x100 branch. Its 274 classes include fruits,
vegetables, nuts, seeds and mushrooms, with different varieties treated as
separate classes.

| Property | Value |
| --- | --- |
| Total images | 196,522 |
| Original training images | 147,416 |
| Test images | 49,106 |
| Classes | 274 |
| Original image size | 100 x 100 pixels |
| Project input size | 32 x 32 RGB pixels |

We resize the original images to 32 x 32 pixels and scale pixel values to
[0, 1]. The notebook splits the original training set into **118,025 training**
and **29,391 validation** images using a fixed seed of 42. The original test
set is reserved for final evaluation.

### Dataset credits and licence

The Fruits-360 dataset was created by **Mihai Oltean**. Credit for the original
images and dataset belongs to its creator and contributors; our group contributes
the project code, analysis and resized image preparation.

- **Source:** [Fruits-360 original dataset repository](https://github.com/fruits-360/fruits-360-100x100)
- **Creator:** [Mihai Oltean](https://mihaioltean.github.io)
- **Copyright:** (c) 2017 onwards, Mihai Oltean
- **Dataset licence:** [Creative Commons Attribution-ShareAlike 4.0 International](https://creativecommons.org/licenses/by-sa/4.0/), also recorded in [LICENSE](LICENSE)
- **Changes made for this project:** Images resized from 100 x 100 to 32 x 32 pixels.

The resized dataset remains under CC BY-SA 4.0. When sharing it, retain attribution,
identify the changes and include the licence link.

## Shared project resources

**Shared report:** [Fruit Classification CNN (OneDrive / SharePoint)](https://ulcampus-my.sharepoint.com/:w:/r/personal/23367989_studentmail_ul_ie/Documents/Fruit%20Classification%20CNN.docx?d=wb0d4a5fec84d40e0830fe739652bddda&csf=1&web=1&e=YspIpL)

The shared Word document is used to collaborate on the project report.
Access is managed through the document's sharing permissions; contact
a group member if you need access.

Source code and notebook changes are tracked in this Git repository.

## Self-hosted GPU server

We use a self-hosted Jupyter server with an **NVIDIA RTX 2060 GPU** and
**TensorFlow 2.17.0** to run our model development, training and experiments.
Team members edit notebooks in VS Code on their own computers and execute
cells using a remote Python kernel on the server.

Access requires a **Tailscale connection**, an invitation from the server owner
and a private Jupyter access token. Follow the
[GPU server setup guide](docs/GPU_Setup_Guide.md) to connect VS Code,
verify GPU availability and shut down your session when finished.

- Code runs on the server, so dataset paths must refer to files stored there.
- GPU memory and computing power are shared; coordinate training runs with the team.
- Shut down your own kernel after use to release GPU memory.
- Keep access tokens in your local `.env` file and out of Git commits.

## Model architecture

Our model is a **ResNet-20**, implemented with TensorFlow/Keras in
[CS4287-Assign-23363185-23390573-23367989.ipynb](model/CS4287-Assign-23363185-23390573-23367989.ipynb). It takes a 32 x 32 RGB image
and outputs a probability for each of the 274 dataset classes.

| Stage | Structure | Output shape per image |
| --- | --- | --- |
| Input | Normalised RGB image | 32 x 32 x 3 |
| Initial convolution | 3 x 3 convolution, 16 filters, batch normalisation, ReLU | 32 x 32 x 16 |
| Residual stage 1 | 3 residual blocks, 16 filters | 32 x 32 x 16 |
| Residual stage 2 | 3 residual blocks, 32 filters; first block downsamples | 16 x 16 x 32 |
| Residual stage 3 | 3 residual blocks, 64 filters; first block downsamples | 8 x 8 x 64 |
| Global average pooling | Averages each feature map over its spatial dimensions | 64 features |
| Classifier | Dense layer with softmax activation | 274 class probabilities |

Each of the **nine residual blocks** contains two 3 x 3 convolutions with batch
normalisation. A ReLU follows the first convolution's batch normalisation and
the addition of the shortcut. Skip connections provide a direct path for
features and gradients through the network. When a block changes the spatial
size or channel count, its shortcut uses a 1 x 1 convolution and batch
normalisation to match the shapes before addition.

The current configuration uses **Adam** with a learning rate of **0.001**,
**sparse categorical cross-entropy** loss and **accuracy** as its training metric.

## Repository structure

```text
CNNproject_gp5/
|-- model/
|   `-- CS4287-Assign-23363185-23390573-23367989.ipynb       # Data preparation and ResNet implementation
|-- Dataset_32x32/
|   |-- Training/                # Resized training images organised by class
|   |-- Test/                    # Resized test images organised by class
|   `-- config/
|       `-- resize_training.py   # Image resizing script
|-- docs/
|   |-- GPU_Setup_Guide.md       # Remote GPU connection instructions
|   `-- CS4287_Group5_CNN_Report.docx
|-- .env_template               # Template for local server credentials
|-- LICENSE                     # Dataset licence and attribution
`-- README.md
```

## Running the notebook

1. Open this repository in VS Code and install Microsoft's **Python** and
   **Jupyter** extensions.
2. Copy `.env_template` to a local `.env` file and replace the token placeholder
   with the private token supplied by the server owner. The `.env` file is
   ignored by Git. VS Code's remote Jupyter connection must still be configured
   manually using the setup guide.
3. Connect to Tailscale and follow the
   [GPU server setup guide](docs/GPU_Setup_Guide.md) to select your own remote
   Python kernel.
4. Open [model/CS4287-Assign-23363185-23390573-23367989.ipynb](model/CS4287-Assign-23363185-23390573-23367989.ipynb). Its current
   dataset path is `/shared-datasets/Dataset_32x32`, which must contain
   `Training/` and `Test/` folders on the server. Update `data_dir` if using
   a different location.
5. Run the GPU check first, then execute the notebook cells in order. The
   notebook uses TensorFlow, Pillow and Matplotlib in the remote environment.
6. Save the notebook and any model outputs, then shut down your own remote
   kernel as described in the setup guide.
