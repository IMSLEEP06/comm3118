import pygame
import time

pygame.init()

# functions
def message(fonts, msg, color, posx, posy):
    # render(text, antialias, color, background=None)
    # antialias - 선을 부드럽게 만드는 그래픽 기법
    mesg = fonts.render(msg, True, color)
    mesg_Rect = mesg.get_rect()
    mesg_Rect.centerx = posx
    mesg_Rect.centery = posy
    screen.blit(mesg, mesg_Rect)

# Frame - while loop 처리 속도
clock = pygame.time.Clock()

# Fonts
# SysFont(글꼴, size, bold=False, italic=False)
# print(pygame.font.get_fonts())
font_gameOver = pygame.font.SysFont(None, 50)
font_madeBy = pygame.font.SysFont(None, 20)

# colors
BLUE = (0,0,255); RED = (255,0,0); WHITE = (255,255,255)
BLACK = (0,0,0); GRAY = (127,127,127)

# screen 
SCR_WIDTH, SCR_HEIGHT = 800, 600
screen = pygame.display.set_mode((SCR_WIDTH, SCR_HEIGHT))
pygame.display.set_caption('Snake Game')

# snake
snake_size = 20
snake_pos_x = int(SCR_WIDTH/2 - snake_size/2)
snake_pos_y = int(SCR_HEIGHT/2 - snake_size/2)
snake_posx_change = 0
snake_posy_change = 0

running = True
while running:
    
    for event in pygame.event.get():
        #print(event)
        if event.type == pygame.QUIT:
            running = False
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_UP:
                snake_posx_change = 0
                snake_posy_change = -10
            if event.key == pygame.K_DOWN:
                snake_posx_change = 0
                snake_posy_change = 10
            if event.key == pygame.K_LEFT:
                snake_posx_change = -10
                snake_posy_change = 0
            if event.key == pygame.K_RIGHT:
                snake_posx_change = 10
                snake_posy_change = 0

    snake_pos_x += snake_posx_change
    snake_pos_y += snake_posy_change

    if snake_pos_x >= (SCR_WIDTH - snake_size) or snake_pos_x - (snake_size/2) < 0 \
        or snake_pos_y >= (SCR_HEIGHT - snake_size) or snake_pos_y - (snake_size/2) < 0:
        running = False
    
    screen.fill(WHITE)
    pygame.draw.rect(screen, GRAY, [0,0, SCR_WIDTH, SCR_HEIGHT], 10) # 10은 사각형 테두리의 크기, 뱀이 외벽의 경계면을 넘어갈때, 이 값을 생각해야 함.
    pygame.draw.rect(screen, BLUE, [snake_pos_x, snake_pos_y, snake_size, snake_size])
    pygame.display.flip()
    clock.tick(30)

message(font_gameOver, 'Game Over', RED, int(SCR_WIDTH/2), int(SCR_HEIGHT/2))
message(font_madeBy, 'made by kig2929kig', GRAY, int(SCR_WIDTH/2), int(SCR_HEIGHT/2) + 30)
pygame.display.update()
time.sleep(2)
pygame.quit()
