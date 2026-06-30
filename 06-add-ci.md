# Step 02 - Add CI

Goal: starting from [steps/05-start](steps/05-start), implement GitHub Actions CI until you are close to [steps/06-add-ci](steps/06-add-ci).

## Outcome

At the end of this step, your repository should validate pull requests and `main` with the Hoverkraft CI contract for a multi-application repository.
The core result is three workflow files with clear responsibilities: one reusable workflow and two thin entrypoints.

Recommended implementation order:

1. create `__shared-ci.yml` and get the job order right first
2. create `pull-request-ci.yml` as a thin wrapper around it
3. create `main-ci.yml` as the second thin wrapper
4. open a pull request and inspect the workflow graph before comparing with the snapshot

## Step 1. Read the contract before writing YAML

Read these pages first:

- <https://docs.hoverkraft.cloud/docs/methodology/golden-paths/application/ci-cd/github/>
- <https://docs.hoverkraft.cloud/docs/methodology/golden-paths/application/ci-cd/github/ci>
- <https://docs.hoverkraft.cloud/docs/methodology/golden-paths/application/ci-cd/github/multi-app>
- <https://docs.hoverkraft.cloud/docs/methodology/best-practices/ci-cd/github-actions/>

You are not trying to invent a pipeline. You are implementing an existing contract.

## Step 2. Create the shared CI workflow

Create `.github/workflows/__shared-ci.yml` first.
Make this the only file that contains the real CI job sequence.

Open a terminal in the application repository root and create the workflow file first:

```bash
mkdir -p .github/workflows
touch .github/workflows/__shared-ci.yml
```

Think of this file as the source of truth for CI behavior. If you later find yourself copying jobs into `pull-request-ci.yml` or `main-ci.yml`, stop and move that logic back here.

Your shared workflow should do this, in this order:

1. run repository-wide linting
2. build `backend-ci`, `frontend-ci`, and `live-data-generator-ci`
3. run CI in a matrix for the three applications
4. build runtime images only after CI passes
5. validate the Helm chart with the built runtime images

Be precise here:

1. declare the workflow with `workflow_call` so other workflows can reuse it instead of duplicating CI logic
2. model the order with explicit job dependencies so the graph clearly shows: lint first, then CI build setup, then application checks, then runtime builds, then chart validation
3. define the check matrix with exactly three application names: `backend`, `frontend`, and `live-data-generator`; if one is missing, the contract is incomplete
4. keep the chart validation step as a consumer of artifacts produced earlier in the workflow; it must reuse the already-built runtime artifacts and must not trigger a second build

Before filling in every command, it often helps to sketch the shared workflow as job blocks and `needs` relationships first. Once the graph shape is correct, add the detailed steps inside each job.

The important part is the contract and the sequence, not decorative YAML.

Read:

- <https://docs.hoverkraft.cloud/docs/methodology/golden-paths/application/ci-cd/github/ci>
- <https://docs.hoverkraft.cloud/docs/methodology/golden-paths/application/ci-cd/github/multi-app>

## Step 3. Add the pull request entrypoint

Create `.github/workflows/pull-request-ci.yml`.

From the same terminal, create the pull request workflow file:

```bash
touch .github/workflows/pull-request-ci.yml
```

Rules:

1. trigger on pull requests targeting `main`
2. keep it thin: trigger, concurrency, top-level permissions, and a single call into `__shared-ci.yml`
3. call `__shared-ci.yml`
4. define `concurrency`
5. start from explicit top-level `permissions: {}`
6. grant permissions per job only

If this file starts growing many build or test jobs, it is no longer thin enough. Move that logic back into `__shared-ci.yml`.

Read:

- <https://docs.hoverkraft.cloud/docs/methodology/golden-paths/application/ci-cd/github/ci>
- <https://docs.hoverkraft.cloud/docs/methodology/best-practices/ci-cd/github-actions/>

## Step 4. Add the mainline entrypoint

Create `.github/workflows/main-ci.yml`.

From the same terminal, create the mainline workflow file:

```bash
touch .github/workflows/main-ci.yml
```

Rules:

1. trigger on pushes to `main`
2. keep it thin: trigger, concurrency if needed, scoped permissions, and a call into `__shared-ci.yml`
3. prune stale preview and runtime image tags
4. call `__shared-ci.yml`
5. regenerate Helm docs after CI succeeds
6. keep permissions explicit and scoped

This file should read like an entrypoint plus a small amount of `main`-specific behavior. It should not become a second copy of the shared workflow.

Read:

- <https://docs.hoverkraft.cloud/docs/methodology/golden-paths/application/ci-cd/github/ci>
- <https://docs.hoverkraft.cloud/docs/methodology/golden-paths/application/ci-cd/github/multi-app>
- <https://docs.hoverkraft.cloud/docs/methodology/best-practices/ci-cd/github-actions/>

## Step 5. Validate CI behavior

Open a pull request and inspect the workflow graph.
You should be able to explain which file owns each responsibility before comparing with the snapshot.
When you inspect the graph, check both the order of the stages and the dependency between them: later stages should wait for earlier ones.

Expected pull request behavior:

1. `pull-request-ci.yml` acts only as a thin entrypoint and delegates the real work to `__shared-ci.yml`
2. the first real job is repository-wide linting
3. after lint passes, the workflow builds the three CI environments used for checks: `backend-ci`, `frontend-ci`, and `live-data-generator-ci`
4. after those builds finish, the workflow runs a matrix with exactly three entries: `backend`, `frontend`, and `live-data-generator`
5. only after every matrix check passes does the workflow build the deployable runtime artifacts
6. the final stage validates the Helm chart with those already-built runtime artifacts, so chart validation appears last in the graph

Expected `main` behavior after merge:

1. stale image tags are pruned
2. the same CI contract runs again
3. Helm docs are regenerated

Expected core file set in your repository after this step:

1. `.github/workflows/__shared-ci.yml`
2. `.github/workflows/pull-request-ci.yml`
3. `.github/workflows/main-ci.yml`

If the graph order is wrong, fix the job dependencies in `__shared-ci.yml`. Do not work around the problem by moving logic into the wrapper workflows.

When this step is done, open a terminal in the application repository root and commit then push your workflow changes:

```bash
git add .github/workflows
git commit -m "feat: add CI workflows"
git push
```

Read:

- <https://docs.hoverkraft.cloud/docs/methodology/golden-paths/application/ci-cd/github/ci>
- <https://docs.hoverkraft.cloud/docs/methodology/best-practices/ci-cd/github-actions/>

## Step 6. Reach parity with the snapshot

If you want to get close to [steps/06-add-ci](steps/06-add-ci), add the repo-hygiene workflows present in the snapshot:

1. `greetings.yml`
2. `semantic-pull-request.yml`
3. `stale.yml`
4. `need-fix-to-issue.yml`

These are useful, but they are not the core CI objective. The real learning target is the three-file CI family:

1. `__shared-ci.yml`
2. `pull-request-ci.yml`
3. `main-ci.yml`

Read:

- <https://docs.hoverkraft.cloud/docs/methodology/best-practices/ci-cd/github-actions/>

## Exit criteria

Before moving to the next step, confirm these points:

1. your CI logic lives in `__shared-ci.yml`
2. PR and `main` workflows are thin wrappers
3. CI runs inside dedicated `ci` images
4. chart validation uses the built runtime images
5. the core workflow files in your repository are `__shared-ci.yml`, `pull-request-ci.yml`, and `main-ci.yml`
6. your latest CI changes are committed and pushed
7. your result is close to [steps/06-add-ci](steps/06-add-ci)

## If you get stuck

1. compare one workflow file at a time with [steps/06-add-ci](steps/06-add-ci)
2. do not duplicate logic between PR and `main`
3. if you are changing application code, you are probably solving the wrong problem
