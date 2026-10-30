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
