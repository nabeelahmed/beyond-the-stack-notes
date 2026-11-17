# How attention uses context

Open [the notebook](article_04_attention_smolLM2.ipynb) to follow a single-head attention calculation, extend it to two heads, and inspect the pretrained SmolLM2-135M model.

## What you’ll explore

- An illustrative three-position example follows queries, keys, values, scores, scaling, causal masking, softmax, and the weighted sum.
- A second head processes the same input. Its output is concatenated with the first head’s output and passed through an output projection.
- Two sentences use the same `' port'` token after different preceding text. A separate sentence changes only the text after that token to check the causal restriction.
- A reconstruction of SmolLM2’s first attention module includes its position processing and grouped-query attention.

The small matrices are invented; the real-model results come from a fixed checkpoint. The comparison shows sensitivity to preceding context, not proof that the model resolves the word’s meaning. The complete block output includes more than attention alone.

## Setup

From the repository root, use the Notes environment:

```sh
python3 -m venv .venv  # Only if creating the environment for the first time.
source .venv/bin/activate
python -m pip install -r 04_attention/requirements.txt
python -m jupyter lab
```

On Windows, activate with `.venv\Scripts\activate`. Open `04_attention/article_04_attention_smolLM2.ipynb`, select the environment’s kernel, restart it, and run the cells in order.

CPU is sufficient. The first run needs internet access to download approximately 270 MB of model weights plus tokenizer and configuration files. Model weights are not included in the repository.

## Reproducibility and limitations

The notebook uses `HuggingFaceTB/SmolLM2-135M`, revision `93efa2f097d58c2a74874c7e644dbc9b0cee75a2`, on CPU with eager attention and caching disabled. Transformers 4.57.6 is pinned because the reconstruction uses version-specific internal helpers. The requirements pin direct dependencies; they are not a complete lockfile.

The saved run completed all 16 code cells. It compares `' port'` (ID `2399`) at zero-based position 5 across the two sentences. The input embeddings match, while the attention-module and first-block outputs differ. The future-only control preserves earlier representations within the stated tolerance. These observations are specific to this experiment.

The reconstruction matched the captured first attention module’s weights and projected output with a maximum absolute difference of `0.0` in the saved run. Results can differ across environments. Displayed vector excerpts are rounded; comparisons use full vectors. Attention weights are not a complete explanation of a prediction or proof of understanding.

## References

- [Attention Is All You Need](https://arxiv.org/html/1706.03762v7)
- [Grouped-Query Attention](https://arxiv.org/abs/2305.13245)
- [Pinned SmolLM2 configuration](https://huggingface.co/HuggingFaceTB/SmolLM2-135M/blob/93efa2f097d58c2a74874c7e644dbc9b0cee75a2/config.json)
- [Transformers 4.57.6 Llama implementation](https://github.com/huggingface/transformers/blob/v4.57.6/src/transformers/models/llama/modeling_llama.py)
