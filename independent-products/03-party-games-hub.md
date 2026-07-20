# A Shared-Screen Party Game Hub, Shipped Start to Finish

*Three party games, one shared screen, zero setup — and a genuinely finished project with nothing left to second-guess.*

## The problem

Party games like Dumb Charades, Pictionary, and antakshari-style song rounds are great until someone has to run them: track scores by hand, hunt down a word list, referee disputes, keep the round moving. That friction is exactly what kills momentum at an actual gathering, where nobody wants to fiddle with an app before they can start playing.

## The constraint

It had to work from one shared screen — a phone or TV everyone in the room can see — with no accounts, no backend, and no setup step. Anyone should be able to open a link and be playing within a minute, on whatever device is already in the room.

## The approach

A static, client-side web app hosting three games — Dumb Charades, Pictionary, and Song Wheel — sharing one underlying engine for teams, scoring, and timers, so adding a new game mode doesn't mean rebuilding the scaffolding around it. Content leans into Bollywood and Indian-pop-culture references rather than generic word lists, which is what makes it feel built for a specific audience instead of a template. A "hold-to-peek" mechanic lets one player privately check their secret word without exposing it to the rest of the room glancing at the shared screen.

## Outcome

Complete and shipped — all planned milestones finished, post-launch code review fixes applied, no open critical bugs, live and playable at [party-games-weld.vercel.app](https://party-games-weld.vercel.app).

## What I'd do differently

The word lists and content are fixed at build time — fine for a personal project, but it means nobody else can make this their own without editing code. If I revisited it, I'd let a host swap in their own words and categories without touching the source.
