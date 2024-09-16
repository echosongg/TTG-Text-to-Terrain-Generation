# TTG: Text-to-Terrain Generation

**Team Members:**

- Oucheng Liu (U7439247)  **[submitter]**
- Tianchen Guo (U7439173)
- Xinni Song (U7439250)



## 0. Abstract

Terrain modeling represents a critical area of research in computer graphics, with applications spanning from gaming to simulation. Traditional methods, such as Perlin noise and value noise, have long been utilized to generate terrains. While effective, these techniques inherently produce random terrains that do not take into account user-specific requirements or detailed specifications. To address this limitation, we introduce a novel approach known as **T**ext-to-**T**errain **G**eneration (**TTG**) which leverages textual descriptions to create corresponding terrains. Due to the absence of pre-existing terrain-descriptive datasets, two bespoke datasets are developed: Distribution-to-Terrain and Description-to-Terrain. Initially, we leverage the Distribution-to-Terrain dataset to train a classifier that can categorize elevation distributions into five basic terrains. Next, the generator, trained with the Description-to-Terrain dataset, translates textual descriptions into specific elevation distributions. Finally, these distributions are converted into comprehensive regional elevation data using the value noise method, and visualized using Terragen 4. Our exploration led us to adopt the Word2Vec model for text vectorization coupled with a Deep Neural Network (DNN) for the generator. The effectiveness of our method was rigorously evaluated through trained classifier, manual assessments, and visualizations, demonstrating that our approach offers an efficient and user-friendly solution for terrain generation.



## 1. Environment Preparation

### Python

```shell
# create a new environment
conda create -n TTG python=3.10
conda activate TTG

# Install packages using pip
pip install pandas numpy matplotlib scikit-learn torch torchvision gensim
```

### C++

- Install VisualStudio2022

### Software

- Download QGIS from https://www.qgis.org/en/site/
- Download Terragen 4from http://planetside.co.uk/whats-new-in-terragen-4/



## 2. Data Preparation

The two datasets mentioned in the paper are placed in the directory:

- Distribution-to-Terrain Dataset: `Distribution_Generation/data/distribution`
- Description-to-Terrain Dataset: `Distribution_Generation/data/text`



## 3. Running

### Step 1: Training the Distribution Classifier

We train the elevation distribution classifier using the `distribution_to_category.ipynb` notebook in the `Distribution_Generation` folder. This classifier can categorize elevation distributions into five basic terrain types: plain, hill, mountain, plateau, and basin.

The model with the best accuracy is saved in `Distribution_Generation/model/best_classifier.pth`.

### Step 2: Training the Distribution Generator

We train the elevation distribution generator (Word2Vec+DNN) using the `text_to_distribution.ipynb` notebook in the `Distribution_Generation` folder. The trained generator can create desired elevation distributions from text descriptions.

The model with the best accuracy is saved in `Distribution_Generation/model/best_generator.pth`.

We also provide three implementation methods based on SVM, textCNN, and RoBERTa in the `Distribution_Generation/comparisons` directory.

### Step 3: Generating and Visualizing the Terrain

We generate elevation distributions for the test set of the Description-to-Terrain Dataset using the `text_to_distribution.ipynb` notebook in the `Distribution_Generation` folder and create three CSV files:

- Elevation distribution generated from text descriptions: `test_distribution.csv`
- Terrain type predicted by the Distribution Classifier for the generated distributions: `test_prediction.csv`
- Actual terrain type corresponding to the text descriptions: `test_true_label.csv`

Next, we run the `Terrain_Generation/3 Generate` file in VisualStudio 2022 and modify the distribution vector in `main.cpp` to generate the terrain file:

- `Debug/got.asc`

Then, we use QGIS to convert the `.asc` file to `.gif` file.

Finally, we import the `.gif` file into Terragen 4 for visualization.

We can also use `Terrain_Generation/visual_elevation.ipynb` to visualize the elevation map.