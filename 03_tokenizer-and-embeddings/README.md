# From Text to Numbers

Open [the notebook](article_03_text_to_numbers_smolLM2.ipynb) to explore tokenization, token IDs, and initial embeddings using the base [SmolLM2-135M checkpoint](https://huggingface.co/HuggingFaceTB/SmolLM2-135M).

## What you will learn

Two synthetic work requests are followed through the tokenizer and embedding lookup. Examples cover character-level BPE, case and whitespace, a rectangular `6 × 4` toy embedding table, five real embedding lookups, an artificial training update, and identical initial vectors for a shared token ID.

Toy values illustrate mechanisms; real tokenizer and embedding outputs are measurements from the pinned checkpoint. No text generation or task-quality evaluation is performed.

## Setup and execution

CPU is sufficient. The first run needs internet access for approximately 270 MB of model weights plus tokenizer/configuration files. Keep model caches outside the repository. No API key or hosted inference service is required.

From the repository root:

```sh
python3 -m venv .venv  # Only for initial environment creation.
source .venv/bin/activate
python -m pip install -r 03_tokenizer-and-embeddings/requirements.txt
python -m jupyter lab
```

On Windows, activate with `.venv\Scripts\activate`. Open `03_tokenizer-and-embeddings/article_03_text_to_numbers_smolLM2.ipynb` and select the kernel belonging to that environment. Restart the kernel, run all cells in order, and save to retain the new results.

The recorded run used Python 3.9.6, Torch 2.8.0, Transformers 4.57.6, Tokenizers 0.22.2, and Hugging Face Hub 0.36.2 on CPU. The dependency ranges are not a complete lockfile or a guarantee that every combination has been tested. Each run prints its runtime versions.

## Reproducibility and limitations

Tokenizer, configuration, and weights use revision `93efa2f097d58c2a74874c7e644dbc9b0cee75a2`. The saved run measured an embedding table of shape `(49152, 576)` and five retrieved vectors of shape `(5, 576)`; all assertions passed.

The BPE toy starts with visible characters; the real tokenizer uses byte-level BPE. The training example moves one artificial row toward an invented target and is not language-model pretraining. Displayed real vectors show only four rounded coordinates per token. Equal initial embeddings establish a lookup property rather than contextual understanding. Saved outputs describe one run; rerun to verify them in another environment.

Two environment warnings about LibreSSL and optional notebook progress widgets were omitted from saved outputs. Other diagnostics are retained. Rerunning may display those warnings depending on the environment.

## References

- [Pinned checkpoint configuration](https://huggingface.co/HuggingFaceTB/SmolLM2-135M/blob/93efa2f097d58c2a74874c7e644dbc9b0cee75a2/config.json)
- [GPT-2 tokenizer documentation](https://huggingface.co/docs/transformers/model_doc/gpt2)
- [Subword Units: Sennrich, Haddow and Birch (2016)](https://aclanthology.org/P16-1162/)
