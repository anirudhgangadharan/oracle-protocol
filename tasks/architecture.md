# Oracle Protocol — Architecture

## State Space
- **Input**: User decisions (text), predictions (probability 0-1), assumptions, adversarial checks
- **Storage**: Single JSON file in GitHub repo via Contents API v3
- **Output**: Calibration curves, Brier scores, bias pattern detection
- **Constraints**: Single HTML file, zero build step, CDN-only deps, GitHub Pages hosting

## Data Flow
```
User Input → React State → GitHub API PUT (base64 JSON) → Git Commit
GitHub API GET → base64 decode → React State → UI Render
```

## Mathematical Core
- Brier Score = (1/N) * Σ(predicted - actual)² where actual ∈ {0, 0.5, 1}
- Calibration Delta = predicted_probability - actual_outcome_value
- Calibration Curve: bin predictions into 10% buckets, compare predicted vs actual rates

## Key Invariants
- SHA must always reflect latest server state before PUT
- PAT never persisted beyond sessionStorage
- All writes are atomic git commits
