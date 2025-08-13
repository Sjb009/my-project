## Question 1.
Screenshots indicating answers are attached
** .status.replicas
** .status  Navigate to the deployment’s status object.
** .replicas Returns the number of replicas currently running, which is the actual state, not just the desired count.
  #for k8-deploy.json
** .fields.subtasks | map(.key)
** .fields.subtasks accesses the array of subtask objects.
map(.key) transforms each object to its key value (e.g., "SAMPLE-123").
Returns a JSON array of all subtask IDs.

## Question 2
https://sjayb.atlassian.net/jira/software/c/projects/SAV/components

Also attached are pictures showing successful integration

## Question 3
Logic file attached alongside repo file named scorecard.json
https://github.com/Sjb009/my-project



## Question 4
1. Check that the GitHub App in Port is installed correctly
Go to Port → Integrations → GitHub and verify:

The GitHub App is installed on the correct GitHub account or organization.

The correct repositories are granted access in the App settings.

Common mistake: The App is installed, but only for some repos, or the one you’re testing isn’t included.

How to test:
From GitHub → Settings → Installed GitHub Apps → click Port App → check if the target repo is listed.

2. Confirm the Workflow’s on: workflow_dispatch trigger
Your GitHub Actions workflow must have:
on:
  workflow_dispatch:
If it’s missing, GitHub won’t allow external services (like Port) to start it.

Common mistake: The workflow is triggered only by push or pull_request events, so Port can’t start it.

3. Check workflow file location & name
Workflow files must be in .github/workflows/ in your repository.

Common mistake: The YAML file is saved in another folder or has a typo in the filename.

4. Match the workflow file name in Port’s action configuration
In Port → Self-Service Action → GitHub Workflow Action, check the workflow file name matches exactly the YAML file name in GitHub (case-sensitive).

Example:

GitHub file: .github/workflows/deploy.yml

Port config: "deploy.yml" (NOT "Deploy.yaml" or "deploy.yaml " with a space)

5. Verify workflow inputs match
If your workflow uses inputs under workflow_dispatch, Port must pass matching parameters.

Example in GitHub:

yaml
Copy
Edit
on:
  workflow_dispatch:
    inputs:
      environment:
        description: 'Deployment environment'
        required: true
        default: 'staging'
Port action must have an input named environment with a valid value.

Common mistake: The input names in Port don’t match exactly the ones in the GitHub YAML.

6. Check GitHub permissions for the Port GitHub App
Go to GitHub → Settings → Installed GitHub Apps → Port App → Permissions and confirm:

Workflows permission is set to Read and write.

If this permission isn’t granted, Port can’t start workflows.

7. Look for GitHub workflow run logs
Go to the target repo → Actions tab → see if a workflow run was created.

If nothing appears, Port probably never triggered it (points back to config issues).

If it appears but is failing, check the GitHub Actions run logs.

8. Check Port’s Action Run logs
In Port → Self-Service → Action Runs → click the failed/hanging run → view logs.

If you see something like:

arduino
Copy
Edit
Failed to find workflow file
or

makefile
Copy
Edit
Forbidden: Missing workflow scope
— that’s your smoking gun.

9. Common gotcha: Branch targeting
If the workflow expects a branch name (via ref), make sure Port is configured to trigger the right branch.

If no ref is set, GitHub defaults to the repo’s default branch (usually main).

