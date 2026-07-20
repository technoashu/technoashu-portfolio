# Kaya: A Translator Built for the Words Generic Translators Get Wrong

*A German↔English translator for migrant caregivers, built for a hackathon, now a live, revenue-validated micro-SaaS at [kayacare.app](https://kayacare.app).*

## The problem

Migrant caregivers working in German elder-care — arriving from India, the Philippines, Indonesia, Morocco, Tunisia, Hungary, Poland, Brazil, and elsewhere — depend on translation for exactly the conversations where being wrong is dangerous: a nurse's handover note, a doctor's instruction, a patient's own description of pain or symptoms. Generic translators fail here in a specific, predictable way — they mangle clinical shorthand. A note like "AZ reduziert" (a common German clinical abbreviation for a patient's general condition) gets flattened into nonsense or a misleadingly literal gloss by tools trained on everyday language, not care documentation.

## The constraint

Built solo, under a hackathon's time pressure (GrowthX AI Weekender, Revenue track) — which forced an immediate answer to "would anyone actually pay for this," not just "does the translation look plausible in a demo."

## The approach

Rather than one more general-purpose translation box, Kaya is built around the specific failure mode it exists to fix:

- **An 88-term curated eldercare/medical glossary** that overrides generic translation for the clinical vocabulary generic models get wrong.
- **Confidence-tier labeling on every output** — confident / likely / uncertain — instead of presenting every translation with the same false certainty. A caregiver relaying a "confident" translation and an "uncertain" one should act differently, and the app tells them which is which.
- **Practice mode**, which quizzes users on clinical facts embedded in a translated scenario (patient identifiers, timing, dosage) — building competence ahead of time, not just translating live in the moment it's needed most.
- **A feedback-driven glossary flywheel** — thumbs-up/down on any translation feeds back into glossary corrections, so the system's weakest spots get fixed by the people actually relying on it.

The stack chains a fallback path rather than betting on one model: Groq Whisper for speech-to-text, Gemini 2.5 Flash-Lite as the primary translator with Groq Llama, MyMemory, and Helsinki-NLP as fallbacks, and Sarvam/ElevenLabs/Web Speech for voice output. Supabase (EU region) handles auth and storage under a privacy-conscious design: raw translated text is never stored, only a hash plus metadata (engine used, confidence tier, whether the glossary was hit) — enough to improve the system without retaining what was actually said.

## Outcome

Kaya is live and deployed, not a demo — real user signups, an active waitlist, real translation events run through the pipeline, and a first paid conversion processed through a live, webhook-verified checkout. Auth, onboarding, telemetry, and an admin dashboard are all shipped and working end to end.

## What I'd do differently

The glossary flywheel currently depends on a person reviewing flagged corrections before they go live. Automating that review step — or at least tightening the confidence-tier boundaries using the feedback already collected — is the natural next iteration now that real usage data exists to tune against.
