# PMC-LLaMA
The official codes for "PMC-LLaMA: Continue Training LLaMA on Medical Papers"

[**Huggingface**](https://huggingface.co/chaoyi-wu/PMC_LLAMA_7B) 

[**Arxiv Version**](https://arxiv.org/abs/2304.14454)

## Latest News:
We have release a new model **MedLLaMA-13B** finetuned with LLaMA-13B on some medical corpus, termed as [**MedLLaMA-13B**](https://huggingface.co/chaoyi-wu/MedLLaMA_13B/blob/main/README.md). It have been proved to be more powerful than both LLaMA-13B and PMC-LLaMA, refering to our benchmark for detail comparison:

Similarly it can be easily loaded with:
```
import transformers
import torch
tokenizer = transformers.LlamaTokenizer.from_pretrained('chaoyi-wu/MedLLaMA_13B')
model = transformers.LlamaForCausalLM.from_pretrained('chaoyi-wu/MedLLaMA_13B')
```
## Introduction:
We continue pre-training LLaMA on 4.8M PubmedCentral papers.

## Environment:
Simply set up the required environment as following:
```
conda install pytorch==1.13.0 torchvision==0.14.0 torchaudio==0.13.0 pytorch-cuda=11.6 -c pytorch -c nvidia
pip install transformers,sentencepiece,datasets
```

## Quick Start:
Check `simple_test.py` for quickly use PMC-LLaMA or you can follow this folowing simple sample.

```
import transformers
import torch
tokenizer = transformers.LlamaTokenizer.from_pretrained('chaoyi-wu/PMC_LLAMA_7B')
model = transformers.LlamaForCausalLM.from_pretrained('chaoyi-wu/PMC_LLAMA_7B')
sentence = 'Hello, doctor' 
batch = tokenizer(
            sentence,
            return_tensors="pt", 
            add_special_tokens=False
        )
with torch.no_grad():
    generated = model.generate(inputs = batch["input_ids"], max_length=200, do_sample=True, top_k=50)
    print('model predict: ',tokenizer.decode(generated[0]))
```

## Data:
The raw training data can be dowloaded from [S2ORC](https://github.com/allenai/s2orc), filter out the papers with PubmedCentral IDs, and you can get the training data we use. 

We will also release a version of training data soon.

## Pre-training:
Check `training.py` and `training.sh` for re-produce our work. 

More details about how to fine-tune LLaMA can refer to [Finetune_LLAMA](https://github.com/chaoyi-wu/Finetune_LLAMA)

## Results:
| Method              | Setting             | USMLE(OOD/ID) | MedMCQA(ID) | PubMedQA(ID) |
|---------------------|---------------------|------------------|--------------|------------------|
| Human (pass)        | Manual*             | 50.0            | --            | 60.0           |
| Human (expert)      | Manual*             | 87.0            | 90.0         | 78.0           |
| InstructGPT-175B    | Zero-shot*          | 46.0            | 44.0         | 73.2           |
| ChatGPT             | Zero-shot*          | 57.0            | 44.7         | 63.9           |
| LLaMA-7B            | Zero-shot*          | 27.1            | 24.3         | 5.2             |
| LLaMA-33B           | Zero-shot*          | 43.4            | 30.3         | 1.8             |
| LLaMA-7B-Full  | Full fine-tuning   | 44.55/35.66     | 48.15        | 73.4          |
| PMC-LLaMA-7B-Full | Full fine-tuning | 44.70/40.61     | 50.54        | 69.5          |
| LLaMA-13B-Full  | Full fine-tuning   | 45.48/39.36     | 51.42        | 77.2          |
| MedLLaMA-13B-Full | Full fine-tuning | **48.15/43.52**     | **54.15**        | **77.7**          |
| LLaMA-7B-PEFT  | PEFT               | 29.38/27.34     | 32.37        | 65.8          |
| PMC-LLaMA-7B$-PEFT | PEFT             | 30.64/28.52     | 34.33        | 68.2          |
| LLaMA-13B-PEFT  | PEFT               | 38.73/38.73     | 39.56        | 65.4          |
| MedLLaMA-13B-Full | PEFT | **39.12/39.98**     | **41.26**        | **69.4**         |

Note that, the manual and zero-shot results with * are referred from [LMFLow](https://github.com/OptimalScale/LMFlow/tree/main/src/lmflow).

## Downstream Training Curve:
<img src="https://github.com/chaoyi-wu/PMC-LLaMA/blob/main/figures/training_curve.png"/>

## Zero-shot Cases:
Note that, due to train on the papers, PMC-LLaMA may generate some citation numbers (LLaMA somtimes will do this as well) and we dismiss them in the cases to show the main contents.

<img src="https://github.com/chaoyi-wu/PMC-LLaMA/blob/main/figures/zero-shot_cases.png"/>

## Acknowledge
Minimal LLaMA -- https://github.com/zphang/minimal-llama

alpaca -- https://github.com/tatsu-lab/stanford_alpaca

LMFLow -- https://github.com/OptimalScale/LMFlow/tree/main/src/lmflow

LLaMA: Open and Efficient Foundation Language Models -- https://arxiv.org/abs/2302.13971

## Contact
If you have any question, please feel free to contact wtzxxxwcy02@sjtu.edu.cn.

