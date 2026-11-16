# powerjob-server

The scheduling server (Spring Boot), its Docker image, the MySQL image with schema and upgrade scripts, and deployment files.

Split from [PowerJob](https://github.com/PowerJob/PowerJob) at upstream commit
`d92ad66277ca54e14e2835b2e5a6aaf01f620684` (release 5.1.7). The Git history of retained files is preserved,
including earlier names of the modules; filtering rewrites commit IDs.

## Build

Install JDK 8 and Maven 3.9, then run from this repository:

```bash
mvn -B clean verify
```

This repository uses the PowerJob libraries from `powerjob-common`, `powerjob-client`.
Check them out next to this repository:

```text
workspace/
  powerjob-client/
  powerjob-common/
  powerjob-server/
```

The root `pom.xml` adds each sibling checkout to the Maven reactor, so their
sources are compiled in the same build and changes there are visible here
without installing anything. The parent POM `tech.powerjob:powerjob` comes from
`../powerjob-common`. A sibling that is not checked out is replaced by its
published `5.1.7` artifact from Maven Central.

JDK 8 is the version upstream targets and the version verified locally.
See [build-verification.json](build-verification.json) for the local build results.

## Included modules

- `powerjob-server/`

## Excluded tests

These upstream tests cannot pass in a clean build and are excluded in their
module's surefire configuration. Upstream CI does not run tests.

- `tech.powerjob.server.test.RepositoryTest` (powerjob-server/powerjob-server-starter): Needs MySQL at localhost:3306 (the test profile datasource); the Spring context cannot start without it.
- `tech.powerjob.server.test.ServiceTest` (powerjob-server/powerjob-server-starter): Needs MySQL at localhost:3306 (the test profile datasource); the Spring context cannot start without it.
- `tech.powerjob.server.test.ServerInfoServiceTest` (powerjob-server/powerjob-server-starter): Needs MySQL at localhost:3306 (the test profile datasource); the Spring context cannot start without it.
- `tech.powerjob.server.test.QueryConvertUtilsTest` (powerjob-server/powerjob-server-starter): Needs MySQL at localhost:3306 (the test profile datasource); the Spring context cannot start without it.

## Test JVM options

`powerjob-server/powerjob-server-starter` runs its tests with `-Duser.timezone=Asia/Shanghai`. Scheduler tests assert wall-clock times in the timezone upstream develops in.

`powerjob-server/powerjob-server-starter` runs each test class in its own JVM (`reuseForks=false`). Upstream's TimezoneTest changes the JVM default timezone, which breaks later tests in a shared JVM.

## Origin

See [UPSTREAM-README.md](UPSTREAM-README.md) for the original project overview
and [split.json](split.json) for the split manifest.

## License

Apache License 2.0; see [LICENSE](LICENSE).
