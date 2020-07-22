# rwe/actions-hlint-run

GitHub Action: Run hlint

See also [rwe/actions-hlint-setup](https://github.com/rwe/actions-hlint-setup), which will install (and cache) hlint.

Executes `hlint` and re-formats the output with a
[problem matcher](https://github.com/actions/toolkit/blob/1cc56db0ff126f4d65aeb83798852e02a2c180c3/docs/commands.md#problem-matchers),
so that the hints are displayed as GitHub annotations.

## Inputs

* `fail-on` (optional, default: _never_): When to mark the check as failed.
  One of: `never`, `status`, `warning`, `suggestion`, or `error`.
* `path` (required): The single file or directory name, or formatted JSON array containing multiple.
  Examples:
  * `- path: src/`
  * `- path: '["src/", "test/"]'`
  * `- path: ${{ toJSON(steps.whatever.changed_dirs) }}` (see: docs on [toJSON](https://docs.github.com/en/actions/reference/context-and-expression-syntax-for-github-actions#tojson))
* `hlint-bin` (optional): The `hlint` binary path, if not already in `PATH`.

## Outputs

The main purpose of this action currently is just to print out GitHub annotations, but it still provides an output.

* `hints`: The generated HLint hints serialized to JSON.

## Example

```yaml
name: lint
on:
  pull_request:
  push:
    branches:
      - master
      - 'releases/*'

jobs:
  hlint:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2

    - name: 'Set up HLint'
      uses: rwe/actions-hlint-setup@v1
      with:
        version: '3.1.6'

    - name: 'Run HLint'
      uses: rwe/actions-hlint-run@v1
      with:
        path: src/
        fail-on: warning
```
