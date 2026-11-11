# Base, Instruct, and Reasoning Models

Open [base_instruct_reasoning.ipynb](base_instruct_reasoning.ipynb). This original article companion compares a base checkpoint with a related post-trained checkpoint, then runs that post-trained checkpoint with thinking disabled and enabled. The article is planned.

## What runs

Four predefined synthetic tasks run under three conditions (12 recorded generations). Two extra base-model examples, shown separately from the scored tasks, illustrate continuation and few-shot prompting. Three short warm-ups are excluded from the recorded timings. No training, paid APIs, external uploads, or execution of generated code occurs.

The notebook loads one checkpoint at a time. Both model downloads together require approximately 2.6 GB of cache space. Use a GPU for practical execution; CPU is selected automatically when no supported accelerator is available and can be slow. GPU float16 weights alone require roughly 1.2 GB for a 0.6B checkpoint, with additional memory needed for loading and generation. CPU float32 weights alone require roughly 2.4 GB. Leave headroom; these are arithmetic estimates, not measured peak memory.

## Setup and execution

Saved results document Python 3.9.6 and the package baseline in `requirements.txt`. This is an observed direct-dependency baseline, not a full dependency lock or a tested fresh installation on every platform.

```bash
git clone https://github.com/nabeelahmed/beyond-the-stack-labs.git
cd beyond-the-stack-labs/bts_blog/base-instruct-reasoning
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
python -m ipykernel install --user --name bts-model-labels --display-name "Python (BTS model labels)"
python -m jupyterlab base_instruct_reasoning.ipynb
```

On Windows, activate with `.venv\Scripts\activate`. If using the standalone deliverable, navigate to its extracted directory instead of cloning. Select the newly created kernel, restart it, and run all cells in order. The first run downloads the pinned checkpoints and tokenizers into the Hugging Face cache. Set `HF_HUB_CACHE` before starting Jupyter if you need a different location. Keep weights outside the repository.

For a headless run from this directory:

```bash
python - <<'PY'
import nbformat
from nbclient import NotebookClient
path = 'base_instruct_reasoning.ipynb'
nb = nbformat.read(path, as_version=4)
NotebookClient(nb, timeout=3600, kernel_name='bts-model-labels', resources={'metadata': {'path': '.'}}).execute()
nbformat.write(nb, path)
PY
```

Execution overwrites `results.json` with the new run. Copy it first if you want to preserve a previous observation.

## Evidence and interpretation

- `results.json` contains exact inputs, raw continuations, generated token IDs, final answers, settings, and task checks.
- The notebook displays each task’s prompt, expected answer, model response, and separate value and format checks. Thinking-mode views also include the full generated response. The notebook records its environment and includes assertions for row coverage and token accounting.
- JSON formatting and expected-value correctness are distinct checks. If the response contains prose, the checker uses the first decodable JSON object for the separate expected-value check. It does not resolve conflicting objects or claims; inspect those responses manually. The tasks do not measure general model quality.
- Sampling uses temperature 0.6, top-p 0.95, top-k 20 and seed 42 in every condition. Exact output and timing may vary with platform and library versions.
- Direct/base tasks allow 256 new tokens; thinking tasks allow 2,048. The few-shot and continuation examples allow 128. Token-limit hits and unclosed reasoning are reported, not hidden.
- Timings exclude loading, downloads, input preparation, and decoding; they include generation and device synchronization. One run per condition is insufficient for speed claims. Run order is fixed.
- Related checkpoints differ through several training stages. The comparison does not isolate SFT. The base model's pretraining already includes reasoning-related data.
- Thinking-mode text is exposed model output, not verified access to all internal computations. A readable trace does not establish faithfulness.

Optional extensions: collect a larger predefined task set; repeat sampled runs with recorded seeds; repeat warmed timings with alternating mode order; investigate truncation by changing only the output cap. Keep any later evaluation set separate from examples used for development.

See the notebook's reference links for the model cards, training recipe, chat templates, and reasoning-faithfulness research. Qwen3 weights use Apache-2.0 and are not included here; third-party materials retain their licenses.

## Saved-run observations

The saved run started on 11 November 2026 UTC and used MPS and float16. All 12 task records and both extra examples are saved. Expected values were found in 2/4 base responses, 1/4 direct responses, and 4/4 thinking responses, using the first decodable object policy. Strict JSON plus correct values passed in 0/4, 0/4, and 2/4 respectively. Code fences account for several formatting failures. One base response hit the limit; all four thinking traces closed. Read the notebook's observations for contradictory outputs and limitations.

A larger model pilot was stopped because of resource pressure. The final Qwen3 run completed; larger-model pilot outputs are not included in the results. Only the larger-model downloads created for this task were removed to recover disk space.
