# Beyond the Stack Notes

Runnable notebooks and small experiments accompanying Beyond the Stack's writing on LLMs and software engineering. Each directory is self-contained and documents its own setup, dependencies, evidence, and limitations.

## Notebooks

| Notebook | Focus |
| --- | --- |
| [What Is an LLM? A Mental Model for Software Engineers](what-is-an-llm/README.md) | A guided walkthrough of tokenisation, transformers, and text generation using a small local model. |
| [Base, Instruct, and Reasoning Models](base-instruct-reasoning/README.md) | A reproducible comparison of a base checkpoint and related post-trained model with thinking disabled and enabled. |

## Local setup

Clone the repository and create one local environment for the current notebooks:

```bash
git clone https://github.com/nabeelahmed/beyond-the-stack-notes.git
cd beyond-the-stack-notes
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r what-is-an-llm/requirements.txt -r base-instruct-reasoning/requirements.txt
```

On Windows, activate the environment with `.venv\Scripts\activate` instead. The optional W&B integration for the LLM walkthrough has its own `requirements-wandb.txt`; it is not needed for either core notebook.

Start JupyterLab from the repository root:

```bash
python -m jupyter lab
```

Then open either notebook listed above. The notebooks use pinned direct dependencies but are not complete lockfiles. Saved outputs document historical runs; package versions, platform, model outputs, and timings can differ on a fresh run.

Model weights, caches, virtual environments, local experiment records, and private inputs belong outside Git. The included examples use synthetic data.
