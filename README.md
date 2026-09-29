# NoiPA: Multi-Agent System for Data Quality

### From a messy CSV to an explained, verified cleaning process

Administrative datasets can contain missing values disguised as text, inconsistent dates, duplicated records, and conflicting columns. This project explores how specialized AI agents can help diagnose and repair these issues while keeping each transformation open to inspection.

Starting from a CSV, the application produces a **cleaned dataset** and a **quality report** explaining the findings, the actions taken, and the verification results. Python computes the evidence; agents interpret it and generate targeted cleaning functions. Ambiguous findings are flagged for review rather than automatically rewritten.

Provided by **Reply** for the **Machine Learning** group project at **LUISS Guido Carli**, academic year **2025/2026**.

**Student team:** Michele Turco, Mattia Sebastiani, Sofia Bruni.

[Explore the documentation](docs/README.md) · [View the presentation](ReplyProject.pdf) · [Run the application](#setup)

## How it works

The workflow separates three questions: **What is wrong? What can be repaired? Did the repair work?**

![Pipeline from CSV ingestion through validation, remediation, cleaner generation, application, verification, and reporting](images/flow_diagrams/01_pipeline_overview.gv.png)

Validation checks the schema, completeness, formatting, anomalies, relationships between columns, and duplicates. A remediation plan selects the actions to apply. For format repairs, agents generate column-specific Python functions, which are checked before application. The pipeline then verifies the cleaned output and assembles its report.

<details>
<summary><strong>A closer look at the architecture</strong></summary>

![Conceptual architecture showing typed contracts, deterministic evidence, specialized agents, and host-side validation](images/flow_diagrams/02_conceptual_architecture.gv.png)

Pandas builds profiles and measures data-quality signals. Pydantic models define the artifacts exchanged between stages. Specialized agents infer types, assess formatting, generate cleaners, and explain findings; local checks validate generated transformations.

The diagram groups conceptual roles: remediation planning is implemented through deterministic rules. The Streamlit interface and CLI share the stage modules in `src/core/`, `src/tools/`, `src/validation/`, and `src/cleaning/`.

Read the [architecture documentation](docs/architecture.md) for the contracts, agent runtime, and design choices.

</details>

## Setup

Clone or download the repository and open a terminal in its root folder. You need Python with pip and an OpenAI API key. Installation and agent-backed stages require internet access.

**1. Create and activate an environment, then install dependencies.**

<details>
<summary>Windows PowerShell</summary>

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
```

</details>

<details>
<summary>macOS / Linux</summary>

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
```

</details>

**2. Create a private `.env` file in the repository root.**

```dotenv
OPENAI_API_KEY=replace_with_your_key
LOGFIRE_SEND=0
```

Logfire tracing is optional and disabled in this example. Agent calls incur API charges and can include sampled cell values. Generated cleaners execute Python locally; behavioral validation is not a security sandbox. See [configuration and operation](docs/architecture.md#environment-and-operation) for details and the current environment-verification limits.

**3. Launch the application.**

```bash
python -m streamlit run app.py
```

Open the local URL printed in the terminal, select or upload a CSV, and run the pipeline. The interface presents the quality report and cleaned output.

<details>
<summary>Dataset locations and saved results</summary>

The app lists CSVs in `Data/` and currently saves uploads there using their supplied filenames. Use a distinct filename to avoid overwriting an existing file.

On case-sensitive systems, `Data/` and the tracked `data/` folder are different. Create `Data/` and place your chosen CSV there, or use the uploader.

Validation artifacts are stored beside the input in `.validation_cache/`. Cleaned CSVs, generated cleaners, and reports are stored in `.cleaning_cache/<dataset-name>/`.

</details>

<details>
<summary>Run from the command line</summary>

Replace the example path with an existing CSV path:

```bash
python -m src.entrypoints.main "path/to/dataset.csv" --stage validate
python -m src.entrypoints.main "path/to/dataset.csv" --stage clean --reuse-validation
python -m src.entrypoints.main --help
```

See [available stages and configuration](docs/architecture.md#environment-and-operation) for cache reuse, concurrency, and reporting options.

</details>

## Explore the project

| To learn about… | Start here |
|---|---|
| The assignment and project objectives | [Project background](docs/project-background.md) |
| How checks and repairs work | [Validation](docs/validation.md) and [cleaning](docs/cleaning-and-reporting.md) |
| What the experiments showed | [Historical results](docs/experiments-and-results.md) |
| What remains to improve | [Limitations and future work](docs/limitations-and-future-work.md) |
| The project presentation | [Presentation PDF](ReplyProject.pdf) |

The [documentation index](docs/README.md) brings these pages together. Documentation is maintained in `docs/`; GitHub Wiki synchronization is planned.
