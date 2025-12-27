BedWars Stats Query Tool
A lightweight, keyboard-driven GUI application for querying Hypixel BedWars statistics of Minecraft players, featuring real-time input capture, automatic UUID resolution, and intuitive stat calculation based on Hypixel's experience formula. Designed for quick access to core BedWars metrics without navigating the Hypixel website or in-game menus.
Disclaimer
This tool was developed with a focus on simplicity and accessibility for casual Minecraft players. It relies on public Mojang and Hypixel APIs, and while efforts have been made to handle edge cases (e.g., missing player data, division by zero errors), API rate limits, server outages, or changes to Hypixel's stat structure may affect functionality. The code uses threading to prevent GUI freezes and implements basic error handling, but limited testing across all environments means unexpected issues may occur. Contributions to improve reliability or add features (e.g., caching, multi-language support) are welcome.
Quick Start
For Users
Prerequisites
Install Python 3.8+ (https://www.python.org/downloads/)
Install required dependencies via command prompt/terminal:
bash
运行
pip install requests keyboard tkinter
Obtain a valid Hypixel API key (https://developer.hypixel.net/) – replace the placeholder key in the config section of the script with your own.
Run the Tool
Download the script file (e.g., bedwars_stats.py)
Execute the script:
bash
运行
python bedwars_stats.py
A transparent, always-on-top GUI window will appear at position +200+200 on your screen.
Query Player Stats
Press Ctrl+/ to activate input mode (the GUI will prompt you to enter a player name)
Type the Minecraft username of the player (use Backspace to correct typos)
Press Enter to confirm the username – the tool will:
Resolve the username to a UUID via Mojang's API
Fetch the player's BedWars data from Hypixel's API
Calculate the BedWars level using Hypixel's experience-to-star formula
Display core stats (Level, KDR, FKDR, WLR, wins/losses) in the GUI
Press Ctrl+/ again to exit input mode, or repeat to query another player.
For Developers
Clone/Modify the Code
Copy the script to your local development environment
Adjust configuration values (e.g., HOTKEY, WINDOW_POSITION, HYPIXEL_API_KEY) in the config section to match your preferences
Modify the stat calculation logic (getBWStar()) if Hypixel updates its experience formula
Test Changes
Run the modified script with:
bash
运行
python bedwars_stats.py
Test edge cases (e.g., non-existent usernames, players with 0 BedWars losses/kills, API timeouts) to validate error handling
Extend functionality by adding new stats (e.g., beds broken, final kills) or UI features (e.g., dark/light mode, custom window size)
Commands (Keyboard Shortcuts)
The tool is entirely keyboard-driven with minimal shortcuts for seamless use:
Shortcut	Function
Ctrl+/	Toggle input mode (activate to enter a username, deactivate to exit)
Alphanumeric keys	Type characters into the username input field (only in active input mode)
Backspace	Delete the last character from the username input (active input mode)
Enter	Confirm the username and trigger a stats query (active input mode)
Configuration
All configurable values are located in the config section at the top of the script for easy modification:
Key	Default Value	Description
HYPIXEL_API_KEY	2447816c-43ca-4463-a51c-086af3de4ef0	Your Hypixel API key (replace with a valid key to avoid rate limits/errors)
HOTKEY	ctrl+/	Shortcut to toggle input mode (supports any keyboard combination supported by the keyboard library)
WINDOW_POSITION	+200+200	Initial position of the GUI window (format: +X+Y pixels from top-left)
GUI Configuration
The GUI window can be customized by modifying the setup_window() method:
overrideredirect(True): Removes window borders (set to False for a standard window with close/minimize buttons)
bg='#114514': Transparent background color (match transparentcolor to keep the window see-through)
attributes('-alpha', 0.9): Window opacity (0 = fully transparent, 1 = fully opaque)
attributes('-topmost', True): Keeps the window above all other applications (set to False to disable)
Core Functionality Breakdown
UUID Resolution (getuuid())
Automatically converts Minecraft usernames to UUIDs using the public Mojang API. If resolution fails (e.g., invalid username, API down), it falls back to Notch’s static UUID to prevent script crashes.
BedWars Level Calculation (getBWStar())
Implements Hypixel’s official experience-to-star formula for BedWars, accounting for prestige levels (each prestige = 100 stars) and experience thresholds for early-level progression (0-4 stars). The formula is based on community-verified Hypixel mechanics and handles values above 5 prestiges correctly.
Hypixel API Integration (get_hypixel_player_data())
Fetches raw player data from Hypixel’s /player endpoint, validates API responses, and extracts BedWars-specific stats (Experience, wins, losses, kills, final kills/deaths). It includes timeout handling (15 seconds) to prevent hanging requests.
Stat Calculation
Core metrics are calculated with safety checks to avoid division-by-zero errors:
KDR: Kills / Deaths (0 if no deaths)
FKDR: Final Kills / Final Deaths (0 if no final deaths)
WLR: Wins / Losses (0 if no losses)
Level: Derived from total BedWars experience via getBWStar()
GUI & Keyboard Handling
The tool uses tkinter for a lightweight GUI and the keyboard library for global hotkey detection. Threading is used to:
Run keyboard listening in the background (avoids blocking the GUI)
Execute API queries outside the main GUI thread (prevents freezes during network requests)
Update the GUI safely via root.after() to comply with tkinter’s thread rules
Troubleshooting
No GUI window appears: Ensure tkinter is installed (included with most Python distributions; on Linux, install via sudo apt-get install python3-tk)
"API request failed" errors: Verify your Hypixel API key is valid, not rate-limited, and the Hypixel API is online (check https://status.hypixel.net/)
Keyboard shortcuts not working: Run the script as Administrator (required for global keyboard hooks on Windows)
Division by zero warnings: The tool automatically returns 0 for stats like KDR/WLR if the denominator is 0 (no action needed)
GUI freezes: The threading implementation prevents this, but if it occurs, close the script and restart (check for unhandled API timeouts)
This tool is designed to be a no-fuss solution for quick BedWars stat checks, prioritizing ease of use over advanced features. It requires no complex setup and works with any Minecraft username, as long as the player has public Hypixel stats and a valid API key is provided.
