import pygame # type:ignore
import time
import random

# Initialize pygame
pygame.init()

# Define colors
white = (255, 255, 255)
yellow = (255, 255, 102)
black = (0, 0, 0)
red = (213, 50, 80)
green = (0, 255, 0)
blue = (50, 153, 213)

# Set display width and height
width = 600
height = 400

# Create the screen object
screen = pygame.display.set_mode((width, height))
pygame.display.set_caption('Snake Game')

# Set the clock for controlling the speed of the game
clock = pygame.time.Clock()

# Define the snake block size and speed
block_size = 10
snake_speed = 20

