# actions-read-repo-props

This repo is a tiny demo to verify that a GitHub Action can **read repository custom properties** (a.k.a. “repo properties”) using the GitHub REST API, and then surface one specific property value (`actions_greeting`) in both the workflow logs and the job summary.

The workflow lives in `.github/workflows/read-repo-custom-properties.yml` and runs on every `push`.

## What it does

On push, the workflow:

1. Calls the REST API endpoint `GET /repos/{owner}/{repo}/properties/values`.
2. Looks for a property named `actions_greeting`.
3. Prints the value and writes it to the **job summary**.

If `actions_greeting` is missing (or empty), the job fails with a clear error.

## Permissions needed

To read custom property values, the REST docs list **repository “Metadata” (read)** as the required permission.

In GitHub Actions terms:

- The workflow uses the built-in `GITHUB_TOKEN`.
- `metadata: read` is always available for `GITHUB_TOKEN` and can’t be disabled.
- The workflow sets `permissions: contents: read` to keep the token minimally-scoped; the metadata permission still applies.

Notes:

- For public repositories, this endpoint can also be called without authentication.
- Custom properties are managed at the organization level; this demo assumes your org has a custom property named `actions_greeting` and it is set for this repository.

## How to use

1. In your organization settings, create a custom property named `actions_greeting` (if it doesn’t exist).
2. Set a value for that property on this repository.
3. Push a commit.

You should see the value in the workflow logs and in the job summary.