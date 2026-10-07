# Pacman Ghost Surge — Virtual Experience Program

## Program overview

This program builds Ghost Surge, a playable survival-mode version of Pacman in Python and Pygame, across seven tasks.

**Scenario.** Retro Forge Games is reviving its Pacman title with a new survival mode, and the role here is junior gameplay programmer. Ghosts double every 30 seconds. Pacman removes ghosts by touching them. The round ends when 128 ghosts are on screen at once.

**Game rules being built.**

- Every 30 seconds, each ghost type doubles: 1, 2, 4, 8, 16, 32.
- Each type (Blinky, Pinky, Inky, Clyde) caps at 32, so the total cap is 128.
- Touching a ghost removes it instead of ending the game.
- Game Over triggers only when 128 or more ghosts are on screen.

**Skills practiced.** Python, Pygame, lists, functions, parameters, loops, timers, collision detection, sprite groups, on-screen text, debugging, VS Code, terminal testing, and Git/GitHub.

## Program rules

Every task follows the same loop: read the brief, write pseudocode, write code, test, commit, submit.

1. **Turn off Copilot inline suggestions.** In VS Code, open Settings, search "inline suggest", and uncheck Editor: Inline Suggest Enabled. Copilot chat may only be used to ask what an error means, never to write the feature.
2. **Pseudocode first.** Before coding any function or loop, write the plan as plain-English comments in the file. In Tasks 3 and 4, submit the pseudocode before writing any Python.
3. **Commit after each working piece.** Use the commit messages listed in each task.
4. **Submit every task the same way.** Push to GitHub, then send the repo link, a one-paragraph summary of what changed, and the deliverable named in that task.
5. **Finish one task before starting the next.** A task sent back for revision gets fixed and resubmitted first.
6. **Stuck rule.** Try for 20 minutes, write down what was tried and the exact error, then ask.

## Task 1: Download and set up the project

Deliverable: a PacmanGhostSurge folder on GitHub that runs the starter game with `python main.py`.

**Brief.** The survival mode starts from the original open-source Pacman game. That repo supplies the art, music, and font. The Ghost Surge starter file, main.py, replaces its game code and marks every spot to build with a TODO.

**What gets downloaded.**

| Item | Where it comes from | What it provides |
| --- | --- | --- |
| Original Pacman game | [hbokmann/Pacman on GitHub](https://github.com/hbokmann/Pacman) | `images/` folder (Pacman and ghost sprites), `pacman.mp3`, `freesansbold.ttf` |
| Ghost Surge starter file | main.py (full code in the Starter code section at the end of this file) | The game code with the survival-mode header and TODO markers |
| Program instructions | PROGRAM.md (this file, sent with this program) | Every task brief, kept in the repo for reference |
| Pygame | Installed with pip in step 4 | The game library |

Python, Git, the GitHub CLI, and VS Code should already be installed. Check with `python --version`, `git --version`, and `gh --version`. If any command is not found, install it: [Python](https://www.python.org/downloads/), [Git](https://git-scm.com/download/win), [GitHub CLI](https://cli.github.com/), [VS Code](https://code.visualstudio.com/).

**Steps (PowerShell).**

1. Download the original game into a new folder named PacmanGhostSurge:

```powershell
cd $HOME\Documents\code
git clone https://github.com/hbokmann/Pacman.git PacmanGhostSurge
cd PacmanGhostSurge
```

2. Remove the original repo's Git history and the files this project does not use:

```powershell
Remove-Item -Recurse -Force .git
Remove-Item pacman.exe, pacman.py
```

3. Save this `PROGRAM.md` into `Documents\code\PacmanGhostSurge`. Then create a new file named `main.py` in that folder and paste in all the code from the Starter code section at the end of this file. Then confirm the folder looks like this:

```powershell
Get-ChildItem
```

Expected: `images`, `freesansbold.ttf`, `main.py`, `pacman.ico`, `pacman.mp3`, `PROGRAM.md`, `README.md`, `.gitattributes`, `.gitignore`.

4. Install Pygame and run the starter:

```powershell
pip install pygame
python main.py
```

The window title should read Pacman Ghost Surge. Pacman and four ghosts appear, music plays, and touching a ghost still ends the game. That is expected until Task 4.

5. Open the folder in VS Code and turn off inline suggestions (Program rules, rule 1):

```powershell
code .
```

6. Start a new repo and push it to a new GitHub repository named PacmanGhostSurge:

```powershell
git init
git add .
git commit -m "Create Pacman Ghost Surge project from starter"
gh repo create PacmanGhostSurge --public --source=. --push
```

**Commit message:** Create Pacman Ghost Surge project from starter

**Submit:** the repo link and a screenshot of the game window running from main.py.

## Task 2: Map the codebase

Deliverable: a filled-in code map that gives the line number and a one-sentence description for each of the eight locations below. No code changes in this task.

**Brief.** Before a new programmer touches gameplay, the lead needs proof they know where everything lives. Every later task edits one of these spots.

**Fill in this table and add it as CODE_MAP.md in the repo.**

| What to find | Line number(s) | What the code does there |
| --- | --- | --- |
| Where Pacman is created |  |  |
| Where Blinky, Pinky, Inky, Clyde are created |  |  |
| Where the game loop starts |  |  |
| Where keyboard input is handled |  |  |
| Where ghosts are updated each frame |  |  |
| Where collision detection happens |  |  |
| Where the score text is drawn |  |  |
| Where Game Over is triggered |  |  |

**Two questions to answer at the bottom of CODE_MAP.md:**

1. What is the difference between `monsta_list` and `all_sprites_list`? What happens if a sprite is in one but not the other?
2. What frame rate is the game set to, and what does the game loop do every frame, in order?

**Commit message:** Add code map

**Submit:** link to CODE_MAP.md.

## Task 3: Build the ghost multiplication system

Deliverable: every 30 seconds, each ghost type doubles and stops at 32, verified by printing each type's count to the terminal.

**Brief.** This is the core survival mechanic. Ghosts must be tracked by type so they can be counted, updated, and doubled separately.

**Part A: Tracking lists (TODO 1).** Create `blinky_instances`, `pinky_instances`, `inky_instances`, and `clyde_instances`, each starting with its original ghost. Then create `ghost_instances` holding all four lists. In `ghost_instances`, index 0 is Blinky, 1 is Pinky, 2 is Inky, and 3 is Clyde.

**Part B: The 30-second timer (TODO 2).** Start a repeating timer with `pygame.time.set_timer()` using the event `pygame.USEREVENT + 1` and 30000 milliseconds.

**Part C: Listen for the timer (TODO 3).** In the event loop, check for `pygame.USEREVENT + 1` and call `duplicate_ghost(ghost_instances, monsta_list, all_sprites_list)`.

**Part D: The duplicate_ghost function (TODO 7).** Signature: `duplicate_ghost(gi, monsta_list, asl)`. For each of the four lists, it must:

1. Save the list's current length in a variable before adding anything.
2. Work out how many new ghosts to add so the list doubles but never passes 32.
3. Create each new ghost and add it to three places: its type list, `monsta_list`, and `asl`.

**Pseudocode checkpoint.** Write Part D as plain-English comments and submit them before writing any Python.

**Test.** Temporarily set the timer to 3000 ms and print each list's length after every call. The expected sequence per type is 1, 2, 4, 8, 16, 32, 32. Set it back to 30000 when done.

**Commit messages:** Add ghost tracking lists · Add ghost duplication timer · Add timer event listener · Add duplicate ghost function

**Submit:** repo link and a screenshot of the terminal printout showing the sequence.

## Task 4: Change the gameplay loop

Deliverable: every ghost moves, the ghost count shows on screen, touching a ghost removes it, and Game Over happens only at 128 ghosts.

**Brief.** With ghosts multiplying, the original single-ghost code breaks. The loop must handle any number of ghosts and the rules must flip from instant death to survival.

**Part A: Update every ghost (TODO 4).** Replace each single-ghost update with a `for` loop over that type's list, so duplicated ghosts move too.

**Part B: Ghost count display (TODO 5).** Each frame, count ghosts with `len(monsta_list)`, render the text `Ghosts: N` with the existing font, and draw it with `screen.blit` near the score.

**Part C: New collision rule (TODO 6).** Remove the instant Game Over on contact. Loop over the list of ghosts Pacman hit this frame, and remove each one from `monsta_list`, `all_sprites_list`, and its own type list.

**Part D: Game Over at 128 (TODO 6).** Each frame, if `len(monsta_list) >= 128`, run the existing Game Over screen.

**Pseudocode checkpoint.** Write Part C as plain-English comments and submit them before coding. Answer this in the comments: why must the ghost also leave its type list?

**Test.** Eat two Blinkys right after the first doubling, then wait for the next timer. Write down what the Ghosts number shows before and after and explain it.

**Commit messages:** Add ghost update loops · Add ghost count display · Update Pacman collision behavior · Add Game Over rule at 128 ghosts

**Submit:** repo link, a screenshot showing the ghost count on screen, and the written test result.

## Task 5: Add score and survival tracking

Deliverable: the screen shows survival time and ghosts removed, and the Game Over screen shows both final numbers.

**Brief.** Players need a goal beyond staying alive. The studio wants two tracked numbers, plus one original scoring rule.

**Required (TODO 8).**

1. A survival timer in seconds, using `pygame.time.get_ticks()` minus the start time, divided by 1000.
2. A ghosts-removed counter that goes up by 1 in the collision loop from Task 4.
3. Both shown on screen during play and on the Game Over screen.

**Design choice (pick one, explain why in the commit message).**

- 1 point per ghost removed
- Score rises each second survived
- Bonus points for every timer cycle that ends with fewer than 32 ghosts

**Commit messages:** Add survival timer · Add ghosts removed counter · Add score rule

**Submit:** repo link and a screenshot of the Game Over screen with both numbers.

## Task 6: QA test and debug

Deliverable: TEST_REPORT.md with every check marked Pass or Fail, plus a log of each bug found and how it was fixed.

**Brief.** Before the build goes to the lead, QA signs off. Run each check, record the result honestly, fix failures, and retest.

**Test checklist (copy into TEST_REPORT.md).**

| # | Check | Pass / Fail | Notes |
| --- | --- | --- | --- |
| 1 | Game opens with no terminal errors |  |  |
| 2 | Pacman and four original ghosts appear |  |  |
| 3 | Timer fires at 30 seconds |  |  |
| 4 | Ghosts double, not triple |  |  |
| 5 | Each type stops at 32 |  |  |
| 6 | Every duplicated ghost moves |  |  |
| 7 | Ghost count on screen matches the real number of ghosts |  |  |
| 8 | Touching a ghost removes it and lowers the count |  |  |
| 9 | Touching a ghost does not end the game |  |  |
| 10 | Game Over happens at 128 ghosts and not before |  |  |
| 11 | Survival time and ghosts removed show correctly |  |  |

**Bug log format.** For each bug: what happened, the exact error text if any, the cause, and the fix.

**Common errors to check against.**

| Symptom | Likely cause | Fix |
| --- | --- | --- |
| `NameError: duplicate_ghost is not defined` | Function defined below the line that calls it | Move the function above the `startGame()` call |
| `IndentationError` | Lines inside a function, if, or for block not lined up | Check indentation level by level |
| Ghosts triple instead of double | List length read while the list is growing | Save the length before the loop that adds ghosts |
| Ghost appears but does not move | Not in the update loop | Make sure every type list is looped |
| Ghost exists but is invisible | Not added to `all_sprites_list` | Add every new ghost to the all-sprites group |

**Commit messages:** Test ghost duplication system · Fix bugs from QA

**Submit:** link to TEST_REPORT.md.

## Task 7: Polish and ship

Deliverable: two polish features working, a README, and a clean commit history on the PacmanGhostSurge GitHub repo.

**Brief.** The core game works. Now make it feel finished and hand it off so someone else could run it.

**Pick any two polish features.**

- Warning message when the ghost count passes 100
- Restart option on the Game Over screen
- Title screen explaining the survival rules
- Sound effect when ghosts duplicate or when Pacman removes one
- High score saved to a text file between runs

**README.md must include:** what Ghost Surge is, the game rules, how to install pygame and run `python main.py`, controls, and a screenshot. Replace the original Pacman README.md with this new one.

**Final check.** Run `git log --oneline` and confirm there is one clear commit per feature. Push everything.

**Commit messages:** one per polish feature, plus Add README

**Submit:** repo link and a 30–60 second screen recording of a full round from start to Game Over (speed the timer up for the recording if needed).

## Starter code: main.py

Create `main.py` in the PacmanGhostSurge folder and paste in everything in the code block below. Each TODO maps to a task.


| TODO | Task | What goes there |
| --- | --- | --- |
| 1 | Task 3, Part A | Ghost tracking lists |
| 2 | Task 3, Part B | 30-second timer |
| 3 | Task 3, Part C | Timer event listener |
| 4 | Task 4, Part A | Loops that move every ghost |
| 5 | Task 4, Part B | Ghost count on screen |
| 6 | Task 4, Parts C and D | Remove-on-touch and Game Over at 128 |
| 7 | Task 3, Part D | duplicate_ghost function |
| 8 | Task 5 | Survival time and ghosts removed |

```python
#################################################################
# Pacman Ghost Surge in Python with PyGame
# Survival Mode Starter File
#
# BASED ON:
#   Pacman in Python with PyGame
#   https://github.com/hbokmann/Pacman
#
# PROJECT GOAL:
#   Turn Pacman into a survival-mode game where ghosts multiply
#   over time.
#
# FEATURES TO BUILD:
#   1. Every 30 seconds, the number of EACH ghost type doubles.
#      1 Blinky becomes 2, then 4, then 8, then 16, then 32.
#   2. Each ghost type stops at 32 ghosts.
#      Blinky = 32 max, Pinky = 32 max, Inky = 32 max, Clyde = 32 max
#      Total max ghost count = 128 ghosts
#   3. Display the current ghost count on screen.
#   4. When Pacman touches a ghost, that ghost is removed
#      instead of causing immediate Game Over.
#   5. Game Over only happens when there are 128 or more ghosts
#      on screen at the same time.
#
# HOW TO RUN:
#   python main.py
#
# Every place to add code is marked with "TODO".
# Use Ctrl+F and search "TODO" to find them all.
#################################################################

import pygame

black = (0,0,0)
white = (255,255,255)
blue = (0,0,255)
green = (0,255,0)
red = (255,0,0)
purple = (255,0,255)
yellow = ( 255, 255, 0)

Trollicon=pygame.image.load('images/Trollman.png')
pygame.display.set_icon(Trollicon)

#Add music
pygame.mixer.init()
pygame.mixer.music.load('pacman.mp3')
pygame.mixer.music.play(-1, 0.0)

# This class represents the bar at the bottom that the player controls
class Wall(pygame.sprite.Sprite):
    # Constructor function
    def __init__(self,x,y,width,height, color):
        # Call the parent's constructor
        pygame.sprite.Sprite.__init__(self)

        # Make a blue wall, of the size specified in the parameters
        self.image = pygame.Surface([width, height])
        self.image.fill(color)

        # Make our top-left corner the passed-in location.
        self.rect = self.image.get_rect()
        self.rect.top = y
        self.rect.left = x

# This creates all the walls in room 1
def setupRoomOne(all_sprites_list):
    # Make the walls. (x_pos, y_pos, width, height)
    wall_list=pygame.sprite.RenderPlain()

    # This is a list of walls. Each is in the form [x, y, width, height]
    walls = [ [0,0,6,600],
              [0,0,600,6],
              [0,600,606,6],
              [600,0,6,606],
              [300,0,6,66],
              [60,60,186,6],
              [360,60,186,6],
              [60,120,66,6],
              [60,120,6,126],
              [180,120,246,6],
              [300,120,6,66],
              [480,120,66,6],
              [540,120,6,126],
              [120,180,126,6],
              [120,180,6,126],
              [360,180,126,6],
              [480,180,6,126],
              [180,240,6,126],
              [180,360,246,6],
              [420,240,6,126],
              [240,240,42,6],
              [324,240,42,6],
              [240,240,6,66],
              [240,300,126,6],
              [360,240,6,66],
              [0,300,66,6],
              [540,300,66,6],
              [60,360,66,6],
              [60,360,6,186],
              [480,360,66,6],
              [540,360,6,186],
              [120,420,366,6],
              [120,420,6,66],
              [480,420,6,66],
              [180,480,246,6],
              [300,480,6,66],
              [120,540,126,6],
              [360,540,126,6]
            ]

    # Loop through the list. Create the wall, add it to the list
    for item in walls:
        wall=Wall(item[0],item[1],item[2],item[3],blue)
        wall_list.add(wall)
        all_sprites_list.add(wall)

    # return our new list
    return wall_list

def setupGate(all_sprites_list):
    gate = pygame.sprite.RenderPlain()
    gate.add(Wall(282,242,42,2,white))
    all_sprites_list.add(gate)
    return gate

# This class represents the ball
# It derives from the "Sprite" class in Pygame
class Block(pygame.sprite.Sprite):

    # Constructor. Pass in the color of the block,
    # and its x and y position
    def __init__(self, color, width, height):
        # Call the parent class (Sprite) constructor
        pygame.sprite.Sprite.__init__(self)

        # Create an image of the block, and fill it with a color.
        # This could also be an image loaded from the disk.
        self.image = pygame.Surface([width, height])
        self.image.fill(white)
        self.image.set_colorkey(white)
        pygame.draw.ellipse(self.image,color,[0,0,width,height])

        # Fetch the rectangle object that has the dimensions of the image
        # image.
        # Update the position of this object by setting the values
        # of rect.x and rect.y
        self.rect = self.image.get_rect()

# This class represents the bar at the bottom that the player controls
class Player(pygame.sprite.Sprite):

    # Set speed vector
    change_x=0
    change_y=0

    # Constructor function
    def __init__(self,x,y, filename):
        # Call the parent's constructor
        pygame.sprite.Sprite.__init__(self)

        # Set height, width
        self.image = pygame.image.load(filename).convert()

        # Make our top-left corner the passed-in location.
        self.rect = self.image.get_rect()
        self.rect.top = y
        self.rect.left = x
        self.prev_x = x
        self.prev_y = y

    # Clear the speed of the player
    def prevdirection(self):
        self.prev_x = self.change_x
        self.prev_y = self.change_y

    # Change the speed of the player
    def changespeed(self,x,y):
        self.change_x+=x
        self.change_y+=y

    # Find a new position for the player
    def update(self,walls,gate):
        # Get the old position, in case we need to go back to it

        old_x=self.rect.left
        new_x=old_x+self.change_x
        prev_x=old_x+self.prev_x
        self.rect.left = new_x

        old_y=self.rect.top
        new_y=old_y+self.change_y
        prev_y=old_y+self.prev_y

        # Did this update cause us to hit a wall?
        x_collide = pygame.sprite.spritecollide(self, walls, False)
        if x_collide:
            # Whoops, hit a wall. Go back to the old position
            self.rect.left=old_x
        else:
            self.rect.top = new_y

            # Did this update cause us to hit a wall?
            y_collide = pygame.sprite.spritecollide(self, walls, False)
            if y_collide:
                # Whoops, hit a wall. Go back to the old position
                self.rect.top=old_y

        if gate != False:
            gate_hit = pygame.sprite.spritecollide(self, gate, False)
            if gate_hit:
                self.rect.left=old_x
                self.rect.top=old_y

#Inheritime Player klassist
class Ghost(Player):
    # Change the speed of the ghost
    def changespeed(self,list,ghost,turn,steps,l):
        try:
            z=list[turn][2]
            if steps < z:
                self.change_x=list[turn][0]
                self.change_y=list[turn][1]
                steps+=1
            else:
                if turn < l:
                    turn+=1
                elif ghost == "clyde":
                    turn = 2
                else:
                    turn = 0
                self.change_x=list[turn][0]
                self.change_y=list[turn][1]
                steps = 0
            return [turn,steps]
        except IndexError:
            return [0,0]

Pinky_directions = [
[0,-30,4],
[15,0,9],
[0,15,11],
[-15,0,23],
[0,15,7],
[15,0,3],
[0,-15,3],
[15,0,19],
[0,15,3],
[15,0,3],
[0,15,3],
[15,0,3],
[0,-15,15],
[-15,0,7],
[0,15,3],
[-15,0,19],
[0,-15,11],
[15,0,9]
]

Blinky_directions = [
[0,-15,4],
[15,0,9],
[0,15,11],
[15,0,3],
[0,15,7],
[-15,0,11],
[0,15,3],
[15,0,15],
[0,-15,15],
[15,0,3],
[0,-15,11],
[-15,0,3],
[0,-15,11],
[-15,0,3],
[0,-15,3],
[-15,0,7],
[0,-15,3],
[15,0,15],
[0,15,15],
[-15,0,3],
[0,15,3],
[-15,0,3],
[0,-15,7],
[-15,0,3],
[0,15,7],
[-15,0,11],
[0,-15,7],
[15,0,5]
]

Inky_directions = [
[30,0,2],
[0,-15,4],
[15,0,10],
[0,15,7],
[15,0,3],
[0,-15,3],
[15,0,3],
[0,-15,15],
[-15,0,15],
[0,15,3],
[15,0,15],
[0,15,11],
[-15,0,3],
[0,-15,7],
[-15,0,11],
[0,15,3],
[-15,0,11],
[0,15,7],
[-15,0,3],
[0,-15,3],
[-15,0,3],
[0,-15,15],
[15,0,15],
[0,15,3],
[-15,0,15],
[0,15,11],
[15,0,3],
[0,-15,11],
[15,0,11],
[0,15,3],
[15,0,1],
]

Clyde_directions = [
[-30,0,2],
[0,-15,4],
[15,0,5],
[0,15,7],
[-15,0,11],
[0,-15,7],
[-15,0,3],
[0,15,7],
[-15,0,7],
[0,15,15],
[15,0,15],
[0,-15,3],
[-15,0,11],
[0,-15,7],
[15,0,3],
[0,-15,11],
[15,0,9],
]

pl = len(Pinky_directions)-1
bl = len(Blinky_directions)-1
il = len(Inky_directions)-1
cl = len(Clyde_directions)-1

# Call this function so the Pygame library can initialize itself
pygame.init()

# Create an 606x606 sized screen
screen = pygame.display.set_mode([606, 606])

# Set the title of the window
pygame.display.set_caption('Pacman Ghost Surge')

# Create a surface we can draw on
background = pygame.Surface(screen.get_size())

# Used for converting color maps and such
background = background.convert()

# Fill the screen with a black background
background.fill(black)

clock = pygame.time.Clock()

pygame.font.init()
font = pygame.font.Font("freesansbold.ttf", 24)

#default locations for Pacman and monstas
w = 303-16 #Width
p_h = (7*60)+19 #Pacman height
m_h = (4*60)+19 #Monster height
b_h = (3*60)+19 #Binky height
i_w = 303-16-32 #Inky width
c_w = 303+(32-16) #Clyde width


#################################################################
# TODO 7 (Task 3, Part D): define duplicate_ghost(gi, monsta_list, asl) here.
# It must sit ABOVE the startGame() call at the bottom of the file.
# Write your pseudocode as comments first.
#################################################################


def startGame():

    all_sprites_list = pygame.sprite.RenderPlain()

    block_list = pygame.sprite.RenderPlain()

    monsta_list = pygame.sprite.RenderPlain()

    pacman_collide = pygame.sprite.RenderPlain()

    wall_list = setupRoomOne(all_sprites_list)

    gate = setupGate(all_sprites_list)

    p_turn = 0
    p_steps = 0

    b_turn = 0
    b_steps = 0

    i_turn = 0
    i_steps = 0

    c_turn = 0
    c_steps = 0

    # Create the player paddle object
    Pacman = Player( w, p_h, "images/Trollman.png" )
    all_sprites_list.add(Pacman)
    pacman_collide.add(Pacman)

    Blinky=Ghost( w, b_h, "images/Blinky.png" )
    monsta_list.add(Blinky)
    all_sprites_list.add(Blinky)

    Pinky=Ghost( w, m_h, "images/Pinky.png" )
    monsta_list.add(Pinky)
    all_sprites_list.add(Pinky)

    Inky=Ghost( i_w, m_h, "images/Inky.png" )
    monsta_list.add(Inky)
    all_sprites_list.add(Inky)

    Clyde=Ghost( c_w, m_h, "images/Clyde.png" )
    monsta_list.add(Clyde)
    all_sprites_list.add(Clyde)

    # TODO 1 (Task 3, Part A): create the four ghost tracking lists
    # and the ghost_instances master list here.

    # TODO 2 (Task 3, Part B): start the 30-second repeating timer here.

    # TODO 8 (Task 5): record the game start time and create a
    # ghosts-removed counter here.

    # Draw the grid
    for row in range(19):
        for column in range(19):
            if (row == 7 or row == 8) and (column == 8 or column == 9 or column == 10):
                continue
            else:
                block = Block(yellow, 4, 4)

                # Set a random location for the block
                block.rect.x = (30*column+6)+26
                block.rect.y = (30*row+6)+26

                b_collide = pygame.sprite.spritecollide(block, wall_list, False)
                p_collide = pygame.sprite.spritecollide(block, pacman_collide, False)
                if b_collide:
                    continue
                elif p_collide:
                    continue
                else:
                    # Add the block to the list of objects
                    block_list.add(block)
                    all_sprites_list.add(block)

    bll = len(block_list)

    score = 0

    done = False

    i = 0

    while done == False:
        # ALL EVENT PROCESSING SHOULD GO BELOW THIS COMMENT
        for event in pygame.event.get():

            # TODO 3 (Task 3, Part C): check for the timer event and
            # call duplicate_ghost here.

            if event.type == pygame.QUIT:
                done=True

            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    Pacman.changespeed(-30,0)
                if event.key == pygame.K_RIGHT:
                    Pacman.changespeed(30,0)
                if event.key == pygame.K_UP:
                    Pacman.changespeed(0,-30)
                if event.key == pygame.K_DOWN:
                    Pacman.changespeed(0,30)

            if event.type == pygame.KEYUP:
                if event.key == pygame.K_LEFT:
                    Pacman.changespeed(30,0)
                if event.key == pygame.K_RIGHT:
                    Pacman.changespeed(-30,0)
                if event.key == pygame.K_UP:
                    Pacman.changespeed(0,30)
                if event.key == pygame.K_DOWN:
                    Pacman.changespeed(0,-30)

        # ALL EVENT PROCESSING SHOULD GO ABOVE THIS COMMENT

        # ALL GAME LOGIC SHOULD GO BELOW THIS COMMENT
        Pacman.update(wall_list,gate)

        # TODO 4 (Task 4, Part A): each block below moves ONE ghost.
        # Change each block into a for loop over that ghost type's list
        # so every duplicated ghost moves too.

        returned = Pinky.changespeed(Pinky_directions,False,p_turn,p_steps,pl)
        p_turn = returned[0]
        p_steps = returned[1]
        Pinky.changespeed(Pinky_directions,False,p_turn,p_steps,pl)
        Pinky.update(wall_list,False)

        returned = Blinky.changespeed(Blinky_directions,False,b_turn,b_steps,bl)
        b_turn = returned[0]
        b_steps = returned[1]
        Blinky.changespeed(Blinky_directions,False,b_turn,b_steps,bl)
        Blinky.update(wall_list,False)

        returned = Inky.changespeed(Inky_directions,False,i_turn,i_steps,il)
        i_turn = returned[0]
        i_steps = returned[1]
        Inky.changespeed(Inky_directions,False,i_turn,i_steps,il)
        Inky.update(wall_list,False)

        returned = Clyde.changespeed(Clyde_directions,"clyde",c_turn,c_steps,cl)
        c_turn = returned[0]
        c_steps = returned[1]
        Clyde.changespeed(Clyde_directions,"clyde",c_turn,c_steps,cl)
        Clyde.update(wall_list,False)

        # See if the Pacman block has collided with anything.
        blocks_hit_list = pygame.sprite.spritecollide(Pacman, block_list, True)

        # Check the list of collisions.
        if len(blocks_hit_list) > 0:
            score +=len(blocks_hit_list)

        # ALL GAME LOGIC SHOULD GO ABOVE THIS COMMENT

        # ALL CODE TO DRAW SHOULD GO BELOW THIS COMMENT
        screen.fill(black)

        wall_list.draw(screen)
        gate.draw(screen)
        all_sprites_list.draw(screen)
        monsta_list.draw(screen)

        text=font.render("Score: "+str(score)+"/"+str(bll), True, red)
        screen.blit(text, [10, 10])

        # TODO 5 (Task 4, Part B): draw "Ghosts: N" on screen here.

        # TODO 8 (Task 5): draw survival time and ghosts removed here.

        if score == bll:
            doNext("Congratulations, you won!",145,all_sprites_list,block_list,monsta_list,pacman_collide,wall_list,gate)

        monsta_hit_list = pygame.sprite.spritecollide(Pacman, monsta_list, False)

        # TODO 6 (Task 4, Parts C and D): replace the instant Game Over below.
        # Touching a ghost should remove it. Game Over should only
        # happen when there are 128 or more ghosts.
        if monsta_hit_list:
            doNext("Game Over",235,all_sprites_list,block_list,monsta_list,pacman_collide,wall_list,gate)

        # ALL CODE TO DRAW SHOULD GO ABOVE THIS COMMENT

        pygame.display.flip()

        clock.tick(10)

def doNext(message,left,all_sprites_list,block_list,monsta_list,pacman_collide,wall_list,gate):
    while True:
        # ALL EVENT PROCESSING SHOULD GO BELOW THIS COMMENT
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_ESCAPE:
                    pygame.quit()
                if event.key == pygame.K_RETURN:
                    del all_sprites_list
                    del block_list
                    del monsta_list
                    del pacman_collide
                    del wall_list
                    del gate
                    startGame()

        #Grey background
        w = pygame.Surface((400,200))  # the size of your rect
        w.set_alpha(10)                # alpha level
        w.fill((128,128,128))          # this fills the entire surface
        screen.blit(w, (100,200))      # (0,0) are the top-left coordinates

        #Won or lost
        text1=font.render(message, True, white)
        screen.blit(text1, [left, 233])

        text2=font.render("To play again, press ENTER.", True, white)
        screen.blit(text2, [135, 303])
        text3=font.render("To quit, press ESCAPE.", True, white)
        screen.blit(text3, [165, 333])

        pygame.display.flip()

        clock.tick(10)

startGame()

pygame.quit()
```
