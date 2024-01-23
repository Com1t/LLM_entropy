# LLM_entropy

**Table Of Contents**

- [Description](#description)
- [Environment](#environment)
- [Prerequisites](#prerequisites)
- [Running the sample](#running-the-sample)

## Description

This repository presents an evaluation of LLaMA inference entropy.
The work is inspired by [EdgeBERT: Sentence-Level Energy Optimizations for Latency-Aware Multi-Task NLP Inference](https://arxiv.org/abs/2011.14203), which leveraged the inherit meaning of entropy to accelerate inference.

This repo extracted [modeling_llama.py](https://github.com/huggingface/transformers/blob/881e966aced6f0f208f43d7b7e7e55bc680f8fa5/src/transformers/models/llama/modeling_llama.py) from huggingface/transformers(commit 881e966) and injected entropy calculation within the inference process.

## Environment

1. Use `startDocker.sh` to create the torch environment.

2. (Optional) `startJupyterLabOnly.sh` can be used to create a Jupyter Lab environment.

## Prerequisites

1. Upgrade pip version and install the huggingface/transformers.
    ```bash
    pip3 install --upgrade pip
    pip3 install transformers
    ```

## Running the sample

To reproduce the experiment
1. Replace the `modeling_llama.py` in your python library by a soft link.
2. Modify the following `modeling_llama.py` variables, `entropy_save_dir` to designated directory for entropy dump, and set `calculate_entropy` and `save_entropy` to True.
3. Run inference as usual
4. The modelling_llama will dump several entropy file
5. Use `heat_map.ipynb` to plot the heat map.
