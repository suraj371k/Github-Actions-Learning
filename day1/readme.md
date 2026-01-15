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