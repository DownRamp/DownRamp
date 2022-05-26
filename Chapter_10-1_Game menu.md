# Game menu


---

## Description: 
A game menu that can be used for anything else

. Will send user to game.

---

## Why: 
Reuseable 

---

## Setup Instructions: 
1. Python3

---

## To do: 
1. Create buttons
2. set background
3. functions for sending to game or other code

---

## Code: 
###!/usr/bin/python3.4


### Setup Python ----------------------------------------------- ###


>import pygame, sys


>import game





### Setup pygame/window ---------------------------------------- ###


mainClock = pygame.time.Clock()


>from pygame.locals import *





pygame.init()


pygame.display.set_caption('Fight Knight')


screen = pygame.display.set_mode((800, 550), 0, 32)


background_img = pygame.image.load('img/Background/background.png').convert_alpha()





font = pygame.font.SysFont(None, 20)





def draw_bg():


    screen.blit(background_img, (0, 0))





def draw_text(text, font, color, surface, x, y):


    textobj = font.render(text, 1, color)


    textrect = textobj.get_rect()


    textrect.topleft = (x, y)


    surface.blit(textobj, textrect)








click = False








def main_menu():


    while True:


        draw_bg()


        draw_text('MAIN MENU', font, (255, 255, 255), screen, 20, 20)





        mx, my = pygame.mouse.get_pos()





        button_1 = pygame.Rect(50, 100, 200, 50)





        button_2 = pygame.Rect(50, 200, 200, 50)





        button_3 = pygame.Rect(50, 300, 200, 50)





        if button_1.collidepoint((mx, my)):


            if click:


                game.Control().mainloop()


        if button_2.collidepoint((mx, my)):


            if click:


                options()


        pygame.draw.rect(screen, (51, 51, 0), button_1)


        screen.blit(font.render('Start', True, (255,0,0)), (100, 120))


        pygame.draw.rect(screen, (51, 51, 0), button_2)


        screen.blit(font.render('Options', True, (255,0,0)), (100, 220))


        pygame.draw.rect(screen, (51, 51, 0), button_3)


        screen.blit(font.render('Character select', True, (255,0,0)), (100, 320))





        click = False





        for event in pygame.event.get():


            if event.type == QUIT:


                pygame.quit()


                sys.exit()


            if event.type == KEYDOWN:


                if event.key == K_ESCAPE:


                    pygame.quit()


                    sys.exit()


            if event.type == MOUSEBUTTONDOWN:


                if event.button == 1:


                    click = True





        pygame.display.update()


        mainClock.tick(60)





def options():


    running = True


    while running:


        screen.fill((0, 0, 0))





        draw_text('options', font, (255, 255, 255), screen, 20, 20)


        for event in pygame.event.get():


            if event.type == QUIT:


                pygame.quit()


                sys.exit()


            if event.type == KEYDOWN:


                if event.key == K_ESCAPE:


                    running = False





        pygame.display.update()


        mainClock.tick(60)








main_menu()


---

## Translation: 
###not /usr/bin/python3 4


### Setup Python ----------------------------------------------- ###


>Import package called  pygame, sys


>Import package called  game





### Setup pygame/window ---------------------------------------- ###


mainClock variable equal to  pygame time Clock function call with parameters: ""


>from pygame locals Import package called  * (means grab everything) 





pygame initiate 


pygame display set_caption function call with parameters: "'Fight Knight'"


screen variable equal to  pygame display set_mode function call with parameters: " function call with parameters: "800, 550", 0, 32"


background_img variable equal to  pygame image load function call with parameters: "'img/Background/background png'" convert_alpha function call with parameters: ""





font variable equal to  pygame font SysFont function call with parameters: "None, 20"





Define a function called  draw_bg function call with parameters: "":


    screen blit function call with parameters: "background_img,  function call with parameters: "0, 0""





Define a function called  draw_text function call with parameters: "text, font, color, surface, x, y":


    textobj variable equal to  font render function call with parameters: "text, 1, color"


    textrect variable equal to  textobj get_rect function call with parameters: ""


    textrect topleft variable equal to   function call with parameters: "x, y"


    surface blit function call with parameters: "textobj, textrect"








click variable equal to  False








Define a function called  main_menu function call with parameters: "":


    while True:


        draw_bg function call with parameters: ""


        draw_text function call with parameters: "'MAIN MENU', font,  function call with parameters: "255, 255, 255", screen, 20, 20"





        mx, my variable equal to  pygame mouse get_pos function call with parameters: ""





        button_1 variable equal to  pygame Rect function call with parameters: "50, 100, 200, 50"





        button_2 variable equal to  pygame Rect function call with parameters: "50, 200, 200, 50"





        button_3 variable equal to  pygame Rect function call with parameters: "50, 300, 200, 50"





        if button_1 collidepoint function call with parameters: " function call with parameters: "mx, my"":


            if click:


                game Control function call with parameters: "" mainloop function call with parameters: ""


        if button_2 collidepoint function call with parameters: " function call with parameters: "mx, my"":


            if click:


                options function call with parameters: ""


        pygame draw rect function call with parameters: "screen,  function call with parameters: "51, 51, 0", button_1"


        screen blit function call with parameters: "font render function call with parameters: "'Start', True,  function call with parameters: "255,0,0"",  function call with parameters: "100, 120""


        pygame draw rect function call with parameters: "screen,  function call with parameters: "51, 51, 0", button_2"


        screen blit function call with parameters: "font render function call with parameters: "'Options', True,  function call with parameters: "255,0,0"",  function call with parameters: "100, 220""


        pygame draw rect function call with parameters: "screen,  function call with parameters: "51, 51, 0", button_3"


        screen blit function call with parameters: "font render function call with parameters: "'Character select', True,  function call with parameters: "255,0,0"",  function call with parameters: "100, 320""





        click variable equal to  False





        for event in pygame event get function call with parameters: "":


            if event type variable equal to variable equal to  QUIT:


                pygame quit function call with parameters: ""


                sys exit function call with parameters: ""


            if event type variable equal to variable equal to  KEYDOWN:


                if event key variable equal to variable equal to  K_ESCAPE:


                    pygame quit function call with parameters: ""


                    sys exit function call with parameters: ""


            if event type variable equal to variable equal to  MOUSEBUTTONDOWN:


                if event button variable equal to variable equal to  1:


                    click variable equal to  True





        pygame display update function call with parameters: ""


        mainClock tick function call with parameters: "60"





Define a function called  options function call with parameters: "":


    running variable equal to  True


    while running:


        screen fill function call with parameters: " function call with parameters: "0, 0, 0""





        draw_text function call with parameters: "'options', font,  function call with parameters: "255, 255, 255", screen, 20, 20"


        for event in pygame event get function call with parameters: "":


            if event type variable equal to variable equal to  QUIT:


                pygame quit function call with parameters: ""


                sys exit function call with parameters: ""


            if event type variable equal to variable equal to  KEYDOWN:


                if event key variable equal to variable equal to  K_ESCAPE:


                    running variable equal to  False





        pygame display update function call with parameters: ""


        mainClock tick function call with parameters: "60"








main_menu function call with parameters: ""

[Github link] https://github.com/DownRamp/Games/blob/main/final_game/menu.py
## Next Steps: 
1. Add reporting function
2. add background music
3. add character roster

---

