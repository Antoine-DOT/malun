# Malun

**Learn Mandarin with clarity.** Malun is a hanzi-first iPhone app for
learners preparing the TOCFL (Taiwan) and the HSK (mainland China), in
traditional or simplified characters.

→ [Download on the App Store](https://apps.apple.com/app/id6828634809) · [malun.app](https://malun.app)

This repository is a showcase. The source code is private; this page explains
what the app does and how it is built and run.

<p align="center">
  <img src="docs/screenshots/1-home.jpg" width="15%" alt="Home: today's goal and progress">
  <img src="docs/screenshots/2-card.jpg" width="15%" alt="Review: a flashcard">
  <img src="docs/screenshots/3-path.jpg" width="15%" alt="A TOCFL level">
  <img src="docs/screenshots/4-quiz.jpg" width="15%" alt="Quiz">
  <img src="docs/screenshots/5-topics.jpg" width="15%" alt="Learn by subject">
  <img src="docs/screenshots/6-learn.jpg" width="15%" alt="Learn: by level, subject, quiz or numbers">
</p>

<p align="center"><sub>Home · Review · Level · Quiz · Topics · Learn (App Store screenshots, demo account)</sub></p>

## What it does

- **Learn**: browse cards by TOCFL level, HSK level or topic (animals, the
  face, countries…), with pinyin, meaning and pronunciation.
- **Review**: spaced repetition. You see the character, tap to reveal it,
  and say how it went, from "Didn't know" to "Instant". A card counts as
  *mastered* once its interval reaches four days.
- **My Cards**: save cards while learning and organise them in your own lists.
- **Quiz**: HSK and TOCFL quizzes in two modes, Classic and Streak, plus a
  numbers drill.
- **Progress**: what is mastered, in progress, due or not started, by level
  and by topic.
- **Daily goal and reminders**: a daily target, a streak, and optional study
  reminders on the phone.
- **Script choice**: traditional or simplified, with Taiwanese pinyin where
  usage differs from the mainland.

The corpus has more than 10,000 active cards and covers TOCFL level 3 (B1)
in full.

## How it is built

| Layer | Choice |
| --- | --- |
| Backend | Python 3.13, Django 5.2 LTS, Django REST Framework |
| Frontend | Django templates and HTMX, no SPA framework |
| Database | PostgreSQL |
| Identity | Keycloak (OpenID Connect), with Sign in with Apple and Google |
| Mobile | iOS app built with Capacitor, subscriptions through the App Store |
| Runtime | Gunicorn behind nginx, Docker Compose |
| Analytics | PostHog |

A few design decisions:

- **A modular monolith.** One Django project split into clear modules. It is
  simpler to run, test and reason about than services, for one developer.
- **Home-made spaced repetition.** Review states and review events are stored
  separately, so the schedule can always be rebuilt from history.
- **Quizzes do not touch the review schedule.** Testing yourself is not the
  same as studying, and mixing the two skews the intervals.
- **Pronunciation through the browser's speech synthesis**, with no audio
  files to host.

## How it is run

Malun is built and tested in [ATL-Lab](https://github.com/Antoine-DOT/atl-lab),
my home lab, and runs in production on a rented VPS in the EU. The VPS pulls
each approved release itself, over HTTPS.

Every change follows the same path: development, then a preview environment
deployed by [atl-preview-controller](https://github.com/Antoine-DOT/atl-preview-controller),
then production. A release is one full commit SHA, and the release script
refuses it unless:

- that exact commit has been deployed to preview and I have tested it there;
- a written approval for that commit exists;
- it descends from what is currently in production;
- the database backup taken just before the migrations has been checked.

Rolling back is scripted too, and refuses to run if it would need to undo a
database migration.

Most of the code is written with AI coding agents (Claude Code and Codex).
I own the product, the design and the reviews, and every release goes through
the same gates whoever wrote the code.

## Status

In production at [malun.app](https://malun.app). The iOS app (version 1.0,
with a free tier and an optional Pro subscription) was approved by Apple and released
on [the App Store](https://apps.apple.com/app/id6828634809) on 16 November 2026. The interface is available in
French and English.

---

Built by [Antoine Leflon](https://antoineleflon.com).
