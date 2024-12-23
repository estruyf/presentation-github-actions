# From Code to Cloud: Automated Deployments with GitHub Actions

This repository contains the slides and demos for the session "From Code to Cloud: Automated Deployments with GitHub Actions".

## Variables

- `WEBSITE_URL`: `https://estruyf.github.io/presentation-github-actions/`
- `WEBSITE_URL`: `https://github-actions.elio.dev`

### Azure Static Web Apps

- Preview: `https://ambitious-mud-08855ab03-preview.westeurope.5.azurestaticapps.net/`
- Production: `https://ambitious-mud-08855ab03.5.azurestaticapps.net/` | `https://github-actions.elio.dev`

## Before you start

- Create a new repository
- Clone this project
- Add the initial files to your repository
- Create two environments in your repository
  - `preview`
  - `production`
- Add the `AZURE_STATIC_WEB_APPS_API_TOKEN` secret to the preview & production environment
- In the pages setting, set the source to `GitHub Actions` instead of a branch
- Ready to start

## Clean up after the session

Remove all workflow runs:

```bash
repo=$(gh repo view --json nameWithOwner | jq -r .nameWithOwner)
gh api repos/$repo/actions/runs --paginate | jq -r -c '.workflow_runs[] | "\(.id)"' | \
xargs -n1 -I % gh api repos/$repo/actions/runs/% -X DELETE | jq -r .status
```

Reset the waiting HTML on the site

```bash
rm -rf ./dist
mkdir ./dist
cp ./public/waiting.html ./dist/index.html
swa deploy ./dist --deployment-token <token> --env production
```

## Quick links

- [Actions](https://github.com/estruyf/presentation-github-actions/actions)
- [Actions settings](https://github.com/estruyf/presentation-github-actions/settings/actions)
- [Environments](https://github.com/estruyf/presentation-github-actions/settings/environments)
- [Secrets](https://github.com/estruyf/presentation-github-actions/settings/secrets/actions)
