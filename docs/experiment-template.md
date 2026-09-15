# Experiment: Short descriptive title

Status: planned / running / complete / stopped

Use one copy per experiment under `reports/`. Replace placeholders with measured values; use "not measured" instead of inventing estimates. Link back to the [research plan](../README.md#research-plan-explicit-versus-latent-reasoning).

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
