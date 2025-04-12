# database_api_pmmp
# DatabaseAPI for Hexo Server

`DatabaseAPI` is a centralized database plugin for the Hexo server, designed to manage player statistics across multiple game modes such as BedWars, EggWars, SkyWars, and more. It provides a simple and unified API for recording and retrieving player data (wins, losses, points) in a shared SQLite database (`hexo_stats.db`). This plugin ensures consistent data management across all plugins in the Hexo server.

This README serves as a comprehensive guide for developers to integrate `DatabaseAPI` into their plugins and call its methods effectively. It includes detailed examples and tables to make the process straightforward.

---

## Table of Contents
1. [Installation](#installation)
2. [Requirements](#requirements)
3. [Setup in Your Plugin](#setup-in-your-plugin)
4. [Available Methods](#available-methods)
5. [Calling Methods: Step-by-Step](#calling-methods-step-by-step)
6. [Example Integrations](#example-integrations)
7. [Common Mistakes to Avoid](#common-mistakes-to-avoid)
8. [Notes](#notes)
9. [Arabic Translation / الترجمة العربية](#arabic-translation--الترجمة-العربية)

---

## Installation
1. Download the `DatabaseAPI` plugin and place it in the `plugins` folder of your PocketMine-MP server.
2. Start or restart the server to enable the plugin.
3. The plugin automatically creates a SQLite database (`hexo_stats.db`) in the server's data directory (`server_data/hexo_stats.db`).

---

## Requirements
- **PocketMine-MP API**: 5.0.0 or higher
- **Dependencies**: None

---

## Setup in Your Plugin
To use `DatabaseAPI` in your plugin (e.g., SkyWars, BedWars, EggWars), follow these steps:

1. **Add Dependency**:
   In your plugin’s `plugin.yml`, include `DatabaseAPI` as a dependency to ensure it loads before your plugin:
   ```yaml
   name: SkyWars
   version: 1.0.0
   main: Hexo\SkyWars
   api: 5.0.0
   depend: [DatabaseAPI]
   description: SkyWars game for Hexo server
Import the DatabaseAPI Class: In your plugin’s main class, import the DatabaseAPI class:
php

Collapse

Wrap

Copy
use Hexo\DatabaseAPI;
Access the API: Use DatabaseAPI::getInstance() to access the DatabaseAPI instance and call its methods:
php

Collapse

Wrap

Copy
$db = DatabaseAPI::getInstance();
$wins = $db->getWins($player, "SkyWars");
Available Methods
The following table lists all methods provided by DatabaseAPI, including their purpose, parameters, and return values.

Method	Description	Parameters	Return Value
addWin(Player $player, string $game): void	Records a win for the player in the specified game.	$player: The player object.<br>$game: Game identifier (e.g., "SkyWars").	None
addLoss(Player $player, string $game): void	Records a loss for the player in the specified game.	$player, $game	None
addPoints(Player $player, string $game, int $points): void	Adds points to the player’s record for the specified game.	$player, $game, $points: Number of points to add.	None
getWins(Player $player, string $game): int	Retrieves the number of wins for the player in the specified game.	$player, $game	Integer (number of wins).
getLosses(Player $player, string $game): int	Retrieves the number of losses for the player in the specified game.	$player, $game	Integer (number of losses).
getPoints(Player $player, string $game): int	Retrieves the total points for the player in the specified game.	$player, $game	Integer (total points).
getPlayerStats(Player $player, string $game): array	Retrieves all stats for the player in the specified game.	$player, $game	Array with keys: wins, losses, points.
displayStats(Player $player, string $game): void	Displays the player’s stats in chat for the specified game.	$player, $game	None
Calling Methods: Step-by-Step
This section provides detailed instructions on how to call DatabaseAPI methods, with examples for BedWars, EggWars, and SkyWars.

1. Getting the DatabaseAPI Instance
To call any method, first get the DatabaseAPI instance:

php

Collapse

Wrap

Copy
use Hexo\DatabaseAPI;

$db = DatabaseAPI::getInstance();
Where to use: Inside methods or event handlers (e.g., onGameEnd, onCommand) where you need to interact with the database.
Example: In a SkyWars plugin:
php

Collapse

Wrap

Copy
public function onGameEnd(Player $player, bool $won): void {
    $db = DatabaseAPI::getInstance();
    // Call methods here
}
2. Recording Wins (addWin)
Use addWin to record a win when a player wins a match.

Example (SkyWars):

php

Collapse

Wrap

Copy
if ($won) {
    $db->addWin($player, "SkyWars"); // Adds 1 win for SkyWars
    $player->sendMessage("§aCongratulations! You won SkyWars!");
}
Example (EggWars):

php

Collapse

Wrap

Copy
if ($won) {
    $db->addWin($player, "EggWars"); // Adds 1 win for EggWars
    $player->sendMessage("§aYou won EggWars!");
}
Example (BedWars):

php

Collapse

Wrap

Copy
if ($won) {
    $db->addWin($player, "BedWars"); // Adds 1 win for BedWars
    $player->sendMessage("§aVictory in BedWars!");
}
3. Retrieving Wins (getWins)
Use getWins to retrieve the number of wins for a player in a specific game.

Example (SkyWars):

php

Collapse

Wrap

Copy
$wins = $db->getWins($player, "SkyWars");
$player->sendMessage("§eYour SkyWars wins: §f$wins");
Example (EggWars):

php

Collapse

Wrap

Copy
$wins = $db->getWins($player, "EggWars");
$player->sendMessage("§eYour EggWars wins: §f$wins");
Example (BedWars):

php

Collapse

Wrap

Copy
$wins = $db->getWins($player, "BedWars");
$player->sendMessage("§eYour BedWars wins: §f$wins");
When to use: After a match, in a /stats command, or when a player joins the lobby.
Returns: An integer (e.g., 5 for 5 wins). Returns 0 if no wins are recorded.
4. Recording Losses (addLoss)
Use addLoss to record a loss when a player loses a match.

Example (SkyWars):

php

Collapse

Wrap

Copy
if (!$won) {
    $db->addLoss($player, "SkyWars"); // Adds 1 loss for SkyWars
    $player->sendMessage("§cBetter luck next time!");
}
Example (EggWars):

php

Collapse

Wrap

Copy
$db->addLoss($player, "EggWars"); // Adds 1 loss for EggWars
Example (BedWars):

php

Collapse

Wrap

Copy
$db->addLoss($player, "BedWars"); // Adds 1 loss for BedWars
5. Adding Points (addPoints)
Use addPoints to add points to a player’s record (e.g., for rewards).

Example (SkyWars):

php

Collapse

Wrap

Copy
$db->addPoints($player, "SkyWars", 50); // Adds 50 points for SkyWars
$player->sendMessage("§aYou earned 50 points!");
Example (EggWars):

php

Collapse

Wrap

Copy
$db->addPoints($player, "EggWars", 30); // Adds 30 points for EggWars
Example (BedWars):

php

Collapse

Wrap

Copy
$db->addPoints($player, "BedWars", 20); // Adds 20 points for BedWars
6. Retrieving All Stats (getPlayerStats)
Use getPlayerStats to get all stats (wins, losses, points) as an array.

Example (SkyWars):

php

Collapse

Wrap

Copy
$stats = $db->getPlayerStats($player, "SkyWars");
$player->sendMessage("§aSkyWars Stats:");
$player->sendMessage("§eWins: §f{$stats['wins']}");
$player->sendMessage("§eLosses: §f{$stats['losses']}");
$player->sendMessage("§ePoints: §f{$stats['points']}");
Example (EggWars):

php

Collapse

Wrap

Copy
$stats = $db->getPlayerStats($player, "EggWars");
$player->sendMessage("§aEggWars Stats: Wins: {$stats['wins']}, Losses: {$stats['losses']}, Points: {$stats['points']}");
Example (BedWars):

php

Collapse

Wrap

Copy
$stats = $db->getPlayerStats($player, "BedWars");
$player->sendMessage("§aBedWars Stats: Wins: {$stats['wins']}, Losses: {$stats['losses']}, Points: {$stats['points']}");
7. Displaying Stats in Chat (displayStats)
Use displayStats to show a pre-formatted stats message in chat.

Example (SkyWars):

php

Collapse

Wrap

Copy
$db->displayStats($player, "SkyWars");
Output:

text

Collapse

Wrap

Copy
Your stats in SkyWars:
Wins: 5
Losses: 3
Points: 150
Example (EggWars):

php

Collapse

Wrap

Copy
$db->displayStats($player, "EggWars");
Example (BedWars):

php

Collapse

Wrap

Copy
$db->displayStats($player, "BedWars");
8. Using Methods in Commands
You can call methods in a command to show stats on demand.

Example (SkyWars Command):

php

Collapse

Wrap

Copy
public function onCommand(CommandSender $sender, Command $command, string $label, array $args): bool {
    if ($command->getName() === "skywars" && $sender instanceof Player) {
        $db = DatabaseAPI::getInstance();
        if (isset($args[0]) && $args[0] === "stats") {
            $db->displayStats($sender, "SkyWars");
            return true;
        } elseif (isset($args[0]) && $args[0] === "wins") {
            $wins = $db->getWins($sender, "SkyWars");
            $sender->sendMessage("§eYour SkyWars wins: §f$wins");
            return true;
        }
        $sender->sendMessage("§cUsage: /skywars <stats|wins>");
        return false;
    }
    return false;
}
Add to plugin.yml:

yaml

Collapse

Wrap

Copy
commands:
  skywars:
    description: Manage SkyWars stats
    usage: /skywars <stats|wins>
Example Integrations
Below are complete examples showing how to integrate DatabaseAPI into different plugins.

SkyWars Plugin
plugin.yml:

yaml

Collapse

Wrap

Copy
name: SkyWars
version: 1.0.0
main: Hexo\SkyWars
api: 5.0.0
depend: [DatabaseAPI]
description: SkyWars game for Hexo server
commands:
  skywars:
    description: Manage SkyWars stats
    usage: /skywars <stats|wins>
SkyWars.php:

php

Collapse

Wrap

Copy
<?php
namespace Hexo;

use pocketmine\plugin\PluginBase;
use pocketmine\event\Listener;
use pocketmine\player\Player;
use pocketmine\command\Command;
use pocketmine\command\CommandSender;
use Hexo\DatabaseAPI;

class SkyWars extends PluginBase implements Listener {
    public function onEnable(): void {
        $this->getServer()->getPluginManager()->registerEvents($this, $this);
    }

    public function onGameEnd(Player $player, bool $won): void {
        $db = DatabaseAPI::getInstance();
        if ($won) {
            $db->addWin($player, "SkyWars");
            $db->addPoints($player, "SkyWars", 50);
            $player->sendMessage("§aCongratulations! You won SkyWars!");
        } else {
            $db->addLoss($player, "SkyWars");
            $db->addPoints($player, "SkyWars", 10);
            $player->sendMessage("§cBetter luck next time!");
        }
        $wins = $db->getWins($player, "SkyWars");
        $player->sendMessage("§eYour SkyWars wins: §f$wins");
    }

    public function onCommand(CommandSender $sender, Command $command, string $label, array $args): bool {
        if ($command->getName() === "skywars" && $sender instanceof Player) {
            $db = DatabaseAPI::getInstance();
            if (isset($args[0]) && $args[0] === "stats") {
                $db->displayStats($sender, "SkyWars");
                return true;
            } elseif (isset($args[0]) && $args[0] === "wins") {
                $wins = $db->getWins($sender, "SkyWars");
                $sender->sendMessage("§eYour SkyWars wins: §f$wins");
                return true;
            }
            $sender->sendMessage("§cUsage: /skywars <stats|wins>");
            return false;
        }
        return false;
    }
}
EggWars Plugin
plugin.yml:

yaml

Collapse

Wrap

Copy
name: EggWars
version: 1.0.0
main: Hexo\EggWars
api: 5.0.0
depend: [DatabaseAPI]
description: EggWars game for Hexo server
EggWars.php:

php

Collapse

Wrap

Copy
<?php
namespace Hexo;

use pocketmine\plugin\PluginBase;
use pocketmine\event\Listener;
use pocketmine\player\Player;
use Hexo\DatabaseAPI;

class EggWars extends PluginBase implements Listener {
    public function onEnable(): void {
        $this->getServer()->getPluginManager()->registerEvents($this, $this);
    }

    public function onGameEnd(Player $player, bool $won): void {
        $db = DatabaseAPI::getInstance();
        if ($won) {
            $db->addWin($player, "EggWars");
            $db->addPoints($player, "EggWars", 30);
            $player->sendMessage("§aYou won EggWars!");
        } else {
            $db->addLoss($player, "EggWars");
            $db->addPoints($player, "EggWars", 5);
            $player->sendMessage("§cBetter luck next time!");
        }
        $wins = $db->getWins($player, "EggWars");
        $player->sendMessage("§eYour EggWars wins: §f$wins");
    }
}
BedWars Plugin
plugin.yml:

yaml

Collapse

Wrap

Copy
name: BedWars
version: 1.0.0
main: Hexo\BedWars
api: 5.0.0
depend: [DatabaseAPI]
description: BedWars game for Hexo server
BedWars.php:

php

Collapse

Wrap

Copy
<?php
namespace Hexo;

use pocketmine\plugin\PluginBase;
use pocketmine\event\Listener;
use pocketmine\player\Player;
use Hexo\DatabaseAPI;

class BedWars extends PluginBase implements Listener {
    public function onEnable(): void {
        $this->getServer()->getPluginManager()->registerEvents($this, $this);
    }

    public function onGameEnd(Player $player, bool $won): void {
        $db = DatabaseAPI::getInstance();
        if ($won) {
            $db->addWin($player, "BedWars");
            $db->addPoints($player, "BedWars", 40);
            $player->sendMessage("§aVictory in BedWars!");
        } else {
            $db->addLoss($player, "BedWars");
            $db->addPoints($player, "BedWars", 15);
            $player->sendMessage("§cBetter luck next time!");
        }
        $wins = $db->getWins($player, "BedWars");
        $player->sendMessage("§eYour BedWars wins: §f$wins");
    }
}
Lobby Plugin (Unified Stats Display)
plugin.yml:

yaml

Collapse

Wrap

Copy
name: Lobby
version: 1.0.0
main: Hexo\Lobby
api: 5.0.0
depend: [DatabaseAPI]
description: Lobby plugin for Hexo server
commands:
  stats:
    description: Show player stats
    usage: /stats
Lobby.php:

php

Collapse

Wrap

Copy
<?php
namespace Hexo;

use pocketmine\plugin\PluginBase;
use pocketmine\command\Command;
use pocketmine\command\CommandSender;
use pocketmine\player\Player;
use Hexo\DatabaseAPI;

class Lobby extends PluginBase {
    public function onCommand(CommandSender $sender, Command $command, string $label, array $args): bool {
        if ($command->getName() === "stats" && $sender instanceof Player) {
            $db = DatabaseAPI::getInstance();
            $eggWarsWins = $db->getWins($sender, "EggWars");
            $bedWarsWins = $db->getWins($sender, "BedWars");
            $skyWarsWins = $db->getWins($sender, "SkyWars");
            $sender->sendMessage("§aYour Stats:");
            $sender->sendMessage("§eEggWars Wins: §f$eggWarsWins");
            $sender->sendMessage("§eBedWars Wins: §f$bedWarsWins");
            $sender->sendMessage("§eSkyWars Wins: §f$skyWarsWins");
            return true;
        }
        return false;
    }
}
Common Mistakes to Avoid
Incorrect Game Identifier:
Use the exact game name (e.g., "SkyWars", not "skywars"). Case matters!
Wrong:
php

Collapse

Wrap

Copy
$db->getWins($player, "skywars"); // Creates a new record
Correct:
php

Collapse

Wrap

Copy
$db->getWins($player, "SkyWars");
Missing Dependency:
Ensure depend: [DatabaseAPI] is in your plugin.yml. Without it, your plugin may crash if DatabaseAPI is not loaded.
Calling Methods Too Early:
Don’t call DatabaseAPI::getInstance() in your plugin’s constructor or global scope. Use it inside methods or event handlers:
php

Collapse

Wrap

Copy
// Wrong
private $db;
public function __construct() {
    $this->db = DatabaseAPI::getInstance();
}

// Correct
public function onGameEnd(Player $player): void {
    $db = DatabaseAPI::getInstance();
    $db->addWin($player, "SkyWars");
}
Not Verifying Data:
After calling addWin, verify with getWins to ensure the data was saved:
php

Collapse

Wrap

Copy
$db->addWin($player, "SkyWars");
$wins = $db->getWins($player, "SkyWars");
$player->sendMessage("Wins after adding: $wins"); // Should increase by 1
Notes
Game Identifier: Use consistent game names (e.g., "SkyWars", "EggWars", "BedWars") across all plugins to avoid duplicate records.
Database: All data is stored in hexo_stats.db, shared across all plugins using DatabaseAPI.
Performance: SQLite is used for simplicity. For large servers, consider switching to MySQL for better performance.
Backup: Regularly back up hexo_stats.db to prevent data loss.
