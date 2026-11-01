# tinycast-fork

[Tinycast](https://github.com/abue-ammar/tinycast) by Abue Ammar, with one change: typing a command's
exact alias and pressing Space jumps into its argument. `yt`, Space, `dogs`, Return searches YouTube.

Personal build, not affiliated with Tinycast. Support the original project.

## What's here

This repo holds no copy of Tinycast's code, only what differs:

- [patches/0001-alias-space.patch](patches/0001-alias-space.patch): the Space behaviour, from
  [Tinycast Space Edition](https://github.com/sermelipharo/tinycast-space). Only an exact, case-sensitive
  alias jumps, and only with the caret at the end. `\` then Space types a plain space. Apps and System
  Settings panes never jump.
- [patches/0002-updater.patch](patches/0002-updater.patch): points the in-app updater here, so an update
  can't replace this build with one that lacks the change.
- [.github/workflows/release.yml](.github/workflows/release.yml): checks hourly for a new stable Tinycast
  release, applies the patches to it, builds, runs Tinycast's tests (with [gate.sh](gate.sh)), signs and
  publishes under the same version number. Each release includes the exact source of its build.

If a patch stops applying or breaks the build, nothing is published, installed copies stay on the last good
version, and the workflow opens a "Build blocked" issue. Fix the patch, then run the workflow by hand with
that tag: `gh workflow run release.yml -f tag=vX.Y.Z`.

## Install

Download the DMG from [Releases](https://github.com/seancxk/tinycast-fork/releases), drag Tinycast to
Applications, then clear the quarantine flag once. The build is self-signed, not notarized by Apple.

```sh
xattr -dr com.apple.quarantine /Applications/Tinycast.app
```

It keeps Tinycast's bundle ID, so it replaces the original and uses the same settings. Grant Accessibility
again the first time. To go back, install the original from its own releases.

## Signing

The `SIGNING_P12_BASE64` and `SIGNING_P12_PASSWORD` secrets hold the certificate, which exists nowhere else.
Don't replace it casually: installed copies only accept updates signed with the same one, and a new one means
reinstalling by hand and granting Accessibility again.

## License

AGPL-3.0, as Tinycast. See [LICENSE](LICENSE).
