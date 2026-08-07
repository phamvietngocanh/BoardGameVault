BoardGameVault Decisions

This document records important architectural and product decisions made during the development of BoardGameVault.

⸻

ADR-001

Title

Player is the central entity.

Status

Accepted

Decision

Players exist independently from user accounts.

Users are only used for authentication.

A User is linked to exactly one Player.

A Player may exist without a User account.

Reason

Most people in a board game group will not create an account immediately.

The application should allow recording matches first and let players claim their existing profile later.

⸻

ADR-002

Title

Store facts, not calculated values.

Status

Accepted

Decision

The database stores only factual information.

Statistics are always calculated from recorded matches.

Stored

* Players
* Games
* Locations
* Matches
* Scores
* Winners
* Notes
* Photos

Never Stored

* Win Rate
* Rankings
* Most Played
* Average Score
* Winning Streak
* Monthly Statistics
* Yearly Statistics
* Head-to-head Statistics

Reason

Calculated data can become inconsistent.

Facts never change.

⸻

ADR-003

Title

BoardGameVault is not a rules engine.

Status

Accepted

Decision

The application never determines who should win.

Users record the actual result.

Reason

Every board game has different rules.

Many games include complex tie-break systems or house rules.

The application records reality instead of trying to understand every game’s rules.

⸻

ADR-004

Title

Multiple winners are supported.

Status

Accepted

Decision

A match may contain zero, one, or multiple winners.

Reason

Some games officially allow tied winners.

Cooperative games may have no individual winner.

⸻

ADR-005

Title

BoardGameGeek is only used once.

Status

Accepted

Decision

BoardGameGeek is queried only when importing a game for the first time.

The imported game is then stored locally.

Future matches always use the local database.

Imported Data

* BoardGameGeek ID
* Game Name
* Cover Image

Reason

Improves performance.

Supports offline-first.

Removes dependency on BoardGameGeek during normal usage.

⸻

ADR-006

Title

Maximum three photos per match.

Status

Accepted

Decision

Each match can store up to three photos.

Reason

BoardGameVault preserves memories rather than acting as a photo gallery.

Three photos are sufficient to remember a game night while minimizing storage usage.

⸻

ADR-007

Title

Players are never deleted.

Status

Accepted

Decision

Players are marked as inactive instead of being deleted.

Reason

Historical match data must always remain valid.

Deleting a player must never remove match history.

⸻

ADR-008

Title

Games are stored locally.

Status

Accepted

Decision

Only the minimum required game information is stored.

Stored

* BoardGameGeek ID
* Name
* Cover Image

Reason

The application only needs enough information to identify and display games.

Additional BoardGameGeek metadata is intentionally ignored.

⸻

ADR-009

Title

Locations are reusable.

Status

Accepted

Decision

Locations are stored as reusable records instead of free text.

Reason

Allows faster match creation.

Enables future location-based statistics.

Avoids duplicated data.

⸻

ADR-010

Title

Every database change requires approval.

Status

Accepted

Decision

No table, column, relationship, or business rule may be added without Product Owner approval.

Process

Proposal

↓

Technical Review

↓

Product Owner Approval

↓

Database Update

Reason

Keeps the database simple, consistent, and intentional.

⸻

ADR-011

Title

Offline-first architecture.

Status

Accepted

Decision

BoardGameVault should continue working without an internet connection whenever possible.

Internet is only required when importing a game from BoardGameGeek or synchronizing data.

Reason

Board game sessions often happen in places with unreliable internet.

The application should never prevent users from recording a match because of connectivity issues.

⸻

ADR-012

Title

Keep the schema minimal.

Status

Accepted

Decision

New tables and columns are added only when they solve a real user problem.

Future features should not influence the current schema.

Reason

A smaller database is easier to maintain, understand, and evolve over time.
