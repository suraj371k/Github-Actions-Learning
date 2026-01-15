## YAML

YML (or YAML) stands for "YAML Ain't Markup Language" - it's a human-readable data serialization format commonly used for configuration files.

## What YAML is Used For

**Configuration files** - Docker, Kubernetes, CI/CD pipelines (GitHub Actions, GitLab CI)

**Data exchange** - Similar to JSON or XML but more readable

**Infrastructure as Code** - Ansible, Terraform configurations

**Application settings** - Database configs, API settings, app parameters

```yaml
# This is a comment

# Key-value pairs
name: John Doe
age: 30
city: New York

# Lists/Arrays
fruits:
  - apple
  - banana
  - orange

# Or inline
colors: [red, green, blue]

# Nested objects
person:
  name: Alice
  age: 25
  address:
    street: 123 Main St
    city: Boston
    zip: 02101

# Multiple items
employees:
  - name: Bob
    role: Developer
    skills:
      - Python
      - Docker
  - name: Sarah
    role: Designer
    skills:
      - Figma
      - CSS
```

## GITHUB ACTIONS

GitHub Actions is a continuous integration and continuous delivery (CI/CD) platform that allows you to automate your build, test, and deployment pipeline. You can create workflows that build and test every pull request to your repository, or deploy merged pull requests to production.

## WORKFLOW

A **workflow** is a configurable automated process that will run one or more jobs. Workflows are defined by a YAML file checked in to your repository and will run when triggered by an event in your repository, or they can be triggered manually, or at a defined schedule.

Workflows are defined in the `.github/workflows` directory in a repository.

## EVENTS

An **event** is a specific activity in a repository that triggers a **workflow** run. For example, an activity can originate from GitHub when someone creates a pull request, opens an issue, or pushes a commit to a repository. You can also trigger a workflow to run on a [schedule](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#schedule), by [posting to a REST API](https://docs.github.com/en/rest/repos/repos#create-a-repository-dispatch-event), or manually.

## JOBS

A **job** is a set of **steps** in a workflow that is executed on the same **runner**. Each step is either a shell script that will be executed, or an **action** that will be run. Steps are executed in order and are dependent on each other. Since each step is executed on the same runner, you can share data from one step to another. For example, you can have a step that builds your application followed by a step that tests the application that was built.

## ACTIONS

An **action** is a pre-defined, reusable set of jobs or code that performs specific tasks within a **workflow**, reducing the amount of repetitive code you write in your workflow files. Actions can perform tasks such as:

- Pulling your Git repository from GitHub
- Setting up the correct toolchain for your build environment
- Setting up authentication to your cloud provider

## RUNNERS

A **runner** is a server that runs your workflows when they're triggered. Each runner can run a single **job** at a time. GitHub provides Ubuntu Linux, Microsoft Windows, and macOS runners to run your **workflows**. Each workflow run executes in a fresh, newly-provisioned virtual machine.

## CI/CD

CI/CD is an automated pipeline that helps teams deliver code faster and more reliably by automatically building, testing, and deploying applications whenever developers make changes

**CI - Continuous Integration:**
Whenever a developer commits code, it's automatically merged with the main codebase, built, and tested. This catches bugs early and prevents integration issues.

**CD - Continuous Deployment/Delivery:**
After tests pass, the code is automatically deployed to staging or production environments without manual intervention.

```yaml
name: Basic CI
on: [push, pull_request]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm install
      - run: npm test

```

## Workflow Breakdown

**`name: Basic-CI`**

- Names of workflow "Basic-CI" - this appears in the Actions tab on GitHub

**`on: [push, pull_request]`**

- **Triggers**: When this workflow runs
- Runs on every `push` to any branch
- Runs on every `pull_request` opened or updated

**`jobs:`**

- Defines what tasks to execute

**`build:`**

- Job name (you can have multiple jobs like `build`, `test`, `deploy`)

**`runs-on: ubuntu-latest`**

- Specifies the operating system for the virtual machine
- Uses the latest Ubuntu Linux server provided by GitHub

**`steps:`**

- Sequential actions to perform in this job
- **`uses: actions/checkout@v4`**
- Downloads your repository code to the runner
- Without this, the runner has an empty filesystem
- **`name: Setup Node`**
- Names this step (appears in logs)

**`uses: actions/setup-node@v4`**

- Installs Node.js on the runner

**`with: node-version: '20'`**

- Specifies Node.js version 20
- **`run: npm install`**
- Executes command to install all dependencies from `package.json`
- **`run: npm test`**
- Runs your test suite defined in `package.json`
- If tests fail, the workflow fails

## Complete Flow

1. Code is pushed to GitHub
2. GitHub spins up an Ubuntu server
3. Downloads your code
4. Installs Node.js 20
5. Installs project dependencies
6. Runs tests
7. Reports success/failure

### Interview Questions

### **1. What is GitHub Actions and how does it differ from traditional CI/CD tools?**

*Expected Answer:* GitHub Actions is a CI/CD platform integrated directly into GitHub. Unlike traditional tools like Jenkins that require separate servers, it runs workflows in GitHub's cloud infrastructure, offers tight GitHub integration, and uses a marketplace of reusable actions.

### **2. What are the different types of events that can trigger a GitHub Actions workflow?**

GitHub Actions workflows can be triggered by three main categories of events: **repository events** (code changes), **scheduled events** (time-based), and **manual events** (on-demand triggers). Let me break down the most commonly used ones

## 1. Repository Events

These trigger based on activities in the repository:

**push** - When code is pushed to any branch

```yaml
`on:
  push:
    branches: [main, develop]
    paths:
      - 'src/**'`
```

**pull_request** - When a PR is opened, updated, or synchronized

```yaml
on:
  pull_request:
    types: [opened, synchronize, reopened]
    branches: [main]
```

**release** - When a release is published

```yaml
on:
  release:
    types: [published, created]
```

## 2. Scheduled Events

For time-based automation using cron syntax:

```yaml
on:
  schedule:
    - cron: '0 0 * * *'  # Daily at midnight UTC
    - cron: '0 */6 * * *'  # Every 6 hours
```

## 3. Manual Triggers

**workflow_dispatch** - Allows manual triggering from the GitHub UI:

```yaml
on:
  workflow_dispatch:
    inputs:
      environment:
        description: 'Deployment environment'
        required: true
        type: choice
        options:
          - development
          - staging
          - production
```

## 4. External Events

**repository_dispatch** - Triggered by external systems via GitHub API:

```yaml
on:
  repository_dispatch:
    types: [deploy-prod]
```

### **3. What is the difference between `runs-on` and `uses` in a workflow?**

 `runs-on` specifies the runner environment (ubuntu-latest, windows-latest, macos-latest) where the job executes. `uses` imports a pre-built action from the marketplace or a repository.

### **4. How do you manually trigger a GitHub Actions workflow?**

Use the `workflow_dispatch` event in the workflow file. This adds a "Run workflow" button in the Actions tab of the repository.

### **5. What is the purpose of the `actions/checkout` action?**

It checks out your repository code into the runner so subsequent steps can access the files. Without it, the runner starts empty.

### 6. **What are GitHub-hosted runners vs self-hosted runners?**

GitHub-hosted runners are virtual machines managed by GitHub (free tier included). Self-hosted runners are your own machines/servers that you configure and maintain for more control or specific requirements.

### 7. **What happens if you don't specify an `on` trigger in your workflow?**

The workflow won't run automatically. You must specify at least one trigger event (like push, pull_request, or workflow_dispatch) for the workflow to execute.