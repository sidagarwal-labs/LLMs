# Learning path: from training a model to choosing experiments

**Status: preparation, not results.** The starter notebook is provided; the models,
simulator, and RL training described here are work to do, not implemented features.

**Do next:** open the [PyTorch starter notebook](../notebooks/00_pytorch_basics.ipynb)
and complete its tensor exercises on CPU. Follow
[the Colab setup and saving instructions](../README.md#colab-first-setup).
Do not install an RL stack or rent a GPU yet.

| Stage | Question | Deliverable | Tracking |
| --- | --- | --- | --- |
| [1. Foundations](#stage-1-foundations) | How do model weights actually learn? | Your tiny GPT, loss curves, and explanations. | [Issue #1](https://github.com/sidagarwal-labs/LLMs/issues/1) |
| [2. Controlled research](#stage-2-controlled-research) | Does writing an inferred rule down improve predictions on unseen problems? | A reproducible answer-only versus explicit-reasoning report. | [Issue #2](https://github.com/sidagarwal-labs/LLMs/issues/2) |
| [3. Reinforcement learning](#stage-3-reinforcement-learning) | Does reward-based training help, and can it improve which experiments a model chooses? | A verified environment and a baseline/SFT/RL comparison. | [Issue #3](https://github.com/sidagarwal-labs/LLMs/issues/3) |

Use completion gates, not a calendar deadline. Each row below can take several
sessions. The longer [README roadmap](../README.md#roadmap-and-resources) is a
reference shelf, not eight prerequisites to finish before doing research.

## What is actually being trained?

- **Stage 1:** initialize a tiny Transformer randomly and update its weights to
  predict the next token.
- **Stage 2:** reuse that architecture and training code. Train fresh models on
  generated observation/answer examples; optionally initialize both variants from
  the same saved checkpoint, but record that choice. One variant also learns to
  produce intermediate steps.
- **Stage 3:** start a separately labeled experiment with a small pretrained open
  model. Supervised fine-tuning (SFT) updates weights from verified examples;
  reinforcement learning (RL) updates weights from rewards for sampled responses.
  Record whether full weights or only low-rank adapters are updated.

At evaluation time, the model uses observations in its context **without weight
updates**. That is different from the training that taught it how to use those
observations. The simulator is not a document index, and this project needs no RAG.
Keep the from-scratch and pretrained results in separate comparison groups.

## Beyond guessing a formula

The scalar rule is a test of the training and evaluation pipeline, not the final
claim. More complexity is useful only when it tests a particular limitation.

| Level | Example | What it tests | When |
| --- | --- | --- | --- |
| 0. Pipeline check | Infer `y = a*x` from a few observations. | Can the generator, tokenizer, model, and checker work together? | Start stage 2 here. |
| 1. Static hidden laws | Infer `y = a*x^p + b` within a declared small family. | Do intermediate steps help generalization to held-out parameters or inputs? | First controlled report. |
| 2. Multiple inputs | Infer `y = a*x + b*z + c*x*z`. | Can the model distinguish the effect of each input? | One follow-up after the first report. |
| 3. Active experiments | Choose the next `(x, z)` to observe, with a fixed number of queries. | Does selecting informative observations outperform fixed or random choices? | Stage 3 extension, after static RL works. |
| 4. Dynamical system | Apply forces to a simulated cart with an unknown spring/drag law, then predict future motion. | Can the model reason about interventions and trajectories, rather than one-step curve fitting? | Later extension, not a prerequisite. |

**Concrete destination:** the model sees a simulated cart's position and velocity.
It can request experiments such as "apply this force for this many steps." The
simulator returns measurements, not its hidden coefficients. After a fixed budget,
the model predicts motion under a new force sequence. A numerical checker scores
those predictions against the simulator. No model-based judge is needed.

Start that extension with one-dimensional, noiseless motion, known mass, bounded
actions, and a tested discrete-time simulator. Validate the simulator against
analytic or independent numerical cases before scoring models. Add noise, partial
observability, or new force-law families **one at a time**. Noise requires
uncertainty-aware evaluation; exact-match scoring will no longer be sufficient.

This is a small system-identification and experimental-design study, not evidence
of discovering new real-world physics. [NewtonBench](https://arxiv.org/abs/2510.07172)
is relevant prior work, so an interactive physics environment by itself is not a
novel contribution. A potential contribution is a careful measurement of whether
RL improves experiment selection under matched budgets.

## Stage 1: foundations

**Goal:** understand the training loop before adding a research task.

### Work in this order

| Session group | Do | Primary resource | Save |
| --- | --- | --- | --- |
| 1. Tensors | Predict shapes; practice indexing, broadcasting, and matrix multiplication. | [PyTorch Tensors](https://docs.pytorch.org/tutorials/beginner/basics/tensorqs_tutorial.html) | Completed sections 1-3 of the starter notebook. |
| 2. Learning weights | Fit `y = 2*x + 1`; plot loss and inspect learned weight/bias. Explain clearing gradients and the optimizer step. | [Autograd](https://docs.pytorch.org/tutorials/beginner/basics/autogradqs_tutorial.html), [Build a Model](https://docs.pytorch.org/tutorials/beginner/basics/buildmodel_tutorial.html), [Optimization](https://docs.pytorch.org/tutorials/beginner/basics/optimization_tutorial.html) | Finish the starter notebook; close [#4](https://github.com/sidagarwal-labs/LLMs/issues/4) only when its criteria pass. |
| 3. Next-token prediction | Build a character bigram model; compare training and validation loss and sample text. Use a documented, appropriately licensed tiny corpus. | [Karpathy: makemore, first lesson](https://www.youtube.com/watch?v=PaCmpygFfXo) | Create `notebooks/01_bigram.ipynb`; work through [#5](https://github.com/sidagarwal-labs/LLMs/issues/5). |
| 4. Attention and GPT | Implement a small decoder-only Transformer, train it, and compare with the bigram model on the same split. | [Karpathy: build GPT](https://www.youtube.com/watch?v=kCc8FmEb1nY); [LLMs from Scratch](https://github.com/rasbt/LLMs-from-scratch) as a reference | Create `notebooks/02_gpt_from_scratch.ipynb`; work through [#6](https://github.com/sidagarwal-labs/LLMs/issues/6). |
| 5. Reliable experiments | Save weights, optimizer state, configuration, training position, and relevant RNG/data-sampling state. Resume a short run. | [PyTorch: saving and loading models](https://docs.pytorch.org/tutorials/beginner/saving_loading_models.html) | A successful resume check and the environment/seed used. Keep weights outside Git. |

Do not copy an entire tutorial and call it your experiment. Credit the source,
rebuild small pieces, change one setting, and explain what changed. If gradients
are opaque, use [the micrograd lesson](https://github.com/karpathy/nn-zero-to-hero)
before continuing.

### Gate before stage 2

- [ ] Each notebook restarts and runs top to bottom.
- [ ] You can explain forward pass, loss, backpropagation, optimizer update, and
  why next-token targets are shifted.
- [ ] You have training and validation curves, not just generated samples.
- [ ] A causal-mask check shows that changing a future token does not change
  earlier-position logits with dropout disabled.
- [ ] Checkpoint/resume works on a small run.
- [ ] You can identify which model code is yours and which was adapted.

**Next action after this gate:** write the stage 2 generator and checker, not a
larger language model or a chat UI. Bigger pretraining and recurrent-depth models
are optional; neither is required to begin the first comparison.

## Stage 2: controlled research

**First question:** with the same model architecture and underlying problems,
does training the model to write the inferred law and calculation before its answer
improve unseen-problem accuracy enough to justify the extra tokens?

### Build and test before training

1. Copy the [experiment template](experiment-template.md) to a new report under
   `reports/`. Write the hypothesis, baseline, success criterion, and budget first.
2. Implement a generator of level-0 problems and an independent answer checker.
   Keep hidden coefficients and expected answers in evaluator metadata, not in
   the model's prompt. Test exact known cases and malformed responses.
3. Before moving to level 1, check **identifiability**: within the declared family,
   do all laws consistent with the observations agree on the requested answer?
   If the target includes naming the law, require a unique canonical law too.
   Otherwise reject/resample or explicitly design an uncertainty task.
4. Define the finite candidate family so a non-neural program can enumerate laws
   consistent with the observations. Use that as a correctness reference and
   baseline. A Transformer need not beat it for the comparison to be informative.
5. Freeze disjoint training/validation/test law assignments and problem examples.
   Canonicalize equivalent rules and check overlap; different seeds do not suffice.
   Keep coefficient-held-out, input-extrapolation, and held-out-form tests separate.
   Use validation only for tuning. Do not mix in test-law solutions during SFT.
6. Produce two training targets for the same problems: **A**, answer only, and
   **B**, generator-verified law/calculation followed by the answer. Parse and score
   the final answer independently of how convincing the explanation sounds.
7. Overfit a tiny training batch as a debugging check, then run a small validation
   pilot. Move beyond level 0 only when both the checker and training pipeline work.

A randomly generated rule is not automatically unseen. Small families repeat;
pretrained models may already know the mathematical form. Even a from-scratch
model can memorize a small training set rather than learn the intended procedure.
State exactly what was held out and what kind of generalization was tested.
[Garg et al.](https://arxiv.org/abs/2208.01066) is useful prior work on training
Transformers to learn function classes in context, not a claim that this project
reproduces their setup.

### Run the comparison

- Reuse the stage 1 training code with a tokenizer that can represent every
  evaluation input; do not make test failures an accidental unknown-token problem.
- Keep initial weights, architecture, underlying training problems, and evaluation
  decoding aligned between A and B. Record any deliberate differences.
- Report a same-problems comparison first, accounting for B's extra tokens. If
  claiming compute efficiency, also run a separately labeled matched-compute
  comparison. Parameter count alone does not establish fairness.
- Compare against the enumerating solver and a simple constant/mean predictor.
  Report final-answer accuracy, sample size, output tokens, latency, failures, and
  variability across seeds when affordable.
- Test one harder dimension first, such as a disjoint coefficient range. Do not
  simultaneously change law form, observation count, and numeric magnitude.

### Gate before stage 3

- [ ] Generator/checker tests pass, including ambiguity and overlap checks.
- [ ] A and B both train; a restart reproduces the evaluation.
- [ ] One completed report shows accuracy and cost, including failed examples.
- [ ] The conclusion distinguishes an empirical result from speculation about
  human-like or hidden reasoning.

**Next action after this gate:** evaluate a small pretrained model on the same
verified task before fine-tuning it. Recurrent depth (variant C in the README) is a
separate optional experiment, not a blocker for RL.

## Stage 3: reinforcement learning

Use a small open instruction-tuned model in a **separate pretrained track**.
Select its checkpoint, revision, license, and size after a memory/runtime pilot.
There is no promise that an arbitrary RL configuration fits free Colab or 8 GB
VRAM: rollouts, context length, gradients, and optimizer state all use memory.

### 3A. First make static reward-based training work

1. Read [Spinning Up: key RL concepts](https://spinningup.openai.com/en/latest/spinningup/rl_intro.html)
   for policy, action, reward, episode, and return. Use it for concepts, not as a
   requirement to install its older training stack.
2. Evaluate the untouched model on validation tasks with a fixed prompt and
   decoding budget. Save predictions, parsed answers, accuracy, and token cost.
3. Train an SFT baseline using verified **training-split** solutions. Use the
   [TRL SFT guide](https://huggingface.co/docs/trl/en/sft_trainer) for the API, but
   choose a tiny configuration rather than running its defaults blindly.
4. Define the reward from the independent checker. Start with binary correctness
   on exact tasks; invalid or non-finite answers receive zero. Correct formatting
   alone must not earn positive reward. Do not evaluate model-supplied code.
5. Continue from that SFT checkpoint with a small GRPO pilot using
   [TRL's GRPO guide](https://huggingface.co/docs/trl/en/grpo_trainer). GRPO samples
   multiple responses and uses their relative rewards to update the policy.
   Inspect whether groups contain any reward variation; all-zero or all-equal
   groups provide no relative learning signal. Fix learnability on validation,
   not by leaking hidden answers.
6. Compare **prompting**, **SFT**, and **SFT then RL**. Add continued SFT under a
   comparable extra-compute budget if claiming RL is better than further SFT.
   Count all generated rollouts, including rejected or incorrect ones.

Test the reward with correct, incorrect, missing, duplicate, malformed, huge, and
non-finite answers before training. Bound input/output sizes and reject unsupported
actions explicitly. Keep evaluator-only state and references out of observations.
For numeric-tolerance tasks, specify the tolerance before running experiments.

Measure held-out prediction accuracy separately from training reward. Read failure
samples for parser exploits, constant guesses, answer leakage, and verbosity that
earns reward without improving predictions.

### 3B. Then learn to choose experiments

Start with level 2's multivariable laws, not the cart simulator. This isolates
experiment selection from simulator complexity.

| Environment component | Proposed starting design |
| --- | --- |
| Hidden state | A law sampled from the frozen split; never exposed in observations. |
| Observation | Allowed input bounds, measurements collected so far, and remaining experiment budget; target inputs in the prediction phase. |
| Action | Request a measurement or end exploration; in the prediction phase, submit final answers. Use a bounded structured format, not arbitrary executable code. |
| Transition | Measurements decrement the budget. Ending exploration or exhausting that budget reveals target inputs and disables further measurements. Specify invalid-action and repeated-query behavior. |
| Episode end | Final submission or a documented turn/token limit; specify how missing predictions are scored. |
| Reward | Accuracy on evaluator-only target points, under a fixed experiment budget. Begin without extra shaping rewards. |
| Logging | Record the action/observation trajectory, token count, experiment count, and final prediction error. Do not feed private evaluator metadata back to the model. |

Reveal target **inputs** before requesting final predictions; their true outputs
remain private. Score inputs distinct from the measured points, using the same
selection rule across policies. Distinguish these within-episode scoring points
from the separate, frozen test-law split. Training rewards must never use
test-law episodes.

Before RL, compare fixed and random experiment schedules and a candidate-elimination
policy that chooses an input where remaining laws disagree. Use the same candidate
family and fitting/answering routine when isolating the effect of query selection.
Then compare the prompted, SFT, and RL policies with identical tool access, action
budgets, and token limits. Plot accuracy versus experiments, not just one reward.

The standard static GRPO example is not an interactive rollout loop. Add and test
multi-turn collection, termination, trajectory rewards, and assistant-token loss
masking before attempting interactive training. Simulator responses are
observations, not policy actions to optimize. Start with a simple local
`reset`/`step` interface; read
[Gymnasium's environment-design guide](https://gymnasium.farama.org/introduction/create_custom_env/).
Consider [OpenEnv](https://github.com/openenv/OpenEnv) only if a framework solves an
actual integration need.

**Next extension:** move to the cart simulator only after active experiment
selection works and the non-LLM baselines are reliable. Hold out entire parameter
settings or dynamics families, not just additional timesteps from training
trajectories. Treat new noise levels as a separate evaluation condition.

### Completion gate and portfolio output

- [ ] Checker tests for static tasks; for the interactive extension, deterministic
  reset/step tests covering budget exhaustion, the exploration-to-prediction
  transition, invalid actions, and hidden-state leakage.
- [ ] A report separating prompting, SFT, and SFT-then-RL results; label 3A static
  and 3B interactive results separately. An unfinished 3B is planned work.
- [ ] Held-out accuracy versus token/experiment cost, with uncertainty or a clear
  small-sample/single-seed limitation.
- [ ] Saved representative failures and a reward-hacking audit.
- [ ] A reproducible configuration, environment versions, code revision, and
  checkpoint provenance. Retain the [$0 default budget](../README.md#compute-and-spending-policy).
- [ ] A short write-up: question, prior work, method, result, limitations, and the
  next experiment. A rigorous negative result is still a valid outcome.

Do not describe the project as an autonomous scientist, a reproduction of
NewtonBench, or evidence of novel physics. The credible claim is the specific
training and generalization behavior your experiment measured.

## End every session with a handoff to yourself

Save your notebook/code and a short note in it:

1. What works, including the latest reproducible command or cell sequence.
2. What you learned or what remains confusing.
3. The **one next action**, linked to the tracking issue.

Use the [learning-blocker template](../.github/ISSUE_TEMPLATE/learning-blocker.md)
when stuck. Do not move to a more complex environment to avoid debugging the
current one.
