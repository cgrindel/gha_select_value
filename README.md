# Select First Non-Empty Value GiHub Action

GitHub Action to select the first non-empty value. This is paritcularly useful when selecting input
values from different input locations.

## Quickstart

```yml
name: Move Major Version Tag on Release

on:
  release:
    types: [ published ]
  workflow_dispatch:
    inputs:
      release_tag:
        required: true
        type: string

jobs:
  move_major_version_tag:
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v2
        with:
          # Fetch all history for all branches and tags
          fetch-depth: 0
      - uses: cgrindel/gha_configure_git_user@v1
      - name: Resolve release_tag
        id: resolve_release_tag
        uses: ./
        with: 
          value0: ${{ github.event.release.tag_name }}
          value1: ${{ github.event.inputs.release_tag }}
      - uses: cgrindel/gha_move_major_version_tag@v1
        with:
          release_tag: ${{ steps.resolve_release_tag.outputs.resolved_value }}

```

