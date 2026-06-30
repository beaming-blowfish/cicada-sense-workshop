# Step 05 - Start From The Baseline

Goal: create the learner repository from the production-grade starting point and prove it works locally before adding GitHub automation.

## Before you begin

The step folders in this repository are snapshots, not the exercise.
Do the work in your own application repository and use this repository only as a guide or checkpoint.
In this step, do not create `.github/workflows/` files yet.

Reference docs:

- <https://docs.hoverkraft.cloud/docs/methodology/best-practices/ci-cd/>
- <https://docs.hoverkraft.cloud/docs/methodology/best-practices/ci-cd/github-actions/>
- <https://docs.hoverkraft.cloud/docs/methodology/golden-paths/application/ci-cd/github/>
- <https://docs.hoverkraft.cloud/docs/methodology/golden-paths/application/ci-cd/github/ci>
- <https://docs.hoverkraft.cloud/docs/methodology/golden-paths/application/ci-cd/github/cd>
- <https://docs.hoverkraft.cloud/docs/methodology/golden-paths/application/ci-cd/github/multi-app>
- <https://docs.hoverkraft.cloud/docs/methodology/best-practices/ci-cd/cicd-release-management/>

## Outcome

At the end of this step, you should have your own GitHub repository based on [steps/05-start](steps/05-start), running locally, with no GitHub Actions CI/CD workflow yet.

## Step 1. Create the learner repository

1. Create a new GitHub repository with these settings: name `cicada-sense`, visibility `Public`, and no initial content.
2. Open a terminal in `/home/coder/work` and clone that empty repository locally:

```bash
git clone git@github.com:<your-org>/cicada-sense.git
cd cicada-sense
```

3. From that same terminal, copy the baseline into the repository root:

```bash
rsync -va ../_cicada-sense-workshop/steps/05-start/ ./
```

4. Keep the repository structure as-is. At the root, you should still have `compose.yaml`, `Makefile`, `application/`, `charts/`, and `docker/`.
5. From the same terminal, commit the baseline and push it to GitHub:

```bash
git add .
git commit -m "chore: initialize repository from workshop baseline"
git push -u origin main
```

At the end of this step, opening your repository root should show the same baseline layout as [steps/05-start](steps/05-start).

Read:

- <https://docs.hoverkraft.cloud/docs/methodology/best-practices/ci-cd/>
- <https://docs.hoverkraft.cloud/docs/methodology/best-practices/ci-cd/github-actions/>

## Step 2. Validate the local baseline

Run the baseline exactly as provided.
In this step, `make ci` is only a local validation command. It does not mean GitHub CI already exists.

1. Open a terminal in the `cicada-sense` repository root and run:

```bash
make setup
```

2. From that same terminal, run:

```bash
make ci
```

3. Treat success as both commands completing without requiring application-code or Helm-chart changes.
4. Confirm the baseline still behaves like a production-grade starting point: Dockerized services, local development flow, and Helm chart in place.

Use this troubleshooting order:

1. if `make setup` fails, fix that first and rerun it
2. only run `make ci` after `make setup` succeeds
3. if `make ci` fails, do not create GitHub workflow files to compensate; fix the local baseline first

If the local baseline is not green, do not start writing GitHub workflows yet.
Fix the local setup problem first, then rerun the same commands.

Read:

- <https://docs.hoverkraft.cloud/docs/methodology/best-practices/ci-cd/>

## Step 3. Understand what is already solved

Before touching GitHub Actions, be clear about the scope.

What is already done in `05-start`:

1. application code
2. Dockerfiles
3. umbrella Helm chart
4. local development commands

What is intentionally missing:

1. pull request GitHub Actions CI
2. mainline GitHub Actions CI
3. release preparation
4. deployment workflows

That means your job in `05-start` is only to prove the baseline works as delivered. You are not designing automation yet.

Read:

- <https://docs.hoverkraft.cloud/docs/methodology/golden-paths/application/ci-cd/github/>

## Step 4. Confirm the repository shape

This workshop targets a multi-application CI/CD shape.
In `05-start`, you are only validating the repository baseline that later CI/CD workflows will use.

You have:

1. one Dockerfile per service
2. a separate CI build setup for `backend`, `frontend`, and `live-data-generator`
3. a separate deployable build for each service
4. one umbrella chart that releases the services together

This matters because you should follow the multi-app guide, not the single-app one.

Read:

- <https://docs.hoverkraft.cloud/docs/methodology/golden-paths/application/ci-cd/github/multi-app>

## Exit criteria

Before moving to the next step, confirm these points:

1. your repository is based on [steps/05-start](steps/05-start)
2. `make setup` works locally
3. `make ci` works locally
4. there is no `.github/workflows/` GitHub Actions CI/CD implementation yet, or at least none of the workshop workflow files exist
5. your latest changes are committed and pushed to `main`

## If you get stuck

1. compare your repository structure with [steps/05-start](steps/05-start)
2. do not invent new application structure
3. do not start with CD before the baseline is stable
