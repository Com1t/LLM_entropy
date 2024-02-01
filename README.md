# LLM_entropy

**Table Of Contents**

- [Description](#description)
- [Environment](#environment)
- [Prerequisites](#prerequisites)
- [Running the sample](#running-the-sample)
	- [Inference](#inference)

## Description

This repository presents an evaluation of LLaMA inference entropy.
The work is inspired by [EdgeBERT: Sentence-Level Energy Optimizations for Latency-Aware Multi-Task NLP Inference](https://arxiv.org/abs/2011.14203), which leveraged the inherit meaning of entropy to accelerate inference.

This repo extracted [modeling_llama.py](https://github.com/huggingface/transformers/blob/881e966aced6f0f208f43d7b7e7e55bc680f8fa5/src/transformers/models/llama/modeling_llama.py) from huggingface/transformers(commit 881e966) and injected entropy calculation within the inference process.

This repository also contains an extra `simple_generate.py` example for simplest access to LLaMA inference.

*Important: this entropy experiment is done with kv-cache, dumping without kv-cache is not supported for heat map generation*

## Environment

1. Use `startDocker.sh` to create the torch environment.

2. (Optional) `startJupyterLabOnly.sh` can be used to create a Jupyter Lab environment.

## Prerequisites

1. Upgrade pip version and install the huggingface/transformers.
    ```bash
    pip3 install --upgrade pip
    pip3 install -r requirements.txt
    ```

2. If bitsandbytes doesn't work, [install it from source.](https://github.com/TimDettmers/bitsandbytes/blob/main/compile_from_source.md) Windows users can follow [these instructions](https://github.com/tloen/alpaca-lora/issues/17).

## Running the sample

To reproduce the experiment
1. Replace the `modeling_llama.py` in your python library by a soft link.
2. Modify the following variables in `simple_generate.py`, `model.model.entropy_save_dir`(Line ~172) to designated directory for entropy dump, and set `model.model.calculate_entropy`(Line ~156) and `model.model.save_entropy`(Line ~157) to True.
3. Run inference using `simple_generate.py`
4. The `modelling_llama.py` will dump several entropy file in `{model.model.entropy_save_dir}` as `{entropy_save_prefix}_{num_gen}_{idx}.pt`
5. Use `entropy.ipynb` to plot the heat map.

NOTE:
1. By default, `simple_generate.py` can be run directly. It will dump the entropy into "entropy_dump" in current directory.
2. The `entropy_save_dir` will automatically be cleaned before running this experiment.
3. `entropy_save_prefix` can also be modified by set `model.model.entropy_save_prefix` variable.
4. In line 171, `model.model.num_gen = 0`, because `num_gen` is accumulated number of generation.

While there are many different usage to this repository, here I only interate over the usage of `simple_generate.py`.

### Inference (`simple_generate.py`)

Example usage:

Use model weight 'decapoda-research/llama-7b-hf' from huggingface
```bash
python simple_generate.py \
    --base_model decapoda-research/llama-7b-hf \
```

Use model weight '/work/llama-7b-chat' from local directory
```bash
python simple_generate.py \
    --base_model /work/llama-7b-chat \
```

### Original Model weight

Although the 'decapoda-research/llama-7b-hf' model weight provides competitive performance, if you want to use the original model weight from Meta, download the model weight from [meta-llama huggingface](https://huggingface.co/meta-llama) or [meta llama](https://ai.meta.com/llama/).

Note 1: Huggingface takes longer time to get authorization from meta, but it's easier to use.
Note 2: If you download the model weight from meta website, follow this [guide](https://huggingface.co/docs/transformers/main/model_doc/llama) to convert it into huggingface format.
