# Snake-game-
Snake game 
import pygame
import time
import random

# Initialize pygame
pygame.init()

# Set up display
width, height = 600, 400
win = pygame.display.set_mode((width, height))
pygame.display.set_caption('Snake Game')

# Colors
black = (0, 0, 0)
white = (255, 255, 255)
green = (0, 255, 0)
red = (213, 50, 80)
blue = (50, 153, 213)

# Snake and food settings
block_size = 10
clock = pygame.time.Clock()
font_style = pygame.font.SysFont(None, 35)

def message(msg, color, position):
    mesg = font_style.render(msg, True, color)
    win.blit(mesg, position)

def game_loop():
    game_over = False
    game_close = False

    # Starting position
    x = width // 2
    y = height // 2
    dx = 0
    dy = 0

    snake = []
    length = 1

    # Food position
    foodx = round(random.randrange(0, width - block_size) / 10.0) * 10.0
    foody = round(random.randrange(0, height - block_size) / 10.0) * 10.0

    while not game_over:

        while game_close:
            win.fill(blue)
            message("You lost! Press Q-Quit or C-Play Again", red, [100, 180])
            pygame.display.update()

            for event in pygame.event.get():
                if event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_q:
                        game_over = True
                        game_close = False
                    if event.key == pygame.K_c:
                        game_loop()

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    dx = -block_size
                    dy = 0
                elif event.key == pygame.K_RIGHT:
                    dx = block_size
                    dy = 0
                elif event.key == pygame.K_UP:
                    dy = -block_size
                    dx = 0
                elif event.key == pygame.K_DOWN:
                    dy = block_size
                    dx = 0

        # Update snake position
        x += dx
        y += dy

        # Check for boundaries
        if x >= width or x < 0 or y >= height or y < 0:
            game_close = True

        win.fill(black)
        pygame.draw.rect(win, green, [foodx, foody, block_size, block_size])

        snake_head = []
        snake_head.append(x)
        snake_head.append(y)
        snake.append(snake_head)

        if len(snake) > length:
            del snake[0]

        for block in snake[:-1]:
            if block == snake_head:
                game_close = True

        for part in snake:
            pygame.draw.rect(win, white, [part[0], part[1], block_size, block_size])

        message(f"Score: {length - 1}", red, [10, 10])
        pygame.display.update()

        # Snake eats food
        if x == foodx and y == foody:
            foodx = round(random.randrange(0, width - block_size) / 10.0) * 10.0
            foody = round(random.randrange(0, height - block_size) / 10.0) * 10.0
            length += 1

        clock.tick(15)

    pygame.quit()
    quit()

game_loop()
