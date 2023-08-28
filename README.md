# thprac-steam-proton-wrapper
## Introduction
A wrapper script for launching the official Touhou games on Steam with the [Touhou Community Reliant Automatic Patcher](https://www.thpatch.net/) (thcrap), and optionally the [Vsync patch](https://en.touhouwiki.net/wiki/Game_Tools_and_Modifications#Vsync_Patches) (vpatch) and [thprac](https://github.com/touhouworldcup/thprac) from the Steam client using [Proton](<https://en.wikipedia.org/wiki/Proton_(software)>) on GNU/Linux.

This script works by setting up and managing a global thcrap instance, launching the configuration tool when needed, all without user intervention.

Before launching a game, this script takes the command Steam would normally run, and alters it to include the patch loader. This creates a seamless integration between the Steam client and thcrap, allowing the games to function as if they weren't patched at all!

### Rationale

On Windows, when launching a game bought on Steam with thcrap, Steam will be able to detect and automatically wrap it, so integration will work fine.

On Linux, this isn't the case. Due to the compatibility layer used to run the games, launching them from outside the Steam client will make Steam unable to detect that they are running. This means that, while using thcrap, Steam integration won't be available, your playtime won't be tracked, and your friends won't be able to see that you are playing weird indie Japanese shmups.

Also, with Steam Play/Proton, it is expected that you can run your games without having to mess around with Wine. For Touhou, it's annoying having to fire up Wine to be able to set-up the translation patches, and then having no proper integration with Steam when trying to play the games.

## How to use
[Here's a video tutorial](https://www.youtube.com/watch?v=gyC_EWNWqPc) by [Maxmani](https://www.youtube.com/c/Maxmani) (outdated).
[Here is a reddit post](https://www.reddit.com/r/SteamDeck/comments/yy3xz9/how_to_use_thcrap_touhou_community_reliant/) to help you set up this on the Steam Deck by [Mino_nosty](https://www.reddit.com/user/mino_nosty/) (somewhat outdated)

**No longer need to set-up thcrap manually**
### 1. Installation
#### For Steam Flatpak (Not recommended, I haven't got it working)

       flatpak install flathub com.valvesoftware.Steam.Utility.thcrap_steam_proton_wrapper

#### Manual installation

Download the script, mark it as executable, and put under `/usr/local/bin/` (or somewhere that you find convenient such as /home/deck/.local/share/thcrap/scripts/):

       curl -O https://raw.githubusercontent.com/redirectto/thprac-steam-proton-wrapper/master/thprac_proton
       chmod +x thprac_proton
       mv thprac_proton /usr/local/bin
or (/home/deck/.local/share/thcrap/scripts/)

       mv thprac_proton /home/deck/.local/share/thcrap/scripts

Download the latest asset from [thprac](https://github.com/touhouworldcup/thprac/releases/) (the .exe file) and put it wherever you please.
You can place it for example on /home/deck/.local/share/thprac/
You can also rename it to thprac.exe or create a symlink to avoid renaming the file on each update.

### 2. Editing the script (optional)
If you have gone through the manual installation, you have the opportunity to change the following variables inside the script:
- The `THCRAP_FOLDER` variable points to where the thcrap installation will be located. You can to change it, for example, if you want it to point to your current thcrap installation.
- The `THCRAP_CONFIG` defines the config file that will be loaded when no other is specified in the launch options.
- The `THPRAC_LOCATION` variable points to where the thprac file is located.

### 3. Setting the launch options
Go to your Steam library -> right click the game -> Properties -> and edit the launch options to:

       thprac_proton -p -- %command%
   
In case you have put your script outside `/usr/local/bin/`, you'll have to provide the full path:
   
       /path/to/thprac_proton -p -- %command%
example (/home/deck/.local/share/thcrap/scripts/)

       /home/deck/.local/share/thcrap/scripts/thprac_proton -p -- %command%

This is the base command, which will run the game with the default config with thprac.

To disable thprac for that game, you have to remove the `-p` flag inside launch options for the game.

To change the config file loaded by thcrap, use the `-c` flag. By default it uses en.js (english translation patch)

To enable vpatch for that game, include the `-v` flag.

**Note that this script does not install vpatch on it's own.**

If you want to launch a game with vpatch, thprac and the Spanish translation, the command would look like this:

       thprac_proton -v -p -c lang_es.js -- %command%
or (generic path)

       /path/to/thprac_proton -v -p -c lang_es.js -- %command%
example (/home/deck/.local/share/thcrap/scripts/)

       /home/deck/.local/share/thcrap/scripts/thprac_proton -v -p -c lang_es.js -- %command%

**Note: the `%command%` always comes at the end**

If you want to use any environment variables in your launch options, you have to put them before the `%command%`, like this:

       thprac_proton -- LC_ALL=ja_JP.UTF-8 %command%
or (generic path)

       /path/to/thprac_proton -- LC_ALL=ja_JP.UTF-8 %command%
example (/home/deck/.local/share/thcrap/scripts/)

       /home/deck/.local/share/thcrap/scripts/thprac_proton -- LC_ALL=ja_JP.UTF-8 %command%

### 4. Running the game
Upon first launch, the script will download and set-up a thcrap instance, if there's not one already, and then launch the configuration tool, so you can generate your config files.

If you are using Big Picture (game mode) I would suggest to keep the updater running on the background due to a bug that prevents the keyboard from working, that can be fixed by switching to a different window.

And thats it!

## Advanced
### Debugging
The script sends all it's output to `/tmp/thprac_proton.log`, which includes the original, the modified launch commands and the command that launches thprac.
