# Todo

- [ ]  Use Reusable workflow
- [ ]  Cache the action
- [ ]  Upload/download using name instead of folder
- [ ]  Automate reviews in GitHub by using workflows
- [ ]  Workflow status badge
- [ ]  Publish a Github package

# The idea

GitHub is designed to help teams of developers and DevOps engineers build and deploy applications quickly

- **Communication**: Consider all of the ways that GitHub makes it easy for a team of developers to communicate about the software development project: code reviews in pull requests, GitHub issues, project boards, wikis, notifications, and so on.
- **Automation**: GitHub Actions lets your team automate workflows at every step in the software-development process, from integration to delivery to deployment. It even lets you automate adding labels to pull requests and checking for stale issues and pull requests.

# Workflow that decrease development time

- Ensure the code passes all unit tests.
- Perform code quality and compliance checks to make sure the source code meets the organization's standards.
- Check the code and its dependencies for known security issues.
- Build the code by integrating new source code from (potentially) multiple contributors.
- Ensure the software passes integration tests.
- Specify the version of the new build.
- Deliver the new binaries to the appropriate filesystem location.
- Deploy the new binaries to one or more servers.
- Determine if any of these tasks don't pass, and report the issue to the proper individual or team for resolution.

# What is github actions?

*GitHub Actions* are packaged scripts to automate tasks in a software-development workflow in GitHub. You can configure GitHub Actions to trigger complex workflows that meet your organization's needs. The trigger can happen each time developers check new source code into a specific branch, at timed intervals, or manually. The result is a reliable and sustainable automated workflow, which leads to a significant decrease in development time.

# What is a GitHub Actions workflow?

A *GitHub Actions workflow* is a process that you set up in your repository to automate software-development lifecycle tasks, including GitHub Actions. With a workflow, you can build, test, package, release, and deploy any project on GitHub.

# Components of Github actions

!image.png

# Configure workflows to run for schedule events

You can configure your workflows to run when specific activity occurs on GitHub, when an event outside of GitHub happens, or at a scheduled time

!image.png

If you want to run every 15 minutes

```yaml
on:
  schedule:
    - cron:  '*/15 * * * *'
```

# **Configure workflows to run for manual events**

You can use the `workflow_dispatch` event.
This event allows you to run the workflow by using the GitHub REST API or by selecting the **Run workflow** button in the **Actions** tab within your repository on GitHub. Using `workflow_dispatch`, you can choose on which branch you want the workflow to run, and set optional `inputs` that GitHub presents as form elements in the UI.

You can also use the `repository_dispatch` event with the GitHub API. (HTTP call)

!image.png

!image.png

# **Configure workflows to run for webhook events**

Example: you can run a workflow for the `check_run` event, but only for the `rerequested` or `requested_action` activity types.

!image.png

# **Triggering the event via API**

!image.png

!image.png

# Use conditional keywords

You can use the conditional `if` keyword in a workflow file to determine whether a step should run or not.

!image.png

# **Disable and delete workflows**

Disabling a workflow can be useful in some of the following situations:

- An error on a workflow is producing too many or wrong requests impacting external services negatively.
- You want to temporarily pause a workflow that isn't critical and is consuming too many minutes on your account.
- You want to pause a workflow that's sending requests to a service that is down.
- You're working on a fork, and you don't need all the functionality of some workflows it includes (like scheduled workflows).

# **Use an organization's templated workflow**

If you have a workflow that multiple teams use within an organization, you don't need to re-create the same workflow for each repository. Instead, you can promote consistency across your organization by using a workflow template defined in the organization's `.github` repository. Any member within the organization can use an organization template workflow, and any repository within that organization has access to those template workflows.

# **Use specific versions of an action**

When referencing actions in your workflow, we recommend that you refer to a specific version of that action rather than just the action itself.

!image.png

# **Avoid duplication by using reusable workflows**

!image.png

# **Why use reusable workflows?**

These are the benefits of using reusable workflows:

- **Consistency.** Teams can follow the same automation standards across all projects.
- **Efficiency.** Instead of copying and pasting steps, you just point to a reusable workflow.
- **Easier updates.** When a process changes, such as by adding a test step, you update it in one location. Then all workflows that use the workflow benefit automatically.
- **Scalability.** Reusable workflows are ideal for platform or DevOps teams that manage multiple services.

# **Identify the event that triggered a workflow**

!image.png

# **What is a workflow trigger?**

A workflow trigger is an event that causes a workflow to run. GitHub supports various types of triggers, including:

- `push` or `pull_request` (based on code changes)
- `workflow_dispatch` (a manual trigger)
- `schedule` (cron jobs)
- `repository_dispatch` (external systems)
- Issue, discussion, and pull request events (for example, `issues.opened`, `pull_request.closed`)

# **Infer the trigger from repository effects**

!image.png

# **Diagnose a failed workflow run**

1. In your repository, go to the **Actions** tab.
2. Find the failed run (typically indicated by a red **X**).
3. Select the failed workflow to open the run summary.
4. In the workflow logs, review the error.
    1. In the run summary, expand each job and step until you find the one that indicates failure.
    2. Select the job or step to view its logs.
    3. Look for:
        - Error messages
        - Stack traces
        - Exit codes

   For example, a failed test might show `npm ERR! Test failed` or `exit code 1`.

5. Check the workflow configuration file.

   Use the `.yml` file to determine:

    - What was each step trying to do?
    - If there are environment variables (`env`) or conditionals (`if`) that affect execution.
    - If the failure is due to a missing dependency, syntax error, or misconfigured step.

   If a step fails, check for the following causes:

    - Were dependencies installed successfully in the preceding step?
    - Do test files exist and pass locally?

# **Common failure scenarios**

!image.png

# **Work with artifacts**

When a workflow produces something other than a log entry, the product is called an *artifact*. For example, the Node.js build produces a Docker container that can be deployed. The container is an artifact that you can upload to storage by using the actions/upload-artifact action. You can later download the artifact from storage by using actions/download-artifact.

Storing an artifact preserves it between jobs. Each job uses a fresh instance of a VM, so you can't reuse the artifact by saving it on the VM. If you need your artifact in a different job, you can upload the artifact to storage in one job, and download it for the other job.

# **Artifact storage**

Artifacts are stored in storage space on GitHub. The space is free for public repositories, and some storage is free for private repositories, depending on the account. GitHub stores your artifacts for 90 days.

!image.png

!image.png

# **Automate reviews in GitHub by using workflows**

In addition to starting a workflow via GitHub events like `push` and `pull-request`, you can run a workflow on a schedule or after some event outside GitHub.

You might want a workflow to run only after a user completes a specific action, such as after a reviewer approves a pull request. For this scenario, you can trigger on `pull-request-review`.

Another action you can take is to add a label to the pull request. In this case, use the pullreminders/label-when-approved-action action.

!image.png

# **Default environment variables and contexts**

To use a default environment variable, specify `$` followed by the environment variable's name.

!image.png

In addition to default environment variables, you can use defined variables as contexts. Contexts and default variables are similar in that they both provide access to environment information, but they have some important differences. Although default environment variables can be used only within the runner, you can use context variables at any point in the workflow. For example, context variables allow you to run an `if` statement to evaluate an expression before the runner is executed.

!image.png

# Contextual information available in a workflow

| Context | Description |
| --- | --- |
| `github` | Information about the workflow run. For more information, see `github` context. |
| `env` | Contains variables that you set in a workflow, job, or step. For more information, see `env` context. |
| `vars` | Contains variables that you set at the repository, organization, or environment level. For more information, see `vars` context. |
| `job` | Information about the currently running job. For more information, see `job` context. |
| `jobs` | For reusable workflows only, contains outputs of jobs from the reusable workflow. For more information, see `jobs` context. |
| `steps` | Information about the steps that ran in the current job. For more information, see `steps` context. |
| `runner` | Information about the runner that is running the current job. For more information, see `runner` context. |
| `secrets` | Contains the names and values of secrets that are available to a workflow run. For more information, see `secrets` context. |
| `strategy` | Information about the matrix execution strategy for the current job. For more information, see `strategy` context. |
| `matrix` | Contains the matrix properties defined in the workflow that apply to the current job. For more information, see `matrix` context. |
| `needs` | Contains the outputs of all jobs that are defined as a dependency of the current job. For more information, see `needs` context. |
| `inputs` | Contains the inputs of a reusable or manually triggered workflow. For more information, see `inputs` context. |

The following table lists restrictions for each context and special function in a workflow. The listed contexts are available only for the indicated workflow key. You can't use them anywhere else. You can use a function anywhere unless it's listed in the following table.

- Table


    | Workflow key | Context | Special functions |
    | --- | --- | --- |
    | `run-name` | `github`, `inputs`, `vars` | None |
    | `concurrency` | `github`, `inputs`, `vars` | None |
    | `env` | `github`, `secrets`, `inputs`, `vars` | None |
    | `jobs.<job_id>.concurrency` | `github`, `needs`, `strategy`, `matrix`, `inputs`, `vars` | None |
    | `jobs.<job_id>.container` | `github`, `needs`, `strategy`, `matrix`, `vars`, `inputs` | None |
    | `jobs.<job_id>.container.credentials` | `github`, `needs`, `strategy`, `matrix`, `env`, `vars`, `secrets`, `inputs` | None |
    | `jobs.<job_id>.container.env.<env_id>` | `github`, `needs`, `strategy`, `matrix`, `job`, `runner`, `env`, `vars`, `secrets`, `inputs` | None |
    | `jobs.<job_id>.container.image` | `github`, `needs`, `strategy`, `matrix`, `vars`, `inputs` | None |
    | `jobs.<job_id>.continue-on-error` | `github`, `needs`, `strategy`, `vars`, `matrix`, `inputs` | None |
    | `jobs.<job_id>.defaults.run` | `github`, `needs`, `strategy`, `matrix`, `env`, `vars`, `inputs` | None |
    | `jobs.<job_id>.env` | `github`, `needs`, `strategy`, `matrix`, `vars`, `secrets`, `inputs` | None |
    | `jobs.<job_id>.environment` | `github`, `needs`, `strategy`, `matrix`, `vars`, `inputs` | None |
    | `jobs.<job_id>.environment.url` | `github`, `needs`, `strategy`, `matrix`, `job`, `runner`, `env`, `vars`, `steps`, `inputs` | None |
    | `jobs.<job_id>.if` | `github`, `needs`, `vars`, `inputs` | `always`, `canceled`, `success`, `failure` |
    | `jobs.<job_id>.name` | `github`, `needs`, `strategy`, `matrix`, `vars`, `inputs` | None |
    | `jobs.<job_id>.outputs.<output_id>` | `github`, `needs`, `strategy`, `matrix`, `job`, `runner`, `env`, `vars`, `secrets`, `steps`, `inputs` | None |
    | `jobs.<job_id>.runs-on` | `github`, `needs`, `strategy`, `matrix`, `vars`, `inputs` | None |
    | `jobs.<job_id>.secrets.<secrets_id>` | `github`, `needs`, `strategy`, `matrix`, `secrets`, `inputs`, `vars` | None |
    | `jobs.<job_id>.services` | `github`, `needs`, `strategy`, `matrix`, `vars`, `inputs` | None |
    | `jobs.<job_id>.services.<service_id>.credentials` | `github`, `needs`, `strategy`, `matrix`, `env`, `vars`, `secrets`, `inputs` | None |
    | `jobs.<job_id>.services.<service_id>.env.<env_id>` | `github`, `needs`, `strategy`, `matrix`, `job`, `runner`, `env`, `vars`, `secrets`, `inputs` | None |
    | `jobs.<job_id>.steps.continue-on-error` | `github`, `needs`, `strategy`, `matrix`, `job`, `runner`, `env`, `vars`, `secrets`, `steps`, `inputs` | hashFiles |
    | `jobs.<job_id>.steps.env` | `github`, `needs`, `strategy`, `matrix`, `job`, `runner`, `env`, `vars`, `secrets`, `steps`, `inputs` | `hashFiles` |
    | `jobs.<job_id>.steps.if` | `github`, `needs`, `strategy`, `matrix`, `job`, `runner`, `env`, `vars`, `steps`, `inputs` | `always`, `canceled`, `success`, `failure`, `hashFiles` |
    | `jobs.<job_id>.steps.name` | `github`, `needs`, `strategy`, `matrix`, `job`, `runner`, `env`, `vars`, `secrets`, `steps`, `inputs` | `hashFiles` |
    | `jobs.<job_id>.steps.run` | `github`, `needs`, `strategy`, `matrix`, `job`, `runner`, `env`, `vars`, `secrets`, `steps`, `inputs` | `hashFiles` |
    | `jobs.<job_id>.steps.timeout-minutes` | `github`, `needs`, `strategy`, `matrix`, `job`, `runner`, `env`, `vars`, `secrets`, `steps`, `inputs` | `hashFiles` |
    | `jobs.<job_id>.steps.with` | `github`, `needs`, `strategy`, `matrix`, `job`, `runner`, env, `vars`, `secrets`, `steps`, `inputs` | `hashFiles` |
    | `jobs.<job_id>.steps.working-directory` | `github`, `needs`, `strategy`, `matrix`, `job`, `runner`, env, `vars`, `secrets`, `steps`, `inputs` | `hashFiles` |
    | `jobs.<job_id>.strategy` | `github`, needs, `vars`, `inputs`, | None |
    | `jobs.<job_id>.timeout-minutes` | `github`, `needs`, `strategy`, `matrix`, `vars`, `inputs` | None |
    | `jobs.<job_id>.with.<with_id>` | `github`, `needs`, `strategy`, `matrix`, `inputs`, `vars` | None |
    | `on.workflow_call.inputs.<inputs_id>.default` | `github`, `inputs`, `vars` | None |
    | `on.workflow_call.outputs.<output_id>.value` | `github`, jobs, `vars`, `inputs` | None |

# **Custom environment variables**

To create a custom variable, you need to define it in your workflow file using the `env` context.

!image.png

# **Set custom environment variables in a workflow**

You can define environment variables that are scoped to the entire workflow by using `env` at the top level of the workflow file. Scope the contents of a job within a workflow by using `jobs.<job_id>.env`. You can scope an environment variable at a specific step within a job by using `jobs.<job_id>.steps[*].env`.

Here's an example that shows all three scenarios in a workflow file:

!image.png

# **Use a default context in a workflow**

The GitHub platform sets default environment variables. They aren't defined in a workflow, but you can use a default environment variable in a workflow in the appropriate context. Most of these variables, other than `CI`, begin with `GITHUB_*` or `RUNNER_*`. The latter two types can't be overwritten. Also, these default variables have a corresponding and similarly named context property. For instance, the `RUNNER_*` series of default variables have a matching context property of `runner.*`.
Here's an example of how to access default variables in a workflow by applying these methods:

!image.png

# **Pass custom environment variables to a workflow**

You can pass custom environment variables from one step of a workflow job to subsequent steps within the job. Generate a value in one step of a job, and assign the value to an existing or new environment variable. Next, you write the variable/value pair to the GITHUB_ENV environment file. You can use the environment file in an action or from a shell command in the workflow job by using the `run` keyword.

The step that creates or updates the environment variable doesn't have access to the new value, but all subsequent steps in a job have access.

Here's an example:

!image.png

# Pass data between jobs

Good catch — this is a case where the exam wants the mechanism that actually **carries the data**, not the keyword that merely creates the dependency.

Here's the distinction:

- **`outputs`** is what actually **defines and exposes the data** — a job declares `outputs:` mapping a name to a value (usually pulled from a step's `$GITHUB_OUTPUT`). This is the feature whose whole purpose is "share a piece of data from this job with others." It's the direct answer to "how do you pass data."
- **`needs`** is necessary to *access* another job's outputs (`needs.job1.outputs.my-output`), and it's what makes `job2` wait for `job1` — but on its own, `needs` doesn't transmit any data. It just declares "this job depends on that job" and gives you the syntax path to reach its outputs. Without `outputs` being set in the first place, `needs` gives you nothing to read.
- **`env`** sets environment variables scoped to a job/step/workflow — it doesn't cross job boundaries by itself.
- **`secrets`** passes secret values into a job/workflow, not general data between jobs.

So the exam is testing: of these four keywords, which one is *the* data-passing mechanism? That's `outputs`. `needs` is a supporting actor — required in practice, but it's the plumbing that lets you *reach* the data, not the thing that *creates* it.

Think of it like a function call: `outputs` is the `return` statement, `needs` is what lets you call the function and use its return value. You'd say "you return data from a function," not "you call functions" when asked how data comes back.

# **Add environment protections**

You can add protection rules for environments defined for your GitHub repository.

# **About environments**

Use environments to describe a general deployment target like production, staging, or development. When a GitHub Actions workflow deploys to an environment, the environment appears on the main page of the repository. You can use environments to require approval for a job to proceed, restrict which branches can trigger a workflow, gate deployments by using custom deployment protection rules, or limit access to secrets.

Each job in a workflow can reference one environment. Any protection rules that you set for the environment must pass before a job that references the environment is sent to a runner. The job can access the environment's secrets only after the job is sent to a runner.

When a workflow references an environment, the environment appears in the repository's deployments.

# **Environment protection rules**

Environment deployment protection rules require specific conditions to pass before a job that references the environment proceeds. You can use deployment protection rules to require a manual approval, delay a job, or restrict the environment to specific branches. You can also create and implement custom protection rules powered by GitHub Apps to use partner systems to control deployments that reference environments that are configured on GitHub.

Here's an explanation of these protection rules:

- **Required reviewers protection rules**

  Use this rule to require a specific person or team to approve workflow jobs that reference the environment. You can list up to six users or teams as reviewers. The reviewers must have at least Read permissions to the repository. Only one required reviewer must approve the job for it to proceed.

  You also can prevent self-reviews for deployments to a protected environment. If you enable this setting, users who initiate a deployment can't approve the deployment job, even if they're a required reviewer. By enabling self-reviews, it ensures that more than one person reviews deployments to protected environments.

  For more information on reviewing jobs that reference an environment with required reviewers, see Review deployments.

- **Wait timer projection rules**

  You can use a wait timer protection rule to delay a job for a specific amount of time after the job is initially triggered before the environment deployment proceeds. The time (in minutes) must be an integer between 1 and 43,200 (30 days). The wait time doesn't count toward your billable time.

- **Branch and tag protection rules**

  You can use deployment branch and tag protection rules to restrict which branches and tags are used to deploy to the environment. You have several options for deployment branch and tag protection rules for an environment.

    - **No restriction** sets no restriction on which branch or tag can deploy to the environment.
    - **Protected branches only** allows only branches with branch protection rules enabled to deploy to the environment. If no branch protection rules are defined for any branch in the repository, then all branches can deploy. The **Selected branches and tags** setting ensures Only branches and tags that match your specified name patterns can deploy to the environment.
    - If you specify `releases/*` as a deployment branch or tag rule, only a branch or tag with a name that begins with `releases/` can deploy to the environment. (Wildcard characters don't match `/`. To match branches or tags that begin with `release/` and contain another single slash, use `release/*/*`.) If you add `main` as a branch rule, a branch named `main` can also deploy to the environment.
- **Custom deployment protection rules**

  You can create custom protection rules to gate deployments to use partner services. For example, you can use observability systems, change management systems, code quality systems, or other manual configurations that you use to assess readiness and provide automated approvals for deployments to GitHub.


# **Scripts in your workflow**

In the preceding workflow snippet examples, the `run` keyword is used to print a string of text. Because the `run` keyword tells the job to execute a command on the runner, you use the `run` keyword to run actions or scripts.

!image.png

You can store the script in your repository, often done in a `.github/scripts/` directory, and then supply the path and shell type using the `run` keyword.

!image.png

# **Cache dependencies with the cache action**

When you build out a workflow, you often need to reuse the same outputs or download dependencies from one run to another. Instead of downloading these dependencies over and over again, you can cache them to make your workflow run faster and more efficiently. Caching dependencies reduces the time it takes to run certain steps in a workflow, because jobs on GitHub-hosted runners start in a clean virtual environment each time.

To cache dependencies for a job, use GitHub's `cache` action. This action retrieves a cache identified by a unique key that you provide. When the action finds the cache, it then retrieves the cached files to the path that you configure. To use the `cache` action, you need to set a few specific parameters:

!image.png

!image.png

# **Pass artifact data between jobs**

Similar to the idea of caching dependencies within your workflow, you can pass data between jobs inside the same workflow. You can pass the data by using the `upload-artifact` and `download-artifact` actions. Jobs that are dependent on a previous job's artifacts must wait for the previous job to complete successfully before they can run. This approach is useful if you have a series of jobs that need to run sequentially based on artifacts uploaded from an earlier job. For example, `job_2` requires `job_1` by using the `needs: job_1` syntax.

!image.png

# **Enable step debug logging in a workflow**

(Very interesting to know)

In some cases, default workflow logs don’t provide enough detail for you to diagnose why a specific workflow run, job, or step fails. In these scenarios, you can enable more debug logging for two options: *runs* and *steps*. Enable this diagnostic logging by setting two repository secrets that require `admin` access to the repository to `true`.

- To enable run diagnostic logging, set the `ACTIONS_RUNNER_DEBUG` secret in the repository that contains the workflow to `true`.
- To enable step diagnostic logging, set the `ACTIONS_STEP_DEBUG` secret in the repository that contains the workflow to `true`.

# **Access the workflow logs in GitHub**

When you think about successful automation, you aim to spend the least amount of time looking at what's automated so that you can focus on what's relevant. But sometimes things don't go as planned, and you need to review what happened. That debugging process can be frustrating.

GitHub has a clear, structured layout to help you quickly move between jobs while you retain the context of the current debugging step.

To view the logs of a workflow run in GitHub:

1. In your repository, go to the **Actions** tab.
2. In the left pane, select the workflow.
3. In the list of workflow runs, select the run.
4. Under **Jobs**, select the job.
5. Read the log output.

If you have several runs inside a workflow, you can select the **Status** filter after you select your workflow and set it to **Failure** to display only the failed runs in that workflow.

# **Access the workflow logs from the REST API**

In addition to viewing logs via GitHub, you can use the GitHub REST API to view logs for workflow runs, rerun workflows, or even cancel workflow runs.

For example, a `GET` request to view a specific workflow run log follows this path:

!image.png

# **Identify when to use an installation token from a GitHub app**

When your GitHub app is installed on an account, you can authenticate it as an app installation by using the `installation access token` for REST and GraphQL API requests

In the following example, you replace `INSTALLATION_ACCESS_TOKEN` with the installation access token:

!image.png

You can also use an installation access token to authenticate for HTTP-based Git access. Your app must have the `Contents` repository permission. You can then use the installation access token as the HTTP password.

# **Options for triggering a CD workflow**

There are several options for starting a CD workflow. In the previous module on CI with GitHub Actions, you learned how to trigger a workflow from a push to the GitHub repository. However, for CD, you might want to trigger a deployment workflow on some other event.

One option is to trigger the workflow with *ChatOps*. ChatOps uses chat clients, chatbots, and real-time communication tools to run tasks. For example, you might leave a specific comment in a pull request that can kick off a bot. That bot might comment back with some statistics or run a workflow.

Another option, and the one we use in our example, is to use labels in your pull request. Different labels can start different workflows. For example, add a *stage* label to begin a deployment workflow to your staging environment, or add a *spin up environment* label to run the workflow that creates the Microsoft Azure resources for you to deploy to. To use labels, your workflow looks like this:

!image.png

# **Control execution with a job conditional**

Often, you only want to run a workflow if a certain condition is true.

!image.png

# **Store credentials with GitHub Secrets**

You never want to expose sensitive information in the workflow file. GitHub Secrets is a secure place to store sensitive information that your workflow needs.

To store information in GitHub Secrets, create a secret on the portal.

!image.png

# **Deploy to Microsoft Azure using GitHub Actions**

You can also search and browse GitHub Actions directly in a repository's workflow editor. From the sidebar, you can search for a specific Action, view featured Actions, and browse featured categories.

# **Create and delete Azure resources by using GitHub Actions**

Because CD is an automated process, you've already decided to use infrastructure as code to create and take down the environments you deploy to. GitHub Actions can automate these tasks on Azure, and you can include these actions in your workflow.

!image.png

Here's an example of the steps in the `set-up-azure-resources` job:

!image.png

# **Remove workflow artifacts from GitHub**

default, GitHub stores any build logs and uploaded artifacts for 90 days before it deletes them. You can customize this retention period based on the type of repository and the usage limits set for your specific GitHub product.

!image.png

## **Manually delete artifacts from your repository**

To manually delete an artifact on GitHub, navigate to the **Actions** tab, select the workflow from the left sidebar, and then select the run you want to see.

Under **Artifacts**, delete the artifact you want to remove.

You can also use the Artifacts REST API to delete artifacts. This API also allows you to download and retrieve information about work artifacts.

## **Change the default retention period**

You can change the default artifact and log retention period for your repository, organization, or enterprise account. Keep in mind that changing the retention period only applies to new artifacts and log files. It doesn't apply to existing objects.

The following example uploads an artifact that persists for 10 days instead of the default 90 days:

!image.png

# **Add a workflow status badge to your repository**

It's helpful to know a workflow's status without having to visit the **Actions** tab to see if it successfully completed. Adding workflow status badges to your repository `README.md` file allows you to quickly see if your workflows are passing or failing. While it's common to add a status badge to a repository `README.md` file, you can also add it any web page. By default, status badges display the workflow statuses on your default branch, but you can also display workflow status badges on other branches using the `branch` and `event` parameters.

Here's an example of what you need to add to a file to see a workflow status badge:

```yaml
!example branch parameter.
```

For example, adding the `branch` parameter along with the desired branch name at the end of the URL shows the workflow status badge for that branch instead of the default branch. This practice makes it easy to create a table-like view within your `README.md` file to display workflow statuses based on branches, events, services, or environments to name a few.

!image.png

You can also create a status badge using GitHub. Navigate to the workflows section within the **Actions** tab and select a specific workflow. The **Create status badge** option allows you to generate the markdown for that workflow and set the `branch` and `event` parameters.

!image.png

# **Add workflow environment protections**

Security is a big deal, so it makes sense to configure your workflow environment with protection rules and secrets. With these elements in place, a job doesn't start or access any defined secrets in the environment until all of the environment's protection rules pass. Currently, protection rules and environment secrets only apply to public repositories.

There are two environment protection rules that you can apply to workflows within public repositories, *required reviewers* and *wait timer*.

- **Required reviewers** allow you to set a specific person or team to approve workflow jobs that reference the job's environment.
- You can use **Wait timer** to delay a job for a specific amount of time after the job has been triggered.

Suppose you need to create a workflow to a production environment that a dev team needs to approve before the deployment occurs. Use the following steps:

1. Create a production environment within the repository.
2. Configure the required reviewers environment protection to require an approval from the specific dev team.
3. Configure the specific job within the workflow to look for the production environment.

You can create and configure new repository environments from the repository's **Settings** tab under **Environments**.

# **What is GitHub Script?**

GitHub Script is an action that provides an authenticated Octokit client and enables JavaScript to be written directly in a workflow file. It runs in Node.js, so you have the power of that platform available when you write scripts.

# **What is Octokit?**

Octokit is the official collection of clients for the GitHub API. One of these clients, rest.js, provides JavaScript access to the GitHub REST interface.

You have always been able to automate the GitHub API via octokit/rest.js, although it could be a chore to properly set up and maintain. One of the great advantages to using GitHub Script is that it handles all of this overhead so you can immediately start using the API. You don't need to worry about dependencies, configuration, or even authentication.

# **What can octokit/rest.js do?**

The short answer is that it can do virtually anything with respect to automating GitHub. In addition to having access to commits, pull requests, and issues, you also have access to users, projects, and organizations. You can retrieve lists of commonly used files, like popular licenses or `.gitignore` files. You can even render Markdown.

# **How is using GitHub Script different from octokit/rest.js?**

The main difference in usage is that GitHub Script provides a preauthenticated octokit/rest.js client named `github`.

So instead of

`octokit.issues.createComment({`

you use

`github.issues.createComment({`.

In addition to the `github` variable, the following variables are also provided:

- `context` is an object that contains the context of the workflow run.
- `core` is a reference to the @actions/core package.
- `io` is a reference to the @actions/io package.

# **Creating a workflow that uses GitHub Script**

GitHub Script actions fit into a workflow like any other action. As a result, you can even mix them in with existing workflows, like those you might have already set up for CI/CD.

You'll start off with a `name` and an `on` clause that specifies that the workflow runs when issues are opened:

!image.png

Next, you'll define a job named `comment` that runs on Linux with a series of steps:

!image.png

In this case, there's only one step: the GitHub Script action.

!image.png

# **Running from a separate file**

You might sometimes need to use a significant amount of code to meet your GitHub Script scenario. When that happens, you can keep the script in a separate file and reference it from the workflow instead of putting all the script inline.

Here's an example of a simple workflow that uses that technique:

!image.png

# **What is GitHub Packages?**

GitHub Packages is a package-management service that makes it easy to publish public or private packages next to your source code.

# **GitHub Packages is a package registry**

GitHub Packages allow you to share your project dependencies within your organization or publicly.

When you work on a project that has package dependencies, it’s important for you to trust them, understand their code, and connect with the community who built them. Within organizations, you also need to be able to quickly find what’s been approved for your use.

GitHub Packages use the same familiar GitHub interface to find public packages anywhere on GitHub, or private packages within your organization or repositories.

# **A standard package manager**

GitHub Packages is compatible with common package-management clients, so you can publish packages with your choice of tools. If your repository is more complex, you may need to publish multiple packages of different types. You can also use webhooks or GitHub Actions to fully customize your publishing and post-publishing workflows.

Are you publishing an open-source package? Many open-source projects store their code on GitHub, so you can publish prerelease versions of your packages for testing within your community, then easily promote specific versions to the public registry of your choice.

# **GitHub Packages is also a container registry**

From complete applications to CLI utilities, containers are another form of distributing code. GitHub Packages allow you to publish and distribute container images. Once published (in public or in private) you can use these images from anywhere, including:

- In your local development environment
- As a base image from your GitHub Codespaces development environment
- As a step to execute into your Continuous Integration (CI) / Continuous Deployment (CD) workflow with GitHub Actions
- On a server or a cloud service

# **Compare GitHub Packages to GitHub Releases**

GitHub Packages are used to publish releases of your libraries to a standard package feed or a container registry. They are meant to leverage the ways the specific package-management client works with that feed, like linking back to the repository in which the package was created as well as the version of the code that was used.

GitHub Releases are used to release a bundle of packaged software, along with release notes and links to binary files. You can download those releases directly from their unique URL and track them back to the specific commit they were created from. You can only download releases as tarballs or ZIP files.

# **Unified identity and permissions**

Let's say you're working on a project using GitHub for hosting source code: JavaScript for the front end, with npm and Java for the back end. You now maintain at least three different sets of user credentials and permissions: for Git, npm, and Maven repositories.

With GitHub Packages, you can use a single set of credentials across your source-code repository, your private npm registry, and your Maven or Gradle private registry. Packages published through GitHub inherit the visibility and permissions assigned at the repository level. A new team member needs read access to a package and its code? Give them read access to the repository and it's done!

# **Build and publish packages from GitHub**

GitHub Actions is another GitHub feature that allows you to automate your software workflows. You can build, test, and deploy your code right from GitHub.

By combining GitHub Actions and GitHub Packages, you can build a workflow that will build and test your code, and then publish it to GitHub Packages by simply pushing code to your repository.

# **Use a workflow to publish to GitHub Packages**

GitHub Packages let you securely publish and consume packages, store your packages alongside your code, and share your packages privately with your team or publicly with the open-source community. You can also use GitHub Actions to automate your packages.

Following is an example of a basic workflow that runs whenever a new release is created in a repository. If the tests pass, then the package is published to GitHub Packages.

!image.png

# **Use GitHub Container Registry to host and manage Docker container images**

GitHub Packages support the use of containers, Kubernetes, and other cloud-native technologies to manage their entire application lifecycle including production operations, development, release, and deployment. GitHub Packages also offers a container registry designed to support the unique needs of container images. You can use GitHub Container Registry to seamlessly host and manage Docker container images in your GitHub organization or personal user account. GitHub Container Registry allows you to configure who can manage and access packages using fine-grained permissions.

With the container registry, you can:

- Store container images within your organization and user account rather than a repository.
- Set fine-grained permissions for the container images.
- Access public container images anonymously.

After you've built the image and authenticated and signed in to the GitHub Container Registry service at `ghcr.io`, you can then tag and push the latest version of the image to the container registry using these commands:

!image.png

# **Authenticate to GitHub Packages**

The way you authenticate into your package manager will depend on your project's ecosystem. Whichever ecosystem you're working with, you'll need three pieces of information:

- Your GitHub username
- A Personal Access Token
- The GitHub Packages endpoint for your package ecosystem

# **Generate a Personal Access Token**

To install, publish, or delete a package, you need an access token. When using your package manager, you must generate a Personal Access Token (PAT). You can generate a PAT via your profile settings.

!image.png

# **Log in into GitHub Packages**

Before publishing or installing packages from GitHub Packages, you'll need to authenticate in your package manager. The endpoint will look like `https://PACKAGE_TYPE.pkg.github.com/OWNER/REPOSITORY`, where `PACKAGE_TYPE` is the type of package ecosystem you're using.

The following table shows you the command to run in order to authenticate to GitHub Packages based on your package ecosystem:

| Your package ecosystem | Command line to authenticate to GitHub Package |
| --- | --- |
| NuGet | `dotnet nuget add source https://nuget.pkg.github.com/OWNER/index.json -n github -u OWNER -p [Your PAT Token]` |
| npm | `bash npm login --registry=https://npm.pkg.github.com` |
| RubyGems | `echo ":github: Bearer GH_TOKEN" >> ~/.gem/credentials` |
| Maven & Gradle | You can directly authenticate while pushing. |

If you want to learn more about how to use GitHub Packages with your project's environment, you can read the documentation here.

# **Install a package**

When you're authenticated, you can easily use published packages in your projects. Each package home page shows you the command to run, depending on your project environment.

!image.png

# **Manage packages**

GitHub Packages offer you several ways to easily manage your package lifecycles and workflows.

You can manage GitHub Packages through the GitHub API and the GraphQL API. These APIs allows you to support advanced integrations scenarios. For example, with GitHub's Webhook feature, you can run code when a new package is published. Imagine you're a maintainer of an open-source project. With webhooks, you could automatically publish a new Tweet or a blog post when a new package is published.

You can also use GitHub Actions to automate package management. With the delete-package-versions action, you can automatically prune the oldest version of your packages while publishing a new version.

# **Create a custom GitHub action**

GitHub Actions is a powerful feature that helps you to go from code to cloud, all from the comfort and convenience of your own repository. Here, you'll learn about the different types of GitHub actions and the metadata, syntax, and workflow commands to create custom GitHub actions.

# **Types of GitHub actions**

!image.png

Actions are individual tasks that you can use to customize your development workflows. You can create your own actions by writing custom code that interacts with your repository to perform custom tasks, or by using actions the GitHub community shares. Navigating through various actions, you'll notice that there are three different types of actions: *Docker container actions*, *JavaScript actions*, and *composite run steps actions*. Let's take a closer look at each action type.

## Docker container actions

Docker containers package the environment with the GitHub Actions code. This means that the action runs in a consistent and reliable environment because all of its dependencies are within that container. If the action needs to run in a specific environment configuration, Docker containers are a good way to go because you can customize the operating system and tools. The downside is that because the job has to build and retrieve the container, Docker container actions are often slower than JavaScript actions.

Before building a Docker container action, you should have some basic understanding of how to use environment variables and the Docker container filesystem. The steps to take to build a Docker container action are then minimal and straightforward:

1. Create a `Dockerfile` to define the commands to assemble the Docker image.
2. Create an `action.yml` metadata file to define the inputs and outputs of the action. Set the `runs: using:` value to `docker` and the `runs: image:` value to `Dockerfile` in the file.
3. Create an `entrypoint.sh` file to describe the docker image.
4. Commit and push your action to GitHub with the following files: `action.yml`, `entrypoint.sh`, `Dockerfile`, and `README.md`.

## **JavaScript actions**

JavaScript actions can run directly on the runner machine, and separate the action code from the environment that's used to run the action. Because of this, the action code is simplified and can execute faster than actions within a Docker container.

As a prerequisite for creating and using packaged JavaScript actions, you need to download Node.js, which includes npm. As an optional step (but one that we recommend) is to use GitHub Actions Toolkit Node.js, which is a collection of Node.js packages that allows you to quickly build JavaScript actions with more consistency.

The steps to build a JavaScript action are minimal and straightforward:

1. Create an `action.yml` metadata file to define the inputs and outputs of the action, as well as tell the action runner how to start running this JavaScript action.
2. Create an `index.js` file with context information about the Toolkit packages, routing, and other functions of the action.
3. Commit and push your action to GitHub with the following files: `action.yml`, `index.js`, `node_modules`, `package.json`, `package-lock.json`, and `README.md`.

## **Composite run steps actions**

Composite run steps actions allow you to reuse actions by using shell scripts. You can even mix multiple shell languages within the same action. If you have many shell scripts to automate several tasks, you can now easily turn them into an action and reuse them for different workflows. Sometimes it's easier to just write a shell script than using JavaScript or wrapping your code in a Docker container.

## **Packaged composite action**

Packaged composite actions bundle multiple steps into a reusable unit. These actions are defined in a repository and can be referenced in workflows across different repositories. Packaging a composite action simplifies workflows, reduces duplication, and improves maintainability.

When creating a packaged composite action, the steps are defined in a single `action.yml` file. This file specifies the inputs, outputs, and the sequence of commands or actions to execute. Packaged composite actions are particularly useful for automating repetitive tasks or combining multiple shell commands into a single reusable action.

## **Add outputs to a composite action**

Composite actions can define outputs that workflows can use to pass data between steps or jobs. Outputs are particularly useful for sharing results or computed values from one action to another.

## **Best practices for composite actions**

!image.png

## **Composite action in a workflow**

Composite actions are a powerful way to simplify workflows by bundling multiple steps into a reusable unit. These actions allow you to define a sequence of commands or actions in a single `action.yml` file, making it easier to maintain and reuse logic across workflows.

## **Benefits of composite actions**

- **Reusability** - Define actions once and use them in multiple workflows.
- **Maintainability** - Reduce duplication by centralizing logic in a single action.
- **Modularity** - Combine multiple shell commands or other actions into a single unit.

# **Develop an action to set up a CLI on GitHub Actions runners**

Many CI/CD workflows require a **specific version of a CLI tool** to interact with cloud services, manage infrastructure, or execute scripts. While GitHub-hosted runners come preinstalled with many tools, they may not include the exact version your workflow needs, especially if it's an older or unsupported version. Instead of installing the required CLI version in every workflow, you can create a **reusable GitHub Action** that:

- Ensures consistent installation of the required CLI version across jobs.
- Simplifies workflows by centralizing the installation logic.
- Optimizes caching for faster workflow execution.

## **Best practices for CLI setup actions**

!image.png

# **Troubleshoot JavaScript actions**

!image.png

# **Use Logging for Debugging**

### **Log messages with `core.info`, `core.debug`, and `console.log`**

```jsx
const core = require('@actions/core');

core.info("This is an info message");
core.debug("This is a debug message");
console.log("This is a raw console log");
```

✅ Use core.debug for debug logs — these only appear when ACTIONS_STEP_DEBUG is set to true.

# **Enable debug logging**

You can enable **step-level debug logs** by setting the following **secret**:

```
ACTIONS_STEP_DEBUG=true
```

To enable **runner diagnostic logs**, set:

```
ACTIONS_RUNNER_DEBUG=true
```

# **Set secrets for debugging**

1. Go to your GitHub repository.
2. Navigate to **Settings** > **Secrets and variables** > **Actions**.
3. Add new secrets with the following names and values:
    - **ACTIONS_STEP_DEBUG**: `true`
    - **ACTIONS_RUNNER_DEBUG**: `true`

# **Inspect workflow logs**

When a workflow fails, click on the failed job in the Actions tab. Expand each step to:

- View detailed logs
- Check standard output (stdout)
- See the exit code of scripts
- Identify unhandled exceptions

# **Best practices for debugging JavaScript actions**

!image.png

# **Troubleshoot Docker container actions**

Docker container actions are powerful for encapsulating complex tools and environments in GitHub Actions workflows. However, debugging these actions can be more challenging than JavaScript actions due to their isolated runtime environment. This unit will guide you through identifying, diagnosing, and resolving issues with Docker-based actions.

## **Common issues in Docker container actions**

| Problem | Cause | Suggested fix |
| --- | --- | --- |
| Action fails to start | `ENTRYPOINT` or `CMD` misconfigured | Confirm Dockerfile uses `ENTRYPOINT` correctly |
| Missing dependencies | Dependencies not installed or misconfigured | Ensure all packages are installed in the image |
| Inputs not received | `INPUT_` environment variables not accessed | Use `process.env.INPUT_<INPUT_NAME>` (or shell equivalent) |
| File not found | Incorrect file path inside container | Use absolute paths or validate directory structure |
| Permission denied | File or script lacks execution permission | Add `RUN chmod +x <script>` in Dockerfile |
| Network-related failures | External services not accessible | Validate network settings and retry logic |

## **Understand the Docker action lifecycle**

Before troubleshooting, it's helpful to understand how Docker container actions run.

### **1. Workflow Trigger**

A GitHub Actions workflow starts in response to a configured event—such as a `push`, `pull_request`, or manual `workflow_dispatch`.

### **2. Runner Setup**

GitHub provisions a fresh virtual machine (the **runner**) to execute the workflow. The runner prepares the environment by downloading action definitions and resolving dependencies.

### **3. Action Resolution**

If the action specifies `runs.using: docker` in its `action.yml` file, GitHub recognizes it as a Docker-based action.

### **4. Image Build or Pull**

GitHub builds the Docker image defined in the action’s `Dockerfile` or pulls a prebuilt image if specified. This image defines the environment in which the action code runs.

### **5. Container Execution**

The runner launches the Docker container, mounts the workspace, and injects environment variables, including secrets and inputs defined in the workflow.

### **6. Entrypoint Runs**

GitHub executes the `entrypoint` command from the Dockerfile inside the container. This is where the custom action logic runs, typically a script or application.

### **7. Result Handling**

Any outputs set by the container action are captured by the runner and passed along to subsequent steps in the workflow. Once complete, the container shuts down and the runner is discarded.

> Note: Docker container actions run in a clean, isolated environment. File system state, installed tools, and environment variables must all be defined within the Dockerfile.
>

## **Debugging techniques**

#### **1. Add Logging**

Use echo, printf, or logging statements in your entrypoint script:

**Bash**

```
echo "Starting Docker action..."
echo "Input VALUE: $INPUT_VALUE"
```

Log all critical inputs and steps to diagnose where the failure occurs.

#### **2. Build and test locally**

Use Docker on your machine to simulate the container behavior:

**Bash**

```
docker build -t my-action .
docker run -e INPUT_NAME=value my-action
```

Ensure environment variables mimic the ones GitHub sets.

#### **3. Use the act CLI to simulate GitHub workflows**

Install act to run your GitHub workflows locally:

**Bash**

```
act -j test-job
```

Great for testing Docker actions in workflows without pushing to GitHub.

#### **4. Validate Dockerfile configuration**

Ensure you define either ENTRYPOINT or CMD. Copy your scripts into the image and give them execute permission:

**Dockerfile**

```
COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
```

### **5. Inspect GitHub logs**

Click on a failed workflow run, expand each step, and examine:

- Docker build logs
- Standard output and standard error from the container
- Exit codes and stack traces 🔍 Logs often reveal missing packages, syntax issues, or permission errors.

### **Example: Entry point error**

**plaintext**

```
Error: container_linux.go:380: starting container process caused "exec: \"/entrypoint.sh\": permission denied"
```

✅ Fix: Add RUN chmod +x /entrypoint.sh in your Dockerfile.

## **Environment variable mapping**

| **GitHub input** | **Docker environment variable** |
| --- | --- |
| `with: name: test` | `INPUT_NAME=test` |
| `GITHUB_WORKSPACE` | Mapped into `/github/workspace` |
| `GITHUB_EVENT_NAME` | Event that triggered the workflow |

**Bash**

```
echo "Input was $INPUT_NAME"
echo "Working dir: $GITHUB_WORKSPACE"
```

## **Use troubleshooting secrets**

Enable additional logging by adding these secrets to your repository or org:

| Secret | Description |
| --- | --- |
| `ACTIONS_STEP_DEBUG` | Enables debug logging |
| `ACTIONS_RUNNER_DEBUG` | Enables runner diagnostics |

## **Best practices for Docker action debugging**

| **Best Practice** | **Why** |
| --- | --- |
| **Use logging generously** | Helps trace failure points |
| **Always set chmod +x** | Avoid permission errors on shell scripts |
| **Validate inputs in your script** | Catch missing or malformed inputs early |
| **Minimize container dependencies** | Smaller images are easier to debug |
| **Test locally with docker run or act** | Faster iterations and immediate feedback |

# **Publish a custom GitHub action**

Here, you'll learn about choosing the right visibility for your action, best practices for documenting and versioning your action, and how to publish your action to the GitHub Marketplace.

## **Choose a location for your action**

!image.png

When creating an action, it's important to first decide where you want that action to live and the visibility of that action, whether it will be `public` or `private`. The action's visibility determines which recommendations and requirements are needed to release that action. Let's take a closer look into these two visibility options.

## **Public**

Workflows in any repository can use public actions. If you're developing an action with the intent to make it open source or make it publicly available through the GitHub Marketplace, we recommend (and in most cases it's required) that the action has its own repository instead of bundling it with other application code. This allows you to version, track, and release the action just like any other piece of software. This makes it easier for the GitHub community to discover the action, narrows the scope of the code base for developers fixing issues and extending the action, and separates the action's versioning from the versioning of other application code.

## **Private**

When an action is in a private repository, only workflows in the same repository can use the action. With private actions, you can store the action's files in any location in your repository. If you plan to combine action, workflow, and application code in a single repository, we recommend storing the action in the `.github` directory. For example, `.github/actions/action-a` and `.github/actions/action-b`.

## **Document your action**

It can be very frustrating to use a new tool or application when the documentation is vague or even missing. It's important to include good documentation with your action so that others can see how it works, whether you plan to make it public or private. The first thing to do is create a good `README.md` file for your action.

The `README.md` file is often the first place developers will look at to see how the action works. This is a great place to include all of the important information for the action. The following is a non-exhaustive list of things to include:

- A detailed description of what the action does.
- Required input and output arguments.
- Optional input and output arguments.
- Secrets the action uses.
- Environment variables the action uses.
- An example of how to use your action in a workflow.

As a general rule, the `README.md` file should include everything a user should know to use the action. If you think it could be useful information, include it in the `README.md`.

## **Release and version your action**

If you're developing an action for other people to use, whether it's public or private, you should define a release-management strategy to control how updates are distributed. Major version updates including necessary critical fixes and security patches that affect compatibility need to be documented clearly.

## **Good practices for release and version management**

A good release-management strategy should include versioning recommendations. Users shouldn't be referencing an action's default branch with the action. This is because the default branch that's likely to contain the latest code (which might or might not be stable) could result in your workflow breaking. Instead, we recommend that users specify a major version when using the action, and to only direct them to a more specific version if they encounter issues. They can do this by configuring their GitHub Actions workflow to target a tag, a commit's SHA, or a specific branch named for a release. Let's take a closer look at these release options.

### **Tags**

Tags can be a good way to manage releases for an action. By using tags, users can easily distinguish between major and minor versions. The following is a list of helpful practices to consider when creating releases:

- Create and validate a release on a release branch (such as `release/v1`) before creating the release tag (for example, `v1.0.2`).
- Use semantic versioning.
- Move the major version tag (such as `v1`, `v2`) to point to the Git ref of the current release.
- Introduce a new major version tag (`v2`) for changes that will break existing workflows.
- Release major versions with a beta tag to indicate their status; for example, `v2-beta`. You can remove the `beta` tag when the release is ready.

Here are some examples of each option.

```yaml
steps:
    - uses: actions/javascript-action@v1
    - uses: actions/javascript-action@v1.0.1
    - uses: actions/javascript-action@v1-beta
```

### **Use a commit's SHA**

Tags are useful and widely used, but one downside to using tags is that they can be deleted or moved. With Git, each commit receives a calculated SHA value, which is unique and can't be modified. Using a commit SHA for versioning will give you the most reliable and secure way to version and use an action. However, often in Git you can abbreviate the SHA hash to the first several characters, and Git will recognize the reference. If you're using the commit's SHA for release management, you need to use the full SHA value and not the abbreviated value.

```yaml
steps:
    - uses: actions/javascript-action@2522385f6f7ba04fe7327647b213799853a8f55c
```

## **Publish an action to the GitHub Marketplace**

!Rendering that says GitHub Marketplace, tools to build on and improve your workflow.

When you're ready to share your action with the GitHub community, you can publish it to the GitHub Marketplace and reach out to millions of GitHub users. Actions published to the GitHub Marketplace are published immediately if all of the requirements are met. Actions that don't meet the requirements need to be reviewed by GitHub before being published. You'll need to ensure that the repository only includes the metadata file, code, and files necessary for the action. Creating a single repository for the action allows you to tag, release, and package the code in a single unit. GitHub also uses the action's metadata on your GitHub Marketplace page.

Following are the requirements to publish an action to the GitHub Marketplace. They apply to both Docker container-based actions and JavaScript-based actions:

- The action must be in a public repository.
- Each repository must contain a single action.
- The action's metadata file (`action.yml` or `action.yaml`) must be in the root directory of the repository.
- The `name` in the action's metadata file must be unique on the GitHub Marketplace.
    - The name can't match a user or organization on GitHub, unless the user or organization owner is publishing the action. For example, only the GitHub organization can publish an action named `github`.
    - The `name` can't match an existing GitHub Marketplace category.
    - The `name` can't match an existing GitHub feature.

You can add the action you've created to the GitHub Marketplace by tagging it as a new release and then publishing it. There are a few guided steps in GitHub that allow you to publish a release of your action. You can find more information on these steps in the Summary section at the end of this module.

# GitHub enterprise models

Deployment models:

- GitHub Enterprise Cloud (GHEC)
- GitHub Enterprise Server (GHES)

## **How users are managed**

GitHub Enterprise Cloud (GHEC) is a cloud-based solution tailored for organizations that need the flexibility and scalability of GitHub's infrastructure while maintaining enterprise-grade security and compliance. It offers features such as centralized user management, data residency options, and integration with identity providers like SAML and SCIM for single sign-on and user provisioning.

GHEC is ideal for globally distributed teams, as it ensures high availability, automated backups, and seamless access to GitHub’s robust collaboration tools. It supports regulatory compliance and provides detailed audit logs for governance. With GHEC, organizations can leverage GitHub’s advanced capabilities without managing on-premises infrastructure.

## **Standard User Model**

This is the traditional user model on GitHub, where users manage their own account.

**Characteristics of standard user model**:

- Users manage their accounts, including usernames, email addresses, and personal settings.
- Authentication methods are configured by users (e.g., OAuth, password).
- The connection to organizations or enterprise accounts is established through invitations.
- Admins have limited control over user accounts once they are connected to an enterprise organization.
- Often used in smaller-scale enterprises or organizations without strict centralized management needs.

## **The Enterprise Managed User (EMU) model**

This is the model designed for organizations requiring centralized user account management, offering better control and compliance features.

**In EMU model**:

- User accounts are fully managed by the enterprise.
- Authentication is handled through the enterprise’s identity provider (e.g., SAML, SCIM).
- Admins have control over usernames, email addresses, and user lifecycle (creation, deactivation).
- Managed users can only join the enterprise they belong to and cannot independently join other organizations.
- Often used by larger enterprises or organizations requiring compliance with stricter governance policies.

## **GitHub Enterprise Cloud (GHEC) with Data Residency**

GitHub Enterprise Cloud (GHEC) with **Data Residency** is an important feature of GHEC for organizations that must comply with data protection regulations by ensuring specific data is stored in particular geographical regions.

**It allows users to**:

1. **Data Residency Regions**:
    - GitHub provides predefined regions (e.g., the United States, Europe) based on your compliance needs.
2. **Data Types Covered**:
    - **Repository Contents**: Code, issues, pull requests, and repository-related data.
    - **Metadata**: Includes audit logs, user events, and settings.
3. **Encryption and Security**:
    - All data in transit and at rest is encrypted.
    - Compliance with regional and global security standards (e.g., ISO 27001, SOC 2).
4. **Regional Storage and Failover**:
    - Data is stored in the selected region with high availability and disaster recovery mechanisms.
    - Certain types of backup data may be replicated outside the region but remain encrypted.

## **GitHub Enterprise Server (GHES)**

GitHub Enterprise Server (GHES) is a self-hosted version of GitHub designed for organizations that require complete control over their GitHub instance. It is ideal for companies that want to host their repositories and associated data on-premises or in a private cloud environment for compliance, security, or performance reasons.

### **Key Features of GitHub Enterprise Server**

1. **Self-Hosting**:
    - GHES is deployed within the organization’s infrastructure, either on-premises or on a private cloud.
    - Provides full control over data storage, backups, and disaster recovery.
2. **Enhanced Security**:
    - Data remains within the organization's environment, ensuring compliance with strict data residency, privacy, or security requirements.
    - Integrates with enterprise authentication systems (e.g., LDAP, SAML, Active Directory).
3. **Customization and Control**:
    - Full control over repository settings, user access, and permissions.
    - Allows custom integrations and extensions, such as internal DevOps tools or CI/CD pipelines.
4. **Performance Optimization**:
    - Optimized for organizations with large repositories or distributed teams.
    - Can be scaled to handle heavy workloads by allocating additional resources (e.g., CPU, RAM).
5. **Compliance**:
    - Ideal for industries with strict regulatory requirements (e.g., healthcare, finance).
    - Offers audit logs, user activity monitoring, and data encryption to meet compliance standards.

### **Components of GHES**

1. **GitHub Application**:
    - The interface and core functionality (e.g., repositories, pull requests, issues) are similar to GitHub.com.
2. **Administrative Dashboard**:
    - Provides tools for managing users, teams, and organizations.
    - Includes settings for repository access, backups, and updates.
3. **System Administration Tools**:
    - Tools for managing server configuration, resource allocation, and performance monitoring.
4. **Backup and Disaster Recovery**:
    - Features for creating and restoring backups to prevent data loss.
5. **Monitoring and Logging**:
    - Built-in monitoring tools like Prometheus and Grafana for server health.
    - Logs for troubleshooting and auditing.

# **Manage actions and workflows**

The content is structured around the level at which the tools presented are available: *enterprise level* or *organizational level*.

## **At enterprise level**

### **Configure a GitHub Actions use policy**

GitHub Actions workflows often contain actions, which are sets of standalone commands to be executed within the workflow. When creating a workflow, you can create your own actions to use, or reference public community actions available from GitHub Marketplace. For this reason, configuring a use policy for workflows and actions in your enterprise is essential to prevent users from using malicious third-party actions.

You have several options in Enterprise Cloud to configure a policy, as well as in Enterprise Server if GitHub Connect is enabled in your enterprise settings.

To configure a GitHub Actions use policy for your enterprise, navigate to your enterprise account and then to **Policies > Actions** in the sidebar. The following options should appear.

!Screenshot of the Actions screen with default options selected.

The dropdown at the top labeled **Enable for all organizations** enables you to decide which organizations in your enterprise can use GitHub Actions (all of them, some of them or none of them), while the three options underneath enable you to define the restriction level of GitHub Actions within these organizations.

If you want to enable only specific actions to be used within your enterprise, select **Allow enterprise, and select non-enterprise, actions and reusable workflows**, and choose the option corresponding to your use case.

!Screenshot of the Actions screen with Allow select actions option selected.

### **Manually sync public actions for Enterprise Server**

Most official GitHub-authored actions come automatically bundled with Enterprise Server, and they're captured at a point in time from the GitHub Marketplace. They include `actions/checkout`, `actions/upload-artifact`, `actions/download-artifact`, `actions/labeler`, and various `actions/setup-` actions, among others. To get all the official actions included on your enterprise instance, browse to the actions organization on your instance: https://HOSTNAME/actions.

As mentioned in the Configure a GitHub Actions use policy section, it's possible to configure Enterprise Server to automatically access the public actions available in the GitHub Marketplace and to configure a use policy for them. However, if you want stricter control over the public actions that should be made available in your enterprise, you can manually download and sync actions into your enterprise instance using the `actions-sync` tool.

## **At organization level**

### **Document corporate standards**

Creating a GitHub Actions workflow often involves writing multiple files and creating several repositories to specify the workflow in itself. Creating also includes the actions, containers, and/or runners to use in the workflow. Depending on the number of users in your Enterprise Cloud or Enterprise Server instance, things can get messy pretty quickly if you don't have corporate standards in place for creating GitHub Actions workflows.

As a best practice, we recommend you document the following in a GitHub wiki or as a markdown file in a repository accessible to all within an organization:

- Repositories for storage
- Files/folders naming conventions
- Location of shared components
- Plans for ongoing maintenance
- Contribution guidelines

## **Create workflow templates**

Workflow templates are a great way to ensure automation is reused and maintained in your enterprise. Both in Enterprise Cloud and Enterprise Server, users with write access to an organization's .github repository can create workflow templates that will be available for use to the other organization's members with the same write access. Workflow templates can then be used to create new workflows in the public and private repositories of the organization.

Creating a workflow template is done in two steps:

1. Create a yml workflow file.
2. Create a json metadata file that describes how the template should be presented to users when they're creating a workflow.

   **Note**

   The metadata file must have the same name as the workflow file. Instead of the .yml extension, it must be appended with .properties.json. For example, a file named octo-organization-ci.properties.json contains the metadata for the workflow file named octo-organization-ci.yml.


Both files must be placed in a public .github repository and in a directory named workflow-templates. You might have to create these if they don't already exist in your organization.

The following is an example of a basic workflow file:

```yaml
name: Octo Organization CI

on:
  push:
    branches: [ $default-branch ]
  pull_request:
    branches: [ $default-branch ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2

      - name: Run a one-line script
        run: echo Hello from Octo Organization
```

Note that the preceding file uses a $default-branch placeholder. When a workflow is created using your template, this placeholder is automatically replaced with the name of the repository's default branch.

Following is the metadata file you would create for the workflow file:

```json
{
    "name": "Octo Organization Workflow",
    "description": "Octo Organization CI workflow template.",
    "iconName": "example-icon",
    "categories": [
        "Go"
    ],
    "filePatterns": [
        "package.json$",
        "^Dockerfile",
        ".*\\.md$"
    ]
}
```

Metadata files use the following parameters:

| Parameter | Description | Required |
| --- | --- | --- |
| name | Name of the workflow template displayed in the list of available templates. | Yes |
| description | Description of the workflow template displayed in the list of available templates. | Yes |
| iconName | Defines an icon for the workflow's entry in the template list. Must be an SVG icon of the same name, and must be stored in the workflow-templates directory. For example, an SVG file named example-icon.svg is referenced as example-icon. | No |
| categories | Defines the language category of the workflow. When a user views the available templates, the templates that match the same language will feature more prominently. | No |
| filePatterns | Enables the template to be used if the user's repository has a file in its root directory that matches a defined regular expression. | No |

Once a workflow template is created, users in your organization can find it under **Actions > New workflow > Workflows created by _your_organization_name**.

!Workflow template example.

## **Reusable Templates for Actions and Workflows**

GitHub Actions allows for **workflow automation**, and a key part of managing workflows efficiently is using **reusable templates**. Reusable templates help standardize and streamline development across multiple repositories, reducing redundancy and improving maintainability.

Reusable templates in GitHub Actions refer to **predefined actions and workflows** that can be referenced and used across multiple projects. They ensure consistency and compliance with enterprise-wide standards.

### **Types of reusable templates**

| **Template Type** | **Purpose** | **Example** |
| --- | --- | --- |
| **Reusable Workflows** | Standardize CI/CD pipelines across repositories. | `ci-pipeline.yml`, `deploy-app.yml` |
| **Reusable Actions** | Encapsulate common automation logic. | `setup-env-action`, `security-scan-action` |
| **Workflow Templates** | Define reusable job structures. | `test-job.yml`, `build-job.yml` |

### **Reusable workflows**

A **reusable workflow** is a workflow defined in a separate repository that can be referenced in multiple projects. This allows organizations to **centralize** their CI/CD logic.

#### **Structure of a reusable workflow**

A reusable workflow is stored in `.github/workflows/` and uses the **`workflow_call`** trigger.

#### **Example: Standardized CI workflow (ci-pipeline.yml)**

```yaml
name: CI Pipeline
on:
  workflow_call:
    inputs:
      node-version:
        required: true
        type: string
    secrets:
      npm-token:
        required: true
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: ${{ inputs.node-version }}
          registry-url: 'https://npm.pkg.github.com/'
      - name: Install Dependencies
        run: npm install
      - name: Run Tests
        run: npm test
```

#### **Using a reusable workflow in another repository**

Once defined, the reusable workflow can be used in any repository via the **`uses:`** keyword.

**Example:** Calling the reusable workflow

```yaml
name: Reusable CI Pipeline
on: push
jobs:
  test:
    uses: org/reusable-workflows/.github/workflows/ci-pipeline.yml@v1
    with:
      node-version: '16'
    secrets:
      npm-token: ${{ secrets.NPM_TOKEN }}
```

#### **Benefits of using a reusable workflow**

- Ensures all repositories follow the same CI/CD structure.
- Reduces redundancy and maintenance overhead.
- Allows for **centralized updates** without modifying each repository.

## **Reusable actions**

A **GitHub Action** is a modular, reusable unit that executes specific automation tasks. Organizations often create custom actions to **encapsulate frequently used logic**.

### **Structure of a reusable action**

A reusable action is defined in an **action repository** with an `action.yml` file.

**Example:** Custom Setup Environment Action

```yaml
name: "Setup Environment"
description: "Sets up Node.js and installs dependencies"
inputs:
  node-version:
    description: "Node.js version"
    required: true
  registry-url:
    description: "NPM Registry URL"
    required: false
    default: "https://registry.npmjs.org/"
runs:
  using: "node16"
  main: "index.js"
```

### **Using a reusable action in a workflow**

Instead of repeating setup steps in every workflow, we use our custom action:

```yaml
name: Build & Test
on: push
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Setup Environment
        uses: org/actions/setup-env@v1
        with:
          node-version: '16'
```

### **Benefits:**

- Reduces duplication of setup logic across repositories.
- Simplifies workflow files, making them more readable.
- Centralizes updates—fixes or improvements in one place reflect across all workflows.

## **Workflow templates**

As discussed earlier, workflow templates help standardize automation across your organization by providing predefined structures for common tasks. These templates are a key part of the broader category of reusable workflows.

In the earlier section "Create workflow templates," we outlined how to build these templates from a `yml` file and a corresponding `.properties.json` metadata file.

To further connect the concept: workflow templates are a form of reusable workflow. When you create and store them in a public `.github` repository under the `workflow-templates/` directory, they allow other organization members to create consistent workflows for their repositories without having to define them from scratch.

By leveraging workflow templates, enterprises can:

- Enforce best practices across repositories.
- Accelerate onboarding and setup for new projects.
- Maintain consistency in CI/CD processes.

# **Control access and usage of actions in your enterprise**

## **Control access to actions within the enterprise**

Access control in GitHub Actions determines:

- **Who can run workflows that use actions.**
- **Which actions can be used within an organization.**
- **How self-hosted runners execute workflows securely.**
- **Who can modify and update shared GitHub Actions.**

Enterprise administrators need to **strike a balance** between giving developers flexibility and ensuring security and governance over automation workflows.

### **Organization-wide policies for controlling actions**

GitHub Enterprise enables administrators to set **organization-wide policies** that control how actions are used across all repositories within an organization. These policies help organizations **restrict the use of third-party actions, enforce security measures, and standardize automation workflows.**

## **Repository-level permissions for actions**

While organization-wide settings apply globally, **repository-level permissions** provide fine-grained control over **who can run workflows and modify actions**.

## **Managing repository-level workflow permissions**

Each repository can define **who can create, edit, and execute workflows**.

| **Permission Level** | **Capabilities** |
| --- | --- |
| **Read** | View workflows, but cannot trigger or edit them. |
| **Write** | Edit workflows, but cannot create new ones. |
| **Admin** | Create, edit, and manage workflow permissions. |

By default, GitHub sets workflow permissions to **"Read and Write"**, but enterprises should **restrict it to "Read" unless explicitly required** to prevent unauthorized modifications.

### **Restricting who can modify organization-owned actions**

If an organization hosts **reusable GitHub Actions** in a dedicated repository, access should be **limited to authorized users**.

- Use **branch protection rules** to prevent unauthorized modifications.
- Require **pull request approvals** for updates to actions.
- Restrict write access using **GitHub Teams & Role-Based Access Control (RBAC)**.

This ensures that any modifications to **core automation actions** are **reviewed before deployment**.

### **Restricting access to external actions**

GitHub Actions allows the use of **third-party actions**, but external actions **can pose security risks** if not properly vetted. Organizations should control which external actions can be used.

### **Risks of using external actions**

- **Malicious Code Execution**: An untrusted external action could introduce vulnerabilities.
- **Dependency Tampering**: A third-party action may introduce **supply chain attacks**.
- **Secrets Exposure**: External actions may inadvertently log secrets.

### **Configuring external action restrictions**

Administrators can **allow or block external actions** in **Organization Settings → Actions → Policies**.

Options include:

1. **Allow only GitHub-verified actions** (recommended for security).
2. **Allow specific external actions** via an allowlist.
3. **Block all external actions** (strictest security approach).

Example: **Allowing Only Verified External Actions**

- Navigate to **Organization Settings → Actions → Policies**.
- Under **"Allowed actions and reusable workflows"**, select:
    - **Allow only actions created by GitHub**.
    - **Manually approve any third-party actions before use**.
- Save the settings.

### **Implementing an allowlist for external actions**

Organizations that need specific third-party actions can **create an allowlist** by specifying trusted repositories.

Example: Allow only GitHub’s official `checkout` action:

**YAML**

```
- name: Checkout Repository
  uses: actions/checkout@v4
```

Administrators can define this allowlist in **Organization Settings → Actions → Policies**.

### **Securing self-hosted runners**

Enterprises that use **self-hosted runners** to execute GitHub Actions need to enforce **additional security measures**.

### **Risks of self-hosted runners**

| **Risk** | **Description** | **Mitigation** |
| --- | --- | --- |
| **Unauthorized Access** | Attackers could hijack runners to execute malicious actions. | Restrict access using **IP allow lists**. |
| **Secret Exposure** | Sensitive credentials may be leaked to compromised runners. | Store secrets in **GitHub Secrets Management** instead of environment variables. |
| **Runner Compromise** | If runners are not isolated, workflows from different teams could interfere. | Use **ephemeral runners** that reset after each job. |

### **Restricting access to self-hosted runners**

1. **Navigate to Organization Settings → Actions → Runners**.
2. **Create a Runner Group** to limit access.
3. **Restrict runner usage**:
    - Allow runners only for internal repositories.
    - Restrict usage to specific teams.

## **Configure organizational use policies for GitHub actions**

This guide provides a detailed explanation of how to configure organizational use policies for GitHub Actions, covering:

1. Allowed actions and reusable workflows.
2. Workflow permissions and security policies.
3. Self-hosted runner restrictions.
4. Secrets management and access control.
5. Monitoring and auditing workflows.

### **Allowed actions and reusable workflows**

One of the **primary security policies** enterprises should enforce is controlling **which GitHub Actions can be used within an organization**. Administrators can configure policies to:

- **Allow or block third-party actions**.
- **Restrict usage to organization-owned actions**.
- **Create an allowlist for approved external actions**.

!Screenshot of the genral actions permissions screen with default options selected

## **Configuring allowed actions policy**

1. Navigate to Organization Settings → Actions → Policies.
2. Under "Allowed Actions and Reusable Workflows", choose one of the following:
    - Allow all actions and reusable workflows (default, least restrictive).
    - Allow only actions created by GitHub (prevents third-party actions).
    - Allow only actions created within the organization (recommended for security).
    - Block all actions except those on an allowlist (strictest control).
3. If using an allowlist, specify the external actions that are permitted:

    ```yaml
    - actions/checkout@v4
    - actions/setup-node@v3
    ```

4. Save the settings.

## **Configuring workflow permissions and security policies**

Workflow permissions define **what access levels GitHub Actions workflows have in repositories**. These settings determine whether workflows **can modify repository content** or **only read** repository data.

### **Configuring default workflow permissions**

GitHub provides two options:

1. **Read-only (Recommended for Security)**
    - Workflows can **read** repository content but **cannot make changes**.
    - Prevents actions from **accidentally or maliciously modifying code**.
2. **Read and Write (Higher Risk)**
    - Workflows **can push commits, create issues, and modify settings**.
    - Should only be enabled for **trusted workflows**.

#### **Steps to configure default workflow permissions**

1. **Navigate to Organization Settings → Actions → General**.
2. **Under "Workflow Permissions"**, choose:
    - **Read-only** (Recommended).
    - **Read and write** (Only for specific cases).
3. **Save the settings**.

### **Enforcing approval for workflow runs**

To **prevent unauthorized workflow executions**, administrators can **require manual approval** before workflows run in **forked repositories**.

#### **Steps to require approval for external contributions**

1. **Navigate to Organization Settings → Actions → Policies**.
2. **Under "Fork Pull Request Workflows"**, select:
    - Require approval for workflows running from forks.
    - Allow automatic execution of forked workflows (not recommended).
3. **Save the settings**.

This policy **prevents security risks** associated with running workflows from **untrusted forks**.

### **Self-hosted runner restrictions**

Self-hosted runners provide more control over CI/CD execution but **introduce security risks** if not properly configured. Organizations should enforce policies to:

- **Restrict which repositories can use self-hosted runners**.
- **Limit access to specific teams**.
- **Use ephemeral (temporary) runners** for security.

### **Configuring runner access**

Administrators can **limit which repositories** have access to self-hosted runners.

#### **Steps to Restrict Self-Hosted Runner Usage**

1. **Navigate to Organization Settings → Actions → Runners**.
2. **Select the Runner Group**.
3. **Set the access level**:
    - Allow only selected repositories to use self-hosted runners.
    - Do not allow public repositories to use enterprise runners.
4. **Save the settings**.

### **Managing secrets and access control**

GitHub Actions often require **API keys, credentials, and environment variables**. These should be **securely stored using GitHub Secrets Management**.

#### **Organizational secrets vs. repository secrets**

| **Type** | **Scope** | **Best Use Case** |
| --- | --- | --- |
| **Repository Secrets** | Specific to one repository. | For repository-specific credentials. |
| **Organization Secrets** | Available to multiple repositories. | For shared enterprise credentials. |

### **Configuring organizational secrets**

1. **Navigate to Organization Settings → Secrets and Variables → Actions**.
2. **Click "New Secret"**.
3. **Enter a Secret Name and Value**.
4. **Choose Repository Access**:
    - All repositories.
    - Selected repositories.
5. **Save the secret**.

### **Best practices for managing secrets**

- **Use environment variables carefully** to prevent secret exposure.
- **Rotate secrets regularly** to enhance security.
- **Use third-party secret managers** (e.g., AWS Secrets Manager, HashiCorp Vault).

### **Monitoring and Auditing GitHub Actions Usage**

Organizations must **monitor** the usage of GitHub Actions to ensure compliance and detect unauthorized activity.

#### **Enabling audit logs for GitHub actions**

Audit logs provide insights into:

- Who ran workflows.
- Which actions were executed.
- What permissions workflows used.

#### **Steps to enable audit logs**

1. Navigate to Organization Settings → Security → Audit Log.
2. Filter logs by "actions" to see workflow execution history.

### **Enforcing log retention policies**

Enterprises should configure **log retention** to comply with security policies. Retaining logs ensures that organizations can review historical workflow executions, detect suspicious activities, and comply with industry regulations.

#### **Steps to configure log retention policies**

1. Navigate to Organization Settings → Security → Audit Log.
2. Set the log retention period to match compliance requirements:
    - 30 days (default).
    - 90 days (recommended for security-conscious organizations).
    - Custom period (as per legal and compliance needs).
3. Save the settings.

Alternatively, use GitHub’s API to set the log retention period.

# **Managing and leveraging reusable components in GitHub Actions**

In the previous unit, you explored how to structure GitHub Actions policies and templates at both the enterprise and organizational levels. In this unit, you'll dive deeper into how to manage and maintain reusable components—like workflows, custom actions, and automation scripts—across multiple repositories.

## **Managing and leveraging reusable components in GitHub Actions**

Reusable components in GitHub Actions refer to **workflows, actions, and scripts** that are used across multiple repositories to streamline development and automation. They help standardize CI/CD pipelines, improve maintainability, and reduce redundant configuration efforts.

### **Types of reusable components**

1. Reusable Workflows
    - Defined once and referenced in multiple repositories.
    - Example: A standard build-and-deploy workflow for all projects in an enterprise.
2. Custom GitHub Actions
    - Created for repeated tasks (e.g., setting up dependencies, running security checks).
    - Example: A company-wide action for enforcing code linting.
3. Shared Scripts
    - Bash, PowerShell, or Python scripts used in workflows to automate tasks.
    - Example: A script to generate compliance reports for code repositories.

### **Creating centralized repositories for reusable components**

A well-structured repository for reusable workflows and actions ensures easy access and standardization. Consider creating the following:

| **Repository type** | **Purpose** | **Example naming convention** |
| --- | --- | --- |
| **Actions Repository** | Stores custom GitHub Actions used across projects. | `org-actions-repo` |
| **Workflows Repository** | Contains reusable workflows for CI/CD automation. | `org-reusable-workflows` |
| **Scripts Repository** | Maintains reusable scripts for automation. | `org-scripts-library` |

**Example structure for an actions repository:**

```
org-actions-repo/
│── setup-node/         # Custom GitHub Action for Node.js setup
│   ├── action.yml
│   ├── Dockerfile
│   ├── README.md
│── security-scan/      # Custom GitHub Action for security scanning
│   ├── action.yml
│   ├── scan.py
│   ├── README.md
│── LICENSE
│── README.md
```

### **Naming conventions for files and folders**

To ensure maintainability and ease of use, follow **consistent naming conventions**.

#### **Repository naming**

- Use **descriptive and consistent** names for repositories:
    - `org-actions-repo`
    - `org-reusable-workflows`
    - `my-random-actions`
- Prefer **hyphenated names** for clarity:
    - `ci-pipeline`
    - `CIPipeline`

#### **Workflow and action naming**

- Follow a structured format:
    - `[scope]-[purpose]`
    - Examples:
        - `ci-test.yml` → For Continuous Integration testing
        - `deploy-app.yml` → Deployment workflow
        - `build-container.yml` → Builds Docker containers
- Use **versioned directories** for long-term maintainability:

    ```
    reusable-workflows/
    ├── v1/
    │   ├── ci-test.yml
    │   ├── deploy-app.yml
    ├── v2/
    │   ├── ci-test.yml
    │   ├── deploy-app.yml
    ```


#### **Action naming**

- **Use PascalCase or kebab-case** for action names:
    - `Run-Linter`
    - `run-linter`
    - `Runlinter`
- **Provide clear descriptions in `action.yml`:**

    ```yaml
    name: "Run Linter"
    description: "A reusable action to run lint checks on JavaScript projects."
    ```


### **Plans for ongoing maintenance of reusable components**

#### **1. Versioning and dependency management**

- **Tag releases** using GitHub releases (`v1.0.0`, `v2.0.0`).
- **Avoid using `latest` tags** in workflows to prevent unexpected breaking changes.
- **Use semantic versioning**:
    - **Patch Updates:** `v1.0.1` → Fixes bugs, minor updates.
    - **Minor Updates:** `v1.1.0` → New features, backward-compatible.
    - **Major Updates:** `v2.0.0` → Breaking changes.

**Referencing versions in workflows**

Instead of:

```yaml
uses: org-actions-repo/setup-node@latest
```

Use a stable version:

```yaml
uses: org-actions-repo/setup-node@v1.2.0
```

#### **2. Automating updates and maintenance**

- Implement a **GitHub Action to automatically test workflows and actions** before merging.
- Use **Dependabot** to detect outdated dependencies in reusable actions.

#### **3. Documenting and providing usage guidelines**

- Each repository should include:
    - **README.md** explaining:
        - Purpose of the component.
        - How to use it.
        - Example workflows.
    - **CHANGELOG.md** to track updates.
    - **CONTRIBUTING.md** outlining best practices for modifications.

#### **4. Security and access control**

- Restrict write access to action repositories.
- Require code reviews and **GitHub CODEOWNERS** for managing changes.
- Use **GitHub Secret Scanning** to prevent hardcoding secrets in workflows.

#### **5. Monitoring and deprecation strategy**

- **Set up GitHub Issues and Discussions** for feedback.
- **Deprecation plan**:
    - Notify users before removing older versions.
    - Offer migration guides for major changes.

## **How to distribute actions for an enterprise**

GitHub Actions enable **automation of workflows**, such as testing, deployment, and security scanning. In an enterprise, distributing actions efficiently allows multiple teams to **reuse common automation tasks** instead of reinventing the same workflows.

There are three main methods for distributing GitHub Actions within an enterprise:

1. **Publishing Actions in GitHub Marketplace** (for public distribution).
2. **Hosting Actions in Organization-Owned Repositories** (for internal enterprise distribution).
3. **Using GitHub Packages to Distribute Containerized Actions** (for more controlled distribution).

### **Best Practices for Distributing Actions in an Enterprise**

| **Best Practice** | **Recommendation** |
| --- | --- |
| **Standardization** | Maintain a **single repository** for reusable actions with **clear documentation**. |
| **Versioning** | Use **semantic versioning** (`v1.0.0`, `v2.0.0`) and avoid `latest` for stability. |
| **Security Reviews** | Require **code reviews and automated security scans** before publishing actions. |
| **Access Control** | Restrict **who can modify and use** internal actions via repository settings. |
| **Regular Updates** | Maintain **a changelog** and notify users of updates to prevent breaking changes. |

### **Comparison of Distribution Methods**

| **Method** | **Use Case** | **Access Control** | **Security Considerations** |
| --- | --- | --- | --- |
| **GitHub Marketplace** | Public distribution | No restrictions | Publicly accessible, must be secure |
| **Organization-Owned Repositories** | Internal enterprise use | Teams and repository roles | Requires strict permission control |
| **GitHub Packages (Containerized Actions)** | Secure internal use | Authentication required | Images must be protected against tampering |

Choosing the right method depends on **business requirements, security needs, and scalability considerations**. Enterprises should implement a **structured approach** to distributing GitHub Actions, ensuring **efficiency, security, and compliance** across their DevOps workflows.

# **Manage runners**

In this section, you explore the different tools and strategies available to you in GitHub Enterprise Cloud and GitHub Enterprise Server to manage the use of GitHub Actions runners in your enterprise.

## **Choose an appropriate runner for your workload**

Two types of runners can execute GitHub Actions workflows: GitHub-hosted runners or self-hosted runners.

**Note**

GitHub-hosted runners are only available for Enterprise Cloud. If you have an Enterprise Server instance, this section does not apply to you.

GitHub-hosted runners offer a quicker and simpler way to run your workflows, while self-hosted runners are a highly configurable way to run workflows in your own custom environment. For example, if you need to use an IP address allowlist for your organization or a specialized hardware configuration for running your workflows, use a self-hosted runner.

The following table compares GitHub-hosted runners versus self-hosted runners. Use it to choose the appropriate runner for your workload.

| **GitHub-hosted runners** | **Self-hosted runners** |
| --- | --- |
| Receive automatic updates for the operating system, preinstalled packages and tools, and the self-hosted runner application. | Receive automatic updates for the self-hosted runner application only. You're responsible for updating the operating system and all other software. |
| GitHub managed and maintained. | Can use cloud services or local machines that you already pay for. Also are customizable to your hardware, operating system, software, and security requirements. |
| Provide a clean instance for every job execution. | Don't need to have a clean instance for every job execution. |
| Use free minutes on your GitHub plan, with per-minute rates applied after surpassing the free minutes. | Are free to use with GitHub Actions, but you're responsible for the cost of maintaining your runner machines. |

## **Manage runners for the enterprise**

Managing runners for the enterprise involves configuring and securing both GitHub-hosted and self-hosted runners to ensure efficient and secure CI/CD workflows. This management includes setting up IP allowlists to control access, enhancing security by restricting runner access to specific IP addresses, and ensuring compliance with organizational policies. Proper configuration of IP allowlists for both GitHub-hosted and self-hosted runners is crucial for maintaining secure and reliable interactions between internal applications and GitHub Actions runners. Regular updates and reviews of these configurations are necessary to adapt to changes in IP address ranges and maintain optimal security.

### **Configuring IP allowlists on GitHub-hosted and self-hosted runners**

Configuring IP allowlists helps control access to runners by restricting them to specific IP addresses. This configuration enhances security by preventing unauthorized access but might require further network configurations.

| **GitHub-hosted runners** | **Self-hosted runners** |
| --- | --- |
| GitHub-hosted runners use dynamic IP addresses, making it difficult to configure precise IP allowlists. | Use static or controlled IPs, allowing precise IP allowlisting or IP-based access control. |
| Organizations must allow GitHub’s published IP ranges. | Can be placed behind firewalls or VPNs for added security. |
| GitHub-hosted runners can be restricted using GitHub’s enterprise security settings. | Require explicit configuration to communicate with external services, enhancing security. |

#### **Allowed IP list**

An **allowed IP list** is a security feature that restricts access to services or resources based on predefined IP addresses. When organizations configure an IP allowlist, they can:

- **Enhance security:** Prevent unauthorized access by allowing only trusted IP addresses.
- **Control network Traffic:** Restrict inbound and outbound requests to known and verified IPs.
- **Improve compliance:** Ensure regulatory compliance by limiting access to authorized networks.

| **GitHub-hosted runners** | **Self-hosted runners** |
| --- | --- |
| Organizations must allow GitHub's published IP ranges, which change periodically. | Admins can define specific IP addresses that are allowed to access the runners. |
| GitHub-hosted runners can be configured via GitHub’s security settings. | Self-hosted runners work well with firewalls, VPNs, or cloud security groups. |

### **Configuring IP allowlists for internal applications to interact with GitHub-Hosted Runners**

To configure IP allowlists for internal applications and systems to interact with GitHub-hosted runners, you can refer to the following official GitHub documentation:

#### **1. Understand GitHub's IP address ranges**

GitHub-hosted runners operate within specific IP address ranges. To ensure your internal applications can communicate with these runners, you need to allow these IP ranges through your firewall. GitHub provides a meta API endpoint https://api.github.com/meta that lists all current IP address ranges used by GitHub services, including IP ranges for Actions runners. Regularly updating your allowlists based on this information is essential, as IP ranges can change.

#### **2. Configure your firewall**

#### **a. Obtain GitHub's IP ranges:**

- Use the meta API endpoint to retrieve the latest IP address ranges used by GitHub Actions runners.

#### **b. Update firewall rules:**

- Add rules to your firewall to permit inbound and outbound traffic to and from these IP ranges. This configuration ensures that your internal systems can interact with GitHub-hosted runners without connectivity issues.

#### **3. Consider using self-hosted runners**

If maintaining an IP allowlist for GitHub-hosted runners is challenging due to frequent changes in IP ranges, consider setting up self-hosted runners within your network. This approach allows you to have more control over the runner environment and network configurations. However, using self-hosted runners requires more maintenance and infrastructure management.

!Screenshot of an empty runners screen.

#### **4. Regularly review and update allowlists**

Since GitHub's IP address ranges can change, it's crucial to periodically review and update your firewall's IP allowlists. Automating this process by scripting the retrieval of IP ranges from GitHub's meta API can help ensure your allowlists remain current without manual intervention.

### **Effects and potential abuse vectors of enabling self-hosted runners on public repositories**

#### **Effects of enabling self-hosted runners**

1. **Customization & performance optimization**
    - Self-hosted runners allow control over hardware, installed software, and environment settings.
    - Workflows can be optimized for performance by using dedicated, high-performance machines.
2. **Cost savings**
    - Unlike GitHub-hosted runners (which have limited free usage), self-hosted runners run on your infrastructure, reducing cost constraints.
3. **State persistence**
    - Self-hosted runners do **not** reset between jobs like GitHub-hosted runners.
    - Allows **caching dependencies**, reusing large datasets, and maintaining persistent states.
4. **Security & maintenance responsibility**
    - **Security patches, dependency updates, and system monitoring** become the runner owner's responsibility.
    - Misconfigurations could expose the runner to external threats.

#### **Potential abuse vectors of self-hosted runners**

Enabling self-hosted runners on public repositories introduces significant security risks. Since **anyone** can trigger workflows by submitting a pull request, attackers can exploit this feature in various ways:

1. **Arbitrary Code Execution (RCE) by malicious actors**
    - Attackers can submit pull requests containing **malicious scripts**, which the self-hosted runner executes automatically.
    - If the runner has **elevated privileges**, the attacker gains **full system access**.
2. **Cryptocurrency mining & resource exploitation**
    - Attackers can abuse self-hosted runners to mine cryptocurrency, causing **unexpected high CPU and GPU usage**.
    - This increases **operational costs** and **reduces availability** for legitimate workflows.
3. **Data exfiltration & credential theft**
    - If secrets (API keys, database credentials, SSH keys) are stored on the runner, attackers could extract them.
    - **Example attack vector**: A malicious pull request could read and send stored environment variables to an external server.
4. **Denial of Service (DoS) attacks**
    - Attackers can flood the repository with numerous pull requests to overload self-hosted runners.
    - If runners are on shared infrastructure, other critical workflows might be disrupted.
5. **Lateral movement & network exploitation**
    - If the self-hosted runner is inside a **corporate network**, an attacker could **pivot** into internal systems.
    - Could lead to **data breaches**, **ransomware attacks**, or **persistent access** to private resources.

#### **Mitigation strategies**

To reduce security risks, follow these best practices:

- Restrict self-hosted runners to **private repositories only**
- Require **workflow approval** for pull requests from external contributors
- Run self-hosted runners in a **secure, isolated environment** (for example, containers, virtual machines)
- Use **firewalls and network rules** to block unauthorized access
- Limit access to **sensitive secrets** and store credentials securely
- **Monitor and log** runner activity to detect anomalies

### **Selecting appropriate runners to support workloads**

#### **Understanding GitHub runners**

GitHub Actions supports two types of runners:

1. **GitHub-hosted runners**
    - Managed by GitHub, automatically provisioned and scaled.
    - Includes **pre-installed software, tools, and dependencies** for common workflows.
    - Available for **Windows, Linux, and macOS**.
    - Recommended for **general automation, open-source projects, and quick setup**.
2. **Self-hosted runners**
    - Managed by the user, providing **full control over environment and resources**.
    - Can be configured for **custom hardware, on-premises, or cloud infrastructure**.
    - Supports **persistent states between jobs**, allowing better caching and custom dependencies.
    - Recommended for **private repositories, enterprise workloads, and performance-intensive tasks**.

#### **Choosing between GitHub-hosted and Self-hosted Runners**

Two types of runners can execute GitHub Actions workflows: GitHub-hosted runners or self-hosted runners.

**Note**

GitHub-hosted runners are only available for Enterprise Cloud. If you have an Enterprise Server instance, this section does not apply to you.

GitHub-hosted runners offer a quicker and simpler way to run your workflows, while self-hosted runners are a highly configurable way to run workflows in your own custom environment. For example, if you need to use an IP address allowlist for your organization or a specialized hardware configuration for running your workflows, use a self-hosted runner.

The following table compares GitHub-hosted runners versus self-hosted runners. Use it to choose the appropriate runner for your workload.

| **GitHub-hosted runners** | **Self-hosted runners** |
| --- | --- |
| Receive automatic updates for the operating system, preinstalled packages and tools, and the self-hosted runner application. | Receive automatic updates for the self-hosted runner application only. You're responsible for updating the operating system and all other software. |
| GitHub managed and maintained. | Can use cloud services or local machines that you already pay for. Also are customizable to your hardware, operating system, software, and security requirements. |
| Provide a clean instance for every job execution. | Don't need to have a clean instance for every job execution. |
| Use free minutes on your GitHub plan, with per-minute rates applied after surpassing the free minutes. | Are free to use with GitHub Actions, but you're responsible for the cost of maintaining your runner machines. |

#### **Choosing the right operating system for runners**

#### **1. Linux runners (default)**

- Best for most workloads
- Fast, cost-effective, and widely supported
- Used in **CI/CD, scripting, Docker, and automation**Example: `ubuntu-latest`, `ubuntu-22.04`

#### **2. Windows runners**

- Needed for **.NET, Windows-based software, and GUI apps**
- Supports **PowerShell, Windows-specific dependencies**Example: `windows-latest`, `windows-2022`

#### **3. macOS runners**

- macOS runners are required for **iOS, macOS, Xcode, and Apple-specific builds**
- Supports **Swift, Objective-C, and macOS applications**Example: `macos-latest`, `macos-13`

#### **Best practices for Selecting Runners**

- Use **GitHub-hosted runners** for general workflows and automation.
- Use **self-hosted runners** for **custom environments, large workloads, or security-sensitive applications**.
- Choose **Linux runners** for most workloads due to performance and cost efficiency.
- Use **Windows or macOS runners** only when required for compatibility.
- **Regularly update and monitor self-hosted runners** to prevent security risks.

### **Contrast GitHub-hosted and self-Hosted runners**

GitHub Actions supports two types of runners for executing workflows:

1. **GitHub-hosted runners** – Managed by GitHub, automatically provisioned, and preconfigured with common development tools.
2. **Self-hosted runners** – Managed by the user, allowing complete control over the environment, resources, and configurations.

This section highlights the key differences between GitHub-hosted and self-hosted runners.

#### **Comparison: GitHub-hosted vs. Self-hosted runners**

| Feature | GitHub-hosted runner | Self-hosted runner |
| --- | --- | --- |
| **Setup & maintenance** | No setup required; GitHub manages everything | User must install, configure, and maintain |
| **Scalability** | Autoscales dynamically | Must manually provision added runners |
| **Security** | High security; fresh virtual environment for each job | Requires manual security hardening |
| **Customization** | Limited; preinstalled tools only | Fully customizable; user can install any dependencies |
| **Performance** | Standardized compute resources | Can use high-performance hardware |
| **State persistence** | Resets after every job | Can persist data between jobs |
| **Cost** | Free for public repos; limited free usage for private repos | No GitHub costs, but requires infrastructure investment |
| **Network access** | No direct access to internal networks | Can access internal/private networks |
| **Use case** | Best for general CI/CD, automation, and open-source projects | Best for enterprise environments, secure builds, and large workloads |

#### **Key differences & considerations**

#### **1. Setup & maintenance**

- **GitHub-hosted runners** require **zero setup**; users can start running workflows immediately.
- **Self-hosted runners** need **manual installation, configuration, updates, and security management**.

#### **2. Security risks**

- GitHub-hosted runners **run in isolated virtual machines** that reset after each job, minimizing attack surfaces.
- Self-hosted runners **persist across jobs**, meaning a compromised runner can be exploited across multiple workflow runs.

#### **3. Performance & cost considerations**

- GitHub-hosted runners **provide a standard environment** but have **usage limits** (for example, free minutes per month for private repositories).
- Self-hosted runners **allow better performance tuning** (for example, running on high-end servers) but require **infrastructure and maintenance costs**.

#### **4. Networking & access**

- GitHub-hosted runners **cannot access private/internal resources** without further configurations.
- Self-hosted runners **can access internal systems**, making them ideal for **private repositories, internal tools, and on-premises deployments**.

#### **When to use each runner?**

**Use GitHub-hosted runners if:**

- You need a **quick and easy setup** without infrastructure management.
- Your workflow **doesn’t require custom dependencies** beyond the preinstalled tools.
- You're working on an **open-source or public repository** with **free hosted runner minutes**.

**Use Self-hosted runners if:**

- Your workflow requires **specific dependencies, configurations, or persistent states**.
- You need to **access private network resources** (for example, on-premises databases, internal services).
- You require **higher performance machines** for **large-scale CI/CD pipelines**.

# Questions

1. **Which workflow command would set the debug message to This is an error message?**
    1. echo::error::This is an error message
2. **Which keywords are required for a valid `action.yml` file?**
    1. `name`, `runs`, `description`
3. **You need to create a custom GitHub Action written in Ruby. Which action type would you choose?**
    1. Docker container action
4.