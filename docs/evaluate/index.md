# Automated evaluation

Automated evaluation of homework consists of the following steps:

1. Write a program that tests the student's work.
1. Obtain the student's work (i.e., download the code).
1. Run the evaluation program.
1. Publish the results of the evaluation.

The key lies in **automating as much as possible of this process**. From a high level, I propose two alternatives:

- [using GitHub and GitHub Actions as a platform](github-actions.md) for these steps,
- or performing them on [your machine using software help](ahk-cli.md).

Before explaining these options, let us start with the first item: [write the software for evaluation](evaluator-software.md). This step, unfortunately, cannot be automated.
