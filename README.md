# HateSpeechDetection
Hate speach detection experiments for Polish language. Data source: https://2019.poleval.pl/index.php/tasks/task6

Repository contains experiments about hate speech classification and usage instruction for deployed system based on these experiments.

Repository structure:
- `data_preprocessing.ipynb` - code for data preparation 
- `simple_approach_establishig_baseline.ipynb` - research on baseline simple machine learning models
- `transformers.ipynb` - experiments with the use of pre-trained BERT-based encoder for text
- `evaluation.ipynb` - evaluation of the best variant for each approach - quality and prediction speed
- `data_augmentation.ipynb`, `simple_approach_augmented_data.ipynb`, `transformers_on_augmented_data.ipynb` - code for performing augmentation on data and experiments for this data version (same as above)
- `mlruns` - directory with saved experiments history


# Experiments

 Main tested factors in `establishing_baseline`:
  - text lemmatized or raw
  - Document Term Matrix or TFIDF matrix as text representation (and their hiperparameters)
  - dimensionality reduction with SVD
  - naive Bayes and linear SVM classifiers (and their hiperparameters)
  
 Experiments in `transformers_baseline`:
  - roberta model used as encoder (RoBERTa‑v2 (base) v4.4 from https://github.com/sdadas/polish-roberta)
  - linear svm and Multilayer Perceptron fitted and at the top of encodings optimized as classifiers
  
 Experimens in `<...>_on_augmented_data.ipynb`:
  - same as above but on augmented training set (see section "data augmentation" below)
  
 Main evaluation metric considered: Micro F1 Score (because of high class imbalance)

## Data augmentation

Data are highly imbalanced and we addressed this issue. In some models in experiments we used weighing of classes as it should improve our models. 

We would like to perform data augmentation but ther is no clear way of augmenting text data (especially short). We came up with an original idea: we extended text of miniority classes with some additional words generated with powerful language model - we took text, we treated it like the starting point and generated the next few words with gpt model. This way we created many variants of each text. What is important - it is reasonable method because it does not affect texts' classes - if it is hate speech, then with some continuation added it will still contain hate speech. Additionally, added text can provide new pieces of information for better model performance because language model adds texts with respect to context.

# Results


| Model name   |      score [Micro F1]      |  Prediction speed [sec/observation] |
|----------|:-------------:|------:|
| classic_ml |  **0.879** | 0.000011 |
| transformer\_encoder\_based |    0.865  |   0.029091 |
| classic_ml on augmented data |  0.873 | - |
| transformer\_encoder\_based on augmented data |    0.854  |   - |
    
 Score in bold beats Poleval 2019 task 6.2 winner (http://2019.poleval.pl/index.php/results/) 
    