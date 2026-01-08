# Orq Evaluate GitHub Action

Run [evaluatorq](https://github.com/orq-ai/orqkit) evaluation files in your CI/CD pipeline.

> **Source Repository**: This action is maintained in the [orqkit monorepo](https://github.com/orq-ai/orqkit). The source code for the CLI and evaluatorq framework lives there.

## Usage

### TypeScript

```yaml
- name: Run evaluations
  uses: orq-ai/cli-evaluate@v1
  with:
    pattern: "**/*.eval.ts"
    orq-api-key: ${{ secrets.ORQ_API_KEY }}
```

### Python

```yaml
- name: Run evaluations
  uses: orq-ai/cli-evaluate@v1
  with:
    pattern: "**/*.eval.py"
    orq-api-key: ${{ secrets.ORQ_API_KEY }}
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `pattern` | Glob pattern for evaluation files (e.g., `**/*.eval.ts` or `**/*.eval.py`) | Yes | - |
| `orq-api-key` | Orq API key for authentication and tracing | No | - |
| `orq-base-url` | Orq API base URL | No | `https://my.orq.ai` |
| `working-directory` | Working directory to run evaluations from | No | `.` |
| `disable-tracing` | Disable OTEL tracing even when API key is set | No | `false` |
| `otel-endpoint` | Custom OTEL endpoint for traces | No | - |

## Outputs

| Output | Description |
|--------|-------------|
| `passed` | Whether all evaluations passed (`true` or `false`) |

## Example Workflow

```yaml
name: Evaluations

on:
  push:
    branches: [main]
  pull_request:

jobs:
  evaluate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Run evaluations
        uses: orq-ai/cli-evaluate@v1
        with:
          pattern: "evaluations/**/*.eval.ts"
          orq-api-key: ${{ secrets.ORQ_API_KEY }}
```

## Tracing

When `orq-api-key` is provided, traces are automatically sent to the Orq platform. You can:
- Use `disable-tracing: true` to disable tracing
- Use `otel-endpoint` to send traces to a custom OTEL collector

## License

MIT - See [LICENSE](LICENSE)

## Related

- [orqkit](https://github.com/orq-ai/orqkit) - Source repository with evaluatorq framework and CLI
- [@orq-ai/evaluatorq](https://www.npmjs.com/package/@orq-ai/evaluatorq) - TypeScript evaluation framework
- [evaluatorq-py](https://pypi.org/project/evaluatorq/) - Python evaluation framework
