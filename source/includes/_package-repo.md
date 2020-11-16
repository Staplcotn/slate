# Our Package Repo

## Using our Repo

```shell
yarn config set @scca:registry http:
```

You must set your NPM/Yarn to fetch packages scoped with `@scca` to use our Gitlab.

- .releaserc
- gitlab-ci with `use-yarn`
- publishConfig in `package.josn`
- https://docs.gitlab.com/ee/user/packages/npm_registry/index.html#publishing-a-package-with-cicd
