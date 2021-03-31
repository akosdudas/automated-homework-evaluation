# GitHub branch protection rules

[GitHub pull requests](../using-github/providing-feedback.md#using-pull-requests) come in handy for seeing the cumulative code changes of the student and for providing feedback. However, it relies on the student performing some mandatory steps: creating a branch and opening a pull request. The more manual steps there are, the bigger the chance of messing something up. The most critical part is working on the right branch in git. If the student pushes content to the wrong branch, it will require manual work to fix it.

Our experience is that despite clear instuctions, the step of creating a new branch and swithing to this new branch is **forgotten in about 10% of the cases**.

## Branch protection rules

GitHub has so-called [_branch protection rules_](https://docs.github.com/en/github/administering-a-repository/managing-a-branch-protection-rule#about-branch-protection-rules), which can help prohibing students from doing something wrong.

**Branch-protection rule for the main/master branch.** By protecting the main/master branch with a rule that requires at least one pull request approval, the student cannot add or merge any code into the main/master branch. Therefore, the main/master branch has the initial starter code and the pull request opened by the student will gather all changes made to the code since.

**Branch-protection rule for all working branches.** You should protect all other branches too by prohibiting force push. If a student wants to "play around" with the automated evaluation and "be smart about it" in any way, these attempts could be hidden by rewriting the past of git using force push. A branch protection rule can prohibit this.

## [Ahk GitHub Monitor](https://github.com/akosdudas/ahk-github-monitor)

Unfortunaltely there is no way to set these branch protection rules on students' repositories using GitHub Classroom. [Ahk GitHub Monitor](https://github.com/akosdudas/ahk-github-monitor) is a GitHub App hosted as an Azure Function that subscribes to events within GitHub and performs certain actions upon specific events. One purpose of this tool is to set the aforementioned branch protection rules on the students' repositories after the repositories are created.

For more information, please refer to the description in the following repository: <https://github.com/akosdudas/ahk-github-monitor>.
