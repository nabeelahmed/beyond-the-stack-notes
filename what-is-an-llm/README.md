# What Is an LLM? A Mental Model for Software Engineers

An original notebook following one synthetic delivery update through a text-generating model.

Open [what_is_an_llm.ipynb](what_is_an_llm.ipynb). Sections 1–7 form a complete walkthrough:

| Section | Demonstration |
| --- | --- |
| 1. You send a prompt | The agreed delivery update and learning question. |
| 2. What is an LLM? | Training versus inference; load the pinned model and inspect its configuration. No training. |
| 3. Text becomes input | The model's tokenizer, token IDs, chat template, and embedding lookup. |
| 4. Using context | Introduce transformers and attention; inspect representation shapes without interpreting individual coordinates. |
| 5. Generating an answer | Cached, manual greedy steps; next-token candidates; a probability chart; full generation and temperature comparisons. |
| 6. Model versus application | Limited output checks and a local notification preview. No message is sent. |
| 7. Engineering implications | Mechanical checks, a run manifest, optional offline W&B tables, and comprehension questions. |

## Prerequisites and setup

Use Git, Python, and a notebook-capable environment. The saved notebook outputs use Python **3.9.6**;
the selected package versions are in `requirements.txt`. This is a tested package baseline rather than
a complete dependency lock or a claim that a fresh installation has been tested. Other Python versions
and platforms need their own compatibility check. No paid model API or GPU is required.

The model is `Qwen/Qwen2.5-0.5B-Instruct`, revision
`7ae557604adf67be50417f59c2c2f167def9a775`. The tokenizer comes from the same revision.
The initial download is approximately 1 GB plus small configuration/tokenizer files. Weights are
cached by Hugging Face outside the repository by default. CPU float32 weights alone occupy about
2 GB; total RAM also includes intermediate tensors and library overhead. Runtime depends on the
machine and is not guaranteed. There are five short full generations, each capped at 64 new tokens,
plus eight manually inspected generation steps and an additional representation-inspection pass.

For a fresh setup:

```bash
git clone https://github.com/nabeelahmed/beyond-the-stack-notes.git
cd beyond-the-stack-notes/what-is-an-llm
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
python -m ipykernel install --user --name bts-what-is-an-llm --display-name "Python (BTS LLM intro)"
python -m jupyterlab what_is_an_llm.ipynb
```

If already cloned, start with the `cd` command. On Windows, activate with
`.venv\Scripts\activate` instead. Select **Python (BTS LLM intro)**, restart the kernel, and run
all cells in order. The notebook uses CPU explicitly for a portable baseline. It does not install
packages or modify an environment. The first `from_pretrained` call downloads the pinned model;
subsequent runs reuse the cache. Set `HF_HUB_CACHE` before launching Jupyter to choose another cache
location. Keep model downloads outside Git.

## Optional W&B tables

The core walkthrough works without W&B. To enable local records, install the optional dependency
and change `LOG_WANDB = False` to `True` in Section 2 before running:

```bash
python -m pip install -r requirements-wandb.txt
```

Section 7 explicitly uses `mode='offline'`. No W&B login or upload is needed. The ignored `wandb/`
directory holds tables containing synthetic inputs/outputs, settings, and measured timings. Do not
replace the synthetic prompt with private data. Uploading and sharing are separate decisions.
This uses W&B Tables rather than an agent tracing integration; it records our observations and does
not expose private reasoning or automatically assess factual accuracy.

## Expected results and limitations

- Prompt text round-trips through its tokenizer; the chat template adds visible role markers.
- Embedding/state shapes and token probabilities come from the selected model, not fabricated data.
- Displayed leading candidates do not sum to one: the omitted vocabulary holds the remaining mass.
- The manual greedy prefix must match built-in greedy generation for the same inputs/settings.
- The five generated answers can differ. They may omit facts, invent details, or reach the token cap;
  these are observations to discuss, not reasons to silently rewrite the saved answers.
- Order-ID/time string checks are limited smoke checks. They do not establish correct meaning,
  cause, tone, one-sentence compliance, or production readiness. Human review is required.
- CPU timing includes the local generation call. It is not TTFT, a hosted-service benchmark, or a
  clean prefill/decode timing comparison. Two sampling seeds are not a quality evaluation.
- No training, retrieval, tools, persistent memory, notification sending, or external model API calls
  are implemented. W&B logging is offline when enabled.

## Running and interpreting the notebook

For your own run, use a restarted kernel and execute the cells in order. Section 7 asserts the
mechanical checks and prints the model revision, runtime versions, device, dtype, and generation
settings. Saved outputs document one historical run; exact wording, probabilities, and timings can
vary.

Optional experiments: change the requested audience to an internal operations log, or change only
the revised time. Run all cells again so that the prompt, generated outputs, and tables agree.

## Sources

- [Qwen model card](https://huggingface.co/Qwen/Qwen2.5-0.5B-Instruct) and its Apache-2.0 model license.
- [Qwen concepts: tokenization and chat templates](https://qwen.readthedocs.io/en/latest/getting_started/concepts.html).
- [Hugging Face causal language modeling](https://huggingface.co/docs/transformers/v4.57.1/en/tasks/language_modeling).
- [Hugging Face generation reference](https://huggingface.co/docs/transformers/v4.57.1/en/main_classes/text_generation).
- [Hugging Face caching explanation](https://huggingface.co/docs/transformers/v4.57.1/en/cache_explanation).
- [Attention Is All You Need — Vaswani et al., 2017](https://arxiv.org/abs/1706.03762).
- [Training language models to follow instructions with human feedback — Ouyang et al., 2022](https://arxiv.org/abs/2203.02155).
- [W&B Tables](https://docs.wandb.ai/models/track/log/log-tables).

The notebook is original companion material, not a copy of the course notebooks. Referenced software,
model weights, and research retain their respective attribution and licensing terms.
