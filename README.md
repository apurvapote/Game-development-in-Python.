# Game-development-in-Python.

import pygame
import time
import random

pygame.init()

display_width = 800
display_height = 600

black = (0,0,0)
white = (255,255,255)
red = (255,0,0)
blue = (0,0,255)

game_display = pygame.display.set_mode((display_width, display_height))
pygame.display.set_caption("Road Rash")
clock = pygame.time.Clock()

carImg = pygame.image.load('car.png')
car_width = 82

def car(x,y):
    game_display.blit(carImg, (x,y))                   #draw image on surface
    
def blocks_dodged(count):
    font = pygame.font.SysFont(None, 25)
    text = font.render('Dodged: ' +str(count), True, blue)
    game_display.blit(text, (0,0))

def blocks(blockx, blocky, blockw, blockh, color):
    pygame.draw.rect(game_display, color, [blockx, blocky, blockw, blockh])
    
def msg_display(text):
    textFont = pygame.font.Font('freesansbold.ttf', 50)
    textSurf, textRect = text_objects(text, textFont)
    textRect.center = ((display_width/2), (display_height/2))
    game_display.blit(textSurf, textRect)
    
    pygame.display.update()
    time.sleep(2)
    game()
    
def text_objects(text, font):
    textSurface = font.render(text, True, red)
    return textSurface, textSurface.get_rect()
    
def crash():
    msg_display('Oops You Crashed!')
    
def game():    
    x = (display_width * 0.45)
    y = (display_height * 0.7)
    
    x_change = 0
    
    block_startx = random.randrange(0, display_width)
    block_starty = -600
    block_speed = 7
    block_width = 100
    block_height = 100
    
    game_exit = False
    dodged = 0
    
    while not game_exit:
        for event in pygame.event.get():            #event per frames/sec
            if event.type == pygame.QUIT:
                pygame.quit()
                quit()
                
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    x_change = -5
                elif event.key == pygame.K_RIGHT:
                    x_change = 5
                    
            if event.type == pygame.KEYUP:
                if event.key == pygame.K_LEFT or event.key == pygame.K_RIGHT:
                    x_change = 0
                                        
        x += x_change
                
        game_display.fill(white)   
        
        #blocks(blockx, blocky, blockw, blockh, color)
        blocks(block_startx, block_starty, block_width, block_height, black)
        block_starty += block_speed
        car(x,y)  
        blocks_dodged(dodged)
        
        
        if x > display_width - car_width or x < 0:
            crash()
            
        if block_starty > display_height:
            block_starty = 0 - block_height
            block_startx = random.randrange(0, display_width)
            dodged += 1
            
            
        if y < block_starty + block_height:
            #print ('y crossover')
            
            if x > block_startx and x < block_startx + block_width or x+car_width > block_startx and x+car_width < block_startx + block_width:
                #print ('x crossover')
                crash()
        
        pygame.display.update()     
        clock.tick(60)                        #60 frames/sec
 
game()
pygame.quit()
quit()    
