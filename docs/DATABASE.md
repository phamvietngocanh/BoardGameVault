BoardGameVault Database

Version: 1.0
Status: LOCKED

⸻

Design Principles

* Store facts, not calculated values.
* Player is the center of the system.
* BoardGameGeek is only used when importing a game for the first time.
* Imported games become part of the local database.
* Statistics are always generated from recorded data.
* No table or column may be added without Product Owner approval.

⸻

Entity Relationship

Users
   │
   │ 1 : 1
   ▼
Players
   │
   │
   ▼
MatchPlayers
   ▲
   │
Matches
   │
   ├──────────► Games
   │
   └──────────► Locations

⸻

Table: users

Purpose

Stores authentication information.

Columns

Column	Type	Nullable	Notes
id	UUID	No	Primary Key
username	VARCHAR(50)	No	Unique
password_hash	TEXT	No	Hashed password
created_at	TIMESTAMP	No	Created time
updated_at	TIMESTAMP	No	Last updated

Relationships

* One User owns exactly one Player.

Business Rules

* Username must be unique.
* Passwords are always stored as hashes.
* User data is used only for authentication.

⸻

Table: players

Purpose

Represents a real person who participates in board games.

Columns

Column	Type	Nullable	Notes
id	UUID	No	Primary Key
user_id	UUID	Yes	FK → users.id
display_name	VARCHAR(100)	No	Player name
avatar_url	TEXT	Yes	Avatar image
is_active	BOOLEAN	No	Default TRUE
created_at	TIMESTAMP	No	Created time
updated_at	TIMESTAMP	No	Last updated

Relationships

* One User ↔ One Player.
* One Player → Many MatchPlayers.

Business Rules

* A Player may exist without a User.
* Player names are not required to be unique.
* Players are never deleted.
* Inactive players remain in history.

⸻

Table: games

Purpose

Stores games imported from BoardGameGeek.

Columns

Column	Type	Nullable	Notes
id	UUID	No	Primary Key
bgg_id	INTEGER	No	BoardGameGeek ID
name	VARCHAR(255)	No	Game name
image_url	TEXT	Yes	Cover image
created_at	TIMESTAMP	No	Imported time
updated_at	TIMESTAMP	No	Last updated

Relationships

* One Game → Many Matches.

Business Rules

* A game is imported from BoardGameGeek only once.
* Future matches always use the locally stored game.
* No automatic synchronization with BoardGameGeek.

⸻

Table: locations

Purpose

Stores reusable play locations.

Columns

Column	Type	Nullable	Notes
id	UUID	No	Primary Key
name	VARCHAR(255)	No	Location name
created_at	TIMESTAMP	No	Created time
updated_at	TIMESTAMP	No	Last updated

Relationships

* One Location → Many Matches.

Business Rules

* Location names are not required to be unique.
* Locations are reused when creating new matches.

⸻

Table: matches

Purpose

Represents one board game match.

Columns

Column	Type	Nullable	Notes
id	UUID	No	Primary Key
game_id	UUID	No	FK → games.id
location_id	UUID	No	FK → locations.id
started_at	TIMESTAMP	No	Match start
ended_at	TIMESTAMP	No	Match end
note	TEXT	Yes	Match notes
image_1_url	TEXT	Yes	Match photo
image_2_url	TEXT	Yes	Match photo
image_3_url	TEXT	Yes	Match photo
created_by	UUID	No	FK → users.id
created_at	TIMESTAMP	No	Created time
updated_at	TIMESTAMP	No	Last updated

Relationships

* One Match → One Game.
* One Match → One Location.
* One Match → Many MatchPlayers.

Business Rules

* Maximum of three photos.
* Match duration is calculated from timestamps.
* Match date is derived from started_at.
* Photos are stored in Supabase Storage; only URLs are stored in the database.

⸻

Table: match_players

Purpose

Stores each player’s participation and result in a match.

Columns

Column	Type	Nullable	Notes
id	UUID	No	Primary Key
match_id	UUID	No	FK → matches.id
player_id	UUID	No	FK → players.id
score	INTEGER	Yes	Player score
is_winner	BOOLEAN	No	Winner flag
created_at	TIMESTAMP	No	Created time

Relationships

* Many MatchPlayers → One Match.
* Many MatchPlayers → One Player.

Business Rules

* A player may appear only once in a match.
* Enforced by UNIQUE(match_id, player_id).
* Score is optional.
* Multiple winners are allowed.
* Zero winners are allowed for cooperative games that are lost.

⸻

Statistics Philosophy

The database stores only factual data.

Examples of stored data:

* Players
* Games
* Locations
* Matches
* Scores
* Winners
* Notes
* Photos

Examples of calculated data:

* Win Rate
* Total Wins
* Most Played Games
* Average Score
* Average Play Time
* Monthly Statistics
* Yearly Statistics
* Head-to-head Statistics
* Winning Streaks

Calculated values are never stored.

⸻

BoardGameGeek Integration

BoardGameGeek is used only when a game is imported for the first time.

The following information is stored locally:

* BoardGameGeek ID
* Game name
* Cover image

All future matches use the locally stored data.

⸻

Schema Governance

Any schema change requires:

1. Proposal
2. Technical Review
3. Product Owner Approval
4. Database Version Update

No table or column may be added without following this process.
