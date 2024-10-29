# fly-destroy-app
Destroys fly.io app if matching app name found

## Required Secrets

`FLY_API_TOKEN` - **Required**. The token to use for authentication. You can find a token by running `flyctl auth token` or going to your [user settings on fly.io](https://fly.io/user/personal_access_tokens).

## Basic Example

```yaml
name: Destroy fly preview app when label removed

on:
  pull_request_target:
    types:
      - unlabeled

env:
  FLY_API_TOKEN: ${{ secrets.FLY_API_TOKEN }}

jobs:
  label_removed:
    if: github.event.label.name == 'Your label here'
    runs-on: ubuntu-latest
    continue-on-error: true
    concurrency:
      group: pr-${{ github.event.number }}
    steps:
      - name: Destroy fly.io preview app
        id: destroy
        uses: iSCJT/fly-destroy-app@v1.1
        with:
          name: 'your-app-name-here'
