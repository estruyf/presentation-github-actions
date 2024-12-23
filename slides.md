---
theme: monomi
highlighter: shiki
transition: slide-left
mdc: true
presenter: dev
title: "From Code to Cloud: Automated Deployments with GitHub Actions"

addons:
  - slidev-addon-components

themeConfig:
  code-padding: "0"
---

# From Code to Cloud: Automated Deployments with GitHub Actions

&mdash; by [Elio Struyf](https://eliostruyf.com)

---
src: ./pages/hello.md
---

---
layout: section
invert: true
---

# Understanding GitHub Actions

## Fundamentals and terminology

---
---

# What are GitHub Actions?

- **CI/CD** service
- **Automate** builds, tests, deployments, issues, etc.
- **Workflow** defined in YAML
- **Triggers** based on events like push, pull requests, etc.
- **Actions** are reusable tasks
- **Runners** are VMs to execute workflows on Windows, macOS, Linux

---
---

# Why GitHub Actions?

- **Integrated** with GitHub
- **Free** for public repositories and self-hosted runners
- **Community** contributed actions
- **Cost-effective** for private repositories ("free" minutes)

---
---

# "Free" minutes per plan

| **Plan** | **Free minutes** |
| --- | --- |
| GitHub Free | 2,000 |
| GitHub Pro | 3,000 |
| GitHub Free for orgs | 2000 |
| GitHub Team | 3,000 |
| GitHub Enterprise Cloud | 50,000 |

---
layout: fact
invert: true
---

# Mind the minute multipliers!

---
---

# Minute what?

## **macOS** and **Windows** runners are more expensive

<br />

| **Runner** | **Multiplier** |
| --- | --- |
| Linux | 1x |
| Windows | 2x |
| macOS | 10x |

<br />

**Large runners** are more expensive (only for orgs and enterprises)

---
layout: section
invert: true
---

# GitHub Actions Terminology

---
---

# GitHub Actions Terminology

```mermaid
flowchart LR
  Events --> |triggers| Workflow --> |starts| Job --> |uses| Runner --- Step --- Action
```

<br />

- **Event**: Trigger for a workflow
- **Workflow**: Automation process
- **Job**: Set of steps that execute on the same runner
- **Runner**: VM to execute workflows
- **Step**: A task that can run commands
- **Action**: A reusable task

---
---

# Types of event triggers

- **push**: Triggered on push to a branch
- **pull_request**: Triggered on pull request
- **schedule**: Triggered on a schedule
- **release**: Triggered on release
- **workflow_call**: Triggered by another workflow
- **workflow_dispatch**: Triggered manually
- **repository_dispatch**: Triggered by an external event

[More triggers](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows)

---
---

# GitHub Actions Workflow

Create a `.github/workflows` directory in your repository.

```yaml {all|2-5|6-8|9-16}{lines:true}
name: Build and Deploy
on:
  push:
    branches:
      - visugxl
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm install
      - run: npm run build
      - uses: actions/upload-artifact@v4
        with:
          name: dist
          path: dist
```

---
layout: section
invert: true
---

# Building Blocks

## Crafting your first GitHub Actions workflow

---
clicks: 1
---

# The demo

```mermaid
flowchart LR
  crafting[1\. Craft a workflow]
  preview[2\. Preview deployment]
  validate[3\. Validation]
  production[4\. Deploy to production]

  classDef startClass fill:#44ffd2,color:black
  classDef endClass fill:#f141a8,color:black

  crafting:::startClass --> preview
  preview --> validate
  validate --> production:::endClass
```

<br />

1. **Crafting**: Create a workflow the first workflow
2. **Preview**: Deploy to GitHub Pages
3. **Validation**: Test the deployment with Playwright
4. **Production**: Deploy to Azure

<vscode-action
    click="1"
    port="37106"
    command="demo-time.runById"
    args="1.crafting.craft-workflow" />

<!--
- Explain the process of what we are going to do in the GH Actions workflow
- Create the `.github/workflows` directory and add a YAML file
- Create a workflow with a name and trigger
- Define jobs and steps
- Add `env` variables
- Add caching
- Test the flow and show the artifacts
- GitHub Variables - https://docs.github.com/en/actions/learn-github-actions/variables
- GitHub Context - https://docs.github.com/en/actions/learn-github-actions/contexts

Highlights:
- How to create a workflow
- Triggers
- Environment variables
- Actions and how to use options like caching
- Artifacts
- Running the first workflow
-->

---
clicks: 1
---

# Variables

- Non-sensitive configuration information
- Levels:
  - Workflow: `env`
  - Repository
  - Environment
  - Organization
- [Default environment variables](https://docs.github.com/en/actions/learn-github-actions/variables#default-environment-variables): `GITHUB_*` and `RUNNER_*`
  - `GITHUB_REPOSITORY`: `owner/repo`
  - `GITHUB_REPOSITORY_OWNER`: `owner`

<vscode-action
    click="1"
    port="37106"
    command="demo-time.runById"
    args="1.crafting.add-variables" />

---
---

# Highlights

1. Workflows created in `.github/workflows` directory
2. Define workflow variables by using `env`
3. Actions options can be added like `with`, `env`, etc.
4. Variables and contexts can be used in the workflow
5. Setting your variables like with `>> $GITHUB_ENV`

<br />

```bash
echo "{environment_variable_name}={value}" >> $GITHUB_ENV
```

<br />

[GitHub Variables](https://docs.github.com/en/actions/learn-github-actions/variables) - 
[GitHub Context](https://docs.github.com/en/actions/learn-github-actions/contexts)

<style>
  .slidev-layout .slidev-code-wrapper code {
    font-size: 16px !important;
    line-height: 22px !important;
  }
</style>

---
layout: section
clicks: 1
invert: true
---

# From Artifact to Deployment

## Deploy your website

<vscode-action
    click="1"
    port="37106"
    command="demo-time.runById"
    args="2.preview.open-workflow" />

<!--
- Start by a dependent job
- Explain how to use the artifacts
- Export the PDF
- Deploy the website to GitHub Pages
  - Using the `GITHUB_TOKEN` secret. It is a GitHub App installation access token, and is limited to the repository that triggers the workflow. [More info](https://docs.github.com/en/actions/security-guides/automatic-token-authentication#modifying-the-permissions-for-the-github_token)
  - Explain that each job runs with default permissions: [workflow permissions](https://github.com/estruyf/github-actions-presentation-test/settings/actions)
  - We can change the permissions
- Using an environment and setting the URL
- Upload artifacts

Highlights:
- Dependent jobs
- Using artifacts
- Permissions on the `GITHUB_TOKEN`
- Environments
- Publish to GitHub Pages
-->

---
---

# Automatic token authentication

- Each workflow job creates a `GITHUB_TOKEN` secret
- Used to authenticate with GitHub API
- `${{ secrets.GITHUB_TOKEN }}`

<v-click>

Comes with **default** permissions depending on the repo settings

![](/slides/workflow-permissions.png)

</v-click>

---
clicks: 1
---

# Elevate the GITHUB_TOKEN permissions

```yaml {5-8}{lines:true}
preview-deploy:
  runs-on: ubuntu-latest
  needs: build

  permissions:
    contents: read
    pages: write
    id-token: write
```

<vscode-action
    click="1"
    port="37106"
    command="demo-time.runById"
    args="2.preview.elevate-token" />

---
clicks: 3
---

# Job output variables

#### Step 1: Set the variable

```yaml
steps:
  - id: step1 # Required
    run: |
      # {name}={value}
      echo "website=https://..." >> $GITHUB_OUTPUT
```

<v-click>

<br /> 

#### Step 2: Define the output

```yaml
jobs:
  job1:
    outputs:
      website: ${{ steps.step1.outputs.website }}
```

</v-click>

<br /> 

<v-click>

#### Step 3: Use the output

```yaml
  job2:
    needs: job1
    steps:
      - run: echo ${{ needs.job1.outputs.website }}
```

</v-click>

<vscode-action
    click="3"
    port="37106"
    command="demo-time.runById"
    args="2.preview.output-variables" />

---
---

# Highlights

- You can depend on other jobs by using the `needs` key
- Use the `actions/upload-artifact` action to upload artifacts
- Permissions on the `GITHUB_TOKEN` can be changed
- Set job output variables with `>> $GITHUB_OUTPUT`
- Add information to the job summary via `>> $GITHUB_STEP_SUMMARY`

---
layout: section
invert: true
clicks: 1
---

# Ensuring Success

## Validating the deployment

<vscode-action
    click="1"
    port="37106"
    command="demo-time.runById"
    args="3.e2e.start" />

<!--
- Create a new dependent job for testing
- Showing the environment and its variables/secrets
- Adding extra caching
- Conditions for the job/action
- Job summary
- Run the new flow and show the results

Highlights:
- Dependent jobs
- Environment variables and secrets
- Caching
- Conditions
- Job summary
-->

---
---

# Caching dependencies

Use `actions/cache` to cache dependencies

```yaml {all|2|4-6}{lines:true}
- name: Cache Playwright
  id: cache-playwright
  uses: actions/cache@v4
  with:
    path: ~/.cache/ms-playwright
    key: playwright-${{ env.PLAYWRIGHT_VERSION }}
```

---
clicks: 3
---

# Conditions

Conditions can be added to jobs and steps to control when they run

```yaml {all|2}{lines:true}
- name: Install Playwright Browsers
  if: steps.cache-playwright.outputs.cache-hit != 'true'
  run: npx playwright install --with-deps
```

<br />

<v-click>

- `if: always()` to always run the step (even if the previous step/job failed)
- Functions: `contains()`, `startsWith()`, `endsWith()`, ...

</v-click>

<vscode-action
    click="3"
    port="37106"
    command="demo-time.runById"
    args="2.preview.caching" />

---

# Highlights

- Use `actions/cache` to cache dependencies
- Conditions can be added to jobs and steps to control when they run
- Each environment can have its variables and secrets
  - Using variables: `vars.{name}`
  - Using secrets: `secrets.{name}`

---
layout: section
invert: true
clicks: 1
---

# Expanding Horizons

## Using environments

<vscode-action
    click="1"
    port="37106"
    command="demo-time.runById"
    args="4.production.start" />

<!--
- Create a new job for deploying to Azure
- Show how to use secrets
- Reusing actions -> templates
- Approval in the workflow for Azure deployment

Highlights:
- Using variables per environment
- Secrets
- Reusing actions with templates
- Approval in the workflow
-->

---
---

# Environments

- Used for a **deployment target**
- Allow you to set **environment-specific variables/secrets**
- **Wait timer** can be set
- **Protection rules** can be set
  - You can set an **approval/review process**
  - Limit branches/tags per environment

<br />

```yaml {all|4-5}{lines:true}
jobs:
  production-deploy:
    runs-on: ubuntu-latest
    environment:
      name: azure
```

---
clicks: 1
---

# Secrets

- Sensitive information
- Levels:
  - Repository
  - Environment
  - Organization
- Access policy on organization-level
- Values are masked in build outputs
- Set as input or environment variable to use them

<vscode-action
    click="1"
    port="37106"
    command="demo-time.runById"
    args="4.production.deploy" />

---
---

# Highlights

- Use **environment-specific** variables and secrets
- Reuse actions with **custom actions**
- Set an **approval process** for the deployment
- Set `secrets.{name}` to use the secret in the workflow

---
layout: section
---

# There is a lot more to discover

<!--
- Show the GitHub Actions Marketplace
- Creating your own actions
- Matrix builds
- Debugging GitHub Actions -> Set the `ACTIONS_RUNNER_DEBUG` variable to `true` in the workflow
- Running actions locally with act - https://github.com/nektos/act
-->

---
---

# Debugging

- Detailed logging via `ACTIONS_RUNNER_DEBUG` secret/variable is `true`
- Enable **debug logging** on workflow re-run

<img src="/slides/debug-logging.png" style="height: 65%; margin: 0 auto;" />

---
---

# Running actions locally

By using the [act](https://nektosact.com/) tool, you can run your workflows locally

```bash
# Install
brew install act

# Run
act
```

---
---

# There is still more to explore

- GitHub Actions Marketplace
- Create your actions and distribute them
- Matrix builds -> automatically create multiple jobs
- Use [GitHub Actions for VSCode extension](https://marketplace.visualstudio.com/items?itemName=GitHub.vscode-github-actions)

---
src: ./pages/thanks.md
---
