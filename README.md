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
[Here's a video tutorial](https://www.youtube.com/watch?v=gyC_EWNWqPc) by [Maxmani](https://www.youtube.com/c/Maxmani) (a bit outdated, but still gives a pretty good idea).

[Here is a reddit post](https://www.reddit.com/r/SteamDeck/comments/yy3xz9/how_to_use_thcrap_touhou_community_reliant/) to help you set up this on the Steam Deck by [Mino_nosty](https://www.reddit.com/user/mino_nosty/)

#### Manual installation

Downlaod [thcrap](https://github.com/thpatch/thcrap/releases/latest/download/thcrap.zip), put it wherever you please and set it up through steam (proton), bottles (soda, wine), lutris (lutris) or wine.
You can place it for example on /home/deck/Applications/Touhou/thcrap

Download the latest asset from [thprac](https://github.com/touhouworldcup/thprac/releases/tag/v2.2.1.4) (the .exe file) and put it wherever you please.
You can place it for example on /home/deck/Applications/Touhou/thprac

Download [thprac_proton](https://github.com/redirectto/thprac-steam-proton-wrapper/blob/master/thprac_proton), mark it as executable, and put somewhere that you find convenient:

		curl -O https://raw.githubusercontent.com/redirectto/thprac-steam-proton-wrapper/master/thprac_proton
		chmod +x thprac_proton
		mv thprac_proton /home/deck/Applications/Touhou/thcrap/scripts

### 1. Editing the script (mandatory)
- The `THCRAP_FOLDER` variable points to where the thcrap installation will be located. You can to change it, for example, if you want it to point to your current thcrap installation.
- The `THCRAP_CONFIG` defines the config file that will be loaded when no other is specified in the launch options.
By default it uses lang_en.js (english translation patch).
- The `PROTON_FOLDER` variable points to where the proton version you are using to launch the game is located since you also need to launch thprac using the same proton version.
- The `THPRAC_FOLDER` variable points to where thprac is located.

### 2. Setting the launch options
Go to your Steam library -> right click the game -> Properties -> and edit the launch options to:

		/path/to/thprac_proton -e "%command%"

		/home/deck/Applications/Touhou/thcrap/scripts/thprac_proton -e "%command%"

In case you have put your script inside `/usr/local/bin/`, you can skip the path to thprac_proton:

		thprac_proton -e "%command%"

This is the base command, which will run the game with the default config without thprac.

Since I'm an Steam Deck user I haven't got working the script without the `-e` flag.

To enable thprac for that game, you have to include the `-p` flag.

To change the config file loaded by thcrap, use the `-c` flag. By default it uses lang_en.js (english translation patch)

To enable vpatch for that game, include the `-v` flag.

**Note that this script does not install vpatch on it's own.**

If you want to launch a game with both thprac and thcrap, use the following command:

		/path/to/thprac_proton -p -e "%command%"

		/home/deck/Applications/Touhou/thcrap/scripts/thprac_proton -p -e "%command%"

If you want to launch a game with vpatch, thprac and the Spanish translation, the command would look like this:

		/path/to/thprac_proton -v -p -c lang_es.js -e "%command%"
		
		/home/deck/Applications/Touhou/thcrap/scripts/thprac_proton -v -p -c lang_es.js -e "%command%"

**Note 1: The flag `-e` and `"%command%"` always come at the end**

**Note 2: I don't know how to make environment variables to work with the `-e` flag, and I cannot test them without the flag, I'm sorry**

If you want to use any environment variables in your launch options, you have to put them before the `%command%`, like this:

		/path/to/thprac_proton -- LC_ALL=ja_JP.UTF-8 %command%

### 3. Running the game
When the thcrap loader window shows up, click on 'Settings and logs' and uncheck 'Keep the updater running in background', so Steam can correctly detect when the game is closed.

If you are using SteamDeck gaming mode and a keyboard I would suggest to enable the thcrap console due to a bug that prevents the keyboard from working that can be fixed by switching windows.
You can enable the console inside /path/to/thcrap/config/config.js.
Don't close the console since you will close the game.

And thats it!

## Advanced
### Debugging
The script sends all it's output to `/tmp/thprac_proton.log`, which only includes the modified launch commands.
