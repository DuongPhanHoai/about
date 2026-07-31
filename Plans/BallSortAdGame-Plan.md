# Ball Sort Puzzle — Ad-Monetized Mobile Game Plan

## Goal

Build a simple, level-based mobile game for **Android and iOS** that monetizes primarily
through advertising. Short stages maximize the number of interstitial ad impressions
per session.

## Recommendation: Ball Sort Puzzle (color sorting game)

Among hyper-casual genres, **Ball Sort Puzzle** is the best fit for these requirements:

- **Very simple model**: tubes with colored balls; tap to move the top ball onto a
  matching color. No physics engine, no character art, no animations beyond simple
  tweens. Buildable with plain Flutter widgets (no Unity license or heavy engine needed).
- **Maximum ad frequency**: levels take 30–90 seconds, so players hit a "Level Complete"
  screen every minute or two — the ideal interstitial slot. Sessions of 10–15 minutes
  yield 5–10+ interstitial impressions.
- **Unlimited stages for free**: levels are procedurally generated (start from a solved
  state, apply reverse-shuffle moves), guaranteeing solvability with tunable difficulty.
  No manual level design; "Level 3,417" costs nothing to produce.
- **Proven market**: Ball Sort / Water Sort clones are consistently top-charting
  ad-monetized games, so the monetization model is validated.

## Monetization design (AdMob)

- **Interstitial** after every level completion (with a frequency cap, e.g. skip if the
  last ad was shown less than 30 seconds ago, to stay within AdMob policy).
- **Rewarded video** for gameplay boosts: +1 undo, add an extra empty tube, skip level.
  These are the highest-eCPM format and players opt in willingly.
- **Banner** anchored at the bottom of the gameplay screen for passive fill.

## Tech stack

- **Flutter** (single codebase for Android + iOS) with plain widgets — no game engine
  needed for this genre.
- **`google_mobile_ads`** plugin for AdMob (interstitial, rewarded, banner), using
  Google's official test ad unit IDs during development.
- **`shared_preferences`** for progress persistence (current level, coins/undo count).

## Implementation steps

1. **Scaffold** a Flutter project in a new `ball_sort_game/` directory, with Android and
   iOS targets and the AdMob app-ID entries in `AndroidManifest.xml` / `Info.plist`
   (test IDs).
2. **Core game logic** (pure Dart, unit-testable): tube/ball model, legal-move rules,
   win detection, undo stack, and a seeded reverse-shuffle level generator whose
   difficulty (tube count, colors, shuffle depth) scales with level number.
3. **Game UI**: home screen (Play, level number), gameplay screen (tubes, tap-to-move
   with a simple ball-lift animation, moves/undo counter, banner ad slot), and a
   level-complete dialog.
4. **Ads layer**: an `AdManager` service that preloads interstitials/rewarded ads, shows
   an interstitial on level complete (with frequency cap), and wires rewarded ads to
   "extra tube" and "+undo" buttons.
5. **Persistence + polish**: save progress with `shared_preferences`, add a sound
   toggle, and run `flutter analyze` plus unit tests on the game logic/generator.
6. **Documentation**: README covering how to swap test AdMob IDs for real ones and how
   to build a release APK/IPA.

## Task checklist

- [ ] Scaffold Flutter project with AdMob config (test IDs)
- [ ] Implement tube/ball model, move rules, win detection, undo, seeded level generator
- [ ] Build home, gameplay, and level-complete screens with tap-to-move interaction
- [ ] Add AdManager: interstitial on level complete, rewarded boosts, bottom banner
- [ ] Persist progress, add settings, run `flutter analyze` and unit tests
- [ ] Write README (release build and AdMob ID swap instructions)

## Notes

- Development and automated tests run locally/CI; publishing to the Play Store /
  App Store and creating a real AdMob account with production ad-unit IDs are manual
  follow-up steps (documented in the README).
- If a different genre is preferred later (e.g. 2048, block puzzle, one-line draw), the
  ads/persistence architecture stays identical — only steps 2–3 change.
