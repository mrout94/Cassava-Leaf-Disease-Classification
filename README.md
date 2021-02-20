# Cassava Leaf Disease Classification
This was an Online Competition hosted on Kaggle from Nov 20 2020 - Feb 19 2021 at [this link](https://www.kaggle.com/c/cassava-leaf-disease-classification)

## Introduction:-
**Cassava Leaf Disease Identification Using Computer Vision**
**Background:-**
* Cassava is the second-largest provider of carbohydrates in Africa.
* Cassava is a key food security crop grown by smallholder farmers because it can withstand harsh conditions.
* **Viral Diseases** are major sources of poor yields of this crop.  

**Necessity:-**
* Existing methods of disease detection require farmers to solicit the help of government-funded agricultural experts to visually inspect and diagnose the plants.
* This makes it highly manual and labour intensive.
* It maybe possible to identify type of disease using Computer Vision, so they can be treated and a yields can be improved.
* While creating the model we need to keep in mind that farmers of Africa might not have a high-end camera to provide crisp images to the pipeline all the time.  

**Data Description:-**
* Set of 21,367 labeled images collected during a regular survey in Uganda.
* Most images crowdsourced from farmers taking photos of their gardens.
* Annotated by experts at the National Crops Resources Research Institute (NaCRRI) in collaboration with the AI lab at Makerere University, Kampala.
* This is in a format that most realistically represents what farmers would need to diagnose in real life.

**Expected Outcome:-**  
Classify each cassava image into four disease categories or a fifth category indicating a healthy leaf.

**Problem Statement:-**  
As inferred from the problem statement this problem is clearly a **Supervised - Muti class classification** problem. And as it revolves around identifying classes from *images* it falls under the umbrella of **Computer Vision**.

Evaluation Metric:- **Accuracy**

## Data Augmentation:-
Since the photos were taken by farmersd themselves and kind of crowd sources, those contained lot of noises and mislabels.  
To account for those and for regularization of the model, the following augmentations nwere done:
1. Label Smoothing to avoid *"over-confidence"* of the model.
2. Random resized crops to try and learn from all reas of the image and not focus on a single area.
3. Hue and Saturation modification to account for differing cameras among users.
4. Random brighness change to account for different times of day when the image might be taken.
5. Random vertical and horizontal flips to make the model orientation independent.
6. Coarse Dropouts for randomly hiding vertain parts of image such that if sees the overall picture and not try to overfit to one particular heavy node.

## Model Selection:-
Most of the top ranking submissions used an ensemle of different models to get better scores.  
But for me that approach might not work in real-life because it will introduce extra latency. And my goal from all these competitions is to gain practical experience that can be used in real life. So I decided to use a single model for submission and trying to optimize it's score.  
  
The models I tried and their scores are compiled below:  
| Model | Score (LB) |
| --- | ----------- |
| Resnet50 | 0.78 |
| MobileNetV2 | 0.83 |
| Xception | 0.85 |
| DenseNet169 | 0.80 |
| NASNetLarge | 0.81 |
| NASNetMobile | 0.76 |
| EfficientNetB4 | 0.87 |
| InceptionV3 | 0.89 |
Thus I chose InceptionV3 as my final model.

## Training Details:-
* Batch Size: 16
* Image Dimension: (299, 299, 3)
* Learning Rate: [Cosine Decay with warm Restarts](https://arxiv.org/abs/1608.03983)
* Early stopping
* 90:10 Train-Validation stratified Split
* Epochs: 40
* Optimizer: Adam
* Loss Function: Categorical Cross Entropy
* No TTA

## Performance:-
**CV Score: 0.85  
LB Score: 0.89  
Rank: 512/3900 (Top 14%)**
