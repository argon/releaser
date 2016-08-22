# Releaser

Simple script to take the release-parts of git flow and automate them for our components.

Creates a release branch from current HEAD.

## release_create

```
release_create <path_to_repository>
```

Creates a new release branch from current HEAD. Should be run from develop.

Steps:

1. Prompts for an updated version number (e.g. `0.2.0`)
1. Prompts for a release name (must be git ref "safe", e.g. `first-release`)
1. Reformats `release_notes.md` with a new header containing release details
1. Creates a new branch (e.g. `release/0.2.0-first-release`)
1. Commits all changes with a suitable message

## release_package

```
release_package <path_to_repository>
```

Performs merges and tagging to update master with the latest version

1. Update `master` from origin
1. Merge release branch into `master`
1. Push `master`
1. Tags `master` in the format `<version><name>`
1. Push tags
1. Creates a new GitHub release with the latest release notes in the body
1. Merges `develop` into release branch and pushes to allow CI to finish

## release_merge

```
release_merge <path_to_repository>
```

Performs final merge of release branch into `develop` and cleans up release branches

1. Merges release branch into `develop` through GitHub API
1. Deletes remote release branch
1. Deletes local release branch

## Prerequisites

### Repository

Protected `develop` branch. Unprotected `master`

### Environment

A valid GitHub Personal Access Token must be available from [`gitcredentials`](https://git-scm.com/docs/gitcredentials)
or in the environment variable `$GITHUB_TOKEN`.

### Files

* `version` - containing the current version number
* `release_notes.md` - Containing up to date release notes with the latest
  changes in the format:
```
### Features

* [..]

### Fixes

* [..]

- - -
```

## Future work

* Provide hooks to different backing-stores for the version number (perhaps by
  overriding the bash function
* Add support for protected master branch (push release, merge with GH API)
