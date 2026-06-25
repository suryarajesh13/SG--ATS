 SG-ATS

SG-ATS is an AI-powered resume checker that evaluates a PDF resume against role-specific rubrics and a general-purpose rubric. It is designed for Singapore-focused job applications, while also supporting a broader general resume review mode for any industry.

## Features

- General resume checking for any role.
- Technical resume checking with a combined **Software / IT Jobs** rubric.
- Singapore-specific rubrics for:
  - Accounting
  - Audit
  - HR
  - Banking
- Optional target job title input for more focused evaluation.
- Resume parsing from PDF into structured JSON.
- Optional GitHub profile enrichment for technical resumes.
- Structured scoring output with evidence, strengths, improvement areas, bonus points, and deductions.
- CSV export in development mode.
- Interactive mode for users who want to choose the resume type before evaluation.

## Available Role Types

The application currently supports these role types:

- `general`
- `software`  → Software / IT Jobs
- `accounting_sg`
- `audit_sg`
- `hr_sg`
- `banking_sg`

## Project Structure

```text
SG-ATS/
├── score.py
├── interactive.py
├── evaluator.py
├── pdf.py
├── github.py
├── models.py
├── transform.py
├── prompt.py
├── prompts/
│   ├── template_manager.py
│   └── templates/
│       ├── resume_evaluation_criteria.jinja
│       ├── resume_evaluation_criteria_general.jinja
│       ├── resume_evaluation_criteria_accounting_sg.jinja
│       ├── resume_evaluation_criteria_audit_sg.jinja
│       ├── resume_evaluation_criteria_hr_sg.jinja
│       ├── resume_evaluation_criteria_banking_sg.jinja
│       ├── resume_evaluation_system_message.jinja
│       ├── resume_evaluation_system_message_general.jinja
│       └── resume_evaluation_system_message_sg.jinja
└── requirements.txt
```

## How It Works

1. A PDF resume is loaded and parsed into structured resume JSON.
2. If the selected role is `software`, GitHub data may be pulled from the resume's GitHub profile link.
3. The resume is converted into text context for the evaluator.
4. A role-specific Jinja template builds the evaluation prompt.
5. The LLM returns structured evaluation output.
6. Results are printed with category scores, evidence, strengths, and areas for improvement.

## Installation

### 1. Clone the repository

```bash
git clone https://github.com/suryarajesh13/SG-ATS.git
cd SG-ATS
```

### 2. Create a virtual environment

```bash
python -m venv .venv
```

Activate it:

**Windows**
```bash
.venv\Scripts\activate
```

**macOS / Linux**
```bash
source .venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

## Configuration

Review and update these files as needed:

- `config.py`
- `prompt.py`
- `providers.py`
- Any environment variables required by your chosen LLM provider

If you use Gemini, Ollama, or another supported provider, make sure the model name and credentials are configured before running the app.

## Usage

### Interactive mode

This is the easiest mode for most users.

```bash
python interactive.py
```

The app will ask you to:
- Choose the type of resume to evaluate.
- Enter the PDF file path.
- Enter an optional target job title.
- Choose an LLM model or accept the default.

### CLI mode

General resume evaluation:

```bash
python score.py path/to/resume.pdf --role-type general
```

Software / IT evaluation:

```bash
python score.py path/to/resume.pdf --role-type software
```

Accounting evaluation:

```bash
python score.py path/to/resume.pdf --role-type accounting_sg
```

Audit evaluation:

```bash
python score.py path/to/resume.pdf --role-type audit_sg
```

HR evaluation:

```bash
python score.py path/to/resume.pdf --role-type hr_sg
```

Banking evaluation:

```bash
python score.py path/to/resume.pdf --role-type banking_sg
```

With a target job title:

```bash
python score.py path/to/resume.pdf --role-type general --target-job-title "Data Analyst"
```

With a custom model:

```bash
python score.py path/to/resume.pdf --role-type software --model gemini-1.5-flash
```

## Output

The evaluator prints:

- Overall score
- Detailed category scores
- Evidence for each category
- Bonus points
- Deductions
- Key strengths
- Areas for improvement

In development mode, evaluation results can also be appended to CSV files for tracking.

## Role Logic

- `general` uses a broad professional resume rubric.
- `software` uses the main technical rubric and may include GitHub enrichment.
- `accounting_sg`, `audit_sg`, `hr_sg`, and `banking_sg` use Singapore-focused structured score dictionaries.

## Notes

- `resume_evaluation_criteria_it_sg.jinja` is no longer needed if you have merged IT into `software`.
- Make sure the file is named `resume_evaluation_criteria_general.jinja` exactly.
- If you use the updated merged version, remove outdated references to `it_sg` from old documentation and scripts.

## Example Commands

```bash
python interactive.py
python score.py ./samples/resume.pdf --role-type general
python score.py ./samples/resume.pdf --role-type software --target-job-title "Backend Engineer"
python score.py ./samples/resume.pdf --role-type banking_sg --model gemini-1.5-pro
```

## Troubleshooting

- **File not found**: Check the PDF path.
- **Template not found**: Verify the prompt template filenames in `prompts/templates/`.
- **Model/provider errors**: Confirm your model configuration and credentials.
- **No GitHub data loaded**: Ensure the resume includes a valid GitHub profile URL.

## Contributing

See `CONTRIBUTING.md` for contribution guidelines.

## License

This project is licensed under the terms of the repository's `LICENSE` file.
