---
title: Buildah version 1.46.0 Release Announcement
layout: default
author: tsweeney
categories: [releases]

tags: community, open source, buildah, hpc, opensource, containers, images, image
---
![buildah logo](https://buildah.io/images/buildah.png)

# Buildah version 1.46.0 Release Announcement

We're pleased to announce the release of [Buildah](https://github.com/podman-container-tools/buildah) [version 1.46.0](https://github.com/podman-container-tools/buildah/releases/tag/v1.46.0), which is now available from GitHub for any Linux distro.  We are shipping this release on Fedora 45.  Buildah will also be shipped on CentOS, OpenSUSE, and Ubuntu soon. In addition, container images will be available at https://quay.io/repository/buildah/stable and https://quay.io/repository/podman-container-tools/buildah.

The Buildah project has continued to grow over the past several weeks, welcoming several new contributors to the mix.  This release features notable changes: 
<!--readmore -->
 * A `--umask` option has been added to the `buildah run` command.
 * Using `--log-level error` or `--log-level fatal` now silences Containerfile step output.
 * Docker-compatible `--keep-git-dir` support has been added for `ADD` of Git sources.
 * Builds now honor `containers.conf` `http_proxy` and environment settings.
 * An `--include` flag has been added to `buildah add` and `buildah copy`.
 * An `--required-path` option has been added to `buildah add` and `buildah copy`.
 * Cache mounts now support `sharing=private`.
 * Directory mode and ownership are preserved for `RUN --mount`.

This release comprises changes made for v1.46.0 and will be included in Podman v6.2.

## Release Changes
### Changes for v1.46.0
   * The `buildah info` command now includes information about the system CDI (container device interface) settings by [@nalind](https://github.com/nalind) in [#7009](https://github.com/podman-container-tools/buildah/pull/7009)
   * The `buildah run` command now accepts an optional `--umask` flag by [@nalind](https://github.com/nalind) in [#7087](https://github.com/podman-container-tools/buildah/pull/7087)
   * The `buildah build` and `buildah push` commands will honor --log-level error/fatal by suppressing output. Previously only --quiet did so but it killed the `RUN`  command output by [@ajoshua2004](https://github.com/ajoshua2004) in [#7088](https://github.com/podman-container-tools/buildah/pull/7088)
   * The `buildah pull` command now recognizes `--policy=newer` as equivalent to `--policy=ifnewer`by [@nalind](https://github.com/nalind) in [#7000](https://github.com/podman-container-tools/buildah/pull/7000)
   * The output format of logs has changed, from using logrus prefixes+quoting to a using log/slog prefixes+quoting by [@mtrmac](https://github.com/mtrmac) in [#7084](https://github.com/podman-container-tools/buildah/pull/7082)
   * An `ADD` Containerfile command from git sources no longer includes the `.git` directory by default. Either  `--keep-git-dir` or `--keep-git-dir=true` is required to include it by [@simonbrauner](https://github.com/simonbrauner) in [#7062](https://github.com/podman-container-tools/buildah/pull/7062)
   * Fixed escape guard and option normalization in parsevolume routines by [@vtushar06](https://github.com/vtushar06) in [#7035](https://github.com/podman-container-tools/buildah/pull/7035)
   * Reject malformed mount options when running `buildah build` by [@leonardomoreira00](https://github.com/leonardomoreira00) in [#6998](https://github.com/podman-container-tools/buildah/pull/6998)
   * The `RUN --mount` Containerfile command no longer resets parent directory mode and ownership with --layers by [@simonbrauner](https://github.com/simonbrauner) in [#6981](https://github.com/podman-container-tools/buildah/pull/6981)
   * Improved error messaging when `lchown` fails in rootless mode due to exhausted subuid/subgid ranges by [@hihry](https://github.com/hihry) in [#7107](https://github.com/podman-container-tools/buildah/pull/7107)
   * Retrieval of build contexts which are specified using HTTPS URLs now respect the value of the `--tls-verify` flag by [@nalind](https://github.com/nalind) in [#6441](https://github.com/podman-container-tools/buildah/pull/6441)
   * Honor `containers.conf` `http_proxy` and `env` settings during image builds, aligning the behavior with `podman run` and fixed regression introduced in 6.0.x by [@unlimitedsola](https://github.com/unlimitedsola) in [#6996](https://github.com/podman-container-tools/buildah/pull/6996)
   * The `buildah build --layers` command now correctly reuses cached layers for `ADD` instructions with git sources when the source content is unchanged by [@ajoshua2004](https://github.com/ajoshua2004) in [#7121](https://github.com/podman-container-tools/buildah/pull/7121)
   * Cache mounts now accept sharing=private, which gives the command a cache directory that nothing else is using by [@akca](https://github.com/akca) in [#7127](https://github.com/podman-container-tools/buildah/pull/7127)
   * Fixed a bug where `buildah manifest push --digestfile` wrote an empty digest file and exited 0 when a push failed by [@Thundercloud12](https://github.com/Thundercloud12) in [#7137](https://github.com/podman-container-tools/buildah/pull/7137)
   * Fixed layer caching for `COPY --link` and `ADD --link` Container file commands with relative destination paths by [@ajoshua2004](https://github.com/ajoshua2004) in [#7135](https://github.com/podman-container-tools/buildah/pull/7135)
   * The `--checksum` option in a `ADD` Containerfile command can now use Git commit SHAs (or a prefix of one) for Git sources, across `buildah add`, `buildah build`, and `buildah copy` by [@ajoshua2004](https://github.com/ajoshua2004) in [#7101](https://github.com/podman-container-tools/buildah/pull/7101)
   * The `buildah copy --include` and `buildah add --include ` commands are the complement to the `buildah copy --exclude` and `buildah add --exclude` commands by [@ajoshua2004](https://github.com/ajoshua2004) in [#6988](https://github.com/podman-container-tools/buildah/pull/6988)
   * Fixed "path escapes from parent" issues when copying through absolute in-container symlinks (e.g. /var/run → /run) by [@Honny1](https://github.com/Honny1) in [#7129](https://github.com/podman-container-tools/buildah/pull/7129)
   * Fixed a panic, "assignment to entry in nil map", during concurrent multi-stage builds when a stage fails while sibling stages are still running while running via podman system service by [@hihry](https://github.com/hihry) in [#7161](https://github.com/podman-container-tools/buildah/pull/7161)
   * Fixed a bug where --parents would leak host or build context ancestor directories when using buildah copy --parents or COPY --parents into the built image as real directory entries by [@ajoshua2004](https://github.com/ajoshua2004) in [#7170](https://github.com/podman-container-tools/buildah/pull/7170)
   * Buildah now properly supports mounting the same file in separate stages while one of the stage executes the file by [@BenjaminSchubert](https://github.com/BenjaminSchubert) in [#7163](https://github.com/podman-container-tools/buildah/pull/7163)
   * Added the `--required-path` option to the `add/copy` commands and it must be used with the `--include` option to make sure certain paths were copied over during `--include` by [@ajoshua2004](https://github.com/ajoshua2004) in [#7157](https://github.com/podman-container-tools/buildah/pull/7157)


* Documentation:
   * Docs: fix typos and phrasing in man pages by [@SatyamPandey-07](https://github.com/SatyamPandey-07) in [#7076](https://github.com/podman-container-tools/buildah/pull/7076)
   * Docs: fix typo, --policy expects ifnewer instead of newer by [@leonardomoreira00](https://github.com/leonardomoreira00) in [#6993](https://github.com/podman-container-tools/buildah/pull/6993)
   * Docs: fix grammar, articles, and SEE ALSO reference in man pages by [@SatyamPandey-07](https://github.com/SatyamPandey-07) in [#7106](https://github.com/podman-container-tools/buildah/pull/7106)
   * Docs(login): note --tls-verify needs = for false by [@locker95](https://github.com/locker95) in [#7022](https://github.com/podman-container-tools/buildah/pull/7022)
   * Update the Maintainers and Governance files to link to community repo by [@Luap99](https://github.com/Luap99) in [#7023](https://github.com/podman-container-tools/buildah/pull/7023)
   * Link to the org wide LLM_POLICY by [@Luap99](https://github.com/Luap99) in [#7018](https://github.com/podman-container-tools/buildah/pull/7018)
   * Link to the org wide SUPPORT.md, CONTRIBUTING.md and SECURITY.md by [@Luap99](https://github.com/Luap99) in [#7116](https://github.com/podman-container-tools/buildah/pull/7116)
   * Add Honny1 as Maintainer by [@Luap99](https://github.com/Luap99) in [#7063](https://github.com/podman-container-tools/buildah/pull/7063)
   * MAINTAINERS.md: move lsm5 to alumni by [@lsm5](https://github.com/lsm5) in [#7128](https://github.com/podman-container-tools/buildah/pull/7128)


* Vendored: 
  * Vendor github.com/containerd/platforms to v1.0.0-rc.5
  * Vendor github.com/mattn/go-shellwords to v1.0.16
  * Vendor github.com/moby/buildkit to v0.34.0
  * Vendor github.com/moby/go-archive to v0.3.3
  * Vendor github.com/moby/moby/client to v0.6.2
  * Vendor github.com/moby/sys/userns to v0.2.1
  * Vendor github.com/opencontainers/cgroups to v0.1.0
  * Vendor github.com/opencontainers/runc to v1.5.2
  * Vendor github.com/openshift/imagebuilder to v1.2.22
  * Vendor github.com/seccomp/libseccomp-golang to v0.12.0
  * Vendor github.com/sirupsen/logrus to v1.10.2
  * Vendor go.podman.io/common to v0.70.0
  * Vendor go.podman.io/image/v5 to v5.42.0
  * Vendor go.podman.io/storage to v1.65.0
  * Vendor golang.org/x/crypto to v0.58.0
  * Vendor golang.org/x/sync to v0.24.0
  * Vendor golang.org/x/sys to v0.49.0
  * Vendor golang.org/x/term to v0.47.0
  * Vendor google.golang.org/grpc to v1.84.0
  * Vendor google.golang.org/protobuf to v1.36.12
  * Vendor tags.cncf.io/container-device-interface to v1.1.1

  
* Tests:
   * Run tests in parallel on testing-farm by [@nalind](https://github.com/nalind) in [#7015](https://github.com/podman-container-tools/buildah/pull/7015)
   * Validate: run the helpers self-test by [@vtushar06](https://github.com/vtushar06) in [#7036](https://github.com/podman-container-tools/buildah/pull/7036)
   * Tests: cover id-mapped capability xattrs by [@giuseppe](https://github.com/giuseppe) in [#7086](https://github.com/podman-container-tools/buildah/pull/7086)
   * Revert part of #7077, use testing.Context() in conformance tests by [@nalind](https://github.com/nalind) in [#7096](https://github.com/podman-container-tools/buildah/pull/7096)
   * Test(bud): cover symbolic COPY/ADD --chmod by [@paoloantinori](https://github.com/paoloantinori) in [#7065](https://github.com/podman-container-tools/buildah/pull/7065)
   * Add more flags to test helpers by [@nalind](https://github.com/nalind) in [#7008](https://github.com/podman-container-tools/buildah/pull/7008)
   * tests: only use cached images for the integration tests by [@Luap99](https://github.com/Luap99) in [#7120](https://github.com/podman-container-tools/buildah/pull/7120)
   * Integration test nits by [@kolyshkin](https://github.com/kolyshkin) in [#7145](https://github.com/podman-container-tools/buildah/pull/7145)
   * Integration tests: set $CONTAINERS_POLICY_JSON by [@nalind](https://github.com/nalind) in [#7104](https://github.com/podman-container-tools/buildah/pull/7104)
   * Tests: stabilize manifest-skip all-platforms test in CI by [@Honny1](https://github.com/Honny1) in [#7148](https://github.com/podman-container-tools/buildah/pull/7148)
   * In the chrootuser test, do not let one bad line end the lookup by [@vtushar06](https://github.com/vtushar06) in [#7047](https://github.com/podman-container-tools/buildah/pull/7047)


* Changes to the build infrastructure:
   * Packit config update to skip TF jobs for unavailable composes and Fedora-45 downstream addition by [@lsm5](https://github.com/lsm5) in [#7017](https://github.com/podman-container-tools/buildah/pull/7017)
   * CI: enable several linters by [@nalind](https://github.com/nalind) in [#7021](https://github.com/podman-container-tools/buildah/pull/7021)
   * Add zizmor workflow by [@Luap99](https://github.com/Luap99) in [#7052](https://github.com/podman-container-tools/buildah/pull/7052)
   * Packit: koji job target taken from rawhide config by [@lsm5](https://github.com/lsm5) in [#7123](https://github.com/podman-container-tools/buildah/pull/7123)
   * Makefile: add install.nobuild target to install ONLY. by [@rahilarious](https://github.com/rahilarious) in [#6984](https://github.com/podman-container-tools/buildah/pull/6984)


## New Contributors
  * [@vtushar06](https://github.com/vtushar06) made their first contribution in [#7047](https://github.com/podman-container-tools/buildah/pull/7047)
  * [@SatyamPandey-07](https://github.com/SatyamPandey-07) made their first contribution in [#7076](https://github.com/podman-container-tools/buildah/pull/7076)
  * [@paoloantinori](https://github.com/paoloantinori) made their first contribution in [#7065](https://github.com/podman-container-tools/buildah/pull/7065)
  * [@hihry](https://github.com/hihry) made their first contribution in [#7107](https://github.com/podman-container-tools/buildah/pull/7107)
  * [@unlimitedsola](https://github.com/unlimitedsola) made their first contribution in [#6996](https://github.com/podman-container-tools/buildah/pull/6996)
  * [@locker95](https://github.com/locker95) made their first contribution in [#7022](https://github.com/podman-container-tools/buildah/pull/7022)

## Try it Out.
 
If you haven’t yet, [install Buildah](https://github.com/podman-container-tools/buildah/blob/main/install.md) from one of the Linux repos or GitHub and give it a spin. We’re betting you'll find it’s an easy and quick way to build containers in your environment without a daemon being involved!

For those of you who contributed to this release, thank you very much for your contributions! If you haven't joined our community yet, don't wait any longer! Come join us on GitHub, where Open Source communities live.
