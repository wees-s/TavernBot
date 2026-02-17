# TavernBot. A RPG Discord Bot.

A Discord bot featuring dice rolling and automated battle mechanics for tabletop-style RPGs, developed in Python using `discord.py`.

## Table of Contents

* Features
* Requirements
* Installation
* Configuration
* Commands
* Battle System
* Project Structure
* How It Works
* Troubleshooting
* License
* Contributing
* Support

---

## Features

* Complete dice rolling system (d2, d4, d6, d10, d20, d100)
* Fully automated 1v1 battle system
* Turn-based combat with fixed 3-second intervals
* Balanced HP and damage system
* Attack and defense mechanics with critical rolls
* Battle log system
* Administrative and debugging commands

---

## Requirements

* Python 3.8 or higher
* discord.py 2.0 or higher
* Discord Developer account
* Discord bot token

---

## Installation

1. Clone the repository:

```bash
git clone <your-repository>
cd <directory-name>
```

2. Install dependencies:

```bash
pip install discord.py
```

3. Configure the bot token in `connection.py`:

```python
bot.run("YOUR_TOKEN_HERE")
```

---

## Configuration

1. Create a bot application in the Discord Developer Portal
2. Enable the following intents:

   * Presence Intent
   * Server Members Intent
   * Message Content Intent
3. Copy the bot token
4. Replace `"xxxx"` in `connection.py` with your token
5. Invite the bot to your server using the OAuth2 URL Generator

---

## Commands

### General Commands

| Command   | Description                        |
| --------- | ---------------------------------- |
| `rp!help` | Displays the full list of commands |

### Dice Rolling

| Command   | Description           |
| --------- | --------------------- |
| `rp!d2`   | Rolls a 2-sided die   |
| `rp!d4`   | Rolls a 4-sided die   |
| `rp!d10`  | Rolls a 10-sided die  |
| `rp!d20`  | Rolls a 20-sided die  |
| `rp!d100` | Rolls a 100-sided die |

### Battle System

| Command                               | Description                       | Example                         |
| ------------------------------------- | --------------------------------- | ------------------------------- |
| `rp!battle @player1 hp1 @player2 hp2` | Starts an automatic battle        | `rp!battle @User1 50 @User2 50` |
| `rp!forfeit`                          | Forfeits the current battle       | `rp!forfeit`                    |
| `rp!resume`                           | Resumes a paused battle           | `rp!resume`                     |
| `rp!log`                              | Displays the last 10 battle turns | `rp!log`                        |

### Administration Commands

| Command    | Description                               |
| ---------- | ----------------------------------------- |
| `rp!debug` | Displays information about active battles |
| `rp!clear` | Clears the battle in the current channel  |

---

## Battle System

### Mechanics

The battle system is fully automated:

* Automatic turns every 3 seconds
* Automatic attack and defense rolls
* HP values between 1 and 100
* Continuous combat until one player reaches 0 HP

### Attack Damage Table

| Roll  | Damage   | Description                            |
| ----- | -------- | -------------------------------------- |
| 0     | 1 (self) | Deals 1 damage to the attacker         |
| 1–4   | 1–4      | Ineffective attack                     |
| 5–10  | 5–10     | Weak attack                            |
| 11–15 | 11–15    | Effective attack                       |
| 16–19 | 16–19    | Highly effective attack                |
| 20    | 25       | Critical hit                           |
| 21    | 20       | Special critical hit and restores 5 HP |

### Defense Table

| Roll  | Reduction      | Description                              |
| ----- | -------------- | ---------------------------------------- |
| 0–4   | 0              | Defense failed                           |
| 5     | 1              | Minimal defense                          |
| 6–9   | Roll value − 2 | Partial defense                          |
| 10–17 | 10             | Near-perfect defense                     |
| 18–19 | 100%           | Perfect dodge                            |
| 20    | 100% + 5       | Parry: avoids damage and deals 5 back    |
| 21    | 100%           | Mastery: avoids damage and restores 5 HP |

### Battle Rules

1. HP must be between 1 and 100
2. Bots cannot participate
3. Players must be different users
4. Only one active battle per channel
5. Players may forfeit at any time
6. The winner is the player with HP greater than 0

---

## Project Structure

```
src/
├── connection.py      # Main bot configuration and command registration
├── rpg_dice.py        # Dice rolling system
├── rpg_rules.py       # Help messages and rules
└── rpg_simulator.py   # Battle system logic
```

### Module Overview

* **connection.py**: Manages the Discord connection and registers all commands
* **rpg_dice.py**: Provides dice rolling utilities
* **rpg_simulator.py**: Implements battle logic, including:

  * Battle state management
  * Damage and defense calculations
  * Automated turn handling
  * Battle logging
* **rpg_rules.py**: Contains formatted help and rule messages

---

## How It Works

### Battle Flow

1. Start: A player uses `rp!battle @opponent own_hp opponent_hp`
2. Validation: Parameters and players are validated
3. Creation: A battle state is created in the channel
4. Automation: The automatic turn loop begins
5. Turn execution:

   * Attacker rolls a d22 for attack
   * Defender rolls a d22 for defense
   * Final damage is calculated
   * Player HP is updated
   * Turn is recorded in the log
6. Check: The system verifies if any player reached 0 HP
7. End: The winner is declared and the battle state is cleared

### Special d22 Die

The system uses a 22-sided die (0–21) to support special mechanics:

* Values 0–19 follow standard behavior
* Value 20 triggers a critical hit
* Value 21 triggers a unique special critical effect

---

## Troubleshooting

### Bot does not respond

* Verify that the token is correct
* Confirm all required intents are enabled
* Check the bot’s permissions in the channel

### Battle does not start

* Ensure no battle is already active in the channel
* Verify HP values are within the allowed range
* Confirm neither participant is a bot

### Battle is stuck

* Use `rp!debug` to inspect the state
* Use `rp!clear` to reset a stalled battle

---

## Contributing

Contributions are welcome. You may:

* Report bugs
* Suggest new features
* Improve documentation
* Submit pull requests

---

## Support

For questions or support, please open an issue in the repository.

---

Developed using Python and discord.py.
