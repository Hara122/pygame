import pygame
import sys
import random

# Initialize Pygame
pygame.init()

# Set up the window
window_size = (400, 708)
screen = pygame.display.set_mode(window_size)

# Load the bird image
bird_image = pygame.image.load("bird.png")
bird_rect = bird_image.get_rect()

# Load the pipe image
pipe_image = pygame.image.load("pipe.png")
pipe_rect = pipe_image.get_rect()
pipe_rect.bottom = 0

# Set the bird's starting position
bird_rect.centerx = window_size[0] // 2
bird_rect.centery = window_size[1] // 2

# Set the bird's gravity
gravity = 1.0

# Set the bird's upward velocity
velocity = 0.0

# Set the pipe's starting position
pipe_rect.left = window_size[0]

# Set the pipe's velocity
pipe_velocity = -5

# Set the gap size between the pipes
gap_size = 100

# Set the flag to indicate if the bird has collided with a pipe
collided = False

# Main game loop
while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            sys.exit()

    # Move the bird upward
    velocity -= gravity

    # Update the bird's position
    bird_rect.centery += velocity

    # Move the pipes
    pipe_rect.left += pipe_velocity

    # Check if the pipes are off the screen
    if pipe_rect.right < 0:
        pipe_rect.left = window_size[0]
        gap_start = random.randint(0, window_size[1] - gap_size)
        pipe_rect.bottom = gap_start

    # Check if the bird has collided with a pipe
    if (bird_rect.right >= pipe_rect.left and
        bird_rect.left <= pipe_rect.right and
        bird_rect.bottom >= pipe_rect.bottom and
        bird_rect.top <= pipe_rect.bottom + gap_size):
        collided = True

    # Check if the bird has hit the ground
    if bird_rect.bottom >= window_size[1]:
        collided = True

    # Draw the bird and pipes
    screen.fill((255, 255, 255))
    screen.blit(bird_image, bird_rect)
    screen.blit(pipe_image, pipe_rect)
    pygame.display.update()

    # If the bird has collided with a pipe or the ground, end the game
    if collided:
        sys.exit()
