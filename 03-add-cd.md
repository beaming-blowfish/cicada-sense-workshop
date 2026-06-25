# Step 03 - Add argocd-app-of-apps

Goal: prepare the GitOps delivery repository that will receive the deployment state for `cicada-sense`.

## Outcome

At the end of this step, you should have an initialized `argocd-app-of-apps` repository with a scaffolded `cicada-sense` application layout for review apps, UAT, and production.

## Step 1. Create the GitOps delivery repository

Before implementing CD in the application repository, create the repository that will receive the deployment state.

Start from this template:

- https://github.com/hoverkraft-tech/argocd-app-of-apps-template

What to do:

1. create your own repository from the template
2. keep the repository in the same GitHub owner or organization if you want to reuse the default deploy contract
3. name it `argocd-app-of-apps` or be ready to update the `deploy-parameters` in later CD workflows
4. run the template initialization command immediately after creating the repository

Bootstrap command:

```bash
make init-repo GITHUB_ORG=my-org BASE_DOMAIN=example.com DEV_CLUSTER_URL=https://dev.example.com PROD_CLUSTER_URL=https://prod.example.com
```

Replace the placeholder values with the real GitHub owner and cluster domains for your setup.

Useful options:

1. `REPO_NAME=my-argocd-repo` if the repository name should not default to the current directory name
2. `DRY_RUN=1` to preview the files that would be updated before writing them

This initialization step replaces the repository placeholders and removes the bootstrap script so the repository starts from a clean, usable GitOps state.

## Step 2. Create the `cicada-sense` application in the GitOps repository

Once the GitOps repository is initialized, scaffold the application layout for this workshop.

Run:

```bash
make new-app APP_NAME=cicada-sense
```

Useful option:

1. `DRY_RUN=1` to preview the generated files before writing them

This command creates the default review, UAT, and production directories for the application, including:

1. `dev/apps/review-apps/cicada-sense/`
2. `dev/manifests/review-apps/cicada-sense/`
3. `prod/apps/uat/cicada-sense/`
4. `prod/manifests/uat/cicada-sense/`
5. `prod/apps/production/cicada-sense/`
6. `prod/manifests/production/cicada-sense/`

After scaffolding, adjust the generated files with the real chart reference, ingress hosts, namespaces, and any environment-specific values needed for this workshop.

## Step 3. Validate the GitOps repository state

Before you return to the application repository, confirm that the GitOps repository is usable.

Check these points:

1. the template placeholders have been replaced
2. the repository bootstrap is complete
3. the `cicada-sense` review, UAT, and production directories exist
4. the generated files are ready to receive the real chart and image references from the CD workflow

Read:

- https://github.com/hoverkraft-tech/argocd-app-of-apps-template#initialize-a-repository-created-from-this-template
- https://github.com/hoverkraft-tech/argocd-app-of-apps-template#adding-a-new-application

## Next step

Once the GitOps repository is ready, continue with [04-add-cd.md](04-add-cd.md) to implement CD in the application repository.

## Exit criteria

Before you move on, confirm these points:

1. you created your own `argocd-app-of-apps` repository from the template
2. you ran the template bootstrap command successfully
3. you scaffolded the `cicada-sense` application in that repository
4. the repository now contains review, UAT, and production application directories for `cicada-sense`

## If you get stuck

1. compare your repository state with the template README, not with the application repository
2. use `DRY_RUN=1` before rerunning template commands
3. do not start writing application CD workflows before this repository is ready