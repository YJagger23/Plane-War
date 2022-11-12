# Plane-War
=================

   * [Environment](#environment)<br>
   * [Stage 1：Generate background and initiate the game](#Stage-1-Generate-background-and-initiate-the-game)<br>
     * [Step 1: Create game background screen](#Step-1-create-game-background-screen)<br>
     * [Step 2: Define the game main structure with the screen](#Step-2-Define-the-game-main-structure-with-the-screen)<br>
     * [Step 3: Initiate and exit game](#Step-3-initiate-and-exit-game)<br>
   * [Stage 2：Create essential components](#Stage-2-Create-essential-components)<br>
     * [Step 1: Create player plane](#Step-1-create-player-plane)<br>
     * [Step 2: Create enemy small plane](#Step-2-create-enemy-small-plane)<br>
     * [Step 3: Create enemy big plane](#Step-3-create-enemy-big-plane)<br>
     * [Step 4: Create bullets](#Step-4-create-bullets)<br>



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

1. ##### Step 1: Create player plane

   1) Initialize the player's image and position by using class 

   ```python
   class Player(pygame.sprite.Sprite): #use the sprite class to define the 'player' class
      def __init__(self, bg_size): 
          pygame.sprite.Sprite.__init__(self) #initialize the player
          self.image = pygame.image.load("images/player.jpg") #load the image as the player's image
          self.rect = self.image.get_rect() #get the position of the player
          self.width, self.height = bg_size[0], bg_size[1]  #set the boundry with the background sizee
          self.rect.left, self.rect.top = \
                        (self.width - self.rect.width)//2,\  #set the initial position of the player in the middle
                        self.height - self.rect.height - 60  #leave some space in the bottom
          self.speed = 10 #set the moving speed for the player
          self.active = True #set the player initial status as active
          #self.invicincible = False 
          self.mask = pygame.mask.from_surface(self.image) #Get a mask of the aircraft image for more accurate collision detection
   ```
   
   2) Define the movement of the player

   ```python
      def moveup (self): #define moveup
          if self.height - self.rect.top > 0: #when the distance between top of the player and the top of the screen>0
              self.rect.top -= self.speed # the distance between the top of the screen and the top of the player will be decreased by the player's speed
          else:
              self.rect.top = 0 #make sure the player will not move above the top of the screen
      
      def movedown (self): #define movedown
          if self.height - self.rect.bottom > 60: #when the distance between the top of the screen and the bottom of player>60
              self.rect.top += self.speed #the distance between the top of the screen and the top of the player will be increased by the player's speed
          else:
              self.rect.bottom = self.height - 60 #make sure the player will not move out of the bottom of the screen
      
      def moveleft (self): #define moveleft
          if self.rect.left-0>0: #when the distance between the between the screen left and the left of the player >0
              self.rect.left -= self.speed #the distance between the screen left and the left of the player will be decreased by the player's speed
          else:
              self.rect.left = 0 #make sure the player will not move out of the left of the screen     
      
      def moveright (self): #define moveright
          if self.width - self.rect.right>0: #when the distance between the screen right and the right of the player >0
              self.rect.left += self.speed #the distance between the screen left and the left of the player will be increased by the player's speed
          else:
              self.rect.right = self.width #make sure the player will not move out of the right of the screen         
   ```
   
   3) Reset the player when crashed
 
   ```python
      def reset(self):
          self.rect.left, self.rect.top = \ #rest the player to the original position 
                        (self.width - self.rect.width)//2,\
                        self.height - self.rect.height - 60
          self.active = True 
   ```
   
2. ##### Step 2: Create enemy small plane
   1) Initialize the enemy's image by using class and generate position by random module
   
   ```python
      from random import * #import the random module for generating random postion of the enemy
      
      class EnemyS(pygame.sprite.Sprite): #use the sprite class to define the 'EnemyS' class
          def __init__(self, bg_size):
              pygame.sprite.Sprite.__init__(self)
              self.image = pygame.image.load("images/enemys.png") #load the image as the enemy's image
              self.rect = self.image.get_rect() #get the position of enemys
              self.width, self.height = bg_size[0], bg_size[1] #set the boundry with the background size
              self.speed = 2 #set the moving speed of enemys
              self.active = True #set the initial status as active
              self.rect.left, self.rect.top = \ #use randint from the random module to generate random position of enemys
                        randint(0, self.width - self.rect.width), \ #set width range from 0 to the right of the screen
                        randint(-5 * self.height, 0) #set height range from 0 to beyond the bottom of the screen
              self.mask = pygame.mask.from_surface(self.image) #Get a mask of the image for more accurate collision detection
   ```                 
         
   2) Define the movement of enemy
   
   ```python
      def move(self):
          if self.height - self.rect.top > 0: #when the distance between the bottom of the screen and the top of enemy >0
            self.rect.top += self.speed #the distance between the top of enemys and the top of the screen will be increased by the speed of enemys
          else: #when the distance reach 0
            self.reset() #reset enemys
   ```  
   
   3) Reset the enemy
   
   ```python
      def reset(self): #reset to a new random position as defined earlier
        self.rect.left, self.rect.top = \
            randint(0, self.width - self.rect.width), \
            randint(-5 * self.height, 0)
        self.active = True #set enemys to be active again
   ``` 
   
3. ##### Step 3: Create bullets
   1) Initialize the bullet's image and position by using class 
   
   ```python
      class Bullet1(pygame.sprite.Sprite): #use the sprite class to define the 'Bullet1' class
          def __init__(self, position):
              pygame.sprite.Sprite.__init__(self)
              self.image = pygame.image.load("images/bullet1.png") #load the image as the bullet's image
              self.rect.left, self.rect.top = position #set the position of enemys
              self.speed = 12 #set the moving speed of bullets
              self.active = False #set the initial status as inactive
              self.mask = pygame.mask.from_surface(self.image) #Get a mask of the image for more accurate collision detection
   ```                 
         
   2) Define the movement of bullet
   
   ```python
      def move(self):
          self.rect.top -= self.speed: #the distance between the top of the bullet and the top of the screen is decreased by bullet's speed when active
          if self.rect.top < 0: #when the bullet is beyond the top of the screen
            self.active = False #set the bullet inactive
   ```  
   
   3) Reset the bullet
   
   ```python
      def reset(self, position): #reset to the position as defined earlier
          self.rect.left, self.rect.top = position
          self.active = True #set bullets to be active again
   ``` 
