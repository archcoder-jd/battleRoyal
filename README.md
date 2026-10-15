# battleRoyal

A text-based battle game written in Java. You pick a warrior, a weapon, and armour and fight the enemy in a randomly chosen arena with random weather. It is a version of the Warsim game.

## Features

- Three warrior classes (Human, Elf, Orc) with different health, strenght, and dexterity ranges
- Three weapons (Dagger, Sword, Axe) and three armour sets (Leather, Chainmail, Platemail)
- Three terrains (Ground, Hill, Sand) and three weather types (Calm, Windy, Stormy) that change how hard it is to land a hit
- Light and heavy attacks, with a save-and-quit option mid-battle
- Save file support: resume a battle from `gameSave.txt`
- Play-again prompt after each match

## Requirements

- Java Development Kit (JDK) 8 or newer. The project was compliled and tested with OpenJDK 21.
- A terminal that can display emoji for the weather labels (optional; the game still runs without it).

## Getting Started

Clone the repository and compile from the project root:

```bash
git clone https://github.com/archcoder-jd/battleRoyal.git
cd battleRoyal
javac Battle.java
```

`javac` finds the packages (`armour`, `terrain`, `utility`, `warrior`, `weapon`, `weather`) in the subfolders automatically.

Run the game from the same folder. The save file is read and written as a relative path, so starting the game from another directory will not find your save.

```bash
java Battle
```

## How to play

### 1. Continue or start fresh

If a save file exists, you are asked:

```
Continue Game?
1) Yes
2) No
```

Choose `1` to load the saved battle, or `2` to start a new one. If `gameSave.txt` is empty, choose `2`.

### 2. Set up the battle

For a new game you will be asked to:

1. **Pick a terrain.** Your choice is pooled with a random terrain, and one of the two is used at random.
2. **Pick your warrior** (Human, Elf or Orc).
3. **Pick your weapon** (Dagger, Sword or Axe).
4. **Pick your armour** (Leather, Chainmail or Platemail).

The weather is chosen at random. The enemy's warrior, weapon and armour are also chosen at random.

### 3. Fight

Turns alternate between you and the enemy. On your turn, pick an attack:

```
1) Light
2) Heavy
3) Save & Quit
```

Heavy attacks deal more damage than light attacks. The enemy picks light or heavy at random on its turn. The first side to reduce the other's health to zero or below wins.

## Game mechanics

### Warriors

Starting stats are rolled randomly within these ranges when a new warrior is created.

| Warrior | Health   | Strength | Dexterity |
|---------|----------|----------|-----------|
| Human   | 75–149   | 50–99    | 50–99     |
| Elf     | 50–99    | 40–69    | 90–164    |
| Orc     | 120–209  | 200–274  | 30–44     |

### Weapons

| Weapon | Base damage | Dexterity cost |
|--------|-------------|----------------|
| Dagger | 50          | 10             |
| Sword  | 100         | 40             |
| Axe    | 180         | 80             |

### Armour

Armour reduces the damage you take from each hit.

| Armour     | Damage reduction | Dexterity cost |
|------------|------------------|----------------|
| Leather    | 10               | 5              |
| Chainmail  | 50               | 30             |
| Platemail  | 100              | 70             |

### Terrain and weather

Terrain and weather add to the difficulty of landing a hit (their dexterity cost is rolled when the battle is created).

| Terrain | Dexterity cost |
|---------|----------------|
| Ground  | 1–3            |
| Hill    | 4–6            |
| Sand    | 7–12           |

| Weather | Dexterity cost |
|---------|----------------|
| Calm    | 1–3            |
| Windy   | 2–7            |
| Stormy  | 10–20          |

### Combat

Each attack is resolved in three steps:

1. **Hit roll.** A number from 1 to 100 is rolled. Each weapon starts from its own base hit target (45 for a Dagger, 57 for a Sword, 65 for an Axe). That target is lowered by the attacker's dexterity and raised by the dexterity costs of the attacker's weapon and armour, plus the terrain and weather costs. The attack hits if the roll is greater than or equal to the target. Higher dexterity makes hits more likely; heavier gear and harsher conditions make them less likely.
2. **Damage.** On a hit, damage is the weapon's base damage plus a strength bonus plus an attack bonus. The attack bonus is larger for heavy attacks. Heavy attacks add damage but do not change the hit chance.
3. **Armour.** The defender's armour value is subtracted from the damage. If nothing gets through, the attack does no damage.

## Save file

Choosing **Save & Quit** (option `3`) on your turn writes the current battle to `gameSave.txt`. The file has eight comma-separated lines, in this order:

| Line | Contents                                      |
|------|-----------------------------------------------|
| 1    | Player warrior: type, health, strength, dexterity |
| 2    | Enemy warrior: type, health, strength, dexterity  |
| 3    | Player weapon: type, damage, dexterity cost       |
| 4    | Enemy weapon: type, damage, dexterity cost        |
| 5    | Player armour: type, armour value, dexterity cost |
| 6    | Enemy armour: type, armour value, dexterity cost  |
| 7    | Terrain: type, dexterity cost                 |
| 8    | Weather: type, dexterity cost                 |

Type codes:

- Warriors: `1` Human, `2` Elf, `3` Orc
- Weapons: `1` Dagger, `2` Sword, `3` Axe
- Armour: `1` Leather, `2` Chainmail, `3` Platemail
- Terrain: `1` Ground, `2` Hill, `3` Sand
- Weather: `1` Calm, `2` Windy, `3` Stormy

`sampleData.txt` shows an example save file.

## Project structure

```
battleRoyal/
├── Battle.java          # Entry point: game loop, setup, save and load
├── armour/              # Armour (abstract), Leather, Chainmail, Platemail
├── terrain/             # Terrain (abstract), Ground, Hill, Sand
├── utility/             # Printer (all console output) and Validator (menu input)
├── warrior/             # Warrior (abstract), Human, Elf, Orc
├── weapon/              # Weapon (abstract), Dagger, Sword, Axe
├── weather/             # Weather (abstract), Calm, Windy, Stormy
├── gameSave.txt         # Save file written by Save & Quit
├── sampleData.txt       # Example save file
└── LICENSE              # MIT
```

Each category has an abstract base class. Each concrete class sets its own stats and implements its own `strike` method, where weapons define their hit-chance formula.

## Known issues

- **Save & Quit still asks "Play again?"** After saving and quitting, the game shows the play-again prompt. Answering `2` ends the program.
- **Empty save file.** Answering `1` (Yes) to "Continue Game?" when `gameSave.txt` is empty does not start a battle. Answer `2` instead.
- **Working directory.** The save file path is relative, so run the game from the project root.
- **String comparisons.** Some checks compare strings with `==` (for example `turn == "Player"`). This works for the literal values used here, but `.equals` would be safer.
- **Compiled files are committed.** The `.class` files and `.DS_Store` are tracked in the repository. Adding a `.gitignore` would keep them out.
- **Music is disabled.** The music-player code in `Battle.java` is commented out.

## License

Released under the MIT License