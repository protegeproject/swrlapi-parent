# Publishing SWRLAPI to Maven Central

For a coordinated five-artifact release, prefer `../swrlapi-project/release/README.md`. That workflow tests all repositories, builds one signed bundle, and publishes the five main artifacts together. The per-repository workflow below remains available but must not be mixed with a coordinated release.

The `maven-central-release` profile uses Central Publisher Portal rather than the retired OSSRH service. Maven Release Plugin still handles version changes and Git tags. The parent configures the forked release build to activate this profile.

## Workstation setup

Use Java 11 or later and Maven. Install GnuPG and make `gpg` available on PATH. Use the existing release signing key; do not generate a replacement merely to work around a missing installation.

In an interactive terminal, check signing before preparing a release:

```sh
export GPG_TTY=$(tty)
gpg --list-secret-keys --keyid-format LONG
printf 'Signing test\n' | gpg --clearsign
```

Generate a Central Publisher Portal user token for an account authorized for `edu.stanford.swrl`. Configure its token username and password in your private Maven settings under server ID `central`. Do not commit credentials. Legacy OSSRH tokens are not a substitute for Portal tokens.

Snapshot publication uses `https://central.sonatype.com/repository/maven-snapshots/`; enable snapshots for the namespace in the Portal before using it.

## Validate first

```sh
mvn -Pmaven-central-release verify
```

This includes signing but does not publish. `-Dgpg.skip=true` can check the build configuration without signing; it does not establish that the release is ready to publish.

For an already prepared release-version checkout, the following creates a signed bundle without uploading:

```sh
mvn -Pmaven-central-release -DskipUpload=true -Dmaven.install.skip=true deploy
```

The bundle is in `target/central-publishing/central-bundle.zip`. A SNAPSHOT checkout follows snapshot deployment behavior rather than creating a release bundle. Do not use `skipPublishing` as a substitute for `skipUpload`: it omits the artifacts entirely.

## Release

Begin with a clean development branch and released versions of all parent and SWRL dependencies. Commit the publishing configuration before release preparation, so it is present in the tagged checkout.

```sh
export SWRLAPI_RELEASE_VERSION=2.1.4
export SWRLAPI_RELEASE_TAG=release-${SWRLAPI_RELEASE_VERSION}
mvn -Pmaven-central-release --batch-mode \
  -Dtag="$SWRLAPI_RELEASE_TAG" \
  -DreleaseVersion="$SWRLAPI_RELEASE_VERSION" \
  -DdevelopmentVersion=2.1.5-SNAPSHOT \
  -DscmCommentPrefix='[ci skip] ' release:prepare
mvn -Pmaven-central-release release:perform
```

`release:prepare` commits and tags, and can push these changes. `release:perform` checks out that tag and uploads its artifacts for Portal validation. With `autoPublish=false`, finish publication in the Portal after reviewing the validated deployment. Do not set `autoPublish=true` unless automatic final publication is intended.

Publish in dependency order: swrlapi-parent, swrlapi, swrlapi-drools-engine, then swrltab-plugin and other consumers. Confirm each release is available before building dependent releases; update their parent/dependency versions from SNAPSHOT to the published version. The auxiliary repositories may still target older snapshots and require their own version updates.

## Interrupted legacy release

Do not resume a `release.properties` generated with `oss-parent`: it can contain `exec.additionalArguments=-Psonatype-oss-release`. Preserve the POM, `pom.xml.releaseBackup`, and `release.properties` before recovery. Inspect commits and remote tags before restarting; do not overwrite an existing release tag.

For this migration, the interrupted state stopped at `generate-release-poms`. Its files were backed up outside the repository under `../release-backups/20261029-135913/`, the development version restored, and the stale active release files removed. No release rollback command, remote tag operation, or publication was performed.

## References

- https://central.sonatype.org/publish/publish-portal-maven/
- https://central.sonatype.org/publish/publish-portal-snapshots/
- https://maven.apache.org/plugins/maven-gpg-plugin/usage.html
