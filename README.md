# SpaceWars
2024 Coding Challenge

## Premise

Turn-based (tick-based) game where players can queue up actions, and once every tick each player's next action is dequeued and executed.  Actions have an order of precedence, where moving happens first, then hits are calculated, then another other action (ship upgrades, weapons upgrades, ship repairs).

Every ship starts off with a certain number of repair credits, and a certain number of upgrade credits.

As long as ships have > 0 health, they can always move at a default speed, and can always fire a default (weak) weapon, regardless of how many upgrade or repair credits they have.

## Entities

- Ship
    - Location (x,y) (where you are)
    - Orientation (0-359) (where you are pointing) **or should this be just Up/UpRight/Right/RightDown/Down/DownLeft/Left/LeftUp? **
    - Health (0-100) (once this reaches 0 you are out of the game)
    - Speed (1-100) (how far you can move in any given tick.  Starts off at 1 but can be upgraded)
    - Shield (0-100) (hits first take out the shield, once the shield is at 0 then remaining hit points go against the ship health)
    - Weapons (a list of available weapons)
- Weapon
    - Name
    - Range (a list of distance and effectiveness, i.e. within 5 distance the weapon is 100% effective, from 5-10 it's 60% effective, 10-12 it's 20% effective, 12+ 0% effective)
    - Power (how many hit points at 100% effectiveness)
    - Cost (how many upgrade points it takes to acquire the weapon)
    - ShotCost (how many upgrade points it takes to fire the weapon)
    - ChargeTurns (how many game ticks it takes to charge the weapon)
- PowerUp (*not in initial version*)
    - Location (where it is on the map)
    - UpgradeCredits (how many bonus credits you get)
    - RepairCredits (how many bonus credits you get)

## Questions
- [ ] Can we require O365 authentication?  We could get user names from the token.  We could even get role membership from the token claims.
- [ ] 
     
## API Endpoints
- /game
    - /join?name={playerName} => returns a token
    - /state?token={playerToken} => returns the game state
      ```json
      {
        "state": "joining (or 'playing', or 'gameOver')",  
        "location": {
            "x": 123,
            "y": 234
        },
        "orientation": {something...degrees or cardinal directions???},
        "ship": {
            "health": 95,
            "speed": 89,
            "shield": 23,
            "weapons": [
                {
                    "name": "Spreadfire",
                    "range": [
                        {
                            "maxDistance": 5,
                            "effectiveness": 1.00
                        },
                        {
                            "maxDistance": 10,
                            "effectiveness": ".50"
                        }
                    ],
                    "power": 25,
                    "purchaseCost": 10,
                    "shotCost": 2,
                    "cargeTicks": 0
                },
                {
                    "name": "Laser of Death",
                    "range": [
                        {
                            "maxDistance": 50,
                            "effectiveness": "1.00"
                        },
                        {
                            "maxDistance": 90,
                            "effectiveness": ".75"
                        },
                        {
                            "maxDistance": 150,
                            "effectiveness": ".4"
                        }
                    ],
                    "power": 40,
                    "purchaseCost": 16,
                    "shotCost": 5,
                    "chargeTicks": 3
                }
            ]
        }
      }
      ```
    - /enqueue?token={playerToken}&action={action}&subAction={subAction}
      |action|possible sub-actions|
      |---|----|
      |turn|right/left|
      |move||
      |charge|weaponName|
      |fire|weaponName|
      |repair||
      |purchase|weaponName|
      |upgrade|upgradeName|
      |reload|weaponName|
    - /clearQueue?token={playerToken}
    