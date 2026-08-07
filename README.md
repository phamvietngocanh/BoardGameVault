BoardGameVault

A modern board game tracker that helps players record matches, preserve game night memories, and generate statistics from real play history.

BoardGameVault is designed with an offline-first philosophy. Once a game is imported, all future matches use locally stored data, ensuring fast performance and reliable access even without an internet connection.

⸻

Vision

BoardGameVault is not a board game rules engine.

It does not calculate who should win.

It records what actually happened at the table.

The application stores facts and generates reports from those facts.

⸻

Core Principles

* Record facts, not calculated values.
* Player is the center of the system.
* Statistics are always generated from recorded data.
* BoardGameGeek is used only when importing a game for the first time.
* Imported games become part of the local library.
* Preserve memories, not just scores.

⸻

Features

Authentication

* Username & Password login
* Link a User to an existing Player
* Simple account management

⸻

Player Management

* Create players
* Link players to user accounts
* Player avatar
* Active / inactive players

⸻

Game Library

* Search games from BoardGameGeek
* Import game name and cover image
* Save games locally
* No repeated BoardGameGeek requests

⸻

Match Recording

Record:

* Game
* Location
* Start time
* End time
* Participants
* Scores (optional)
* Winner(s)
* Notes
* Up to 3 photos

Supports:

* Score-based games
* Winner-only games
* Cooperative games
* Multiple winners (ties)

⸻

Statistics

Generated directly from recorded matches.

Examples:

* Total matches
* Total wins
* Monthly wins
* Yearly wins
* Most played games
* Win rate
* Head-to-head records
* Average score
* Average play time
* Most played locations

No statistics are permanently stored in the database.

⸻

Technology Stack

* Flutter
* Supabase
* PostgreSQL
* Riverpod
* GoRouter
* Hive
* Dio

⸻

Database Philosophy

The database stores only factual information.

Examples of stored data:

* Players
* Games
* Matches
* Scores
* Winners
* Locations
* Notes
* Photos

Examples of calculated data:

* Win rate
* Rankings
* Most played
* Average score
* Longest streak

Calculated data is never stored.

⸻

BoardGameGeek Integration

BoardGameGeek is only used when a game is imported for the first time.

The application stores:

* BoardGameGeek ID
* Game name
* Cover image

After import, all future matches use the local database.

⸻

Roadmap

v0.1

* Project foundation
* Documentation
* Database design

v0.2

* Authentication
* Player management

v0.3

* Game library

v0.4

* Match recording

v0.5

* Match history

v0.6

* Statistics

v1.0

* Stable release

⸻

License

MIT License
