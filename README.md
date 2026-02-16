<h2 align="center">
  <p><a href="https://arxiv.org/abs/2503.02972">LingOly-TOO Benchmark</a>: Disentangling Reasoning from Knowledge</p>
</h2>

LingOly-TOO  is a reasoning benchmark for Large Language Models. It was carefully designed to reduce the effect of memorisation in model performance estimates.

## Links
 - 📊 [Leaderboard](https://huggingface.co/spaces/jkhouja/lingoly-too)
 - 🌍 [Website](https://oxrml.com/lingoly-too/)
 - 📎 [Paper](https://arxiv.org/abs/2503.02972)

## Quick Start
- Coming soon: Support for [Inspect Evals](https://inspect.ai-safety-institute.org.uk/evals/)

If you want a more customizable way to run the benchmark, follow the instructions below.

## Requirements


To install requirements:

```setup
pip install -r requirements.txt

# To override failing on single package:
cat requirements.txt | xargs -n 1 pip install
```

We recommend installing the requirements as a conda virtual environment. 

```conda
conda create --name lingoly --file requirements.txt
```
If instead you prefer venv over conda to  manage dependencies:
```
python -m venv ./venv
source venv/bin/activate
```


## How to use this benchmark
The questions are stored in two zip files, `./testing/data/splits/benchmark.jsonl.zip` and `./creation/data/ann_puzzles.zip`. The former contains the cleaned and validated questions ready for loading into prompts. The latter contains the annotated linguistic problems files so that they can be re-processed as necessary. The zip files are password protected with the password `lingoly` to impede web scraping.

To run the benchmark on one of the models from this paper, (listed in `./testing/data/model_list.json`), install the requisite packages listed in `requirements.txt`, add any necessary API keys to the environment, and then execute one of the following commands: 

```
python benchmark_model.py MODELNAME
```

to generate model responses in the full context case, or `python benchmark_model.py MODELNAME --no_context True`, to generate model responses in the no-context cases. These are both in the `./testing/code` directory. 

Once the model has run, use the `scoring.py` script in the same way to score the model.

To run the benchmark on a new model, you can add the model to the provided python code by updating the `./testing/data/model_list.json`. If the model provider is one of the providers used in the paper, then the API or Hugging Face connections will work off the shelf. For other providers, the `./testing/code/load_questions.py` file provides loaders which prepare the questions in the correct format (including chat templates) for custom uses.


### To unzip data for inspection:
Unzip the test file:
```
unzip -P lingoly testing/data/benchmark.zip -d testing/data/
```

### Steps to create the data
The annotated data should exist in `./creation/data/formatted`

1. Run obfuscation
Execute the following commands with the number of obfuscation to generate per problem
```
cd creation/code
python obfuscate.py -n 6
```

1. Run data validation
```
python validate.py
```

The obfuscated data ready for benchmarking will be persisted in `./creation/data/benchmark_obf.zip`


### Example:
To run a model (e.g. GPT_4.5, Gemma_7B):
```
cd testing/code
testing/code$ python benchmark_model.py GPT_4.5
```
To score responses from a model after you run it:
```
testing/code$ python scoring.py GPT_4.5
```

## Reporting issues or bugs
Please submit a new issue for questions or reporting a bug.

## Citation

If you use this work, please cite:

```bibtex
@inproceedings{
khouja2026lingolytoo,
title={{LingOly}-{TOO}: Disentangling Reasoning from Knowledge with Templatised Orthographic Obfuscation},
author={Jude Khouja and Lingyi Yang and Simeon Hellsten and Karolina Korgul and Vlad A. Neacșu and Harry Mayne and Ryan Othniel Kearns and Andrew M. Bean and Adam Mahdi},
booktitle={The Fourteenth International Conference on Learning Representations},
year={2026},
url={https://arxiv.org/abs/2503.02972}
}
```
