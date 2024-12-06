# SecureCodeBox Penetration Testing Pipeline



This repository contains a GitHub Actions workflow designed as a proof of concept (POC) for automating penetration tests using [SecureCodeBox](https://www.securecodebox.io/). The pipeline demonstrates the practical application of Kubernetes, Helm, and SecureCodeBox scanners to conduct authenticated scans (advanced ZAP scan) on the Juice-Shop demo application.

## Features
- **Automated Deployment**: Deploys a Kubernetes Kind cluster, SecureCodeBox operator, and Juice-Shop target.
- **Penetration Testing**: Uses the ZAP-Advanced scanner to perform a full authenticated scan.
- **Scan Monitoring**: Monitors scan status and retrieves detailed findings.
- **Baseline Comparison**: Compares scan results with a baseline to detect regressions or improvements.
- **Continuous Integration**: Saves findings and updates reports directly to the repository.

## Workflow Overview
The workflow is triggered on:
- **Push Events**: When changes are pushed to the `main` branch.
- **Manual Dispatch**: Using the `workflow_dispatch` event.

### Jobs and Steps
1. **Set Up Kubernetes Cluster**: Deploys a Kind cluster and configures `kubectl`.
2. **Install Helm**: Installs Helm for managing SecureCodeBox components.
3. **Deploy SecureCodeBox**: Installs the SecureCodeBox operator.
4. **Deploy Juice-Shop**: Sets up the Juice-Shop demo target application.
5. **Deploy ZAP-Advanced Scanner**: Installs the ZAP-Advanced scanner.
6. **Initiate Scan**: Runs a ZAP scan against the Juice-Shop target.
7. **Monitor Scan Progress**: Waits for the scan to complete.
8. **Retrieve Results**: Downloads scan findings and generates summary reports.
9. **Baseline Comparison**: Compares the findings with a baseline to track issues and improvements.
10. **Save Reports**: Commits findings and updates baseline reports in the repository.

### Pipeline logic 

The following figure depicts the underlying logic of the pipeline, highlighting how it handles various scenarios it may encounter.

![image](https://github.com/user-attachments/assets/19b14d1a-45ea-4f17-81fc-5241c41c7f54)

## Environment Variables
The workflow uses the following environment variables:
- `TARGET_APP`: Name of the target application (e.g., `juice-shop`).
- `TARGET_APP_HELM`: Helm chart location for the target app.
- `SCANNER`: Name of the scanner (e.g., `zap-advanced`).
- `SCANNER_HELM`: Helm chart location for the scanner.
- `SCAN_NAME`: Name of the scan to be initiated.

## Prerequisites
- A GitHub repository with the workflow file added in `.github/workflows/`.
- Kubernetes and Helm installed locally (for testing purposes).
- A configured `scan.yaml` file describing the scan parameters.

## Usage
1. Clone this repository:
   ```bash
   git clone https://github.com/GHARBIyasmine/SecureCodeBox-Penetration-testing.git
   cd SecureCodeBox-Penetration-testing

2. Push changes to the `main` branch to trigger the pipeline:
   ```bash
   git add .
   git commit -m "Add workflow"
   git push origin main

3. Monitor the workflow runs under the **Actions** tab in your GitHub repository.

## Scan Results

- Findings are saved in the repository as `findings.json`.
- Summary reports are generated and stored in `report-summary.json`.

## Notes

- Ensure the `GITHUB_TOKEN` secret is configured in your repository for report commits.
- Findings and baseline reports are committed to the repository with [skip ci] to avoid triggering redundant workflows.




