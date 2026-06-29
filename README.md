# Think & Speak

Think & Speak is a mobile party/trivia game for Android, designed and released on Google Play.

The project was created as a self-initiated product where I worked across product design, UI/UX, React Native development, testing, release, and iteration based on real user feedback.

## Live product

Google Play: [Think & Speak on Google Play](https://play.google.com/store/apps/details?id=com.christos.thinkandspeak&pcampaignid=web_share)  
Portfolio case study: [Think & Speak case study](https://cgeorgoulakis.framer.website/think-and-speak)

## My role

I worked on the project end to end, including:

* Product concept and game rules
* User flows and onboarding
* Mobile UI design in Figma
* React Native and Expo development
* Multilingual content structure
* Play Store release assets
* User testing and feedback based improvements
* Analytics and monetization flow planning

## Product overview

Think & Speak is a local multiplayer party game where players answer prompt based questions under time pressure. Each round includes a checker role, guessing players, scoring, timers, and answer validation.

The main design challenge was to make the game understandable quickly, even when several players share one device and switch roles during the game.

## Key features

* Android app released on Google Play
* Local multiplayer flow for 2 to 5 players
* Onboarding and role explanation
* Timer based gameplay
* Score tracking
* Multilingual content
* Feedback states for correct and missed answers
* Interstitial and rewarded ad flow planning
* Firebase Analytics event tracking

## Design process

The design process focused on making the rules clear before the first round, reducing confusion during role switching, and keeping the interface readable during fast gameplay.

Main UX decisions included:

* Separating setup, explanation, gameplay, and score screens clearly
* Keeping the main gameplay screen simple and focused
* Using feedback states to help the checker manage answers quickly
* Testing the game with real users to find confusing moments
* Iterating the onboarding and gameplay flow based on feedback

## Technical overview

The app was built with:

* React Native
* Expo
* TypeScript
* Zustand for state management
* Firebase Analytics
* Google Mobile Ads
* Google Play Console

## Selected technical areas

Some of the technical work included:

* Managing game state across rounds
* Handling player setup and role switching
* Structuring multilingual question content
* Tracking gameplay events for analytics
* Preparing the Android build for Google Play release

The full production repository is private, but selected code examples and project structure notes are included here to show how the app was built.

## AI assisted workflow

AI tools were used during the project to speed up development, debug implementation issues, explore alternative flows, and improve code structure. I used AI as a working partner, while reviewing and testing the output myself before adding it to the project.

Examples of where AI helped:

* Debugging React Native and Expo issues
* Refactoring state logic
* Generating alternative UI implementation ideas
* Improving content structure
* Preparing release related checklist items

## What I learned

This project helped me understand the difference between designing static screens and designing a product people actually use. Releasing the app made me work through practical constraints around onboarding, performance, content structure, analytics, monetization, and store submission.

It also strengthened my interest in roles where design and development are closely connected.

## Screenshots

<img width="273" height="486" alt="Frame 8437 1" src="https://github.com/user-attachments/assets/e3f91664-105f-42fb-9442-8a12566dabd9" />
<img width="273" height="486" alt="Frame 32 (2) 1" src="https://github.com/user-attachments/assets/9ca456eb-873b-4da8-b4e1-6bf473252359" />
<img width="273" height="486" alt="Frame 33 (3) 1" src="https://github.com/user-attachments/assets/a4d5be10-d8ee-4c30-a0b0-a2015db87650" />


## Private source code note

The full production source code is kept private because it contains project setup, release configuration, and third party service integrations. I am happy to walk through the project structure, technical decisions, and selected code examples in an interview.

# Selected Code Snippets

This file includes selected, sanitized snippets from Think & Speak. The full production repository is private, but these examples show some of the main implementation areas: multilingual content, mobile UI, analytics, and game state logic.

## 1. Multilingual question structure

This snippet shows how questions were structured with scoring logic and translations.

```ts
import { LocalizedQuestionDefinition } from "../types";

export const SAMPLE_QUESTIONS: LocalizedQuestionDefinition[] = [
  {
    id: "t10_intuition_1",
    category: "Intuition",
    thinkerPoints: [1, 1, 2, 2, 2, 3, 3, 4, 5, 5],
    predictorWillFindPoints: [2, 2, 3, 3, 3, 4, 4, 5, 6, 6],
    predictorWillMissPoints: [6, 6, 5, 5, 5, 4, 4, 3, 2, 2],
    translations: {
      en: {
        questionText: "10 annoying things that can happen during a road trip",
        answerTexts: [
          "Traffic",
          "Getting hungry",
          "Too many bathroom stops",
          "No signal for music or maps",
          "Phone battery dying",
          "Getting carsick",
          "Getting lost",
          "Someone talking too much",
          "Sitting in the middle seat",
          "Someone snoring",
        ],
      },
      da: {
        questionText: "10 irriterende ting der kan ske på en roadtrip",
        answerTexts: [
          "Trafik",
          "At blive sulten",
          "For mange toiletstop",
          "Ingen signal til musik eller GPS",
          "Telefonen løber tør for strøm",
          "Blive køresyg",
          "Fare vild",
          "En der taler for meget",
          "At sidde på midtersædet",
          "En der snorker",
        ],
      },
    },
  },
];
```

## 2. Language selection modal

This snippet shows the first language selection experience in the app.

```tsx
import { AppLanguage, useSettingsStore } from "@/store/SettingsStore";
import { useT } from "@/translations/useT";
import React from "react";
import {
  Modal,
  Pressable,
  StyleSheet,
  Text,
  TouchableOpacity,
  View,
} from "react-native";

type Props = {
  visible: boolean;
};

const LANG_ORDER: AppLanguage[] = ["en", "el", "de", "fr", "da", "es"];

const LANG_FLAG: Record<AppLanguage, string> = {
  en: "🇬🇧",
  el: "🇬🇷",
  de: "🇩🇪",
  fr: "🇫🇷",
  da: "🇩🇰",
  es: "🇪🇸",
};

export function LanguageSelectionModal({ visible }: Props) {
  const t = useT();

  const completeLanguageSelection = useSettingsStore(
    (s) => s.completeLanguageSelection,
  );

  const handleSelect = (language: AppLanguage) => {
    completeLanguageSelection(language);
  };

  return (
    <Modal visible={visible} transparent animationType="fade">
      <View style={styles.backdrop}>
        <Pressable style={StyleSheet.absoluteFill} />
        <View style={styles.modalBox}>
          <Text style={styles.title}>{t("languageGate.title")}</Text>
          <Text style={styles.subtitle}>{t("languageGate.subtitle")}</Text>

          <View style={styles.list}>
            {LANG_ORDER.map((code) => {
              const label = t(`settings.languages.${code}` as any);

              return (
                <TouchableOpacity
                  key={code}
                  style={styles.row}
                  onPress={() => handleSelect(code)}
                  activeOpacity={0.85}
                >
                  <View style={styles.rowLeft}>
                    <Text style={styles.flag}>{LANG_FLAG[code]}</Text>
                    <Text style={styles.rowText}>{label}</Text>
                  </View>
                </TouchableOpacity>
              );
            })}
          </View>
        </View>
      </View>
    </Modal>
  );
}
```

## 3. Analytics initialization

This snippet shows how analytics are initialized once and connected to game events.

```ts
import { Analytics } from "../../src/services/analytics/Analytics";
import { bindGameAnalytics } from "../../src/services/analytics/bindings/bindGameAnalytics";

let cleanup: null | (() => void) = null;
let started = false;

export function initAnalytics() {
  if (started) return;
  started = true;

  Analytics.init();

  if (!cleanup) {
    cleanup = bindGameAnalytics();
  }
}

export function shutdownAnalytics() {
  cleanup?.();
  cleanup = null;
  started = false;
}
```

## 4. Game analytics binding

This snippet shows how game start and game finish events are detected from state changes.

```ts
export function bindGameAnalytics() {
  let started = false;
  let finished = false;
  let matchStartTs: number | null = null;

  let prevHasQuestion = false;
  let prevIsMatchStart = false;
  let prevLastTurn: TurnRecap | null = null;

  const unsub = useGameStore.subscribe((state) => {
    const isMatchStart = state.currentRound === 1 && state.turnInRound === 0;
    const hasQuestion = !!state.currentQuestion;

    const startEdge =
      isMatchStart && hasQuestion && (!prevHasQuestion || !prevIsMatchStart);

    if (startEdge && !started) {
      const matchId = makeMatchId();
      AnalyticsSession.setMatchId(matchId);

      started = true;
      finished = false;
      matchStartTs = Date.now();

      Analytics.track(AnalyticsEvent.GAME_START, {
        match_id: matchId,
        player_count: state.players.length,
        round_count: state.rounds,
        timer_seconds: state.timerSeconds,
        category_count: state.categories.length,
        build_type: __DEV__ ? "dev" : "prod",
      });
    }

    prevHasQuestion = hasQuestion;
    prevIsMatchStart = isMatchStart;

    const lastTurn = state.lastTurn;

    if (lastTurn && lastTurn !== prevLastTurn) {
      const matchId = AnalyticsSession.getMatchId() ?? makeMatchId();

      const isFinalTurn =
        lastTurn.round === state.rounds &&
        lastTurn.turnInRound === state.players.length - 1;

      if (isFinalTurn && !finished) {
        finished = true;

        const winner = state.players
          .map((p, idx) => ({ idx, score: p.score }))
          .sort((a, b) => b.score - a.score)[0];

        const durationMs =
          matchStartTs != null
            ? Math.max(0, Date.now() - matchStartTs)
            : undefined;

        Analytics.track(AnalyticsEvent.GAME_FINISH, {
          match_id: matchId,
          player_count: state.players.length,
          round_count: state.rounds,
          total_turns: state.rounds * state.players.length,
          winner_index: winner?.idx ?? 0,
          winner_score: winner?.score ?? 0,
          duration_ms: durationMs,
        });
      }
    }

    prevLastTurn = lastTurn;
  });

  return () => {
    unsub();
  };
}
```

## 5. Game state and match flow

This snippet shows the central game logic: preparing turns, resolving scoring, advancing rounds, and resetting the match.

```ts
export type Player = {
  name: string;
  score: number;
};

export type GameMode = "CLASSIC" | "THINKER_ONLY";

export type TurnRecap = {
  round: number;
  turnInRound: number;
  thinkerIndex: number;
  predictorIndex: number | null;
  question: Question;
  predictions: Prediction[];
  foundAnswerIndexes: number[];
  thinkerPoints: number;
  predictorPoints: number;
};

function shouldQueueInterstitialAfterTurn(params: {
  gameMode: GameMode;
  playersCount: number;
  rounds: number;
  completedTurnCount: number;
}): boolean {
  const { gameMode, playersCount, rounds, completedTurnCount } = params;

  if (playersCount <= 0 || rounds <= 0) return false;
  if (completedTurnCount <= 0) return false;

  const realTotalTurns = playersCount * rounds;

  if (completedTurnCount >= realTotalTurns) return false;

  const interval = gameMode === "THINKER_ONLY" ? 7 : playersCount <= 2 ? 4 : 5;
  const remainingTurnsAfterAd = realTotalTurns - completedTurnCount;

  if (remainingTurnsAfterAd < 2) return false;

  return completedTurnCount % interval === 0;
}
```
