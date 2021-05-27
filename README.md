# firstproject
import pygame
import sys
import time
import random

restart=1

check_error=pygame.init()

if check_error[1]>0:
    print("found{0}an error".formate(check_error))
    sys.exit(-1)
else:
    print("game is successfully executed!")

#surface
playsurface=pygame.display.set_mode((720,460))
pygame.display.set_caption("snake game")

#colors
red=(255,0,0)
green=(0,255,0)
blue=(0,0,255)
white=(255,255,255)
black=(0,0,0)
 
#fps frame per second controller . page will refresh each and every second
fpscontroller = pygame.time.Clock()

#important variable
snakepos=[100,50]
snakebody=[[100,50],[90,50],[80,50]]

#foodspawn of snake
foodspawn=True
foodpos=[random.randrange(1,72)*10,random.randrange(1,46)*10]

#direction of snake when it starts
direction="RIGHT"
change_to=direction
 
score=0

#display the greetings
playsurface.fill(white) #filling the window with white color
pfont=pygame.font.SysFont('monoco',35)
psurf=pfont.render('Snake game',True,blue)
prect=psurf.get_rect()#it gives a small rectangularblock which is used to print the snake game greetings
prect.midtop=(360,200)
playsurface.blit(psurf,prect)
pygame.display.flip()  #used to refresh the screen
time.sleep(3)    #declering how long the snake game logo will appear in the screen


def gameover():
    gfont=pygame.font.SysFont('monoca',35)
    gsurf=gfont.render('GAME OVER',True,red)
    grect=gsurf.get_rect()
    grect.midtop=(360,25)
    playsurface.blit(gsurf,grect)
    showscore(0)
    pygame.display.flip()# refreshes the screen
    time.sleep(2)
    pygame.quit()
    sys.exit()


    #showscore
def showscore(choice=1):
    sfont=pygame.font.SysFont('monoco',24)
    sffont=pygame.font.SysFont('monoco',42)
    ssurf=sfont.render('score: {0}'.format(score),True,black)
    srect=ssurf.get_rect()
    if choice==1:  
        srect.midtop=(40,10)
    else:
        ssurf=sfont.render('score : {0}'.format(score),True,black)
        srect.midtop=(340,100)

    playsurface.blit(ssurf,srect)



#main logic of the game
while True:
    for event in pygame.event.get():
        if event.type ==pygame.QUIT:
            pygame.quit()
            sys.exit()
        elif event.type==pygame.KEYDOWN:
            if event.key==pygame.K_RIGHT :#or event.type == ord('d'):
                change_to='RIGHT'
            if event.key==pygame.K_LEFT :#or  event.type == ord('a'):
                change_to='LEFT'
            if event.key==pygame.K_UP :#or event.type == ord('w'):
                change_to='UP'
            if event.key==pygame.K_DOWN :#or event.type==ord('s'):
                change_to='DOWN'
            if event.key==pygame.K_ESCAPE:
                pygame.event.post(pygame.event.Event(pygame.QUIT))

         #validating the snake's direction
    if change_to=='RIGHT' and not direction=='LEFT':
        direction='RIGHT'
    if change_to=='LEFT' and not direction=='RIGHT':
        direction='LEFT'
    if change_to=='UP' and not direction=='DOWN':
        direction='UP'
    if change_to=='DOWN' and not direction=='UP':
        direction='DOWN'

              #move on directions
    if direction=='RIGHT':
        snakepos[0]+=10
    if direction=='LEFT':
        snakepos[0]-=10
    if direction[1]=='UP':
        snakepos-=10
    if direction[1]=='DOWN':
        snakepos+=10


    #snake mechanism
    snakebody.insert(0,list(snakepos))
    if snakepos[0]==foodpos[0] and snakepos[1]==foodpos[1]:
        score+=1
        foodspawn=False
    else:
        snakebody.pop()
    if foodspawn==False:
        foodpos=[random.randrange(1,72)*10,random.randrange(1,46)*10]
    foodspawn=True

    #drawings
    playsurface.fill(white)

    for pos in snakebody:
        pygame.draw.rect(playsurface, green,pygame.Rect(pos[0],pos[1],10,10))

    pygame.draw.rect(playsurface,blue,pygame.Rect(foodpos[0],foodpos[1],10,10))

    if snakepos[0]>710 or snakepos[0]<0:
        gameover()
    if snakepos[1]>450 or snakepos[1]<0:
        gameover()

    for block in snakebody[1:]:
        if snakepos[0]==block[0] and snakepos[1]==block[1]:
            gameover()

    showscore()
    pygame.display.flip()
    fpscontroller.tick(20)
