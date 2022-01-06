# Path filtering + config splitting on CircleCI

Forked from [circle-makotom/circle-advanced-setup-workflow](https://github.com/circle-makotom/circle-advanced-setup-workflow), with only minor changes.

This repository demonstrates an advanced use case of setup workflow feature on CircleCI. For instance, it implements both path filtering and config splitting.

## Files

* `.circleci/config.yml` implements both 1) the setup workflow, and 2) common resources (i.e., jobs and commands) for main workflows/jobs.
* `module-a/.circleci/config.yml`, `module-b/.circleci/config.yml`, and `module-c/.circleci/config.yml` implement independent modular configs for module A, B, and C, respectively.

## How does it work?

1.  Upon the initial trigger, CircleCI triggers the setup job `setup-dynamic-config` defined in `.circleci/config.yml`.
2.  Given a list of directories, detect which subdirectories (herein modules) have changes. (cf. `list-changed-modules`)
3.  Fetch `path-to-module/.circleci/config.yml` for each module to build, and merge all the fetched `config.yml` (along with the config defining common resources, i.e., `.circleci/config.yml`) using `yq`. (cf. `merge-modular-configs`)
4.  Trigger execution of the merged config.
