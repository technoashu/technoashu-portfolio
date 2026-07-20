# A White-Label "On-Trip Companion" App for Independent Travel Agents

*A PWA that turns a travel agent's itinerary PDF into a private, installable app for each trip — validated first on a real family trip before a line of the general platform was built.*

## The problem

An independent travel agent's deliverable is usually a PDF or a WhatsApp thread — an itinerary that gets buried, a hotel confirmation that gets lost, no single place a traveler can check "what's happening today" without texting the agent again. Agents have no easy way to give each trip its own polished, branded experience without building a custom app for every client, which no small agency can afford to do.

## The constraint

The temptation with a platform idea like this is to build the general, multi-tenant version first and hope agents want it. The constraint imposed here was the opposite: prove real-world usefulness on one real trip before generalizing anything.

## The approach

The product is a PWA (installable, works offline) that becomes a private "on-trip companion" for a single trip: itinerary, hotel details, day-by-day discoveries, emergency contacts, and a built-in translator. Two tracks, deliberately sequenced:

- **Track A — dogfooding, then a fictional demo.** The reference build was first forked and populated for my own family's real trip, to prove the app was actually useful under real travel conditions rather than just plausible in a mockup. From that, a fully fictional demo tenant — a Bali honeymoon couple, live at [voyages-unlimited-demo.vercel.app](https://voyages-unlimited-demo.vercel.app) — is now being used to solicit direct feedback from independent travel agents, ahead of any monetization push.
- **Track B — the general platform.** A Supabase-backed, multi-tenant version where any agent could spin up a new branded trip from a parameterized config, rather than a one-off fork per client. This track is intentionally paused: it doesn't get built until agent feedback on Track A says the concept is worth generalizing.

## Outcome

A real, working reference build proven against an actual trip, and a fictional demo currently in front of real travel agents collecting feedback — with the more expensive, general-purpose platform work deliberately held back until that feedback justifies it.

## What I'd do differently

The agent-feedback loop could have started earlier, in parallel with building the dogfood tenant rather than after it — the risk of building on solo intuition first is ending up with features that were exactly right for one family's trip and no one else's.
