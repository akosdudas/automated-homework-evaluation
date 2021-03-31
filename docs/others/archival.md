# Archival of student work

Once a semester is completed, you will need to archive the course's materials. These are the steps I take.

1. Disable the invitation link in GitHub Classroom. When you [set up the assignments](../using-github/collecting-submissions.md#set-up-github-classroom), you get an invitation link to share with the students. These links should be disabled as soon as the submission deadline passes to prohibit abusing the link.

1. Download all assignments from GitHub. Use [Classroom assistant](../using-github/collecting-submissions.md##classroom-assistant) to download all repositories from GitHub and archive them according to your institution's policy.

1. [Archive the classroom](https://docs.github.com/en/education/manage-coursework-with-github-classroom/manage-classrooms#archiving-or-unarchiving-a-classroom) itself on GitHub Classroom's website. This operation does nothing to the underlying content, but it will be explicit that this classroom is no longer used.

1. Archive each student repository. To prohibit any modifications to the repositories, it is best to make them read-only by archiving them. Archiving the classroom (previous step) does not touch the repositories, so it must be performed manually. I use a short script to do this.

    You will need a [personal access token](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token) from GitHub.

    ``` csharp
    // This is C# code using the official Octokit NuGet package
    string token = @"<your personal access token>";
    var gitHubClient = new GitHubClient(new ProductHeaderValue("<any text>"),
                        new InMemoryCredentialStore(new Credentials(token)));
    while (true)
    {
        // add your organization name and the repository prefix here
        var searchResult = await gitHubClient.Search.SearchRepo(
            new SearchRepositoriesRequest($"org:<org-name> archived:false <repo-prefix>"));

        if (searchResult.Items.Count == 0)
            break;

        foreach (var repo in searchResult.Items)
        {
            await gitHubClient.Repository.Edit(repo.Id,
                            new RepositoryUpdate(repo.Name) { Archived = true });
            Console.WriteLine(repo.FullName);
        }

        // looks like archive is async, and a repository is returned again
        // in the next search phase after archive is started
        await Task.Delay(TimeSpan.FromSeconds(60));
    }  
    ```

1. Alternative to archival: [delete the classroom](https://docs.github.com/en/education/manage-coursework-with-github-classroom/manage-classrooms#deleting-a-classroom). You probably should not do this right after the semester ends, but maybe after a year or so.

    !!! danger ""
        Deleting the classroom also deletes the repositories from the organization!
