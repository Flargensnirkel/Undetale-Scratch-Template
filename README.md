# Undetale-Scratch-Template
An unfinished Undertale fangame built in Scratch

This contains no actual fangame content, but I thought I would make it available in case it could be useful to anyone wanting to make a Sans-style fangame in Scratch

Please note that the project file is already too large to be saved on scratch, so you'll want to use an offline editor.

## Features
### Implemented
- The menu system
- A text-based format for designing and implementing Sans-style attacks
- The red and blue soul modes
- Accurate KR health draining
- A basic system for writing text on the screen
### Not Implemented
- Healing (Currently, items are consumed upon use, but do not heal)
- The enemy health system, sparing system, sprites and dialogue
- The text above the menu buttons (Name, HP, LV, etc.)
- Attack indicators
- Some attack events

## How to use
I made this so long ago, I'm not entirely sure how to use it. Scratch code is rather difficult to read, so this is the best overview I can give of what different things do:
### Sprites
- `Soul`: Contains all scripts pertaining to the soul and its movement, as well as the death animation. Also handles the incrementation of the turn counter.
- `Bullet board`: Contains the scripts for drawing the bullet board (the rectangle where the menu items appear and where the soul is during attacks)
- `Buttons`: Contains the scripts for the four main menu buttons (FIGHT, ACT, ITEM, MERCY)
- `Bone`: Contains all scripts pertaining to the bone attacks
- `Health bar`: Self explanatory
- `GUI`: Contains some unused menu button sprites, and handles the game over screen
- `Attack`: Handles attack parsing
- `Text`: Handles writing various types of text to the screen in the Undertale font
- `Attack bar`: Self explanatory
- `GB`: Contains all scripts and sprites pertaining to Gaster blaster attacks

### Important Scripts
- Menu system can by modified in the script on the far right of the `Soul` sprite (`define [set menu to ID (ID)]`)
- General setup script (including inventory) and control loop (including movement and blue soul physics) is at the top right of the `Soul` sprite (`when I recieve [start]`)

### Attack Parser
Included is a semi-complete system for implementing custom attacks as a list. The atk_s0 list (the placeholder attack) is provided as an example:
```
0; bones; 2; -200; -180; 40; 0; x; y; 0; 15; 10; 0; -1; 0; 11; 40; -15; 10; 0; 1; 0
0; bone; 3.1; 20; -20; 50
0.5; gb; 1; 3; 0; -59; 90; 3; 1000
2; slam; -90
3; end
```
Each line is to be treated as an element of the list.

A list of commands is provided in a comment at the top of the attack sprite, but for clarity, it will also be provided here:
```
# Comment

t+ bone t- x y len rot rotX rotY velX velY velRot accX accY accRot
t+ bones t- x y len rot rotX rotY velX velY velRot accX accY accRot # spX spY spRot spVelX spVelY spVelRot

t+ gb t* t- x y rot size start

t+ slam dir
t+ velocity+ vel [not implemented]
t+ velocity* vel [not implemented]
t+ direction* dir [not implemented]

t+ sizeBB t- top bottom width [not implemented]
t+ rotBB t- velRot [not implemented]

t+ tBubble text disableMovement [not implemented]
t+ USTBubble t- text disableMovement [not implemented]

t+ special ID
```
Where:

- `t+` is the time (in seconds since the beginning of the attack) that the event begins.
- `t-` is the time that the event ends.
- `t*` is the time that the event (specifically the Gaster blaster) fires.
- `x` and `y` are the position (in the case of the bone, the center).
- `len` is the length of the bone.
- `rot` is the rotation in degrees.
- `rotX` and `rotY` are the center of rotation. For rotation about the center of mass, use 'x' and 'y' respectively.
- `velX`, `velY`, and `velRot` are the velocity arguments. They are added to `x`, `y`, and `rot` respectively each frame.
- `accX`, `accY`, and `accRot` are the acceleration arguments. They are added to `velX`, `velY`, and `velRot` respectively each frame.
- `#` is the number of bones created.
- `spX`, `spY`, `spRot`, `spVelX`, `spVelY`, `spVelRot` are used specifically for the `bones` command. They are used to differentiate the bones. The initial bone is created with the arguments given, and each bone after that has these values added to `x`, `y`, `rot`, `velX`, `velY`, and `velRot`, respectively. For example, if `spX` is equal to 50, the bones will be spaced 50 pixels apart.
- `size` is the size of the Gaster blaster
- `start` represents the location on the edge of the screen where the Gaster blaster appears. Starts at the top left corner, and walks along the edge. For example, in the sample code above, `start` is 1000. 480 pixels right, 360 pixels down, and 160 pixels left is (80. -180).
- `dir` is the direction (0 is right, 90 is down, 180 is left, and -90 is up)
- `vel` is the velocity of the soul to either add to (+) or set to (*), where positive values work against gravity and negative values work with it.
- `top`, `bottom` and `width` define the boundaries of the bullet board
- `text` is the text displayed in the text bubble
- `disableMovement` is whether or not to disable movement while the text box is active (0 for false, 1 for true)
