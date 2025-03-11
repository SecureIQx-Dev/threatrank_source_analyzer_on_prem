# Vulnerability Analyzer

> This is a GitHub Actions Resuable Workflow. You will need to first set this repo up in your own repository and use it in other repos.

## Setup Reusable Repo

- ### First setup your own repo called `vulnerability_analyzer`. Make sure to create the repo with private visibility.
- ### Then add the files of this repo to the newly created repository.
- ### Then update these values in `.github/workflows/vulnerability-analyzer.yml`. Set `REPO_USER` and `REPO_NAME` as your newly created repo's name and your user name. Also set `ISSUE_CREATION` to `true` or  `false` according to your preference.
  ![image](https://github.com/user-attachments/assets/98f5ff36-ce07-41b7-a469-16aab48650d6)
- ### Then Go to the settings page of the repo and set `Actions permissions` to `Allow all actions and reusable workflows` and hit save.
  ![420515692-062bfa83-9b01-4e22-9609-3cf27bd6a2e3](https://github.com/user-attachments/assets/5f0c02ac-e057-4693-981d-97ae2cb3808d)
- ### Then Scroll down and set `Workflow permissions` to `Read repository contents and packages permissions`.
  ![420515816-fd3f5d3d-c3d1-476a-931b-14d3d1278709](https://github.com/user-attachments/assets/b720fc7b-bfcd-4837-9445-98c1d61b0650)
- ### After these steps the Reusable Repe setup is complete.

## How to build python script after changes

- ### First you need to be in a Linux environment
- ### After that make sure to all the python dependencies are installed, run `pip install -r requirements.txt` after locating where your requirements.txt file is location. in the repo the file is located in `.github/workflows/vulnerability-analyzer` change directory into that location and do the pip install.
- ### Then, open up a terminal and `cd` change directory to the location that your `vulnerability_analyzer.py` is located. `cd .github/workflows/vulnerability-analyzer/utils` from the root of the repo.
- ### Then run this command to build the python script. `pyinstaller --onefile --hidden-import=tiktoken_ext.openai_public --hidden-import=tiktoken_ext .github/workflows/vulnerability-analyzer/utils/vulnerability_analyzer.py`. This command should create two folders and one file. goto `dist` folder and copy the build file into the `utils` folder in the repo. then delete the `dist`, `build` and `vulnerability_analyzer.spec` folders and files.
- ### Finally push the changes.

## Setup other repo's to use the reusable repo

- ### As shown in the demo video, First you will need to create a new repository. This can be done in your alredy available repo also.
- ### Then you will need to setup the required credentials in the repo. Just go to repo Settings -> Secrets and variables -> Actions and setup the `OPENAI_API_KEY` and `PAT`.
  ![420585008-22f20ce1-24ba-486e-9fdf-b282035b9f8b](https://github.com/user-attachments/assets/02f2028f-3f8b-4af5-a733-218ff0a4d561)
- ### Then inside the repo, create a folder called `.github`, then inside that create a folder called `workflows`.
  ![420585051-e52b1412-78c1-477d-8d23-1b0635cbbeaa](https://github.com/user-attachments/assets/2f8a5ce8-7dc0-47c9-88cb-93b3fa69174e)
- ### Then inside the `workflows` folder, create a file called `main.yml` and include the code below. You may need to change the `name` of the task as you prefer.

```
name: mayhem-demo

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  vulnerability-scan:
    permissions:
      contents: write
      issues: write
      pull-requests: write
      security-events: write
    uses: AlphaArchangel/vulnerability_analyzer_v2/.github/workflows/vulnerability-analyzer.yml@main
    with:
      repo_path: "."  # Path to scan
      cvss_lower_bound: "HIGH"  # Optional: default is "high"
      epss_percentile_lower_bound: "0.00"  # Optional: default is "0.05"
    secrets:
      openai_api_key: ${{ secrets.OPENAI_API_KEY }}
      pat: ${{ secrets.PAT }}
```

- ### Now commit the changes to the file.
- ### The GitHub actions workflow should be triggered automatically and do the vulnerability analysis.
  ![420585126-3d55388f-07eb-4c12-973f-bace915379af](https://github.com/user-attachments/assets/a3d9b8bb-258a-456d-ad7b-4295e37060cc)
- ### Once the workflow is completed, it should create an Artifact that includes the reports, you can download it Artifacts section.
  ![420518011-ef5861a6-a5b1-4f59-bf32-de0de00d6958](https://github.com/user-attachments/assets/aca28d33-0b4b-414e-aeb6-36a47cdd1a0e)
- ### If issue creation is set to true and a High Level Threat was discovered, the workflow will automatically create a GitHub issue and notify the user. 
  ![image](https://github.com/user-attachments/assets/7050fe74-bb60-4d8a-8c51-a70a2e1a1f34)

## Please refer the provided demo video for more details.
