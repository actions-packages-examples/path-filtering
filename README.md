# push-event-filtering

Example for filtering on the `push` event in GitHub Actions.

## Context

The following filters can be applied to the `push` event:

* `types`
* `branches`
* `branches-ignore`
* `tags`
* `tags-ignore`
* `paths`

### Applying both the `branches` and `paths` filters

If the `push` event is filtered by one or more `branches` and one or more `paths`, the workflow will only trigger when contents on one of the matching `paths` is matched _and_ when that update happens on one of the matching `branches`.

Given this workflow file:

```yaml
name: Main

on:
  push:
    branches:
      - 'feature/**'
      - 'bug/**'
    paths:
      - 'alpha/**'
      - 'zeta/**'

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - run: exit 0

```

The only case this triggers is when any contents in the `alpha/` directory or `zeta/` directory are modified on a branch beginning with either `feature/` or `bug/`.

However, this workflow will **never** trigger when:

* A new branch is created beginning with either `feature/` or `bug/`. Creating a new branch triggers the `push` event without filters.
* Any contents in the `alpha/` directory are modified on a branch that doesn't begin with `feature/` or `bug/`.
* Any contents in the `zeta/` directory are modified on a branch that doesn't begin with `feature/` or `bug/`.

### Documentation

* [Workflow syntax for GitHub Actions - `on.<event_name>.types`](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/workflow-syntax-for-github-actions#onevent_nametypes)
* [Workflow syntax for GitHub Actions - `on.<push|pull_request>.<branches|tags>`](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/workflow-syntax-for-github-actions#onpushpull_requestbranchestags)
* [Workflow syntax for GitHub Actions - `on.<push|pull_request>.paths`](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/workflow-syntax-for-github-actions#onpushpull_requestpaths)
