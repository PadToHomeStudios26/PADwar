# PADWAR: COMPLETE BATTLE

Ein 3D-Actionspiel für PC, iPad und Smartphone — gebaut mit React Three Fiber (Three.js), TypeScript und TanStack Start.

## Features

- **Echte 3D-Welt** mit vier Karten (u. a. Dust Basin) und Ego-Perspektive zu Fuß, Third-Person in Fahrzeugen
- **Waffen & Fahrzeuge**: Pistole, Shotgun, MG, Sniper, Raketenwerfer; Jeep und Panzer
- **Spielmodi**: Eroberung (Zonen A/B/C), Team-Deathmatch (bis 50 Kills), Wellenmodus (10 Wellen), Zombie-Modus (8 Wellen)
- **Taktische KI**: Gegner nutzen Deckung, greifen fair an; Verbündete kämpfen an deiner Seite
- **Respawn**: Nach 5 Sekunden am Teamstützpunkt mit voller Gesundheit und Munition
- **Token-Shop**: Waffen, Upgrades, Spezialmunition und Skins — Progress wird lokal gespeichert
- **Steuerung**: Maus/Tastatur am PC, Touch-Steuerung auf iPad und Smartphone
- **Offline-fähig**: Reines Browser-Spiel ohne Server — die Speicherung läuft über localStorage

## Entwicklung starten

Du brauchst Node.js 20+ (oder Bun).

```sh
npm install     # oder: bun install
npm run dev     # Entwicklungsserver
```

## Bauen & Tests

```sh
npm run build   # Produktions-Build
npm test        # Ökonomie- und Modi-Tests (Vitest)
npm run lint    # ESLint
```

## Code-Struktur

| Pfad | Inhalt |
| --- | --- |
| `src/game/engine.ts` | Imperative Spiel-Engine (Kampf, KI, Fahrzeuge, Modi, Respawn) |
| `src/game/economy.ts` | Preise, Belohnungen und Shop-Regeln (Single Source of Truth) |
| `src/game/data.ts` | Waffen, Karten, Modi-Konfigurationen |
| `src/game/sound.ts` | Sound-Engine |
| `src/components/padwar/` | Menü, HUD und React-Sichtschicht |
| `src/routes/index.tsx` | Einstiegsroute mit dem Spiel-Canvas |

## Spiellogik testen

Die Shop- und Modi-Regeln liegen in `src/game/economy.ts` bzw. `src/game/data.ts` und sind über Tests abgesichert:

```sh
npm test
```

## Offline-Version

Neben diesem Projekt gibt es `PadWar.html` — das komplette Spiel als eine einzige Datei (ca. 1,4 MB), die per Doppelklick im Browser ohne Installation läuft.

## Technologie

- TanStack Start · React 19 · TypeScript
- React Three Fiber + Three.js
- Tailwind CSS v4
- Vitest für Spiellogik-Tests
