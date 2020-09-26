# Better SM64 Non-Stop
This project is forked from the [Super Mario 64 decomp project](https://github.com/n64decomp/sm64).

This is essentially SM64 Non-Stop but with animation and the ability to save.

# What is SM64 Non-Stop?

[SM64 Non-Stop](https://sites.google.com/site/sm64gameshark/codes/non-stop) is a modification to Super Mario 64 that prevents mario from going back to the castle when getting a star.
However, there are a few problems:
- There are no animations when getting a star or a key.
- In certain cases you have to manually exit the level to continue playing
- Since there is no dialog box or animation when getting a star, the game never saves. This also means getting a game over makes you lose all your progress.

There are workarounds but I prefer a more polished experience.

Better SM64 Non-Stop aims to correct most of these problems.

# What are the differences between the regular SM64 Non-Stop and this hack?

- When getting a star it still play the animation, then the game diplays a dialog box that asks you if you want to save, just like getting a "100 coins" type star.
- When getting a key, it plays the key cutscene and it exits the level. Just like the vanilla game.

# What was actually modified in the code?

There are in fact very few changes from the original source code:
- In `u32 interact_star_or_key(struct MarioState *m, UNUSED u32 interactType, struct Object *o)` located in `src/game/interaction.c` the code processes interractions with a star or a key. In this function there's a variable called `noExit` that is equal to 0 when mario should exit the level after this interaction and different to 0 otherwise.
The first fix was to set this variable to something other to 0 (I chose 1). So Mario never exits the level.
However, I wanted Mario to exit the level when getting a key. So I added a condition that checks if Mario is in a Bowser level. If he is in a Bowser level, the behavior is the same as in the original game. If he's not, then the game considers the interaction as if mario got a "100 coin star" type of star.

- In `u8 get_cutscene_from_mario_status(struct Camera *c)` located in `src/game/camera.c` It checks which cutscene the game should play. In the `ACT_STAR_DANCE_WATER` case I replaced `cutscene = determine_dance_cutscene(c);` with `cutscene = CUTSCENE_DANCE_DEFAULT;` since the camera was messed up in the first case. However, I think this issue shouldn't be fixed here, and it should be fixed in either `s32 determine_dance_cutscene(UNUSED struct Camera *c)` (located in the same file) or in `u32 interact_star_or_key(struct MarioState *m, UNUSED u32 interactType, struct Object *o)`.


