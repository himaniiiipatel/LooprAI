This repository contains an Azure Pipelines configuration that automatically detects any Markdown files (`*.md`) in pull requests (PRs). If such files are found, it runs the `markdownlint` tool to check for linting violations. This ensures that all Markdown files in the repository adhere to the project's style guidelines.

1. [Overview](#overview)
2. [Setup Instructions](#setup-instructions)
3. [How the Pipeline Works](#how-the-pipeline-works)
4. [How to Trigger the Linting Process](#how-to-trigger-the-linting-process)
5. [Common Linting Violations and Fixes](#common-linting-violations-and-fixes)
6. [File Structure](#file-structure)

---

The purpose of this CI pipeline is to:
1. Detect `.md` files in a pull request.
2. If `.md` files are found, it runs `markdownlint` to check for common Markdown syntax issues.
3. If any linting violations are detected, the pipeline will fail and provide detailed feedback.
4. If no Markdown files are found, the pipeline exits successfully without running linting.

---

- **Azure DevOps account**: You must have an Azure DevOps account and a project to use this pipeline.
- **Markdown files**: The repository must include `.md` files to be linted.


1. **Create a repository**: If you don’t have an existing repository, create one in Azure Repos.
2. **Add the pipeline configuration**: Create the `.azure-pipelines.yml` file in the root of your repository with the following content (see above).
3. **Commit the changes**: Push the `.azure-pipelines.yml` file to your repository.
4. **Create a pull request**: Once your repository is set up, create a pull request (PR) that includes `.md` files. The pipeline will automatically run and check for linting violations.

---

The pipeline is triggered whenever a PR is opened or updated with changes to the `main` or `release/*` branches. The steps in the pipeline are as follows:

1. **Checkout Code**: The latest version of the code is checked out from the repository.
2. **Detect Markdown Files**: The pipeline scans the PR for changes to `.md` files. If no such files are found, the pipeline exits successfully without running linting.
3. **Run markdownlint**: If Markdown files are detected, `markdownlint-cli` is installed and run against the changed `.md` files.
4. **Report Violations**: Any linting violations will cause the pipeline to fail, with the specific issues reported in the log output.

---

To trigger the linting process:
1. **Open a pull request**: Create or update a PR that modifies or adds `.md` files (e.g., `README.md`, `CONTRIBUTING.md`).
2. **Pipeline Runs Automatically**: The pipeline will automatically detect the changes in the PR and run `markdownlint` on any `.md` files.
3. **Review the Results**: If linting violations are found, the pipeline will fail, and you will receive detailed feedback in the Azure Pipelines log.

---

Here are some common violations that `markdownlint` might detect, along with how to fix them:

- **Violation**: `MD009 - Trailing spaces`
  - **Fix**: Remove any extra spaces at the end of lines.

- **Violation**: `MD010 - Hard tabs`
  - **Fix**: Replace any tabs with spaces (typically 2 or 4 spaces per tab).

- **Violation**: `MD013 - Line length`
  - **Fix**: Break long lines (over 80 characters) into shorter lines.

- **Violation**: `MD001 - Header levels should only increment by one`
  - **Fix**: Ensure that headers follow a logical hierarchy (e.g., `#`, `##`, `###`).

---

The typical file structure of your repository should look like this:

.
├── .azure-pipelines.yml        # Pipeline configuration file
├── README.md                   # Explanation of the pipeline, setup instructions, and common linting violations
├── CONTRIBUTING.md             # Example Markdown file (optional, for testing linting)
├── docs/
│   └── guide.md                # Example Markdown file (optional, for testing linting)
