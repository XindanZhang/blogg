This page is for differentiate three basic concepts of LLMs.

## Pre-training

The model is exposed to a large amount of data, which could be unstructured or weakly labeled. This phase aims to let the model learn basic patterns, features, and representations from the data. For example, in natural language processing, models like BERT and GPT are pre-trained on massive text corpora to understand language syntax, semantics, and contextual relationships

## Post-training
This stage focuses on further training the model after pretraining. It often involves using reinforcement learning or other methods to fine-tune the model for specific tasks or to improve its general performance.

The above belongs to training phase.

## What is Test-time compute?

This is inference/test phase.

> Test-time compute refers to the computational resources utilized during the inference phase—the process where a model generates outputs in response to user prompts or queries. Unlike the training phase, which is a resource-intensive but one-time endeavor, inference occurs every time the model is deployed, making the efficient management of test-time compute critical for both performance and practical scalability.
>
>Test time compute refers to the amount of compute that is used to generate completions from a language model (LLM) at test or inference time. This is in contrast to (pre- or post-) training compute, which is the amount of compute used to train the model on a large corpus of data.
