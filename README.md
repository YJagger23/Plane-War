# Plane-War
=================

   * [Environment](#environment)<br>
   * [Stage 1：Generate background and essential components](#Stage-1-Generate-background-and-essential-components)<br>
               * ​             [Create game background screen   <em>(LN_01)</em>](#create-game-background-screen---ln_01)<br>
               * ​             [Know coordinates   <em>(LN_02)</em>](#know-coordinates---ln_02)<br>
               * ​             [Create main window  <em>(LN_03)</em>](#create-main-window--ln_03)<br>


# Environment

1. ##### Install pygame
    Terminal -> python3 -m pip install pygame

# Stage 1: Generate background and essential components

1. ##### Step 1: Create game background screen   *(LN_01)*

```python
bg_size = width, height = 480, 700 #set the size of the background

screen = pygame.display.set_mode(bg_size) #set the screen of the game

pygame.display.set_caption("Airplane War - Group 09") #set name of the game

background = pygame.image.load("images/background.png") #load background picture
```
