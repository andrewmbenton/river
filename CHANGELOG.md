# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Changed

- `Stop` and `StopAndCancel` have been changed to respect the provided context argument. When that context is cancelled or times out, those methods will now immediately return with the context's error, even if the Client's shutdown has not yet completed. Apps may need to adjust their graceful shutdown logic to account for this. PR #79.

## [0.0.10] - 2023-11-26

### Added

- Added `Example_scheduledJob`, demonstrating how to schedule a job to be run in the future.

### Fixed

- Fixed a panic in the periodic job enqueuer caused by sometimes trying to reset a `time.Ticker` with a negative or zero duration. Fixed in PR #73.

### Changed

- `DefaultClientRetryPolicy`: calculate the next attempt based on the current time instead of the time the prior attempt began.

## [0.0.9] - 2023-11-23

### Fixed

- **DATABASE MIGRATION**: Database schema v3 was introduced in v0.0.8 and contained an obvious flaw preventing it from running against existing tables. This migration was altered to execute the migration in multiple steps.

## [0.0.8] - 2023-11-21

### Changed

- License changed from LGPLv3 to MPL-2.0.
- **DATABASE MIGRATION**: Database schema v3, alter river_job tags column to set a default of `[]` and add not null constraint.

## [0.0.7] - 2023-11-20

### Changed

- Constants renamed so that adjectives like `Default` and `Min` become suffixes instead of prefixes. So for example, `DefaultFetchCooldown` becomes `FetchCooldownDefault`.
- Rename `AttemptError.Num` to `AttemptError.Attempt` to better fit with the name of `JobRow.Attempt`.
- Document `JobState`, `AttemptError`, and all fields its fields.
- A `NULL` tags value read from a database job is left as `[]string(nil)` on `JobRow.Tags` rather than a zero-element slice of `[]string{}`. `append` and `len` both work on a `nil` slice, so this should be functionally identical.

## [0.0.6] - 2023-11-19

### Changed

- `JobRow`, `JobState`, and a other related types move into `river/rivertype` so they can more easily be shared amongst packages. Most of the River API doesn't change because `JobRow` is embedded on `river.Job`, which doesn't move.

## [0.0.5] - 2023-11-19

### Changed

- Remove `replace` directive from the project's `go.mod` so that it's possible to install River CLI with `@latest`.

## [0.0.4] - 2023-11-17

### Changed

- Allow River clients to be created with a driver with `nil` database pool for use in testing.
- Update River test helpers API to use River drivers like `riverdriver/riverpgxv5` to make them agnostic to the third party database package in use.
- Document `Config.JobTimeout`'s default value.
- Functionally disable the `Reindexer` queue maintenance service. It'd previously only operated on currently unused indexes anyway, indexes probably do _not_ need to be rebuilt except under fairly rare circumstances, and it needs more work to make sure it's shored up against edge cases like indexes that fail to rebuild before a clien restart.

## [0.0.3] - 2023-11-13

### Changed

- Fix license detection issues with `riverdriver/riverpgxv5` submodule.
- Ensure that river requires the `riverpgxv5` module with the same version.

## [0.0.2] - 2023-11-13

### Changed

- Pin own `riverpgxv5` dependency to v0.0.1 and make it a direct locally-replaced dependency. This should allow projects to import versioned deps of both river and `riverpgxv5`.

## [0.0.1] - 2023-11-12

### Added

- This is the initial prerelease of River.
