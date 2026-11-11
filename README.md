# Beyond the Stack Notes

Runnable notebooks and small experiments accompanying Beyond the Stack's writing on LLMs and software engineering. Each directory is self-contained and documents its own setup, dependencies, evidence, and limitations.

## Notebooks

| Notebook | Focus |
| --- | --- |
| [What Is an LLM? A Mental Model for Software Engineers](what-is-an-llm/README.md) | A guided walkthrough of tokenisation, transformers, and text generation using a small local model. |
| [Base, Instruct, and Reasoning Models](base-instruct-reasoning/README.md) | A reproducible comparison of a base checkpoint and related post-trained model with thinking disabled and enabled. |

## Getting started

Clone the repository, then follow the README in the notebook directory you want to run:

```bash
git clone https://github.com/nabeelahmed/beyond-the-stack-notes.git
cd beyond-the-stack-notes
```

The notebooks use pinned direct dependencies but are not complete lockfiles. Saved outputs document historical runs; package versions, platform, model outputs, and timings can differ on a fresh run.

Model weights, caches, virtual environments, local experiment records, and private inputs belong outside Git. The included examples use synthetic data.
