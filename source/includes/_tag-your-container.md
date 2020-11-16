# Tag your container

```shell
# Install Semantic Release
yarn add -D semantic-release @semantic-release/gitlab-config
```

Here's how it works:

1. [Semantic Release](https://semantic-release.gitbook.io/semantic-release/) checks if your commit message is prefixed with `fix:` or `feat:`.

2. SR will tag your commit following [semantic versioning](https://semver.org/) guidelines, for which you will create a `GL_TOKEN` below.

3. Gitlab starts a CI job that builds your container and versions it with the `$CI_COMMIT_TAG`.

To tag your commit, Semantic Release needs a **Personal Access Token** (like from [Install Private Packages](#install-private-packages)), or a **Project Token** (explained below) to alter your project for you.
I've tried using the Job Token, but I don't think they have write-capabilities.

```yaml
# Put in your gitlab-ci.yml. Runs Semantic Release.
bumpver:
  stage: bumpver
  image: gitlab.staplcotn.com:5701/devops/cibuilders/sr-docker:v1.0.2
  rules:
    - if: "$CI_COMMIT_TAG == null"
  script:
    - npx semantic-release
---
# The JSON object below goes into your .releaserc file.
```

### Configure the CI Job

In your `.gitlab-ci.yml`, add the job to the right. It uses an image that has Semantic Release already installed to save time.

```json
{
  "branch": "master",
  "extends": "@semantic-release/gitlab-config",
  "plugins": [
    "@semantic-release/npm",
    {
      "npmPublish": false
    }
  ]
}
```

### Configure Semantic Release

Create a `.releaserc` file to your project and put the JSON object to the right inside of it.

If you want Semantic Release **to publish your package**, change `npmPublish` to `true`.

### Create a GL_TOKEN

1.  Create a **Project Token** under your project's **Settings > Access Tokens**

2.  Create a `GL_TOKEN` environment variable under your project's **Settings > CI/CD**

<!-- Using Semantic Release **prevents**:

- **Errors when manually tagging commits**
- **Inconsistent versioning leading to broken applications** -->
