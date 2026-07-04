# Step 05 - Start From The Baseline

Goal: clone the prepared learner repository and prove the baseline works locally before adding GitHub automation.

## Before you begin

The step folders in this repository are snapshots, not the exercise.
Do the work in your own application repository and use this repository only as a guide or checkpoint.
For this workshop run, the learner repository baseline is already prepared to save time.
In this step, do not create `.github/workflows/` files yet.

## Outcome

At the end of this step, you should have the learner GitHub repository cloned locally, still matching [steps/05-start](steps/05-start), with no GitHub Actions CI/CD workflow yet.

## Step 1. Clone the learner repository

The repository is already created and populated for this session.

1. Clone the repository locally:

```bash
git clone git@github.com:<your-org>/cicada-sense.git
cd cicada-sense
```

2. Confirm the cloned repository still has the expected root layout:

	1. `compose.yaml`
	2. `Makefile`
	3. `application/`
	4. `charts/`
	5. `docker/`

If the repository no longer matches [steps/05-start](steps/05-start), realign it before continuing.

At the end of this step, opening your repository root should show the same baseline layout as [steps/05-start](steps/05-start).

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

## Step 4. Confirm the repository shape

This workshop targets a multi-application CI/CD shape.
In `05-start`, you are only validating the repository baseline that later CI/CD workflows will use.

You have:

1. one Dockerfile per service
2. a separate CI build setup for `backend`, `frontend`, and `live-data-generator`
3. a separate deployable build for each service
4. one umbrella chart that releases the services together

This matters because you should follow the multi-app guide, not the single-app one.

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
