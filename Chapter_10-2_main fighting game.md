# main fighting game

---

## Description: 
A large pygame to show moving characters with physics attached to it

---

## Why: 
Gives you a start to a real game

---

## Setup Instructions: 
1. python3
2.  run 

---

## To do: 
1. Show characters
2.  add background
3.  move charaters
4.  make characters jump
5.  Add fall function
6.  add collisions to borders
7.  add lives
8.  add health
9.  add damage
10.  remove one life when you leave boundaries
11.  flip image when turning
12.  show different animations

---

## Code: 
>import pygame


>import random


>import math





pygame.init()





clock = pygame.time.Clock()


fps = 60





### game window


bottom_panel = 150


screen_width = 800


screen_height = 400 + bottom_panel


move_size = 20


jump_size = 50


screen = pygame.display.set_mode((screen_width, screen_height))


pygame.display.set_caption('Fight Knight')


### define colours


red = (255, 0, 0)


green = (0, 255, 0)


gameover, player1_dead, player2_dead = False, False, False


### define fonts


font = pygame.font.SysFont('Times New Roman', 26)








class _Physics(object):


    def __init__(self):


        self.velocity = [0, 0]


        self.grav = 0.4


        self.fall = False





    def physics_update(self):


        if self.fall:


            self.velocity[1] += self.grav


        else:


            self.velocity[1] = 0








class Fighter(_Physics, pygame.sprite.Sprite):


    def __init__(self, x, y, name, max_hp, lives, jumps, speed):


        _Physics.__init__(self)


        pygame.sprite.Sprite.__init__(self)


        self.strength = 10


        self.name = name


        self.max_hp = max_hp


        self.hp = max_hp


        self.alive = True


        self.animation_list = []


        self.frame_index = 0


        self.action = 0###0:idle, 1:attack, 2:hurt, 3:dead


        self.update_time = pygame.time.get_ticks()


        self.flip = False


        self.frame_start_pos = None


        self.total_displacement = None


        self.speed = speed


        self.jump_power = -9.0


        self.jump_cut_magnitude = -3.0


        self.on_moving = False


        self.collide_below = False


        self.attack_animation = False


        self.lives = lives


        self.jumps = jumps





        if(self.name == "Knight"):


            self.dir = "right"


        else:


            self.dir = "left"


        ###load idle images


        temp_list = []


        for i in range(8):


            img = pygame.image.load(f'img/{self.name}/Idle/{i}.png')


            img = pygame.transform.scale(img, (img.get_width() * 3, img.get_height() * 3))


            temp_list.append(img)


        self.animation_list.append(temp_list)


        ###load attack images


        temp_list = []


        for i in range(8):


            img = pygame.image.load(f'img/{self.name}/Attack/{i}.png')


            img = pygame.transform.scale(img, (img.get_width() * 3, img.get_height() * 3))


            temp_list.append(img)


        self.animation_list.append(temp_list)


        ###load hurt images


        temp_list = []


        for i in range(3):


            img = pygame.image.load(f'img/{self.name}/Hurt/{i}.png')


            img = pygame.transform.scale(img, (img.get_width() * 3, img.get_height() * 3))


            temp_list.append(img)


        self.animation_list.append(temp_list)


        ###load death images


        temp_list = []


        for i in range(10):


            img = pygame.image.load(f'img/{self.name}/Death/{i}.png')


            img = pygame.transform.scale(img, (img.get_width() * 3, img.get_height() * 3))


            temp_list.append(img)


        self.animation_list.append(temp_list)


        self.image = self.animation_list[self.action][self.frame_index]


        self.rect = self.image.get_rect()


        self.rect.center = (x, y)


        self.x = x


        self.y = y





    def check_keys(self, keys):


        self.velocity[0] = 0


        if self.name == "Knight":


            if keys[pygame.K_a]:


                if self.dir == "right":


                    self.dir = "left"


                    self.flip = not self.flip


                self.velocity[0] -= self.speed


            if keys[pygame.K_d]:


                if self.dir == "left":


                    self.dir = "right"


                    self.flip = not self.flip


                self.velocity[0] += self.speed


        else:


            if keys[pygame.K_LEFT]:


                if self.dir == "right":


                    self.dir = "left"


                    self.flip = not self.flip


                self.velocity[0] -= self.speed


            if keys[pygame.K_RIGHT]:


                if self.dir == "left":


                    self.dir = "right"


                    self.flip = not self.flip


                self.velocity[0] += self.speed





    def get_position(self, obstacles):


        if not self.fall:


            self.check_falling(obstacles)


        else:


            self.fall = self.check_collisions((0,self.velocity[1]),1,obstacles)


        if self.velocity[0]:


            self.check_collisions((self.velocity[0],0), 0, obstacles)





    def check_falling(self, obstacles):


        if not self.collide_below:


            self.fall = True


            self.on_moving = False





    def check_moving(self, obstacles):


        if not self.fall:


            now_moving = self.on_moving


            any_moving, any_non_moving = [], []


            for collide in self.collide_below:


                if collide.type == "moving":


                    self.on_moving = collide


                    any_moving.append(collide)


                else:


                    any_non_moving.append(collide)


            if not any_moving:


                self.on_moving = False


            elif any_non_moving or now_moving in any_moving:


                self.on_moving = now_moving





    def check_collisions(self, offset, index, obstacles):


        unaltered = True


        self.rect[index] += offset[index]


        while pygame.sprite.spritecollideany(self, obstacles):


            self.rect[index] += (1 if offset[index]<0 else -1)


            unaltered = False


        return unaltered





    def check_above(self, obstacles):


        self.rect.move_ip(0, -1)


        collide = pygame.sprite.spritecollideany(self, obstacles)


        self.rect.move_ip(0, 1)


        return collide





    def check_below(self, obstacles):


        self.rect.move_ip((0,1))


        collide = pygame.sprite.spritecollide(self, obstacles, False)


        self.rect.move_ip((0,-1))


        return collide





    def jump(self, obstacles):


        if not self.fall and not self.check_above(obstacles):


            self.velocity[1] = self.jump_power


            self.fall = True


            self.on_moving = False





    def jump_cut(self):


        if self.fall:


            if self.velocity[1] < self.jump_cut_magnitude:


                self.velocity[1] = self.jump_cut_magnitude





    def pre_update(self, obstacles):


        self.frame_start_pos = self.rect.topleft


        self.collide_below = self.check_below(obstacles)


        self.check_moving(obstacles)





    def update(self, obstacles, keys):


        global gameover, player1_dead, player2_dead


        animation_cooldown = 100


        self.check_keys(keys)


        self.get_position(obstacles)


        self.physics_update()


        start = self.frame_start_pos


        end = self.rect.topleft


        self.total_displacement = (end[0]-start[0], end[1]-start[1])


        if self.flip:


            self.image = pygame.transform.flip(self.animation_list[self.action][self.frame_index], True, False)


        else:


            self.image = self.animation_list[self.action][self.frame_index]


        ###handle animation


        ###check if enough time has passed since the last update


        if pygame.time.get_ticks() - self.update_time > animation_cooldown:


            self.update_time = pygame.time.get_ticks()


            self.frame_index += 1





        if self.rect.center[0] < 0 or self.rect.center[1] < 0 or \


                self.rect.center[0] > screen_width or self.rect.center[1] > screen_height:


            if self.lives >0:


                self.death()


                if self.lives <= 0:


                    self.alive = False


                    gameover = True


                    if self.name == "Knight":


                        player1_dead = True


                    else:


                        player2_dead = True


                else:


                    self.reset()





        ###if the animation has run out then reset back to the start


        if self.frame_index >= len(self.animation_list[self.action]):


            if self.action == 3:


                self.frame_index = len(self.animation_list[self.action]) - 1


                if self.lives <= 0:


                    self.alive = False


                    gameover = True


                    if self.name == "Knight":


                        player1_dead = True


                    else:


                        player2_dead = True


                else:


                    self.reset()


            else:


                self.idle()





    def idle(self):


        ###set variables to idle animation


        self.action = 0


        self.frame_index = 0


        self.update_time = pygame.time.get_ticks()





    def attack(self, target):


        dist = math.dist([target.rect.center[0], target.rect.center[1]], [self.rect.center[0], self.rect.center[1]])


        if dist < 100 and target.hp > 0:


            ###deal damage to enemy


            rand = random.randint(-5, 5)


            damage = self.strength + rand


            target.hp -= damage


            ###run enemy hurt animation


            target.hurt()


            ###check if target has died


            if target.hp < 1:


                target.hp = 0


                target.alive = False


                target.death()


            ###set variables to attack animation


            self.action = 1


            self.frame_index = 0


            self.update_time = pygame.time.get_ticks()


        else:


            self.action = 1


            self.frame_index = 0


            self.update_time = pygame.time.get_ticks()





    def hurt(self):


        ###set variables to hurt animation


        if self.hp > 0 and self.alive:


            self.action = 2


            self.frame_index = 0


            self.update_time = pygame.time.get_ticks()





    def death(self):


        ###set variables to death animation


        self.alive = False


        self.action = 3


        self.hp = 0


        self.frame_index = 0


        self.update_time = pygame.time.get_ticks()


        self.lives -=1





    def reset(self):


        if self.lives >0:


            self.alive = True


            self.hp = self.max_hp


        self.frame_index = 0


        self.action = 0


        self.update_time = pygame.time.get_ticks()


        self.rect.center = (self.x, self.y)





    def draw(self):


        screen.blit(self.image, self.rect)








class Block(pygame.sprite.Sprite):


    """A class representing solid obstacles."""


    def __init__(self, color, rect):


        """The color is an (r,g,b) tuple; rect is a rect-style argument."""


        pygame.sprite.Sprite.__init__(self)


        self.rect = pygame.Rect(rect)


        self.image = pygame.Surface(self.rect.size).convert()


        self.image.fill(color)


        self.type = "normal"








class HealthBar:


    def __init__(self, x, y, hp, max_hp):


        self.x = x


        self.y = y


        self.hp = hp


        self.max_hp = max_hp





    def draw(self, hp):


        ###update with new health


        self.hp = hp


        ###calculate health ratio


        ratio = self.hp / self.max_hp


        pygame.draw.rect(screen, red, (self.x, self.y, 150, 20))


        pygame.draw.rect(screen, green, (self.x, self.y, 150 * ratio, 20))








class DamageText(pygame.sprite.Sprite):


    def __init__(self, x, y, damage, colour):


        pygame.sprite.Sprite.__init__(self)


        self.image = font.render(damage, True, colour)


        self.rect = self.image.get_rect()


        self.rect.center = (x, y)


        self.counter = 0





    def update(self):


        ###move damage text up


        self.rect.y -= 1


        ###delete the text after a few seconds


        self.counter += 1


        if self.counter > 30:


            self.kill()








class Control(object):


    def __init__(self):


        self.done = False


        self.fps = 60.0


        ###load images


        ###background image


        self.background_img = pygame.image.load('img/Background/background.png').convert_alpha()


        ###panel image


        self.panel_img = pygame.image.load('img/Icons/panel.png').convert_alpha()


        ###load victory and defeat images


        self.obstacles = self.make_obstacles()


        self.level = pygame.Surface((screen_width, screen_height-bottom_panel-10)).convert()


        self.level_rect = self.level.get_rect()


        self.damage_text_group = pygame.sprite.Group()


        self.player1 = Fighter(200, 260, 'Knight', 100, 3, 1, 4)


        self.player2 = Fighter(550, 270, 'Bandit', 100, 3, 1, 4)


        self.player1_health_bar = HealthBar(100, screen_height - bottom_panel + 40, self.player1.hp, self.player1.max_hp)


        self.player2_health_bar = HealthBar(500, screen_height - bottom_panel + 40, self.player2.hp, self.player2.max_hp)


        self.keys = pygame.key.get_pressed()


        self.viewport = screen.get_rect(bottom=self.level_rect.bottom)





    def update_viewport(self, speed):


        for i in (0, 1):


            first_third = self.viewport[i] + self.viewport.size[i] // 3


            second_third = first_third + self.viewport.size[i] // 3


            player_center = self.player1.rect.center[i]


            mult = 0


            if speed[i] > 0 and player_center >= first_third:


                mult = 0.5 if player_center < self.viewport.center[i] else 1


            elif speed[i] < 0 and player_center <= second_third:


                mult = 0.5 if player_center > self.viewport.center[i] else 1


            self.viewport[i] += mult * speed[i]


        self.viewport.clamp_ip(self.level_rect)





    def draw_text(self, text, font, text_col, x, y):


        img = font.render(text, True, text_col)


        screen.blit(img, (x, y))





    def draw_bg(self):


        screen.blit(self.background_img, (0, 0))





    def make_obstacles(self):


        floor = Block(pygame.Color("darkgreen"), (100, screen_height - bottom_panel, 890, 10))


        static = []


        ###static.append(Block(pygame.Color("darkgreen"), (100, 100, 200, 25)))


        ###static.append(Block(pygame.Color("darkgreen"), (700, 100, 200, 25)))


        ###static.append(Block(pygame.Color("darkgreen"), (400, 50, 200, 25)))


        ###static.append(Block(pygame.Color("darkgreen"), (400, 200, 200, 25)))


        return pygame.sprite.Group(floor, static)





    def draw_panel(self, player1, player2):


        global gameover, player1_dead, player2_dead


        ###draw panel rectangle


        screen.blit(self.panel_img, (0, screen_height - bottom_panel))


        ###show knight stats


        self.draw_text(f'{player1.name} HP: {player1.hp}', font, red, 100, screen_height - bottom_panel + 10)


        self.draw_text(f'{player2.name} HP: {player2.hp}', font, red, 500, screen_height - bottom_panel + 10)


        self.draw_text(f'Lives: {player1.lives}', font, red, 100, screen_height - bottom_panel + 70)


        self.draw_text(f'Lives: {player2.lives}', font, red, 500, screen_height - bottom_panel + 70)


        if gameover:


            if player1_dead:


                self.draw_text(f'Winner Player 2', font, green, 500, screen_height - bottom_panel + 100)


            if player2_dead:


                self.draw_text(f'Winner Player 1', font, green, 100, screen_height - bottom_panel + 100)





    def event_loop(self):


        for event in pygame.event.get():


            if event.type == pygame.QUIT or self.keys[pygame.K_ESCAPE]:


                self.done = True


            elif event.type == pygame.KEYDOWN:


                if event.key == pygame.K_LSHIFT:


                    self.player1.jump(self.obstacles)


                elif event.key == pygame.K_RSHIFT:


                    self.player2.jump(self.obstacles)


                elif event.key == pygame.K_f:


                    if self.player1.lives > 0 and self.player1.hp > 0:


                        self.player1.attack(self.player2)


                elif event.key == pygame.K_j:


                    if self.player2.lives > 0 and self.player2.hp > 0:


                        self.player2.attack(self.player1)


            elif event.type == pygame.KEYUP:


                if event.key == pygame.K_SPACE:


                    self.player1.jump_cut()


                elif event.key == pygame.K_f:


                    self.player1.jump_cut()





    def update(self):


        self.keys = pygame.key.get_pressed()


        self.player1.pre_update(self.obstacles)


        self.player2.pre_update(self.obstacles)





        self.obstacles.update(self.player1, self.obstacles)


        self.player1.update(self.obstacles, self.keys)


        self.obstacles.update(self.player2, self.obstacles)


        self.player2.update(self.obstacles, self.keys)





        self.update_viewport(self.player1.total_displacement)


        self.update_viewport(self.player2.total_displacement)





    def display_fps(self):


        caption = "{} - FPS: {:.2f}".format("FIGHT KNIGHT", clock.get_fps())


        pygame.display.set_caption(caption)





    def mainloop(self):





        while not self.done:


            clock.tick(fps)





            ###draw background


            self.draw_bg()


            ###draw panel


            self.draw_panel(self.player1, self.player2)


            self.player1_health_bar.draw(self.player1.hp)


            self.player2_health_bar.draw(self.player2.hp)


            self.obstacles.draw(self.level)





            ###draw fighters


            self.keys = pygame.key.get_pressed()


            self.player1.pre_update(self.obstacles)


            self.player2.pre_update(self.obstacles)





            self.player1.update(self.obstacles, self.keys)


            self.player1.draw()


            self.player2.update(self.obstacles, self.keys)


            self.player2.draw()





            ###control player actions


            ###reset action variables


            attack = False


            target = None


            self.event_loop()


            self.update()


            pygame.display.update()


            clock.tick(self.fps)


            self.display_fps()


        pygame.quit()








if __name__ == "__main__":


    Control().mainloop()



---

## Translation: 
>Import package called  pygame


>Import package called  random


>Import package called  math





pygame initiate





clock variable equal to  pygame time Clockfunction call with parameters: ""


fps variable equal to  60





### game window


bottom_panel variable equal to  150


screen_width variable equal to  800


screen_height variable equal to  400 + bottom_panel


move_size variable equal to  20


jump_size variable equal to  50


screen variable equal to  pygame display set_modefunction call with parameters: "function call with parameters: "screen_width, screen_height""


pygame display set_captionfunction call with parameters: "'Fight Knight'"


### Define a function called ine colours


red variable equal to  function call with parameters: "255, 0, 0"


green variable equal to  function call with parameters: "0, 255, 0"


gameover, player1_dead, player2_dead variable equal to  False, False, False


### Define a function called ine fonts


font variable equal to  pygame font SysFontfunction call with parameters: "'Times New Roman', 26"








class _Physicsfunction call with parameters: "object":


    Define a function called  initiate class


        self velocity variable equal to  [0, 0]


        self grav variable equal to  0 4


        self fall variable equal to  False





    Define a function called  physics_updatefunction call with parameters: "self":


        if self fall:


            self velocity[1] +variable equal to  self grav


        else:


            self velocity[1] variable equal to  0








class Fighterfunction call with parameters: "_Physics, pygame sprite Sprite":


    Define a function called  __init__function call with parameters: "self, x, y, name, max_hp, lives, jumps, speed":


        _Physics __init__function call with parameters: "self"


        pygame sprite Sprite __init__function call with parameters: "self"


        self strength variable equal to  10


        self name variable equal to  name


        self max_hp variable equal to  max_hp


        self hp variable equal to  max_hp


        self alive variable equal to  True


        self animation_list variable equal to  []


        self frame_index variable equal to  0


        self action variable equal to  0###0:idle, 1:attack, 2:hurt, 3:dead


        self update_time variable equal to  pygame time get_ticksfunction call with parameters: ""


        self flip variable equal to  False


        self frame_start_pos variable equal to  None


        self total_displacement variable equal to  None


        self speed variable equal to  speed


        self jump_power variable equal to  -9 0


        self jump_cut_magnitude variable equal to  -3 0


        self on_moving variable equal to  False


        self collide_below variable equal to  False


        self attack_animation variable equal to  False


        self lives variable equal to  lives


        self jumps variable equal to  jumps





        iffunction call with parameters: "self name variable equal to variable equal to  "Knight"":


            self dir variable equal to  "right"


        else:


            self dir variable equal to  "left"


        ###load idle images


        temp_list variable equal to  []


        for i in rangefunction call with parameters: "8":


            img variable equal to  pygame image loadfunction call with parameters: "f'img/{self name}/Idle/{i} png'"


            img variable equal to  pygame transform scalefunction call with parameters: "img, function call with parameters: "img get_widthfunction call with parameters: "" * (means grab everything)  3, img get_heightfunction call with parameters: "" * (means grab everything)  3""


            temp_list appendfunction call with parameters: "img"


        self animation_list appendfunction call with parameters: "temp_list"


        ###load attack images


        temp_list variable equal to  []


        for i in rangefunction call with parameters: "8":


            img variable equal to  pygame image loadfunction call with parameters: "f'img/{self name}/Attack/{i} png'"


            img variable equal to  pygame transform scalefunction call with parameters: "img, function call with parameters: "img get_widthfunction call with parameters: "" * (means grab everything)  3, img get_heightfunction call with parameters: "" * (means grab everything)  3""


            temp_list appendfunction call with parameters: "img"


        self animation_list appendfunction call with parameters: "temp_list"


        ###load hurt images


        temp_list variable equal to  []


        for i in rangefunction call with parameters: "3":


            img variable equal to  pygame image loadfunction call with parameters: "f'img/{self name}/Hurt/{i} png'"


            img variable equal to  pygame transform scalefunction call with parameters: "img, function call with parameters: "img get_widthfunction call with parameters: "" * (means grab everything)  3, img get_heightfunction call with parameters: "" * (means grab everything)  3""


            temp_list appendfunction call with parameters: "img"


        self animation_list appendfunction call with parameters: "temp_list"


        ###load death images


        temp_list variable equal to  []


        for i in rangefunction call with parameters: "10":


            img variable equal to  pygame image loadfunction call with parameters: "f'img/{self name}/Death/{i} png'"


            img variable equal to  pygame transform scalefunction call with parameters: "img, function call with parameters: "img get_widthfunction call with parameters: "" * (means grab everything)  3, img get_heightfunction call with parameters: "" * (means grab everything)  3""


            temp_list appendfunction call with parameters: "img"


        self animation_list appendfunction call with parameters: "temp_list"


        self image variable equal to  self animation_list[self action][self frame_index]


        self rect variable equal to  self image get_rectfunction call with parameters: ""


        self rect center variable equal to  function call with parameters: "x, y"


        self x variable equal to  x


        self y variable equal to  y





    Define a function called  check_keysfunction call with parameters: "self, keys":


        self velocity[0] variable equal to  0


        if self name variable equal to variable equal to  "Knight":


            if keys[pygame K_a]:


                if self dir variable equal to variable equal to  "right":


                    self dir variable equal to  "left"


                    self flip variable equal to  not self flip


                self velocity[0] -variable equal to  self speed


            if keys[pygame K_d]:


                if self dir variable equal to variable equal to  "left":


                    self dir variable equal to  "right"


                    self flip variable equal to  not self flip


                self velocity[0] +variable equal to  self speed


        else:


            if keys[pygame K_LEFT]:


                if self dir variable equal to variable equal to  "right":


                    self dir variable equal to  "left"


                    self flip variable equal to  not self flip


                self velocity[0] -variable equal to  self speed


            if keys[pygame K_RIGHT]:


                if self dir variable equal to variable equal to  "left":


                    self dir variable equal to  "right"


                    self flip variable equal to  not self flip


                self velocity[0] +variable equal to  self speed





    Define a function called  get_positionfunction call with parameters: "self, obstacles":


        if not self fall:


            self check_fallingfunction call with parameters: "obstacles"


        else:


            self fall variable equal to  self check_collisionsfunction call with parameters: "function call with parameters: "0,self velocity[1]",1,obstacles"


        if self velocity[0]:


            self check_collisionsfunction call with parameters: "function call with parameters: "self velocity[0],0", 0, obstacles"





    Define a function called  check_fallingfunction call with parameters: "self, obstacles":


        if not self collide_below:


            self fall variable equal to  True


            self on_moving variable equal to  False





    Define a function called  check_movingfunction call with parameters: "self, obstacles":


        if not self fall:


            now_moving variable equal to  self on_moving


            any_moving, any_non_moving variable equal to  [], []


            for collide in self collide_below:


                if collide type variable equal to variable equal to  "moving":


                    self on_moving variable equal to  collide


                    any_moving appendfunction call with parameters: "collide"


                else:


                    any_non_moving appendfunction call with parameters: "collide"


            if not any_moving:


                self on_moving variable equal to  False


            elif any_non_moving or now_moving in any_moving:


                self on_moving variable equal to  now_moving





    Define a function called  check_collisionsfunction call with parameters: "self, offset, index, obstacles":


        unaltered variable equal to  True


        self rect[index] +variable equal to  offset[index]


        while pygame sprite spritecollideanyfunction call with parameters: "self, obstacles":


            self rect[index] +variable equal to  function call with parameters: "1 if offset[index]<0 else -1"


            unaltered variable equal to  False


        return unaltered





    Define a function called  check_abovefunction call with parameters: "self, obstacles":


        self rect move_ipfunction call with parameters: "0, -1"


        collide variable equal to  pygame sprite spritecollideanyfunction call with parameters: "self, obstacles"


        self rect move_ipfunction call with parameters: "0, 1"


        return collide





    Define a function called  check_belowfunction call with parameters: "self, obstacles":


        self rect move_ipfunction call with parameters: "function call with parameters: "0,1""


        collide variable equal to  pygame sprite spritecollidefunction call with parameters: "self, obstacles, False"


        self rect move_ipfunction call with parameters: "function call with parameters: "0,-1""


        return collide





    Define a function called  jumpfunction call with parameters: "self, obstacles":


        if not self fall and not self check_abovefunction call with parameters: "obstacles":


            self velocity[1] variable equal to  self jump_power


            self fall variable equal to  True


            self on_moving variable equal to  False





    Define a function called  jump_cutfunction call with parameters: "self":


        if self fall:


            if self velocity[1] < self jump_cut_magnitude:


                self velocity[1] variable equal to  self jump_cut_magnitude





    Define a function called  pre_updatefunction call with parameters: "self, obstacles":


        self frame_start_pos variable equal to  self rect topleft


        self collide_below variable equal to  self check_belowfunction call with parameters: "obstacles"


        self check_movingfunction call with parameters: "obstacles"





    Define a function called  updatefunction call with parameters: "self, obstacles, keys":


        fetch global variable  gameover, player1_dead, player2_dead


        animation_cooldown variable equal to  100


        self check_keysfunction call with parameters: "keys"


        self get_positionfunction call with parameters: "obstacles"


        self physics_updatefunction call with parameters: ""


        start variable equal to  self frame_start_pos


        end variable equal to  self rect topleft


        self total_displacement variable equal to  function call with parameters: "end[0]-start[0], end[1]-start[1]"


        if self flip:


            self image variable equal to  pygame transform flipfunction call with parameters: "self animation_list[self action][self frame_index], True, False"


        else:


            self image variable equal to  self animation_list[self action][self frame_index]


        ###handle animation


        ###check if enough time has passed since the last update


        if pygame time get_ticksfunction call with parameters: "" - self update_time > animation_cooldown:


            self update_time variable equal to  pygame time get_ticksfunction call with parameters: ""


            self frame_index +variable equal to  1





        if self rect center[0] < 0 or self rect center[1] < 0 or \


                self rect center[0] > screen_width or self rect center[1] > screen_height:


            if self lives >0:


                self deathfunction call with parameters: ""


                if self lives <variable equal to  0:


                    self alive variable equal to  False


                    gameover variable equal to  True


                    if self name variable equal to variable equal to  "Knight":


                        player1_dead variable equal to  True


                    else:


                        player2_dead variable equal to  True


                else:


                    self resetfunction call with parameters: ""





        ###if the animation has run out then reset back to the start


        if self frame_index >variable equal to  lenfunction call with parameters: "self animation_list[self action]":


            if self action variable equal to variable equal to  3:


                self frame_index variable equal to  lenfunction call with parameters: "self animation_list[self action]" - 1


                if self lives <variable equal to  0:


                    self alive variable equal to  False


                    gameover variable equal to  True


                    if self name variable equal to variable equal to  "Knight":


                        player1_dead variable equal to  True


                    else:


                        player2_dead variable equal to  True


                else:


                    self resetfunction call with parameters: ""


            else:


                self idlefunction call with parameters: ""





    Define a function called  idlefunction call with parameters: "self":


        ###set variables to idle animation


        self action variable equal to  0


        self frame_index variable equal to  0


        self update_time variable equal to  pygame time get_ticksfunction call with parameters: ""





    Define a function called  attackfunction call with parameters: "self, target":


        dist variable equal to  math distfunction call with parameters: "[target rect center[0], target rect center[1]], [self rect center[0], self rect center[1]]"


        if dist < 100 and target hp > 0:


            ###deal damage to enemy


            rand variable equal to  random randintfunction call with parameters: "-5, 5"


            damage variable equal to  self strength + rand


            target hp -variable equal to  damage


            ###run enemy hurt animation


            target hurtfunction call with parameters: ""


            ###check if target has died


            if target hp < 1:


                target hp variable equal to  0


                target alive variable equal to  False


                target deathfunction call with parameters: ""


            ###set variables to attack animation


            self action variable equal to  1


            self frame_index variable equal to  0


            self update_time variable equal to  pygame time get_ticksfunction call with parameters: ""


        else:


            self action variable equal to  1


            self frame_index variable equal to  0


            self update_time variable equal to  pygame time get_ticksfunction call with parameters: ""





    Define a function called  hurtfunction call with parameters: "self":


        ###set variables to hurt animation


        if self hp > 0 and self alive:


            self action variable equal to  2


            self frame_index variable equal to  0


            self update_time variable equal to  pygame time get_ticksfunction call with parameters: ""





    Define a function called  deathfunction call with parameters: "self":


        ###set variables to death animation


        self alive variable equal to  False


        self action variable equal to  3


        self hp variable equal to  0


        self frame_index variable equal to  0


        self update_time variable equal to  pygame time get_ticksfunction call with parameters: ""


        self lives -variable equal to 1





    Define a function called  resetfunction call with parameters: "self":


        if self lives >0:


            self alive variable equal to  True


            self hp variable equal to  self max_hp


        self frame_index variable equal to  0


        self action variable equal to  0


        self update_time variable equal to  pygame time get_ticksfunction call with parameters: ""


        self rect center variable equal to  function call with parameters: "self x, self y"





    Define a function called  drawfunction call with parameters: "self":


        screen blitfunction call with parameters: "self image, self rect"








class Blockfunction call with parameters: "pygame sprite Sprite":


    """A class representing solid obstacles """


    Define a function called  __init__function call with parameters: "self, color, rect":


        """The color is an function call with parameters: "r,g,b" tuple; rect is a rect-style argument """


        pygame sprite Sprite __init__function call with parameters: "self"


        self rect variable equal to  pygame Rectfunction call with parameters: "rect"


        self image variable equal to  pygame Surfacefunction call with parameters: "self rect size" convertfunction call with parameters: ""


        self image fillfunction call with parameters: "color"


        self type variable equal to  "normal"








class HealthBar:


    Define a function called  __init__function call with parameters: "self, x, y, hp, max_hp":


        self x variable equal to  x


        self y variable equal to  y


        self hp variable equal to  hp


        self max_hp variable equal to  max_hp





    Define a function called  drawfunction call with parameters: "self, hp":


        ###update with new health


        self hp variable equal to  hp


        ###calculate health ratio


        ratio variable equal to  self hp / self max_hp


        pygame draw rectfunction call with parameters: "screen, red, function call with parameters: "self x, self y, 150, 20""


        pygame draw rectfunction call with parameters: "screen, green, function call with parameters: "self x, self y, 150 * (means grab everything)  ratio, 20""








class DamageTextfunction call with parameters: "pygame sprite Sprite":


    Define a function called  __init__function call with parameters: "self, x, y, damage, colour":


        pygame sprite Sprite __init__function call with parameters: "self"


        self image variable equal to  font renderfunction call with parameters: "damage, True, colour"


        self rect variable equal to  self image get_rectfunction call with parameters: ""


        self rect center variable equal to  function call with parameters: "x, y"


        self counter variable equal to  0





    Define a function called  updatefunction call with parameters: "self":


        ###move damage text up


        self rect y -variable equal to  1


        ###delete the text after a few seconds


        self counter +variable equal to  1


        if self counter > 30:


            self killfunction call with parameters: ""








class Controlfunction call with parameters: "object":


    Define a function called  initiate class


        self done variable equal to  False


        self fps variable equal to  60 0


        ###load images


        ###background image


        self background_img variable equal to  pygame image loadfunction call with parameters: "'img/Background/background png'" convert_alphafunction call with parameters: ""


        ###panel image


        self panel_img variable equal to  pygame image loadfunction call with parameters: "'img/Icons/panel png'" convert_alphafunction call with parameters: ""


        ###load victory and Define a function called eat images


        self obstacles variable equal to  self make_obstaclesfunction call with parameters: ""


        self level variable equal to  pygame Surfacefunction call with parameters: "function call with parameters: "screen_width, screen_height-bottom_panel-10"" convertfunction call with parameters: ""


        self level_rect variable equal to  self level get_rectfunction call with parameters: ""


        self damage_text_group variable equal to  pygame sprite Groupfunction call with parameters: ""


        self player1 variable equal to  Fighterfunction call with parameters: "200, 260, 'Knight', 100, 3, 1, 4"


        self player2 variable equal to  Fighterfunction call with parameters: "550, 270, 'Bandit', 100, 3, 1, 4"


        self player1_health_bar variable equal to  HealthBarfunction call with parameters: "100, screen_height - bottom_panel + 40, self player1 hp, self player1 max_hp"


        self player2_health_bar variable equal to  HealthBarfunction call with parameters: "500, screen_height - bottom_panel + 40, self player2 hp, self player2 max_hp"


        self keys variable equal to  pygame key get_pressedfunction call with parameters: ""


        self viewport variable equal to  screen get_rectfunction call with parameters: "bottomvariable equal to self level_rect bottom"





    Define a function called  update_viewportfunction call with parameters: "self, speed":


        for i in function call with parameters: "0, 1":


            first_third variable equal to  self viewport[i] + self viewport size[i] // 3


            second_third variable equal to  first_third + self viewport size[i] // 3


            player_center variable equal to  self player1 rect center[i]


            mult variable equal to  0


            if speed[i] > 0 and player_center >variable equal to  first_third:


                mult variable equal to  0 5 if player_center < self viewport center[i] else 1


            elif speed[i] < 0 and player_center <variable equal to  second_third:


                mult variable equal to  0 5 if player_center > self viewport center[i] else 1


            self viewport[i] +variable equal to  mult * (means grab everything)  speed[i]


        self viewport clamp_ipfunction call with parameters: "self level_rect"





    Define a function called  draw_textfunction call with parameters: "self, text, font, text_col, x, y":


        img variable equal to  font renderfunction call with parameters: "text, True, text_col"


        screen blitfunction call with parameters: "img, function call with parameters: "x, y""





    Define a function called  draw_bgfunction call with parameters: "self":


        screen blitfunction call with parameters: "self background_img, function call with parameters: "0, 0""





    Define a function called  make_obstaclesfunction call with parameters: "self":


        floor variable equal to  Blockfunction call with parameters: "pygame Colorfunction call with parameters: ""darkgreen"", function call with parameters: "100, screen_height - bottom_panel, 890, 10""


        static variable equal to  []


        ###static appendfunction call with parameters: "Blockfunction call with parameters: "pygame Colorfunction call with parameters: ""darkgreen"", function call with parameters: "100, 100, 200, 25"""


        ###static appendfunction call with parameters: "Blockfunction call with parameters: "pygame Colorfunction call with parameters: ""darkgreen"", function call with parameters: "700, 100, 200, 25"""


        ###static appendfunction call with parameters: "Blockfunction call with parameters: "pygame Colorfunction call with parameters: ""darkgreen"", function call with parameters: "400, 50, 200, 25"""


        ###static appendfunction call with parameters: "Blockfunction call with parameters: "pygame Colorfunction call with parameters: ""darkgreen"", function call with parameters: "400, 200, 200, 25"""


        return pygame sprite Groupfunction call with parameters: "floor, static"





    Define a function called  draw_panelfunction call with parameters: "self, player1, player2":


        fetch global variable  gameover, player1_dead, player2_dead


        ###draw panel rectangle


        screen blitfunction call with parameters: "self panel_img, function call with parameters: "0, screen_height - bottom_panel""


        ###show knight stats


        self draw_textfunction call with parameters: "f'{player1 name} HP: {player1 hp}', font, red, 100, screen_height - bottom_panel + 10"


        self draw_textfunction call with parameters: "f'{player2 name} HP: {player2 hp}', font, red, 500, screen_height - bottom_panel + 10"


        self draw_textfunction call with parameters: "f'Lives: {player1 lives}', font, red, 100, screen_height - bottom_panel + 70"


        self draw_textfunction call with parameters: "f'Lives: {player2 lives}', font, red, 500, screen_height - bottom_panel + 70"


        if gameover:


            if player1_dead:


                self draw_textfunction call with parameters: "f'Winner Player 2', font, green, 500, screen_height - bottom_panel + 100"


            if player2_dead:


                self draw_textfunction call with parameters: "f'Winner Player 1', font, green, 100, screen_height - bottom_panel + 100"





    Define a function called  event_loopfunction call with parameters: "self":


        for event in pygame event getfunction call with parameters: "":


            if event type variable equal to variable equal to  pygame QUIT or self keys[pygame K_ESCAPE]:


                self done variable equal to  True


            elif event type variable equal to variable equal to  pygame KEYDOWN:


                if event key variable equal to variable equal to  pygame K_LSHIFT:


                    self player1 jumpfunction call with parameters: "self obstacles"


                elif event key variable equal to variable equal to  pygame K_RSHIFT:


                    self player2 jumpfunction call with parameters: "self obstacles"


                elif event key variable equal to variable equal to  pygame K_f:


                    if self player1 lives > 0 and self player1 hp > 0:


                        self player1 attackfunction call with parameters: "self player2"


                elif event key variable equal to variable equal to  pygame K_j:


                    if self player2 lives > 0 and self player2 hp > 0:


                        self player2 attackfunction call with parameters: "self player1"


            elif event type variable equal to variable equal to  pygame KEYUP:


                if event key variable equal to variable equal to  pygame K_SPACE:


                    self player1 jump_cutfunction call with parameters: ""


                elif event key variable equal to variable equal to  pygame K_f:


                    self player1 jump_cutfunction call with parameters: ""





    Define a function called  updatefunction call with parameters: "self":


        self keys variable equal to  pygame key get_pressedfunction call with parameters: ""


        self player1 pre_updatefunction call with parameters: "self obstacles"


        self player2 pre_updatefunction call with parameters: "self obstacles"





        self obstacles updatefunction call with parameters: "self player1, self obstacles"


        self player1 updatefunction call with parameters: "self obstacles, self keys"


        self obstacles updatefunction call with parameters: "self player2, self obstacles"


        self player2 updatefunction call with parameters: "self obstacles, self keys"





        self update_viewportfunction call with parameters: "self player1 total_displacement"


        self update_viewportfunction call with parameters: "self player2 total_displacement"





    Define a function called  display_fpsfunction call with parameters: "self":


        caption variable equal to  "{} - FPS: {: 2f}" formatfunction call with parameters: ""FIGHT KNIGHT", clock get_fpsfunction call with parameters: """


        pygame display set_captionfunction call with parameters: "caption"





    Define a function called  mainloopfunction call with parameters: "self":





        while not self done:


            clock tickfunction call with parameters: "fps"





            ###draw background


            self draw_bgfunction call with parameters: ""


            ###draw panel


            self draw_panelfunction call with parameters: "self player1, self player2"


            self player1_health_bar drawfunction call with parameters: "self player1 hp"


            self player2_health_bar drawfunction call with parameters: "self player2 hp"


            self obstacles drawfunction call with parameters: "self level"





            ###draw fighters


            self keys variable equal to  pygame key get_pressedfunction call with parameters: ""


            self player1 pre_updatefunction call with parameters: "self obstacles"


            self player2 pre_updatefunction call with parameters: "self obstacles"





            self player1 updatefunction call with parameters: "self obstacles, self keys"


            self player1 drawfunction call with parameters: ""


            self player2 updatefunction call with parameters: "self obstacles, self keys"


            self player2 drawfunction call with parameters: ""





            ###control player actions


            ###reset action variables


            attack variable equal to  False


            target variable equal to  None


            self event_loopfunction call with parameters: ""


            self updatefunction call with parameters: ""


            pygame display updatefunction call with parameters: ""


            clock tickfunction call with parameters: "self fps"


            self display_fpsfunction call with parameters: ""


        pygame quitfunction call with parameters: ""








if __name__ variable equal to variable equal to  "__main__":


    Controlfunction call with parameters: "" mainloopfunction call with parameters: ""


[Github link] https://github.com/DownRamp/Games/blob/main/final_game/game.py
## Next Steps: 
1. Add bigger area
2.  add a character roster
3.  add platforms to jump on
4.  add a ranking system

---

