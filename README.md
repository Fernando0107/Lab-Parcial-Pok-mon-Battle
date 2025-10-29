# ⚡ Parcial No. 2: Pokémon Battle 🧩

<img width="1200" height="500" alt="image" src="https://github.com/user-attachments/assets/8f623c8e-2de7-4b80-a56c-45306d9e0c9f" />

## Overview

You will build:

* 🐍 A Flask API (N-tier: Controllers → Services → Repositories → PokéAPI).
* 🚫 No terminal app — all interaction is via the API only.
* 📘 Pokémon API Documentation: [https://pokeapi.co/docs/v2](https://pokeapi.co/docs/v2)

---

## 🌐 External API (PokéAPI) — used only by your Repository

🔗 **Endpoints:**

* GET [https://pokeapi.co/api/v2/pokemon?limit={number}](https://pokeapi.co/api/v2/pokemon?limit={number})
* GET [https://pokeapi.co/api/v2/pokemon/{name-or-id}](https://pokeapi.co/api/v2/pokemon/{name-or-id})
* GET [https://pokeapi.co/api/v2/pokemon-form/{name-or-id}](https://pokeapi.co/api/v2/pokemon-form/{name-or-id})

---

## 🧱 Your Flask API — Create at least 3 endpoints

All of your endpoints must call PokéAPI via your layers. Suggested set:

* ⚙️ **GET /api/v1/pokemon?limit=10** → Lists at least 10 Pokémon
  🧩 Calls PokéAPI (list) via repository.
  **Response example:**

  ```
  { "count": 10, "results": [{"id":1,"name":"bulbasaur"}, ...] }
  ```

* 💪 **GET /api/v1/pokemon/<name_or_id>/stats** → Stats for a Pokémon
  ⚔️ Calls PokéAPI (detail) and reduces to hp/attack/defense (you choose).
  **Response Example:**

  ```
  { "id":25, "name":"pikachu", "stats":[{"name":"hp","base":35}, ...] }
  ```

* 🌀 **GET /api/v1/pokemon/<name_or_id>/forms** → Pokémon forms and types
  🧠 Calls the PokéAPI `pokemon-form` endpoint and returns the available forms and types for that Pokémon.
  **Response Example (simplified):**

  ```json
  {
    "id": 6,
    "name": "charizard",
    "types": ["fire", "flying"],
    "is_mega": false
  }
  ```

* ⚡ **POST /api/v1/battle/start** → Simple battle simulation (server-side)
  **Body:**

  ```
  { "pokemon": "pikachu" }
  ```

  * `pokemon` (required): name or id of the selected pokemon.

  🧠 The Service fetches the challenger from PokéAPI, selects a random pokemon opponent, and chooses a winner randomly.
  **Response Example:**

  ```
  {
    "challenger": {"id":25,"name":"pikachu"},
    "opponent": {"id":74,"name":"geodude"},
    "winner_slot":"opponent",
    "winner_name":"geodude"
  }
  ```

---

## 🏗️ Architecture & Requirements

* 🧩 Architecture: Controllers (Flask routes) → Services (business logic) → Repositories (HTTP to PokéAPI).
* 🧱 OOP: Service and Repository must be classes. Must apply other OOP concepts such as abstract classes, inheritance, attributes, and properties.
* 🚨 Error handling: Map custom exceptions to JSON + proper status codes (400/404/502); no raw stack traces in responses.

---

## 🧭 Required Flow (API-only)

1. 📋 **List All Pokémons**

   * Call GET `/api/v1/pokemon?limit=10`
   * Returns 10 names with ids.

2. ⚔️ **Battle Simulator (API)**

   * Client sends **POST** `/api/v1/battle/start` with body:

     ```json
     { "pokemon": "<name-or-id>" }
     ```
   * Server:

     * Fetches challenger via repository (PokéAPI).
     * Picks a random pokemon opponent.
     * Randomly selects a winner (no turn-by-turn combat).
   * Response contains challenger, opponent, and the winner.

3. 🌀 **Show Pokémon Forms**

   * Call GET `/api/v1/pokemon/<name_or_id>/forms`
   * Returns form and type information for that Pokémon (e.g., Charizard has fire and flying forms).

4. 📊 **Get Stats for Pokémon** (Optional)

   * Call GET `/api/v1/pokemon/<name_or_id>/stats`
   * Returns reduced stats (hp/attack/defense) and basic info.

---

## 🧾 Evaluation (100 pts)

| 🧩 Criterion                  | Pts | Description                                                       |
| ----------------------------- | --: | ----------------------------------------------------------------- |
| 🔗 Endpoints (≥3)             |  25 | All required endpoints exist; correct methods & status codes.     |
| 🌐 External API integration   |  20 | Calls to PokéAPI only via Repository; robust on errors/timeouts.  |
| ⚙️ Architecture & OOP         |  20 | Clear N-tier; Service/Repository classes; clean responsibilities. |
| 🎯 Battle endpoint (API-only) |  20 | Battle implemented as a single POST endpoint returning a winner.  |
| 🚨 Error handling & responses |  10 | Proper JSON error shapes and codes.                               |
| 🧹 Code quality & README      |   5 | Clean code; README explains run steps and endpoints.              |

---

## ✅ Deliverables Checklist

* 🧩 Flask app with required endpoints
* 📘 README with setup/run instructions and endpoint docs
* 🧱 Clear separation of Controllers / Services / Repositories

---
