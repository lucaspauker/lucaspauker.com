---
title: "LLMs Unleashed: The Power of Fine-Tuning"
date: 2023-07-23
draft: false
tags: ["AI", "CS"]
---

_Disclaimer: This article mentions [https://terra-cotta.ai/](https://terra-cotta.ai/), an LLM experimentation platform I am building_

## Introduction

ChatGPT, Bard, and other large language models (LLMs) are very useful for a wide variety of tasks from writing code to answering complex questions to aiding with education. However, these models are ultimately limited by the data that they are trained on. Also, these models are trained to be able to answer a wide variety of questions which may not be sufficient for domain-specific questions. Fine-tuning is essential in order to make these models accurately answer domain-specific questions and be useful for difficult tasks. Furthermore, fine-tuning may be cheaper for inference.

Fine-tuning is the process of training an LLM  on your own custom data. The fine-tuning process begins with a generic LLM that is pretrained on a large amount of text data. Fine tuning updates the generic model’s parameters or weights by using a smaller dataset for a target task.

One example of a task where fine-tuning is necessary is getting a medical diagnosis from raw medical records, such as electronic health records or medical imaging reports. ChatGPT is unlikely to perform this task well since it lacks specialized knowledge in medical domains and direct experience with real medical cases. ChatGPT will usually generate a confident response to any query, but the response cannot always be trusted due to hallucinations, where the model returns incorrect information [[1]](https://arxiv.org/pdf/2305.14552v1.pdf). Hallucinations are common for difficult tasks. Fine-tuning a language model specifically on validated, trusted medical data is essential to ensure accurate and reliable results. A fine-tuned model would learn the domain-specific knowledge and likely be able to return accurate diagnoses for this task.

The idea of fine-tuning has a strong research pedigree. The approach can be traced back to 2018, when two influential papers were published. The first paper, “Improving Language Understanding by Generative Pre-Training” by Radford et al. [[2]](https://s3-us-west-2.amazonaws.com/openai-assets/research-covers/language-unsupervised/language_understanding_paper.pdf) introduces the GPT model that is now used in ChatGPT. GPT used self-supervised learning to train an LLM on a large amount of unlabeled data. In the paper, the authors show that their GPT model could achieve state-of-the-art results on multiple tasks by fine-tuning their model on specific datasets. Similarly, “BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding” by Devlin et al. [[3]](https://arxiv.org/pdf/1810.04805.pdf) introduced BERT, a novel transformer model and showed the ability for it to be fine-tuned and achieve state-of-the-art performance on multiple tasks after fine-tuning. The image below shows how BERT can be fine-tuned on specific tasks such as SQuAD.

![Image from BERT paper [3]](/images/finetuning/bert_finetuning.png)
_Image from BERT paper [[3]](https://arxiv.org/pdf/1810.04805.pdf)_

## Fine-tuning vs. prompting

Fine-tuning and  prompting are  two ways to solve tasks with LLMs. Fine-tuning adapts  an LLM with a dataset by updating the model’s parameters. On the other hand, prompting refers to a user inputting  specific instructions or text as prompts into a generalized (not fine-tuned) LLM to guide the model's response. One-shot and few-shot prompting are examples of prompting techniques. In the image below, we can see how zero-shot, one-shot, and few-shot prompting compare to fine-tuning. Note that the prompts for the prompting examples are longer than the fine-tuning prompt.

![Image from GPT-3 paper [4]](/images/finetuning/prompting_vs_finetuning.png)
_Image from GPT-3 paper [[4]](https://arxiv.org/pdf/2005.14165.pdf)_

Both fine-tuning and prompting are valuable techniques for using LLMs. Depending on the specific use case, one method or the other may be better. In general, fine-tuning is better for complex tasks where the user has a labeled dataset and may be cheaper in the long run due to cheaper inference costs, depending on the LLM provider.

For simple tasks, prompting has advantages over fine-tuning. First, prompting is faster to iterate on since you do not need to train a new model every time you update the prompt or change your dataset. Second, fine-tuning requires a labeled dataset, while prompting does not. Therefore, if you do not have training data or only have a few examples, prompting could be better. In general, it makes sense to start with prompting and seeing if it can solve your task before trying fine-tuning.

Fine tuning is better for complex tasks where the model’s generated output must be accurate and trusted. In the GPT-3 paper, “Language Models are Few-Shot Learners” by Brown et al. [[4]](https://arxiv.org/pdf/2005.14165.pdf), the authors compare few shot prompting on GPT-3 to fine-tuned models for many different tasks. Often, GPT-3 cannot outperform the fine-tuned models, especially on complex tasks. Each task is measured with performance measures such as accuracy or BLEU score (an NLP metric). One task where few-shot GPT-3 greatly underperforms fine-tuned models is natural language inference (NLI) tasks. In this task, there are two sentences and the model has to predict if the second sentence logically follows from the first. The non-fine-tuned model likely does not perform well since this task is difficult and requires an understanding of logic. Intuitively, fine-tuning outperforms prompting for complex tasks since one can train on an unlimited number of domain specific data points.

![Image from GPT-3 paper [4]](/images/finetuning/anli_graph.png)
_Image from GPT-3 paper [[4]](https://arxiv.org/pdf/2005.14165.pdf)_

Furthermore, inference is significantly cheaper with fine-tuned models when compared to prompted models due to the reduction in the amount of instruction required during prediction. Since fine-tuning allows developers to incorporate task-specific knowledge directly into the model's parameters,, fine-tuned models can generate accurate responses with minimal need for explicit instructions or prompts during inference. On the other hand, prompted models heavily rely on explicit prompts or instructions for each prediction, which can be computationally expensive and resource-intensive, especially for large-scale applications.

## Steps to fine-tune a model
Fine-tuning an LLM involves many steps including curating your data and picking the best architecture. Here are the general steps to fine-tune a LLM model:

1. Define task and dataset
1. Select LLM architecture
1. Update model weights
1. Select hyperparameters
1. Evaluate model
1. Deploy model

OpenAI lets you fine-tune their GPT-3 models on your custom data (in the future, they will add support for GPT-3.5 and GPT-4). We built a free platform to easily guide you through the steps above to fine-tune OpenAI LLMs here: [https://terra-cotta.ai/](https://terra-cotta.ai/). With the platform, you can also compare fine-tuned models to prompted models.

In conclusion, fine-tuning is a powerful technique that allows developers to leverage the knowledge and capabilities of pretrained language models while adapting them to specific real-world tasks, leading to improved accuracy and cost performance for a wide range of natural language processing applications.

## Citations
1. [https://arxiv.org/pdf/2305.14552v1.pdf](https://arxiv.org/pdf/2305.14552v1.pdf)
1. [https://s3-us-west-2.amazonaws.com/openai-assets/research-covers/language-unsupervised/language_understanding_paper.pdf](https://s3-us-west-2.amazonaws.com/openai-assets/research-covers/language-unsupervised/language_understanding_paper.pdf)
1. [https://arxiv.org/pdf/1810.04805.pdf](https://arxiv.org/pdf/1810.04805.pdf)
1. [https://arxiv.org/pdf/2005.14165.pdf](https://arxiv.org/pdf/2005.14165.pdf)

