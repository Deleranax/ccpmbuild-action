# CCPM Build Action

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)

A GitHub Action to automatically build and deploy CCPM repositories for ComputerCraft (CC:Tweaked).

This action helps you maintain a CCPM package repository directly on GitHub by automatically building your packages and deploying them to a distribution branch.

## Usage

### Basic Example

```yaml
name: Build CCPM Repository

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Build CCPM repository
        uses: deleranax/ccpmbuild-action@v1
```

### Advanced Example

```yaml
name: Build CCPM Repository

on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Build CCPM repository
        uses: deleranax/ccpmbuild-action@v1
        with:
          minify: 'true'
          source-path: '.'
          branch-name: 'dist'
          keep: 'true'
          keep-history: 'false'
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `minify` | Whether to minify the Lua source code in packages | No | `true` |
| `source-path` | The repository source path containing `manifest.json` and `packages` directory | No | `.` |
| `branch-name` | The distribution branch name that will contain the built packages pool and index | No | `dist` |
| `keep` | Whether to keep old package versions in the pool | No | `false` |
| `keep-history` | Whether to maintain git history (one commit per build) in the distribution branch | No | `false` |

## Prerequisites

### Required Permissions

This action requires write access to your repository:

```yaml
permissions:
  contents: write
```

### Repository Settings

- **Force push must be enabled** on the target branch (configured in repository settings under branches)
- The distribution branch will be created automatically if it doesn't exist

### Repository Structure

Your repository should follow the standard CCPM repository structure with:
- A `manifest.json` file
- A `packages` directory containing your package sources

For more information about CCPM repository structure, see [deleranax/ccpm](https://github.com/deleranax/ccpm).

## Examples

### Keep Package History

Maintain all previous versions of your packages:

```yaml
- uses: deleranax/ccpmbuild-action@v1
  with:
    keep: 'true'
```

### Maintain Git History

Keep one commit per build instead of orphaning the branch:

```yaml
- uses: deleranax/ccpmbuild-action@v1
  with:
    keep-history: 'true'
```

### Unminified Builds

Build packages without minification (useful for debugging):

```yaml
- uses: deleranax/ccpmbuild-action@v1
  with:
    minify: 'false'
```

### Custom Paths

Use a non-standard source directory and branch name:

```yaml
- uses: deleranax/ccpmbuild-action@v1
  with:
    source-path: 'repository'
    branch-name: 'packages'
```

## Real-World Example

See [deleranax/ccpm](https://github.com/deleranax/ccpm) for a working example of this action in production.

## License

This project is licensed under the GNU General Public License v3.0 - see the [LICENSE](LICENSE) file for details.
