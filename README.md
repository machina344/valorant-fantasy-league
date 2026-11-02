# Valorant Fantasy League

A single-file fantasy game for the Valorant esports scene: found an org, open card
packs built from real vlr.gg event statistics (every Masters and Champions 2021–2026
plus the 2026 domestic leagues), manage form, stamina, mental and a coach, and climb
ladders against AI clubs — and against other people's clubs on a shared online
leaderboard.

## Hosting on GitHub Pages

The whole game is `index.html`. To serve it:

1. Repo **Settings → Pages**
2. Source: **Deploy from a branch**, branch `main`, folder `/ (root)`
3. Save. After a minute the game is live at
   `https://<user>.github.io/valorant-fantasy-league/`

## Online ladder

The page signs every visitor in anonymously with Firebase Auth and syncs their club
to a Firestore collection (`clubs/<uid>`). Everyone can read the leaderboard; each
account can write only its own club document, enforced by these Firestore rules:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /clubs/{uid} {
      allow read: if request.auth != null;
      allow write: if request.auth != null
                   && request.auth.uid == uid
                   && request.resource.data.size() < 30;
    }
  }
}
```

Challenging another club simulates the series locally against their synced squad;
only the challenger's rating and record change. The same file also runs as a
claude.ai artifact, where it uses the artifact's built-in shared database instead.

Saves are per-browser (localStorage); the cloud stores only the club snapshot.

## Data

Player cards are generated from public match statistics on [vlr.gg](https://www.vlr.gg).
Ratings are era-adjusted to the 2026 level of play. Coach cards are rated by event
placement. Roles and nationalities are partly estimated. This project displays no
copyrighted artwork and no photographs of players.

## Legal

This is a free, non-commercial fan project.

Valorant Fantasy League was created under Riot Games' "Legal Jibber Jabber" policy
using assets owned by Riot Games. Riot Games does not endorse or sponsor this project.
