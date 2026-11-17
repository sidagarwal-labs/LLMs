# LLMs

> **Research goal: Explicit versus latent reasoning in small Transformers under constrained compute.**

A learning-in-public project: understand PyTorch, build a small GPT from scratch, investigate when written reasoning steps or additional internal computation help, and finish with a small reinforcement-learning (RL) environment.

**Starting point:** some Python experience, no PyTorch experience, and a limited personal compute budget. **Current status:** Milestone 1 started; no trained models or benchmark results yet. See [Plan and progress](#plan-and-progress).

This is an educational and experimental project, not a claim of reproducing ChatGPT-level capability.

## Plan and progress

The work is organized into three milestones. Each has a pinned tracking issue with "done when" criteria. Start the next milestone only when the current one is done.

**Follow the [guided learning path](docs/learning-path.md)** for ordered sessions, exercises, completion gates, and the next action at each stage. The longer resource list below is a reference, not a requirement to finish every tutorial.

| Milestone | Goal | Roadmap steps | Tracking |
| --- | --- | --- | --- |
| 1. Foundations (**current**) | Learn PyTorch and train a tiny GPT from scratch. | 0–2 | [#1](https://github.com/sidagarwal-labs/LLMs/issues/1) · [milestone](https://github.com/sidagarwal-labs/LLMs/milestone/1) |
| 2. Hidden-law world | Test whether writing down an inferred law helps a tiny Transformer more than answering directly. Recurrent depth is optional. | 5; 6 optional | [#2](https://github.com/sidagarwal-labs/LLMs/issues/2) · [milestone](https://github.com/sidagarwal-labs/LLMs/milestone/2) |
| 3. RL environment | Compare prompting, supervised fine-tuning (SFT), and SFT followed by group relative policy optimization (GRPO). Then extend to choosing experiments under a fixed budget. | 7 | [#3](https://github.com/sidagarwal-labs/LLMs/issues/3) · [milestone](https://github.com/sidagarwal-labs/LLMs/milestone/3) |

Roadmap steps 3–4 (a larger language model and a chat model) are optional. Milestones 2–3 only need step 3's checkpoint and resume practice.

**Why invented laws?** They let us control the task, define disjoint law/problem splits, and check noiseless answers without a model-based judge. Random generation alone does not prevent memorization: small families repeat, and pretrained models may know the mathematical forms. Report exactly what was held out.

**Is this just guessing a formula?** That is the warm-up. The [complexity ladder](docs/learning-path.md#beyond-guessing-a-formula) leads from scalar rules to multiple inputs, active experiment selection, and eventually predicting a simulated cart's motion under unfamiliar dynamics. [NewtonBench](https://arxiv.org/abs/2510.07172) is relevant prior work on interactive law discovery. Our proposed question is narrower: can training improve useful experiment selection under matched budgets? These extensions are planned, not implemented.

## Start here: the first learning session

**Use Google Colab. No local installation, paid subscription, API key, or GPU purchase is needed to begin.**

1. Open the official [PyTorch Tensors tutorial](https://docs.pytorch.org/tutorials/beginner/basics/tensorqs_tutorial.html) and select **Run in Google Colab**. Start with a CPU runtime; tensor fundamentals do not need a GPU.
2. Work through the examples actively: predict tensor shapes, change values, and explain the results in your own words.
3. Open the [starter notebook in Colab](https://colab.research.google.com/github/sidagarwal-labs/LLMs/blob/main/notebooks/00_pytorch_basics.ipynb). It contains exercise prompts only; write the code and notes yourself. Link the tutorial you used. This step is tracked in [#4](https://github.com/sidagarwal-labs/LLMs/issues/4).
4. Complete the [first-session checklist](#first-session-checklist), then follow [the GitHub saving workflow](#saving-and-reopening-your-work).
5. When stuck, share the notebook link and a small reproducible example using the [learning-blocker issue template](https://github.com/sidagarwal-labs/LLMs/issues/new?template=learning-blocker.md).

Do not try to finish all the resources below before writing code. Complete one small milestone at a time.

## Roadmap and resources

The order puts fundamentals before GPT and a controlled answer-only versus explicit-reasoning experiment before reinforcement learning. Recurrent depth is optional. [Plan and progress](#plan-and-progress) groups these steps into three milestones.

| Step | What to learn or build | Starting resources | Evidence of completion |
| --- | --- | --- | --- |
| 0. PyTorch foundations | Tensors, shapes, matrix multiplication, gradients, neural-network layers, and the training loop. | [PyTorch: Learn the Basics](https://docs.pytorch.org/tutorials/beginner/basics/intro.html). For backpropagation intuition, use the micrograd lesson in [Karpathy's Zero to Hero](https://github.com/karpathy/nn-zero-to-hero). | An annotated notebook; a tiny model whose training loss decreases; an explanation of forward pass, loss, backward pass, and optimizer update. |
| 1. First language model | Character encoding, next-token prediction, a bigram model, sampling, and train/validation splits. | The first makemore lesson in [Zero to Hero](https://github.com/karpathy/nn-zero-to-hero). | A reproducible baseline, held-out loss, generated samples, and notes on what it cannot learn. |
| 2. GPT from scratch | Causal self-attention, multiple heads, positional information, feed-forward layers, residual connections, and normalization. | [Let's build GPT: from scratch, in code, spelled out](https://www.youtube.com/watch?v=kCc8FmEb1nY). Use [Raschka's LLMs from Scratch](https://github.com/rasbt/LLMs-from-scratch) as the structured reference. | A small decoder-only Transformer implemented and trained from random initialization; loss curves and samples; checks for causal masking and tensor shapes. |
| 3. A better small LM | Tokenization, batching, checkpoint/resume, and controlled scaling. Start small before considering roughly 10-30M parameters. | The tokenizer lesson in [Zero to Hero](https://github.com/karpathy/nn-zero-to-hero); [LLMs from Scratch](https://github.com/rasbt/LLMs-from-scratch); a manageable subset of [TinyStories](https://huggingface.co/datasets/roneneldan/TinyStories). | A documented data split and tokenizer, measured runtime/memory, a resumed run, and a repeatable evaluation. |
| 4. Mini chat model | Conversation formatting, user/assistant roles, supervised fine-tuning (SFT), assistant-token loss masking, and generation stopping. | Instruction fine-tuning material in [LLMs from Scratch](https://github.com/rasbt/LLMs-from-scratch). Later, inspect [nanochat](https://github.com/karpathy/nanochat) as an end-to-end reference. | A simple notebook chat loop and a held-out set of narrow instruction-following tasks. Document failures, not just good conversations. |
| 5. Explicit reasoning | Answer-only versus intermediate-step training, answer verification, and optional inference-time sampling. | [Build a Reasoning Model from Scratch](https://github.com/rasbt/reasoning-from-scratch), which starts from a pretrained model. Apply the concepts to small, generated tasks in the from-scratch track. | A comparison on unseen problems using final-answer accuracy, output length, and latency. Longer explanations alone do not count as progress. |
| 6. Recurrent-depth reasoning | Share Transformer-block weights across repeated internal updates. Train for the iteration counts being studied before testing depth generalization. | [Universal Transformers](https://arxiv.org/abs/1807.03819); [Scaling up Test-Time Compute with Latent Reasoning](https://arxiv.org/abs/2502.05171); [Huginn research code](https://github.com/seal-rg/recurrent-pretraining). | Accuracy-versus-iterations and accuracy-versus-latency plots, with parameter-matched and compute-matched baselines reported separately. |
| 7. RL and further experiments | Learn policy-gradient basics, then correctness-reward RL; investigate group-relative policy optimization (GRPO) after the supervised baseline and verifier work. | Reinforcement-learning material in [Reasoning from Scratch](https://github.com/rasbt/reasoning-from-scratch); [TRL's GRPOTrainer](https://huggingface.co/docs/trl/main/en/grpo_trainer) for [Milestone 3](#milestone-3-a-hidden-law-rl-environment). | A controlled comparison against SFT, including rollout cost, answer accuracy, and checks for reward exploitation. |

### Two clearly labeled tracks

- **From-scratch track:** implement the model, initialize its weights randomly, and train it yourself. This is the main learning and architecture-research track.
- **Pretrained track (Milestone 3; optional earlier):** adapt an existing small model. Record the model, revision, license, and fine-tuning method. Do not describe this as pretraining a model from scratch.

A tiny story model will not become a broadly knowledgeable assistant just by learning conversation formatting. The two tracks answer different questions and should not be mixed in results tables.

The linked Huginn and nanochat projects are references, not beginner launch recipes. Huginn's full training is far beyond this project's budget; nanochat's advertised speedrun targets an 8-GPU H100 node. Do not run those defaults as a first experiment.

## Colab-first setup

### What belongs where

| Place | Purpose |
| --- | --- |
| GitHub | The source of truth for notebooks, reusable code, configurations, small plots, and written results. |
| Colab runtime | Temporary compute for interactive learning and training. Installed packages, variables, and local files are not permanent. |
| Google Drive or another explicitly chosen persistent store | Private, persistent training checkpoints and larger artifacts. Check storage capacity before a long run. |
| Local PC, later | Optional development and training; not required for the first Colab milestones. |

Google's [Colab FAQ](https://research.google.com/colaboratory/faq.html) documents availability and runtime limits. Free GPU access is not guaranteed or unlimited. Student offers may vary by eligibility; do not assume a particular GPU or allowance.

### Saving and reopening your work

1. In your own notebook, use **File > Save a copy in Drive** if you want a Drive working copy.
2. To publish a milestone, select **File > Save a copy to GitHub**. Authorize GitHub access in Colab if prompted; never paste credentials into a notebook.
3. Select repository **`sidagarwal-labs/LLMs`**, branch **`main`**, and a descriptive file path such as `notebooks/00_pytorch_basics.ipynb`. Save with a meaningful commit message.
4. Review what will be public. Remove credentials, private Drive links, personal data, and sensitive cell outputs. Keep useful small plots and explanations.
5. Open [the repository](https://github.com/sidagarwal-labs/LLMs) and confirm the file and latest changes actually appear.
6. For the next session, use **File > Open notebook > GitHub** in Colab and select this repository. Reopen the latest version rather than an old tab. Run cells from the top.

**Saving a notebook is not the same as saving a training checkpoint.** Colab's GitHub action saves that notebook, not the entire runtime filesystem. It is also not continuous, bidirectional Git synchronization.

The starter notebook's direct Colab link is:

```text
https://colab.research.google.com/github/sidagarwal-labs/LLMs/blob/main/notebooks/00_pytorch_basics.ipynb
```

It contains prompts and empty cells, not solutions. Model code and completed exercises are intentionally left for the learning milestones.

If the save-to-GitHub option is unavailable, download the notebook through **File > Download > Download .ipynb**, then upload it with GitHub's **Add file > Upload files**. Review outputs before either method.

More detail: Google's [Using Colab with GitHub guide](https://colab.research.google.com/github/googlecolab/colabtools/blob/main/notebooks/colab-github-demo.ipynb).

### Runtime and dependency habits

- Start with CPU for PyTorch basics. For training, use **Runtime > Change runtime type** and select an available GPU only when needed.
- Begin with Colab's provided PyTorch environment. Do not blindly upgrade every package; install missing dependencies only when a lesson needs them.
- Record the Python/PyTorch versions, actual GPU, configuration, and seed alongside each experiment. Colab environments can change between sessions.
- Before considering a notebook reproducible, restart its runtime and run all cells in order. Do not rely on variables created by earlier, unrecorded execution.
- For longer training, save checkpoints periodically to persistent storage and test resuming a short run first. A resumable checkpoint needs optimizer state and training progress as well as model weights; reproducible continuation also needs relevant RNG/data-sampling state.
- Disconnect the runtime when finished. Never use keep-alive workarounds to bypass Colab limits.

## First-session checklist

Keep this first session small. These are exercises to implement, not a requirement to understand Transformers yet.

- [ ] Open the starter notebook and link the tutorial used.
- [ ] Create a tensor with shape `(2, 3)`; inspect its shape, data type, and device.
- [ ] Practice indexing, reshaping, and broadcasting. Explain one operation that fails because the shapes do not match.
- [ ] Multiply a `(2, 3)` matrix by a `(3, 2)` matrix; predict and verify the output shape.
- [ ] Add a short "What I learned / What confused me" section.
- [ ] Restart the runtime, run the notebook top to bottom, save to GitHub, and verify the saved version.

**Next sessions:** follow [Autograd](https://docs.pytorch.org/tutorials/beginner/basics/autogradqs_tutorial.html), [Build a Model](https://docs.pytorch.org/tutorials/beginner/basics/buildmodel_tutorial.html), and [Optimization](https://docs.pytorch.org/tutorials/beginner/basics/optimization_tutorial.html). Fit a tiny regression problem such as `y = 2x + 1`, inspect the loss, and explain why gradients must be cleared between optimizer steps. Then move to the bigram language model.

If the math feels opaque, pause for Karpathy's micrograd lesson rather than trying to memorize training-loop code.

## Research plan: explicit versus latent reasoning

**Main question:** under a small compute budget, when do written intermediate steps help more than additional internal recurrent computation?

Here, **explicit reasoning** means generating intermediate solution tokens. **Latent recurrent-depth reasoning** means updating hidden states through repeated, weight-shared blocks without emitting a text token for each internal update. Ordinary Transformers also compute in hidden states; the distinguishing feature here is recurrence in depth, not the mere presence of hidden states.

Neither a plausible written explanation nor extra internal iterations proves human-like reasoning.

### First controlled experiment: the hidden-law world

This is Milestone 2 ([#2](https://github.com/sidagarwal-labs/LLMs/issues/2)). Use generated problems with exact, independently checked answers. Each problem samples a hidden law from a small family, for example `y = a·x^p + b` with small integers, shows several `(x, y)` observations, and asks for `y` at a new `x`.

A model trained from scratch has no pretrained outside knowledge, but can still memorize training examples or learn shortcuts. Training updates its weights across problems; using observations for a new prediction happens in context without weight updates (compare [Garg et al., 2022](https://arxiv.org/abs/2208.01066)). Check that observations identify the requested answer within the declared family, and a unique canonical law if naming it is part of the target. Begin with `p = 1` and `b = 0` as a pipeline check. Evaluate held-out parameters, input extrapolation, and held-out forms separately. The [stage 2 guide](docs/learning-path.md#stage-2-controlled-research) includes a non-neural solver baseline and leakage checks. Avoid scraped datasets and paid model-generated training data for this first study.

| Variant | Architecture | Training target |
| --- | --- | --- |
| A: answer-only baseline | Standard fixed-depth Transformer with distinct block weights. | Final answer only. |
| B: explicit scratchpad | The same standard architecture as A. | The law and its evaluation steps, produced by the generator, followed by the final answer. |
| C: recurrent depth (optional follow-up) | A Transformer with a weight-shared block repeated for a controlled number of iterations. | Final answer only. |

For C, begin with a small fixed number of iterations. Later, investigate training with variable iteration counts and testing beyond the trained range. More iterations are a hypothesis to test, not a guaranteed improvement.

### Make the comparison fair

- Generate disjoint train, validation, and test problems. Different random seeds alone do not guarantee no duplicates; check overlap and prevent equivalent-problem leakage.
- Use the same underlying problems, tokenizer, answer checker, and documented decoding policy. Produce B's scratchpads from a verified procedure rather than trusting unverified generated explanations.
- Keep familiar-difficulty evaluation separate from harder, length-generalization tests. Do not tune on the held-out test set.
- Run **parameter-matched** and **compute-matched** comparisons separately. Report actual parameter counts, training tokens, and estimated FLOPs or clearly labeled measured GPU time. These quantities are not interchangeable.
- Include scratchpad tokens in B's training and inference cost. Count repeated block applications in C's cost. Weight sharing reduces parameters but does not make extra computation free; training activation memory can also grow with iteration count.
- Measure latency on the same hardware and record generation limits. For GPU timing, account for asynchronous execution and use warm-up runs.
- Use multiple seeds when the budget permits. Label a single small run as exploratory, not a conclusive result.

Record final-answer accuracy, evaluation size, output tokens, recurrence count, latency, peak GPU memory, training time, and monetary cost. Save failures as well as successes.

Use the [experiment report template](docs/experiment-template.md) before launching a run so the hypothesis and budget are explicit.

### Ideas after the baseline works

Change one factor at a time:

- Learned position embeddings versus rotary position embeddings (RoPE).
- LayerNorm versus RMSNorm.
- Context length, tokenizer vocabulary, and model width/depth.
- Fixed versus variable recurrent iteration counts.
- Answer-only versus short versus detailed scratchpads.
- Greedy decoding versus self-consistency sampling, counting all sampled tokens.
- SFT versus verifiable-reward RL after the answer checker is tested.

Do not begin with distributed training, a large chat UI, a complex agent framework, or a full RL stack. Those add moving parts without answering the first research question.

### Milestone 3: a hidden-law RL environment

Milestone 3 ([#3](https://github.com/sidagarwal-labs/LLMs/issues/3)) uses the pretrained track: it adapts a small open instruction-tuned model rather than the from-scratch model and reports results separately. Follow the [stage 3 guide](docs/learning-path.md#stage-3-reinforcement-learning) for static training first, then interactive experiment selection.

- **Environment:** the Milestone 2 generator presents observations from a hidden law. Laws can be richer than in Milestone 2, such as a made-up force law `F = c·m1·m2 / d^p` with an unusual exponent. The model states a law and predicts values at held-out query points.
- **Reward:** computed by an independent checker from prediction accuracy. Invalid output receives zero; formatting alone must not earn positive reward. Before training, test known-bad, malformed, and non-finite outputs.
- **Comparison:** prompting, SFT on generator-verified training solutions, then GRPO starting from the SFT checkpoint with [TRL's GRPOTrainer](https://huggingface.co/docs/trl/en/grpo_trainer). Compare continued SFT at a similar extra-compute budget before claiming an RL advantage. Judge success by held-out accuracy, with held-out law forms reported separately, rather than by training reward.
- **Reward-hacking audit:** read samples. Look for parser exploits, degenerate guesses, or reward rising while held-out accuracy does not.
- **Interactive extension:** let the model choose experiments within a fixed budget before predicting. Compare fixed, random, and candidate-elimination experiment selection with the same tool access and budgets. Multi-turn rollout integration must be tested separately from static GRPO; a dynamical simulator comes later.

The spending policy below still applies: try free Colab or the local GPU first.

## Compute and spending policy

**Initial paid-compute budget: $0.** Start with free Colab and existing hardware; no paid API is required.

The available local machine has an **RTX 2070 with 8 GB VRAM**, a **Ryzen 7 2700X**, and approximately **16 GB system RAM**. It is a useful fallback for small-model experiments, not a requirement to leave Colab now. Model size alone does not determine memory use; context length, batch size, optimizer state, and precision matter too.

Before any paid run:

1. Make the experiment work on a tiny subset, including evaluation and checkpoint/resume.
2. Benchmark a short run and estimate the full runtime. Do not assume advertised throughput applies to this model.
3. Set an explicit dollar cap, maximum duration, and stopping criterion.
4. Include persistent storage and data-transfer charges in the estimate.
5. Configure provider-side automatic shutdown where available and a training-duration limit. Budget alerts alone may not stop billing.
6. Confirm the rented instance is actually stopped or terminated afterward; a completed script does not necessarily stop GPU charges.

Do not buy new hardware until repeated measurements demonstrate a real limitation.

## Keeping the GitHub project useful

The starter includes this overview, the [guided learning path](docs/learning-path.md), a [starter notebook](notebooks/00_pytorch_basics.ipynb), an [experiment report template](docs/experiment-template.md), a [learning-blocker template](.github/ISSUE_TEMPLATE/learning-blocker.md), and [Git ignore rules](.gitignore). It intentionally has no training framework or dependency lockfile yet. Progress is tracked with [GitHub milestones](https://github.com/sidagarwal-labs/LLMs/milestones) and the pinned issues listed in [Plan and progress](#plan-and-progress).

Add structure only as it becomes useful:

| Future location | What to put there |
| --- | --- |
| `notebooks/` | Numbered learning notebooks with explanations, exercises, and small outputs. |
| `src/llm_lab/` | Reusable model, training, and evaluation code extracted from working notebooks. |
| `configs/` | Small, reproducible experiment configurations. |
| `tests/` | Fast checks for tokenization, causal masking, loss masking, answer verification, and checkpoint/resume. |
| `reports/` | Completed experiment reports, compact metrics, plots, and representative failures. |

For every milestone, publish:

- What was implemented and which parts came from a tutorial or pretrained model.
- How to reproduce it, including the code revision, environment, configuration, data provenance, and seed.
- The result and baseline, with a plot or compact table where useful.
- What failed, what remains uncertain, and the next experiment.

Keep datasets, weights, credentials, and large run artifacts out of ordinary Git commits. The ignore rules help with **local Git operations**, but they do not remove sensitive notebook outputs or protect uploads made directly through Colab/GitHub.

Credit tutorials and retain required notices when adapting their code. Check dataset and model licenses independently; for example, [TinyStories](https://huggingface.co/datasets/roneneldan/TinyStories) lists its own dataset license. Publicly accessible material is not automatically unrestricted.

## Getting help when stuck

[Open a learning-blocker issue](https://github.com/sidagarwal-labs/LLMs/issues/new?template=learning-blocker.md), or share the same information in our next conversation:

1. The milestone and tutorial section.
2. The GitHub notebook link and the failing cell or smallest code example.
3. What you expected versus what actually happened.
4. The full traceback, with credentials and personal information removed.
5. The runtime type, available GPU if relevant, and package versions.
6. What you tried, and whether restarting and running all cells reproduces the issue.

Conceptual questions are welcome too: state the explanation you currently believe and where it stops making sense.
