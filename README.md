# battleRoyal

A turn-based, You pick a warrior, a weapon, and armour and fight the enemy in a randomly chosen arena with random weather. It is a version of the Warsim game.

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