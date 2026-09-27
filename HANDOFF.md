# Fanis Dance — handoff

8-bit dance game. One file, `index.html`: vanilla JS, canvas and Web Audio. No build step and no dependencies,
apart from the Press Start 2P font from Google Fonts.
Live as a claude.ai artifact (v4): https://claude.ai/artifact/GyapaFXyFtQSAhiXV3nmjX
(`index.html` here is the same page wrapped in a full HTML document, ready for GitHub Pages.)

## What's in it
- Fighter select, 15 slots: Fanis (open); Amarildo (unlock: 8 perfect beats in a row);
  Mama Emy (unlock: keep one party going 60 s); 12 "stay tuned". Tapping the select title 5 times unlocks everyone (cheat).
- Each fighter has their own sprite, 6 moves (3 unlock by combo), song and stage:
  Fanis = disco, Cycladic village; Amarildo = boom-bap, graffiti street; Emy = kalamatianos in 7/8, taverna.
- Tap on the beat: PERFECT/GOOD/MISS judging, one tap per beat, notes converge on a track under the dancer.
- Party mode (unlocked fighters back you up at combo 4), dance-off (2 players take turns, best of 3, accuracy %),
  13 trophies plus personal bests. Everything is stored in localStorage (`fanis_*` keys).

## Hard-won gotchas
- iOS audio: a looping silent <audio> element plays alongside Web Audio so the ring/silent switch doesn't mute it,
  and audio retries on touchend/click (iOS ignores touchstart/pointerdown). Don't remove either.
- Visual timing follows the audio clock minus output latency (`audioLag`); scheduling doesn't subtract it.
- Every stat is per device. The artifact's shared db would make it org-only, so a shared leaderboard needs its own backend.

## Open decisions (Kostas)
1. Public repo + GitHub Pages: needs Fanis, Amarildo and Emy to agree to their names and pixel likenesses being public.
2. Leaderboard backend: the shared personal Supabase project (dgnxbcoxdpdloplkcmzs, which also holds Ledger and Overtime),
   with `fd_` tables and RLS, or a freed-up separate project. Check that project's sign-up settings first
   (Ledger uses an `allowed_emails` table). Scores are client-reported, so they can be faked.
3. Plan: optional login (only to post scores), `fd_players` (nickname) and `fd_scores` (fighter, streak, party time, combo),
   RLS = authenticated users read everything and write only their own rows, plus a leaderboard screen.
