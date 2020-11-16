# Semantic Release

## What Semantic Release does for you

[Semantic Release](https://semantic-release.gitbook.io/semantic-release/) automates:

- Versioning of your package (via `git` tags)
- Releasing your package on NPM/Gitlab.

  SR does this by checking if commit message is prefixed with `fix:` or `feat:`. Then, if configured in your CI (which is easy to do), SR will tag your commit following [semantic versioning](https://semver.org/) guidelines.

  Using Semantic Release **prevents**:

  - **Errors when manually tagging commits**
  - **Inconsistent versioning leading to broken applications**

## Semantic Release Setup

Semantic Release needs to be setup in both your _project_ and in your _CI Variables_ available under **Settings > CI/CD** in Gitlab.

### Using CI Templates

1. In your `gitlab-ci.yml`, include the `use-yarn.yml`
