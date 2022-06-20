# Jest Buildkite Reporter [![Build & Release](https://github.com/vital-software/jest-buildkite-reporter/actions/workflows/build-and-release.yml/badge.svg?branch=main)](https://github.com/vital-software/jest-buildkite-reporter/actions/workflows/build-and-release.yml) [![Build status](https://badge.buildkite.com/a8851c8af61c24e1b43bc6f028878cef5af43f15130d3db98e.svg?branch=main)](https://buildkite.com/vital/jest-buildkite-reporter?branch=main)

Report Jest test results in [Buildkite](https://buildkite.com/) output as Annotations.

## Usage

Install the package:

```bash
npm install -D jest-buildkite-reporter
# or
yarn add -D jest-buildkite-reporter
```

Add it your Jest `reporters` configuration. If you don't have any reporters you should also add the `default` one or you will lose the console output.

```javascript
module.exports = {
  reporters: ["default", "jest-buildkite-reporter"],
};
```

The reporter only run when it detect that buildkite is present,
so there is no need to conditionally include it.

### Inside docker

When your tests are running in a docker containter they won't have access to buildkite by default and some environment variables need to be passed to them.

When `jest` is called as a `RUN` step inside the `Dockerfile` it need to specify the following args:

```dockerfile
ARG BUILDKITE
ARG BUILDKITE_AGENT_ACCESS_TOKEN
ARG BUILDKITE_JOB_ID

RUN yarn run jest
```

And they need to be passed to the `docker build` command:

```bash
docker build --build-arg BUILDKITE --build-arg BUILDKITE_AGENT_ACCESS_TOKEN --build-arg BUILDKITE_JOB_ID .
```

## Authoring

This projects uses [Semantic Release](https://github.com/semantic-release/semantic-release). To publish a new version, ensure you have pushed to the `main` branch,
and use one of the following commit message types to trigger a release:

| Commit message                                                                                                                                                                                   | Release type               |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------- |
| `fix(pencil): stop graphite breaking when too much pressure applied`                                                                                                                             | ~~Patch~~ Fix Release      |
| `feat(pencil): add 'graphiteWidth' option`                                                                                                                                                       | ~~Minor~~ Feature Release  |
| `perf(pencil): remove graphiteWidth option`<br><br>`BREAKING CHANGE: The graphiteWidth option has been removed.`<br>`The default graphite width of 10mm is always used for performance reasons.` | ~~Major~~ Breaking Release <br /> (Note that the `BREAKING CHANGE: ` token must be in the footer of the commit) |

## License

This project is using the [MIT](LICENSE) license.

## Similar projects

- [junit-annotate-buildkite-plugin](https://github.com/buildkite-plugins/junit-annotate-buildkite-plugin) Official Buildkite plugin for JUnit
- [jest-teamcity-reporter](https://github.com/winterbe/jest-teamcity-reporter) Jest reporter for TeamCity
- [jest-teamcity](https://github.com/itereshchenkov/jest-teamcity) Another Jest reporter for TeamCity
- [bugcrowd-test-summary-buildkite-plugin](https://github.com/bugcrowd/test-summary-buildkite-plugin) BugCrowd reporter for BuildKite
