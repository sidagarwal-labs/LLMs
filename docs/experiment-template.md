# Experiment: Short descriptive title

Status: planned / running / complete / stopped

Use one copy per experiment under `reports/`. Replace placeholders with measured values; use "not measured" instead of inventing estimates. Link back to the [research plan](../README.md#research-plan-explicit-versus-latent-reasoning) and the relevant stage in the [learning path](learning-path.md). Mark inapplicable fields explicitly.

## Question and hypothesis

- Question:
- Hypothesis and why it might hold:
- One factor being changed:
- Baseline:
- Track: from-scratch / pretrained adaptation
- Success criterion decided before the run:

## Budget and stopping rule

- Provider and hardware:
- Maximum paid spend:
- Maximum duration and training steps/tokens:
- Early stopping or failure conditions:
- Checkpoint location and interval (do not publish private links):
- Resume test completed:
- Paid-instance shutdown plan, if applicable:

## Reproduction

- Date:
- Code commit:
- Notebook or command:
- Configuration:
- Python, PyTorch, and other relevant package versions:
- GPU and driver/runtime details:
- Precision:
- Seeds:
- Checkpoint or base-model identifier and revision:

## Data and evaluation

- Source or generator version and license:
- Dataset size and train/validation/test split:
- Duplicate and equivalent-problem leakage checks:
- Held-out law/parameter assignments versus held-out points within an episode:
- Observation identifiability and handling of ambiguous targets:
- Tokenizer and vocabulary:
- Scratchpad generation and validation, if applicable:
- Answer parsing and correctness checks:
- Familiar-difficulty test:
- Harder or longer-problem test:
- Decoding policy and output limits:
- Evaluation sample count:

## Model and compute controls

- Architecture, parameter count, context length, and batch size:
- Training objective and optimizer:
- Recurrence counts during training and evaluation, if applicable:
- Comparison type: parameter-matched / compute-matched / exploratory
- What is held constant:
- What differs, and why:
- Training tokens, including scratchpads:
- Compute accounting method; distinguish estimated FLOPs from wall-clock time:
- Latency methodology: hardware, warm-up, synchronization, repetitions, and whether tokenization is included:

## Environment and RL controls (when applicable)

- Static prediction / interactive experiment selection / dynamics:
- Simulator version, independent correctness checks, and noise model:
- Model-visible observations versus evaluator-only hidden state and answers:
- Allowed actions, bounds, invalid-action behavior, and episode termination:
- Experiment budget and token budget per episode:
- Reward formula, parser, numeric tolerance, and known-bad output tests:
- Prompting / SFT / SFT-then-RL checkpoint lineage:
- Continued-SFT compute control, if claiming an RL advantage:
- Fixed/random experiment schedules and non-neural solver baseline:
- Rollout collection, assistant-token loss masking, and reward-variation checks:
- Total sampled rollout tokens and environment calls, including failures:
- Reward-hacking audit and training-reward versus held-out-accuracy checks:

## Results

Do not combine incompatible comparison groups in one headline number. Report seed variability when available.

| Variant / seed | Test set / N | Final-answer accuracy | Mean output tokens | Recurrent iterations | Mean latency / problem | Peak GPU memory | Training time | Paid cost |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Baseline | | | | | | | | |
| Experiment | | | | | | | | |

- Plots or small metrics files:
- Successful examples:
- Failure examples:
- Any aborted runs or invalid measurements:
- For interactive tasks: prediction accuracy/error versus experiment count, with
  the same action and token budgets across policies:

## Interpretation

- Did the evidence support the hypothesis?
- What alternative explanations remain?
- Limitations: sample size, seeds, compute mismatch, contamination, or unmeasured costs:
- For scratchpads: did answer accuracy improve, rather than just explanation length?
- For recurrent depth: did extra iterations justify their latency, and did behavior change outside trained iteration counts?
- For RL: did held-out correctness improve, rather than only the reported reward?

## Next step

- Keep, revise, or reject the change:
- Next single-factor experiment:
- Links to related reports or learning-blocker issues:
