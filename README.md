import pygame
import time
import random

# Initialize Pygame
pygame.init()

# Define colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (213, 50, 80)
GREEN = (0, 255, 0)
BLUE = (50, 153, 213)

# Set screen width and height
SCREEN_WIDTH = 600
SCREEN_HEIGHT = 400
SCREEN = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
pygame.display.set_caption('Snake Game')

# Set clock and speed
clock = pygame.time.Clock()
SNAKE_SPEED = 15

# Define snake block size and font
SNAKE_BLOCK = 10
FONT_STYLE = pygame.font.SysFont("bahnschrift", 25)
SCORE_FONT = pygame.font.SysFont("comicsansms", 35)

# Function to display the score
def your_score(score):
    value = SCORE_FONT.render("Your Score: " + str(score), True, WHITE)
    SCREEN.blit(value, [0, 0])

# Function to draw the snake
def our_snake(snake_block, snake_list):
    for x in snake_list:
        pygame.draw.rect(SCREEN, GREEN, [x[0], x[1], snake_block, snake_block])

# Function to display a message
def message(msg, color):
    mesg = FONT_STYLE.render(msg, True, color)
    SCREEN.blit(mesg, [SCREEN_WIDTH / 6, SCREEN_HEIGHT / 3])

# Game loop function
def gameLoop():
    game_over = False
    game_close = False

    # Initial snake position
    x1 = SCREEN_WIDTH / 2
    y1 = SCREEN_HEIGHT / 2

    # Change in position
    x1_change = 0
    y1_change = 0

    # Snake initial length
    snake_List = []
    Length_of_snake = 1

    # Randomly place the food
    foodx = round(random.randrange(0, SCREEN_WIDTH - SNAKE_BLOCK) / 10.0) * 10.0
    foody = round(random.randrange(0, SCREEN_HEIGHT - SNAKE_BLOCK) / 10.0) * 10.0

    # Main game loop
    while not game_over:

        while game_close:
            SCREEN.fill(BLUE)
            message("You Lost! Press C-Play Again or Q-Quit", RED)
            your_score(Length_of_snake - 1)
            pygame.display.update()

            for event in pygame.event.get():
                if event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_q:
                        game_over = True
                        game_close = False
                    if event.key == pygame.K_c:
                        gameLoop()

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    x1_change = -SNAKE_BLOCK
                    y1_change = 0
                elif event.key == pygame.K_RIGHT:
                    x1_change = SNAKE_BLOCK
                    y1_change = 0
                elif event.key == pygame.K_UP:
                    y1_change = -SNAKE_BLOCK
                    x1_change = 0
                elif event.key == pygame.K_DOWN:
                    y1_change = SNAKE_BLOCK
                    x1_change = 0

        if x1 >= SCREEN_WIDTH or x1 < 0 or y1 >= SCREEN_HEIGHT or y1 < 0:
            game_close = True
        x1 += x1_change
        y1 += y1_change
        SCREEN.fill(BLUE)
        pygame.draw.rect(SCREEN, RED, [foodx, foody, SNAKE_BLOCK, SNAKE_BLOCK])
        snake_Head = []
        snake_Head.append(x1)
        snake_Head.append(y1)
        snake_List.append(snake_Head)
        if len(snake_List) > Length_of_snake:
            del snake_List[0]

        for x in snake_List[:-1]:
            if x == snake_Head:
                game_close = True

        our_snake(SNAKE_BLOCK, snake_List)
        your_score(Length_of_snake - 1)

        pygame.display.update()

        # Check if snake eats food
        if x1 == foodx and y1 == foody:
            foodx = round(random.randrange(0, SCREEN_WIDTH - SNAKE_BLOCK) / 10.0) * 10.0
            foody = round(random.randrange(0, SCREEN_HEIGHT - SNAKE_BLOCK) / 10.0) * 10.0
            Length_of_snake += 1

        clock.tick(SNAKE_SPEED)

    pygame.quit()
    quit()

# Run the game
gameLoop()

