## Stack notes — Data / ML

### Toolchain & commands
- Experiments tracked in {{exp_tracker}}; datasets versioned via {{data_versioning}};
  artifacts live in {{artifact_store}} — never in git. Train/eval: `{{train_cmd}}` /
  `{{eval_cmd}}`.

### Agent failure modes
- Metric claims without seeds — "improved" means: same data version, fixed seeds, and
  the command that reproduces it, or it's noise. Commit what produced the number.
- Test-set leakage: tuning on the eval split, normalizing before the split, dedup across
  splits skipped — check the boundary every time features change.
- Comparing runs across different data slices/versions as if equivalent.
- Notebooks drifting from the module code ({{notebook_policy}}: notebooks explore,
  modules own logic that ships).
- Committing datasets/weights/caches into git — {{artifact_store}} exists for that.

### Verification norm
Proven = the eval command re-run clean on the pinned data version with stated seeds;
regressions in {{key_metrics}} called out, not averaged away.

### Interview add-ons
- Experiment tracker? (infer: wandb/mlflow/tensorboard in deps)
- Dataset versioning + artifact store? (infer: dvc/lakefs configs, bucket refs)
- Notebook policy? (infer: notebooks/ dir vs src/ split)
- Key metrics + the eval command that owns them? (infer: eval scripts, CI)
