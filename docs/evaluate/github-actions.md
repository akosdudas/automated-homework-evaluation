# GitHub Actions

[GitHub Actions](https://github.com/features/actions) is a continuous integration (CI) platform offering from GitHub. It enables the definition of so-called [_workflows_](https://docs.github.com/en/actions/learn-github-actions/introduction-to-github-actions#the-components-of-github-actions) executed in an environment managed by GitHub. We can use the _workflow_ to define our assessment tasks if the students [submit their solutions in GitHub repositories](../using-github/collecting-submissions.md).

Why GitHub Actions? Mainly because [using GitHub](../using-github/index.md) enables us to cover multiple steps of the homework submission-evaluation process.

!!! info "Don't want to or cannot use GitHub?"
    Check out the [alternative option here](ahk-cli.md).

## Running the evaluation

The pre-requisites of running the evaluation in GitHub are the following.

1. You need a GitHub organization with [GitHub Education benefits](https://education.github.com/teachers) providing you with free credits for executing Actions in the cloud.
1. Have the students [submit their work in GitHub](../using-github/collecting-submissions.md).
1. Include a workflow definition in the starter code repository specifying the assessment process.

The workflow definition is a yaml file placed in the starter code repository. When the student's repository is created, the contents, along with the workflow definition, are copied from the starer repository. Here are two sample workflow definitions.

### Example 1

The first example is from the presented sample at <https://github.com/akosdudas/ahk-sample-startercode/blob/master/.github/workflows/evaluate.yml>.

```yaml linenums="1"
name: Evaluation

on:
  pull_request:
    types: [opened, synchronize, ready_for_review, labeled]

jobs:
  evaluate:
    runs-on: ubuntu-latest

    timeout-minutes: 3

    if: github.event.pull_request.draft == false

    steps:
      - name: Checkout
        uses: actions/checkout@v2
        with:
          fetch-depth: 1

      - name: Check neptun.txt
        uses: akosdudas/ahk-action-neptuncheck@v1

      - name: Prepare .NET SDK
        uses: actions/setup-dotnet@v1
        with:
          dotnet-version: "2.1.607"

      - name: Build sln
        run: dotnet build

      - name: Evaluate
        uses: docker://ghcr.io/akosdudas/ahk-sample:evaluator-v1

      - name: Publish results
        uses: akosdudas/ahk-action-publish-result-pr@v1
        with:
          input-file: "result.txt"
          image-extension: ".png"
          github-token: "${{ secrets.GITHUB_TOKEN }}"
```

This workflow definition works on [_pull requests_](../using-github/providing-feedback.md#using-pull-requests); this is what you see on lines 4-5. If the student sets the pull request as [_draft_](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-requests#draft-pull-requests), the evaluation is skipped (line 13), allowing the student to make changes to the code without running the assessment.

Since the evaluator application is containerized, the [Ubuntu virtual environment](https://github.com/actions/virtual-environments) is used for the CI execution (line 9). (GitHub also has Windows and macOS environments.)

Specifying a timeout (line 11) prohibits long-running workflows, which would only eat up the minutes available in GitHub; the evaluation should not take more than a minute, including obtaining the source code and pulling container images.

The assessment starts in line 16 with the checkout of the source code (to get the student's code). Next, there is a preliminary check verifying the existence of a text file that contains the student's identifier; see the reason [here](../using-github/collecting-submissions.md#not-using-a-student-roster). If the student forgot to upload this file, the assessment does not proceed.

The next step is building the source code to get the executable versions (lines 24-30). If the build fails, the student is automatically notified.

And now comes the actual assessment. Since the evaluator application is containerized, the workflow only needs to trigger the execution of this container. The working directory with the source code and the built binaries are mounted into the container, so the application can access it.

The final step is publishing the results of the assessment (line 35). The evaluator application writes the results to a text file, and the [custom action](https://github.com/akosdudas/ahk-action-publish-result-pr) reads this text file to send the contents into the pull request thread ([see example here](https://github.com/akosdudas/ahk-sample-studentsolution/pull/1#issuecomment-646669774)).

### Example 2

The second example simplifies the process by eliminating the pull request and the container too.

```yaml linenums="1"
name: Evaluation

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest

    timeout-minutes: 3

    steps:
      - name: Checkout
        uses: actions/checkout@v1

      - name: Check neptun.txt
        uses: akosdudas/ahk-action-neptuncheck@v1

      - name: Prepare .NET SDK
        uses: actions/setup-dotnet@v1
        with:
          dotnet-version: "5.0.200"

      - name: Build .NET code
        run: dotnet build

      - name: Run .NET unit tests
        run: dotnet test
```

Suppose you distribute the evaluator code as [unit tests](evaluator-software.md#alternative-approach-unit-tests). In that case, you can run them with a simple command (line 27) after preparing the appropriate SDK and building the code (lines 18-24).

There are no pull requests here either. The trigger for evaluation is a push (line 3), so every time the student pushed code to the GitHub repository, this workflow will execute. The results will be limited to console output of the process on [GitHub's web interface](https://docs.github.com/en/actions/learn-github-actions/introduction-to-github-actions#viewing-the-jobs-activity).

## Tips

A few tips to keep in mind when using this approach.

**Keep the workflow file universal and straightforward.** The workflow definition file is copied to the student's repository. You cannot make changes to it. The file must be "universal," that is, changes you might need to make in the evaluation process should not be part of this file. Containerizing the evaluator application is an excellent way of ensuring that the workflow file needs no change; any update to the assessment process happens in the container, which you can update.

**Limit the number of evaluations.** Be explicit about how many executions students are allowed. The assessment takes time and consumes the minutes you have in GitHub. The students should not use this online assessment to verify their work every step of the way. The automated evaluation should be the last step in the process after they checked their work locally. But do not limit it to one. This method aims to enable the student to correct the code if something does not work according to expectations.

**Prepare for occasional outages.** There are far too many moving parts in this process. Sometimes things will break. GitHub Actions has outages sometimes; pulling containers fail occasionally; the execution times out from time to time; the .NET SDK download fails randomly; etc. These things happen. Either prepare to help students through transient errors (i.e., by manually [re-running the failed workflow](https://docs.github.com/en/actions/managing-workflow-runs/re-running-a-workflow)), or let them know how to do this on their own.

**Set up a self-hosted runner if the free CI minutes are not enough or you need specialized software/hardware.** GitHub allows you to use [self-hosted runners](https://docs.github.com/en/actions/hosting-your-own-runners/about-self-hosted-runners) instead of the ones provided in the cloud. You can prepare your customized environment in these runners. But before going down this path, also consider that you will need stable infrastructure and monitoring to do this.
