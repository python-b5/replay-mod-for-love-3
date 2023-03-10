# Replay Mod for LOVE 3

This mod adds replays (sometimes known as "demos") to LOVE 3. They are recorded in the background and are stored as simple text files.

## Installation

**Highest currently supported LOVE 3 version**: `1.3.1.14`

Downloads can be found in the Releases tab, and are labeled with the LOVE 3 version they support.

To install, download the source code from a release that supports the LOVE 3 version you want, extract the `.zip` file, and copy the files to your base LOVE 3 install directory. Then, run `install.bat`, and the mod will be installed. The original `data.win` file will be saved as `vanilla.win`, to allow for easy uninstallation (just delete `data.win` and rename the backup file).

The mod will be updated as soon as possible after new updates come out (though since it does take time to update a mod, the length of which can vary depending on whether the new LOVE 3 version created any conflicts, it may take a couple days to do so). Each version of the mod only works with one specific version of LOVE 3.

## Replay file format

The first five lines are metadata that specifies the options selected when the run was recorded. If these lines do not follow this format, the replay will not appear in the replays menu. They are as follows:

- **Line 1:** The date and time the replay was created, in the format `YYYY-MM-DD HH:MM:SS`. This doesn't do anything except get displayed in the replays menu. Technically, it doesn't even *have* to be a timestamp; that's just what it is in replays recorded in-game. Time isn't stored here since it would make the first line in the replays menu too long.
- **Line 2:** The gamemode being played. It must be one of:
    - `unlimited`
    - `arcade`
    - `yolo`
    - `speedrun`
    - `single` + `custom`
        - These gamemodes can take arguments; `yolo` and `speedrun` can be appended to the line to enable those modes respectively (for example, `single yolo speedrun` would enable both). The order doesn't matter.
- **Line 3:** The content being played. Depending on the gamemode, this line will be:
    - **If the gamemode is `single`:** The room name of the level being played.
    - **If the gamemode is `custom`:** The folder name of the custom level being played.
    - **Otherwise:** The levelset being played. It must be one of:
        - `love3`
        - `love`
        - `kuso`
        - `loveremastered`
        - `love123classic`
        - `love123remastered`
        - `<3`
        - `endless`
- **Line 4:** Whether mirror mode is active. This must be `true` or `false`. Mirror mode is purely cosmetic, but is supported since runs using it will likely be slower. If this is `true` and the gamemode `endless`, the first level will always be flipped regardless of the random seed (this is a quirk of the vanilla game as well).
- **Line 5:** The random seed. This must be an integer. This is used to ensure death animations remain constant; if this is not set to the same seed that was used when recording the replay, deaths will cause a desync. On endless mode, this will determine the level order and whether the first level is mirrored.
- **Line 6:** The spriteset (or selected character). It must be one of:
    - `fiveEight`
    - The method to access the character select hasn't been revealed yet! The other options are a secret for now...
- **Line 7:** The trace flag. It must be an integer between `0` and `18`.
- **Line 8:** The trace flag's length. It must be an integer between `1` and `58`.

Only the information contained in the first four lines is displayed in the replays menu.

The remaining lines are the replay itself. Each line is a single frame, and contains 8 0s/1s (one for each input; 0 means unpressed and 1 means pressed). If a line does not follow this format, it will be skipped over. They are in the following order (down and pause are left out since they are not used in gameplay):

1. up
2. left
3. right
4. jump
5. checkpoint
6. die
7. reset checkpoint
8. slow motion

Replays recorded in-game are saved with the format `<date> <time>.txt`, where `date` is the current date in the format `YYYY-MM-DD` and `time` is the current time in the format `HH-MM-SS`. They are saved in the folder `%localappdata%/LOVE3/replays`, and are displayed in-game with the most recent replays at the top of the list.
