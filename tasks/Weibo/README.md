# Weibo task 
Weibo is a Chinese named entity recognition dataset and contains 4 named entity types. <br>
Weibo respectively contains 1,350/270/270 instances for training/dev/test.

## Dataset
The Weibo NER dataset using BMES tagging schema can be find [HERE](https://drive.google.com/file/d/1ZRE5r-PbdNF1KeklZbt4CZrAWnc8agys/view?usp=sharing)  
Download the corpus and save data at `[WEIBO_DATA_PATH]`


## Train and Evaluate
Download ChineseBERT model and save at `[CHINESEBERT_PATH]`.  
Run the following scripts to train and evaluate. 

For ChineseBERT-Base (see [chinesebert_base.sh](./chinesebert_base.sh)), 

```bash 
CUDA_VISIBLE_DEVICES=0 python3 $REPO_PATH/tasks/Weibo/Weibo_trainer.py \
--lr 4e-5 \
--max_epochs 10 \
--max_length 150 \
--weight_decay 0.01 \
--hidden_dropout_prob 0.2 \
--warmup_proportion 0.02  \
--train_batch_size 2 \
--accumulate_grad_batches 1 \
--save_topk 20 \
--val_check_interval 0.25 \
--classifier multi \
--gpus="1" \
--optimizer torch.adam \
--bert_path [CHINESEBERT_PATH] \
--data_dir [WEIBO_DATA_PATH] \
--save_path [OUTPUT_PATH] 
```

For ChineseBERT-Large (see [chinesebert_large.sh](./chinesebert_large.sh)), 

```bash
CUDA_VISIBLE_DEVICES=1 python3 $REPO_PATH/tasks/Weibo/Weibo_trainer.py \
--lr 2e-5 \
--max_epochs 5 \
--max_length 150 \
--weight_decay 0.001 \
--hidden_dropout_prob 0.1 \
--warmup_proportion 0.02 \
--train_batch_size 1 \
--accumulate_grad_batches 1 \
--save_topk 20 \
--val_check_interval 0.25 \
--gpus="1" \
--optimizer torch.adam \
--precision=16 \
--bert_path [CHINESEBERT_PATH] \
--data_dir [WEIBO_DATA_PATH] \
--save_path [OUTPUT_PATH] 
```

## Result
The evaluation metric is Span-Level F1. 
Result of our model and previous models are:

base model: 

| Model  |  Test Precision |  Test Recall |  Test F1 |  
|  ----  | ----  | ----  | ----  |
| BERT | 67.12 | 66.88 |  67.33 |
| RoBERTa | 68.49 | 67.81 | 68.15 |
| ChineseBERT | 68.27 | 69.78 | 69.02 |

large model:

| Model  |  Test Precision |  Test Recall |  Test F1 |  
|   ---- | ----  | ----  | ----  |
| RoBERTa-large |  66.74 | 70.02 | 68.35 |
| ChineseBERT-large | 68.75 | 72.97 | 70.80 |
