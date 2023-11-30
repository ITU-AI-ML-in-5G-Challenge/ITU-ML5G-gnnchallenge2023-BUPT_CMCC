# Graph Neural Networking Challenge 2023 by Team BUPT_CMCC
## Project Structure
- ckpt: folder containing our trained CBR+MB model and MB model.
  - cbr_mb_ipg_onRate_cv: this folder contains five trained CBR+MB models.
  - mb_ibg_onRate_cv: this folder contains five trained MB models.
  - cbr_mb_model: this is the best-performing model among the five CBR+MB models in terms of validation results. The content in this folder is same to that in "ckpt/cbr_mb_ipg_onRate_cv/0"
  - mb_model: this is the best-performing model among the five MB models in terms of validation results. The content in this folder is same to that in "ckpt/mb_ibg_onRate_cv/3"
- data: folder containing the datasets for training two models.
  - [data_generator_cbr_mb.py](data_generator_cbr_mb.py): python script that can be used to extract the required CBR+MB dataset.
  - [data_generator_mb.py](data_generator_mb.py): python script that can be used to extract the required MB dataset.
  - [data_generator_test.py](data_generator_test.py): python script that can be used to extract the required test dataset.
- [models.py](models.py): python module which contain the two models used for training.
- [models_predict.py](models_predict.py): python module which contain the two models used to predict the test dataset. 
- [requirements.txt](requirements.txt): file containing the environment in which we run the program.
- predict.zip: compressed package that contains our predicted results.

## Install dependencies 

Our environment is **Python 3.9.16**. You can install the required packages through the requirements.txt provided by us.
```
pip install -r requirements.txt
```

## Obtain the result directly through our trained model
After configuring the environment, you can directly obtain the result by executing the following commands.
```
python predict_separate.py --te-path ./data/data_test_separate
```
## Reproduce the full training pipeline
### Extract required features form the dataset
*We have prepared the extracted dataset in the 'data' folder. To quickly train the model, you can jump to the next step.*

Specifically, we add `IPG` and `On-Rate` for the CBR+MB model, and `IBG `and `On-Rate` for the MB model. 

We prepare three Python files to extract the dataset under the 'data' folder, where 'data_generator_cbr_mb.py' and 'data_generator_mb.py' are used to extract the training dataset and 'data_generator_test.py' is used to extract the test dataset , since the samples in the test dataset are mixed.

To extract the training dataset of CBR+MB and MB, you can use the following command 
```
python data_generator_cbr_mb.py

python data_generator_mb.py
```

Unlike the script file provided by the baseline, we modify the path of the input file and output file. So you can make the modifications on lines 35 and 37 of the corresponding files.

### Train the model

You can use the same commands as the baseline to train our models. We train our models through 5-fold cross validation.
```
python train.py -ds CBR+MB -cfv

python train.py -ds MB -cfv
```

On lines 327 and 328 of train.py, you can change the paths for ckpt and tensorboard.

### Predict the test dataset

We utilize the 'model_predict.py' for prediction. Please note that this module cannot be used for training. It will compare the predicted value with sum of the transmission delay before outputting the final delay. If the predicted value is smaller than the sum of the transmission delay, the predicted value will be assigned as the sum of the transmission delay.

We use 'predict_separate.py' to predict the test dataset. Since we use two models for prediction, you need to modify the path of the models and training datasets in lines 240, 241, 257, and 258 of 'predict_separate.py'. After that, you can run the following command:
```
python predict_separate.py --te-path "path/to/test/dataset"
```
## Credits
Zicheng Wang, Yuanjie Duan, Lingqi Guo, Danyang Chen

