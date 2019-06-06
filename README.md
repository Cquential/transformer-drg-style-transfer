# Introduction
This repository have scripts and Jupyter-notebooks to perform all the different steps involve in **Delete, Retrieve and Generate** mechanism. 
The mechanism is used for **_text style transfer_** for the case when **_parallel corpus_** for both the style is not available. This mechanism works on the assumption that the text of any style is made of **two** parts: **1. Content** and **2. Attributes** . Below is a simpe example of a resturent review.
```
The food was great and the service was excellent.
Content: The food was and the service was
Attributes: great, excellent
```
We generate the sentence in the different style for a given content in in two different ways. First way is known as the **Delete and Generate** in which model transfers the style of the text (i.e. Positive -> Negative, Romantic -> Humarous, Republican -> Democrats) by choosing attributes automatically learnt during the training. Second way is known as the **Delete, Retrieve and Generate** in which Generator uses attributes provided by the user to generate sentence from the content. Below as the few example.

**Generate Negative text with Delete and Generate**
```
Content: The food was and the service was
Output: The food tasteless and the service was horrible.
```

**Generate Negative text with Delete, Retrieve and Generate**
```
Content: The food was and the service was
Attributes: blend, slow
Output: The food was blend and the service was slow.
```




The process of style transfer consist multiple steps. 
1. Prepare Training data


Next section describes steps requies from preparing the data to run inference. 
## Steps
### 1. Classifier Training:
We have used [BERT](https://arxiv.org/abs/1810.04805) for classification. This classification trainings helps to find the attributes from the sentence. We have come up with a noval way to choose one perticular head of BERT model which captures the important details which is responsible for better classification. 
  * Classifier Training Data Preparation: **_BERT_Classification_Training_data_preparation.ipynb_** notebook creates training, testing and dev data. Modify the the paths of the input and output files.
  * BERT Classifier Training: Run the below command to train BERT classifier.
  ```bash
  export BERT_DATA_DIR = Path of the directory containing output previous step (train.csv, dev.csv)
  export BERT_MODEL_DIR = Path to save the classifier trained model
  ```
  
  ``` bash
  python run_classifier.py \
  --data_dir=$BERT_DATA_DIR \
  --bert_model=bert-base-uncased \
  --task_name=yelp \
  --output_dir=$BERT_MODEL_DIR \
  --max_seq_length=70 \
  --do_train \
  --do_lower_case \
  --train_batch_size=32 \
  --num_train_epochs=1 \
  ```

