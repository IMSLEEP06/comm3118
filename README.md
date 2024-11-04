import pygame
import time
import random

pygame.init()

def message(fonts, msg, color, posx, posy):
    mesg = fonts.render(msg, True, color)
    mesg_Rect = mesg.get_rect()
    mesg_Rect.centerx = posx
    mesg_Rect.centery = posy
    screen.blit(mesg, mesg_Rect)

clock = pygame.time.Clock()

font_gameOver = pygame.font.SysFont('comicsansms', 50)
font_madeBy = pygame.font.SysFont(None, 20)
font_score = pygame.font.SysFont(None, 30)

BLUE = (0,0,255); RED = (255,0,0); WHITE = (255,255,255)
BLACK = (0,0,0); GRAY = (127,127,127); YELLOW = (255,255,0)
LIGHT_GREEN = (175,215,70)

SCR_WIDTH, SCR_HEIGHT = 800, 600
screen = pygame.display.set_mode((SCR_WIDTH, SCR_HEIGHT))
pygame.display.set_caption('Snake Game')

snake_list = []
snake_size = 20
snake_speed = 5
snake_pos_x = int(SCR_WIDTH/2 - snake_size/2)
snake_pos_y = int(SCR_HEIGHT/2 - snake_size/2)
snake_posx_change = 0
snake_posy_change = 0
snake_tail = 1

def snake(snake_size, snake_list):
    for pos in snake_list:
        pygame.draw.rect(screen, BLUE, [pos[0], pos[1], snake_size, snake_size])

foodx = None
foody = None
def food():
    global foodx, foody
    while True :
        foodx = random.randrange(10, SCR_WIDTH - snake_size, snake_size) 
        foody = random.randrange(10, SCR_HEIGHT - snake_size, snake_size)
        food_pos = [foodx,foody]
        if food_pos in snake_list :
            continue
        else :
            break
score = 0 

def game_score(score):
    value = 'Score : ' + str(score)
    message(font_score, value, YELLOW, SCR_WIDTH/2, 30)

food()
running = True
while running:
    
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_UP:
                snake_posx_change = 0
                snake_posy_change = -20
            if event.key == pygame.K_DOWN:
                snake_posx_change = 0
                snake_posy_change = 20
            if event.key == pygame.K_LEFT:
                snake_posx_change = -20
                snake_posy_change = 0
            if event.key == pygame.K_RIGHT:
                snake_posx_change = 20
                snake_posy_change = 0

    snake_pos_x += snake_posx_change
    snake_pos_y += snake_posy_change

    if snake_pos_x >= (SCR_WIDTH - snake_size) or snake_pos_x - (snake_size/2) < 0 \
        or snake_pos_y >= (SCR_HEIGHT - snake_size) or snake_pos_y - (snake_size/2) < 0:
        running = False
    
    screen.fill(LIGHT_GREEN)
    pygame.draw.rect(screen, GRAY, [0,0, SCR_WIDTH, SCR_HEIGHT], 10)
    pygame.draw.rect(screen, RED, [foodx, foody, 20, 20])
    #pygame.draw.rect(screen, BLUE, [snake_pos_x, snake_pos_y, snake_size, snake_size])
    snake_head = []
    snake_head.append(snake_pos_x)
    snake_head.append(snake_pos_y)
    snake_list.append(snake_head)
        
    if len(snake_list) > snake_tail:
        del snake_list[0]
    snake(snake_size, snake_list)
    game_score(score)
    pygame.display.flip()
    
    if snake_pos_x == foodx and snake_pos_y == foody:
        food()
        score += 10
        print(score)
        snake_tail +=1
    clock.tick(snake_speed)

message(font_gameOver, 'Game Over', RED, int(SCR_WIDTH/2), int(SCR_HEIGHT/2))
pygame.display.update()
time.sleep(2)
pygame.quit()
