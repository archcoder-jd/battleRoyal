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