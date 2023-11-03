# Counting Changelog

This format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/) and this project mostly adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

As a public API does not exist, whether to increment the major, minor, or patch version, is arbitrarily determined by how much the changes will impact the end user. A new set of guidelines, differing from SemVer, is used:

- **Major**: A functionality change that the end user will notice, and need to adapt to.
- **Minor**: A functionality change that the end user will notice, but will not need to adapt to \[significantly\].
- **Patch**: A change that the end user will not notice, **OR** any bug fixes that the end user does not need to *adapt* to (without account for notice).

## [Unreleased]
### Added
- Basic information about OP in message resends (for context in replies)

### Changed
- Updated documentation link to new site

### Fixed
- Bot deleting its own messages thinking they were edited (underlying issue not fixed, this is just a stopgap)
- Issue that caused some exceptions to not be handled properly

## [2.1.0] - 2023-10-28
### Added
- Ability for OP to delete select messages via reaction
- Additional functions to the evaluator:
    - `floor`/`rounddown`/`round_down`
    - `ceil`/`roundup`/`round_up`
    - `round`
    - `sqrt`/`sqroot`/`squareroot`
    - `sin`
    - `cos`
    - `tan`
    - `degrees`
    - `radians`
    - `abs`
    - `bitxor`
    - `bitor`
    - `log`
    - `ln`
- Allow bot devs to bypass message resend mechanism via `//` in front of the message, for unexpected circumstances
- Descriptive outputs for when user input is assumed to be an expression, but could not be parsed
- Documentation (this document), and `help` keyword to link to it
- `pi` and `e` as constants
- Support for replies and image attachments in message resends
- Versioning of plugin (in footer of embeds)

### Changed
- Limit on iterable length (default → `10000`)
- Raised grace period for duplicate messages to account for extra evaluation time (`0.75` → `1`)
- Raised limits on power calculations (`1000` → `10000`)

### Removed
- All random functions (`rand()`, `randint()`)

### Fixed
- Edge cases resulting from parallel on_message & on_message_edit/on_message delete handling

### Security
- Fixed a problem with the backup timeout system that prevented it from working properly

## [2.0.0] - 2023-10-26
### Added
- Automatically attempt to recover when count is lost through a multitude of methods
- Command to allow bot developers to override count in exceptional circumstances (bug caused by bot, etc.)
- Grace period for duplicate numbers in count
- Prevent users from counting twice in a row
- Protections for message edits & deletes that prevent last number from getting changed/deleted
- Support for evaluation of a wide variety of expressions

### Changed
- Bot now resends non-count messages as an embed, with the last count at the bottom

### Removed
- "please refrain from chatting" message (superseded by message resending)

### Fixed
- Parallel on_message processing that could result in edge case

## [1.0.0] - 2023-10-14
### Added
- Initial Release

<!-- Hide ToC entries for ### and under, as those are the repetitive "Added", "Changed", etc -->
<style>
.md-sidebar--secondary .md-nav__list .md-nav__list {display: none}
</style>
