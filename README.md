# CodeGuard AI

CodeGuard AI is a Python code review tool that combines static analysis with AI-assisted review to identify security vulnerabilities, performance issues, and code quality problems.

The project uses three specialized review agents, each focused on a different aspect of code analysis. Static analysis is performed first, followed by AI-assisted explanations and recommendations. The results are then combined into a scored Markdown report.

## Features

* Security vulnerability detection
* Performance analysis using Python AST
* Code quality and maintainability checks
* AI-assisted explanations
* Severity-based scoring
* Overall code quality score
* Automated Markdown reports
* Suggested code improvements
* Interactive Gradio interface
* Local AI model inference

## Architecture

```text
Python Code
    |
    v
CodeReviewOrchestrator
    |
    +-------------------+-------------------+
    |                   |                   |
    v                   v                   v
Security Agent   Performance Agent   Quality Agent
    |                   |                   |
    v                   v                   v
Static Scanner      AST Checker        AST Checker
    |                   |                   |
    +-------------------+-------------------+
                        |
                        v
                  Report Agent
                        |
             +----------+----------+
             |          |          |
             v          v          v
          Scoring    Summary    Suggested Fix
```

## Analysis

### Security

The security scanner detects common patterns such as:

* SQL injection
* Command injection
* Hardcoded secrets
* Insecure deserialization
* Weak hashing
* Insecure randomness
* Debug mode

Static findings are passed to the AI model for additional explanation.

### Performance

The Performance Agent uses Python AST analysis to identify:

* Nested loops
* Nested loops over the same collection
* I/O operations inside loops
* Repeated string or sequence concatenation
* `pandas.iterrows()` usage

### Code Quality

The Code Quality Agent checks for:

* Long functions
* Missing function docstrings
* Non-descriptive parameter names
* Bare `except:` clauses
* Repeated code

## Scoring

Each category starts at 100 points. Findings reduce the score according to severity:

| Severity | Penalty |
| -------- | ------: |
| Critical |      30 |
| High     |      18 |
| Medium   |      10 |
| Low      |       4 |

The overall score is weighted as follows:

| Category     | Weight |
| ------------ | -----: |
| Security     |    40% |
| Performance  |    30% |
| Code Quality |    30% |

The numerical score is calculated from static findings and does not depend on the AI-generated text.

## AI Model

The project uses:

```text
deepseek-ai/deepseek-coder-1.3b-instruct
```

through Hugging Face Transformers.

The model is loaded once and shared across the review agents. CUDA is used when available, with CPU as a fallback.

## Technologies

* Python
* PyTorch
* Hugging Face Transformers
* DeepSeek Coder
* Python AST
* Pandas
* Gradio
* Google Colab

## Running the Project

The project is provided as a Jupyter Notebook and is designed to run in Google Colab.

Install the required dependencies:

```bash
pip install -q transformers accelerate torch pandas gradio
```

Then run the notebook cells in order. The final cells launch the Gradio interface.

The interface allows users to:

1. Paste or load Python code.
2. Run the analysis.
3. Review findings and scores.
4. Download the generated report.
5. Generate suggested code improvements.

## Limitations

CodeGuard AI is an assisted code review tool rather than a replacement for professional security scanners or human review.

The current implementation relies partly on pattern-based detection, so false positives and missed vulnerabilities are possible. Performance findings are based on code structure rather than runtime benchmarking, and generated code improvements are not automatically tested.

## Disclaimer

Findings and suggested fixes should be reviewed and tested before being applied to production code.
