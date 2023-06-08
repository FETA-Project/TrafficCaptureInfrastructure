# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## 1.3.2 - 2022.09.07
### Added
- Support for adding Script descriptions when creating new scripts.
- Created the `PyTCI` library.
- Created the `ts-hanicparser` library for parsing HANIC filters.
- Warning validator for the HANIC filter field.

### Fixed
- Removed residual Flake8 error and warnings for the web interface.
- Fixed the required authorization for creating jobs without a script.
- Fixed error when setting an empty string as capture filter.
- Show real progress in job dialog.
- Job stuck in processing when TCI Master does not have job file ready.

### Changed
- Updated the CI runner to use Fedora 36.
- Updated Flake8 to version 4.0.0.
- Changed HANIC filter field to TextArea.
- Increased the default table pagination to 10 items.

## 1.3.1 - 2022.08.02
### Added
- Url scheme support (http/https) to `config.toml`
- User applications (API Keys).

### Fixed
- Multiple lines in one progress bar in job dialog.
- Recognizes 'url' setting in `config.toml`
- Progress bar progress when job starts.
- Refreshing token could cause freezing.

### Changed
- Brighter color to 'To process' badge in jobs.

## 1.3.0 - 2022.07.18
Note: The whole frontend has been reworked, many changes not listed.
### Added
- Support for line colors.
- Support for server URL configuration.
- Logs can be viewed and downloaded from API.

### Fixed
- Error message when script fails shows `stderr` and its return code.
- Downloading of raw pcap file now works (can also download pcapng).
- Output files correctly deleted when deleting a job.
- Inactive lines automatically removed.

### Changed
- Updated package versions in `requirements.txt`.
- Code review including dependency updates.
- Removed 5000-character limit for job filter.
- Frontend changed to angular (it is now a seperate unit).
- Many of the api calls changed to accomodate frontend change.
- Support for config file (removed .env, added config.toml).

## 1.2.0 - 2021.10.06
### Added
- GitLab's security checking using `Bandit`, `ESLint`, `Semgrep`, and `Secret detector`.
- Log files now include information about the PCAP scripts.
- Documentation can now include `Graphviz` diagrams.

### Fixed
- Password length limit is checked in the password setter.
- Checking whether the user is attempting to log in as a `System` user is done directly in the `login` method.
- All tests drop tables before running.
- The user page is rendered again after all requests finish.

### Changed
- Job filter length limit increased to 5000 characters.
- User password length limit increased to 500 characters.
- When a job is downloaded, the file names correspond to the job name.


## 1.2.0-rc.3 - 2021.09.17
### Fixed
- Changing path to directory with temporary files is now possible.

## 1.2.0-rc.2 - 2021.09.17
### Fixed
- Boolean values in config file are now correctly converted from strings.


## 1.2.0-rc.1 - 2021.09.16
Note: This is a very large release, many smaller changes are not listed.
### Added
- A REST API to interface with the system programmatically.
- LDAP support, so users can connect to the system using an existing account.
- Traffic can now be captured using TCPDump on the TCI backend.
- Support for database migrations.
- Local testing and development is possible using Docker containers.
- Added automatic tests and coverage reports.
- Code quality is automatically measured using Flake8.

### Changed
- A complete backend restructure and rewrite. Notable:
    - Database models are now contained in separate files, each containing one database table.
    - All important methods for dealing the database models are contained within them.
- Logs are now split on a weekly basis.

### Fixed
- Captured traffic can now be processed using Docker containers.


## 1.1.1 - 2021.03.07
### Added
- Line overview which shows the number of pending jobs per line.
- Scripts can no longer be deleted if assigned to some job in the [waiting, running, done] state.
- A Docker image for processing jobs.

### Changed
- Administrators can delete other administrators.


## 1.1.0 - 2020.12.13
### Added
- Table pagination for all tables.
- Better log scrolling.
- Progress bar in job modal dialog.
- Line state is shown with color.
- HANIC filter hint in new job form.
- Job table contains more information.
- Hints when creating a new script.

### Changed
- Wrong username and password return the same error message, and take the same amount of time.
- Buttons no  longer overlap.
- All pages have unified design.
- Most errors are handled/presented in the same way.
- Checkboxes don't uncheck on wrong form input.
