# Plane-War
=================

   * [Environment](#environment)<br>
   * [Stage 1：Generate background and initiate the game](#Stage-1-Generate-background-and-initiate-the-game)<br>
     * [Step 1: Create game background screen](#Step-1-create-game-background-screen)<br>
     * [Step 2: Define the game main structure with the screen](#Step-2-Define-the-game-main-structure-with-the-screen)<br>
     * [Step 3: Initiate and exit game](#Step-3-initiate-and-exit-game)<br>
   * [Stage 2：Create essential components](#Stage-2-Create-essential-components)<br>
     * [Step 1: Create player's plane](#Step-1-create-player's-plane)<br>



# Environment

1. ##### Install pygame
    Terminal -> python3 -m pip install pygame

# Stage 1: Generate background and initiate the game

1. ##### Step 1: Create game background screen

```python
bg_size = width, height = 480, 700 #set the size of the background
screen = pygame.display.set_mode(bg_size) #set the screen of the game
pygame.display.set_caption("Airplane War - Group 09") #set title of the game screen
background = pygame.image.load("images/background.png") #load background picture
```

2. ##### Step 2: Define the game main structure with the screen

```python
def main(): #define the game of the initial structure
    while True:
        screen.blit(background, (0, 0)) #put background into the screen at the origin (0,0)-top left position
        for event in pygame.event.get(): #create a temporary event loop
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
        pygame.display.flip() #Update the full display Surface to the screen
```

3. ##### Step 3: Initiate and exit game 

```python
#import essential modules
import sys
import pygame
import traceback

pygame.init() #initiate the game

if __name__ == "__main__": #operate the game
    try:
        main()
    except SystemExit: #exit the game
        pass
    except:
        traceback.print_exc()  #using traceback module to identify and record errors if any
        pygame.quit()
        input()
```

# Stage 2: Create essential components

1. ##### Step 1: Create player's plane

