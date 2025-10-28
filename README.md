# ⚡ Lab (Parcial): Pokémon Battle 🧩

<img width="1200" height="500" alt="image" src="https://github.com/user-attachments/assets/8f623c8e-2de7-4b80-a56c-45306d9e0c9f" />


## Overview

You will build:

* 🐍 A Flask API (N-tier: Controllers → Services → Repositories → PokéAPI).
* 💻 A Terminal app (Python script) that uses requests to call your Flask API (not PokéAPI directly).
* 🚫 No GUI — all interaction is via the terminal menu shown below.
* 📘 Pokémon API Documentation: [https://pokeapi.co/docs/v2](https://pokeapi.co/docs/v2)

---

## 🌐 External API (PokéAPI) — used only by your Repository

🔗 **Endpoints:**

* GET [https://pokeapi.co/api/v2/pokemon?limit={number}](https://pokeapi.co/api/v2/pokemon?limit={number})
* GET [https://pokeapi.co/api/v2/pokemon/{name-or-id}](https://pokeapi.co/api/v2/pokemon/{name-or-id})
* GET [https://pokeapi.co/api/v2/move/{name-or-id}](https://pokeapi.co/api/v2/move/{name-or-id})

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

* 🎲 **GET /api/v1/battle/opponent?max_id=151** → Returns a random opponent
  🧠 Service chooses a random id; repository fetches from PokéAPI.
  **Response Example:**

  ```
  { "id":74, "name":"geodude", "stats":[...], "moves":["tackle","mud-slap", ...] }
  ```

* ⚡ **POST /api/v1/battle/attack** → Local-only attack simulation
  **Body:**

  ```
  {"attacker":"pikachu","defender":"geodude","move":"thunderbolt"}
  ```

  🧮 The Service fetches attacker/defender + move via repository (PokéAPI),
  then computes a single attack (opponent does not attack back).

  **Response Example:**

  ```
  {
    "attacker":"pikachu","defender":"geodude","move":"thunderbolt",
    "hit":false,"reason":"Attack missed or no effect","damage":0,
    "attacker_attack":55,"defender_defense":100
  }
  ```

---

## 🏗️ Architecture & Requirements

* 🧩 Architecture: Controllers (Flask routes) → Services (business logic) → Repositories (HTTP to PokéAPI).
* 🧑‍💻 Terminal client: Must use `requests` to call your Flask API only.
* 🧱 OOP: Service and Repository must be classes.
  Must apply other OOP concepts such as abstract classes, inheritance, attributes, and properties.
* 🚨 Error handling: Map custom exceptions to JSON + proper status codes (400/404/502); no raw stack traces in responses.

---

## 💻 Terminal App

A single Python script (e.g., `client.py`) that shows a menu, reads user input, and calls your Flask API using requests.

---

## 🎮 CLI Features

Example Menu:

```
------ Pokémon Battle! 🔥 ------

1. List All Pokémons
2. Battle Simulator ⚔️
3. Get Stats for Pokémon 📊
4. Quit ❌

Select an option: _
```

---

## 🧭 Required Flow

1. 📋 **List All Pokémons**

   * Calls GET /api/v1/pokemon?limit=10
   * Displays 10 names with ids.

2. ⚔️ **Battle Simulator**

   * Calls GET /api/v1/pokemon?limit=10 and prints them as a numbered list (1–10) to choose from.
   * User picks one (by number or name—you choose).
   * App then calls GET /api/v1/battle/opponent?max_id=151 to fetch a random opponent.
   * Shows chosen vs random.
   * Prompts for a move name (e.g., the first 5 moves).
   * Calls POST /api/v1/battle/attack.
   * Displays result (hit/miss, damage, reason).

3. 📊 **Get Stats for Pokémon**

   * Prompts: “Enter name or id:”
   * Calls GET /api/v1/pokemon/<name_or_id>/stats
   * Prints reduced stats and (optionally) a few move names.

4. 🚪 **Quit**

   * Exit the client gracefully.

---

## 🖥️ Example CLI Output (Illustrative Only)

```
------ Pokémon Battle! 🐉 ------

1. List All Pokémons
2. Battle Simulator
3. Get Stats for Pokémon
4. Quit

Select an option: 1

Showing 10 Pokémon:
[1] bulbasaur 🌿 (id: 1)
[2] ivysaur 🌱 (id: 2)
[3] venusaur 🌺 (id: 3)
[4] charmander 🔥 (id: 4)
[5] charmeleon 🔥 (id: 5)
[6] charizard 🐉 (id: 6)
[7] squirtle 💧 (id: 7)
[8] wartortle 💦 (id: 8)
[9] blastoise 🌊 (id: 9)
[10] caterpie 🐛 (id: 10)

Press ENTER to continue...

Select an option: 2

Pick your Pokémon (1-10 or name): ⚡ pikachu
Fetching random opponent...
Your Pokémon: ⚡ pikachu
Opponent: 🪨 geodude

Enter a move to use (e.g., thunderbolt ⚡): thunderbolt
Attacking...

Result:
- Move: thunderbolt ⚡
- Hit: false ❌
- Damage: 0
- Reason: Attack missed or no effect
- Attacker Attack: 55
- Defender Defense: 100

Press ENTER to continue...

Select an option: 3
Enter Pokémon name or id: charmander 🔥

Stats for charmander:
- hp: 39 ❤️
- attack: 52 💪
- defense: 43 🛡️

Press ENTER to continue...

Select an option: 4
Goodbye! 👋
```

---

## 🧾 Evaluation (100 pts)

| 🧩 Criterion                  | Pts | Description                                                         |
| ----------------------------- | --: | ------------------------------------------------------------------- |
| 🔗 Endpoints (≥3)             |  25 | All required endpoints exist; correct methods & status codes.       |
| 🌐 External API integration   |  20 | Calls to PokéAPI only via Repository; robust on errors/timeouts.    |
| ⚙️ Architecture & OOP         |  20 | Clear N-tier; Service/Repository classes; clean responsibilities.   |
| 🎮 Terminal app (flows)       |  20 | Uses requests to call your API; implements required menu and flows. |
| 🚨 Error handling & responses |  10 | Proper JSON error shapes and codes.                                 |
| 🧹 Code quality & README      |   5 | Clean code; README explains run steps and endpoints.                |

---

## ✅ Deliverables Checklist

* 🧩 Flask app with required endpoints
* 💻 Terminal client (`client.py`) that calls your API with requests
* 📘 README with setup/run instructions and endpoint docs
* 🧱 Clear separation of Controllers / Services / Repositories
* ⚔️ Simple, deterministic single-attack simulation on `/battle/attack`

---
