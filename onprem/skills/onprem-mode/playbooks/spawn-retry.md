# Spawn retry

The model-failure path. Use when a subagent spawn failed, timed out, or was rejected, and the next step is choosing a model for the retry.

1. Record what failed: the role, the model attempted, and the error.
2. Is the attempted model allowlisted?
   - No → the rule or the spawn violated the lock. Apply **no-fallback-to-banned**: re-spawn on the role's default allowlisted model, and flag the source for an **audit** run.
   - Yes → continue.
3. Re-spawn on a different allowlisted model for the same role. Up to two retries, three attempts total counting the original. Each retry uses a slug different from the previous attempt.
4. All retries failed → stop. Tell the user which models were tried and that no allowlisted model worked. Never degrade to a banned model. Never pick a model by availability alone.
