import pygame
import sys
import random

# Constants
WIDTH, HEIGHT = 600, 400
GRID_SIZE = 20
SNAKE_SIZE = 20
BULLET_SIZE = 10
GUN_COOLDOWN = 20  # Cooldown frames between shots
FPS = 10
DEPTH_OFFSET = 8  # Depth offset for pseudo-3D effect
MAX_HEALTH = 100

# Colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
BLUE = (0, 0, 255)
RED = (255, 0, 0)
GREEN = (0, 255, 0)  # New color for the food

# Load Guy Fieri image
guy_fieri_image = pygame.image.load("guy_fieri.png")

# Initialize Pygame
pygame.init()

# Initialize Pygame mixer
pygame.mixer.init()

# Load background music
pygame.mixer.music.load("free_bird.mp3")
pygame.mixer.music.set_volume(0.5)  # Adjust volume (0.0 to 1.0)
pygame.mixer.music.play(-1)  # Play music in a loop

# Initialize Pygame display
screen = pygame.display.set_mode((WIDTH, HEIGHT), 0, 32)
surface = pygame.Surface(screen.get_size())
surface = surface.convert()
clock = pygame.time.Clock()

# Player Snake Class
class Snake:
    def __init__(self, color=BLUE, initial_health=MAX_HEALTH, initial_size=1):
        self.length = initial_size
        self.positions = [((WIDTH // 2), (HEIGHT // 2))]
        self.direction = random.choice([UP, DOWN, LEFT, RIGHT])
        self.image = pygame.transform.scale(guy_fieri_image, (SNAKE_SIZE, SNAKE_SIZE))
        self.color = color
        self.gun_cooldown = 0
        self.health = initial_health
        self.money = 0

    # Other methods...

# In-App Purchase Constants
HEALTH_PRICE = 2.99
SIZE_PRICE = 4.99

# Money Reward Constants
HURT_REWARD = 1

# In-App Purchase Function
def purchase_health(snake):
    # Process in-app purchase for more health
    print(f"Transaction successful! You've purchased more health for ${HEALTH_PRICE}.")
    snake.health += 20  # Increase health by 20

def purchase_size(snake):
    # Process in-app purchase for a bigger snake
    print(f"Transaction successful! You've purchased a bigger snake for ${SIZE_PRICE}.")
    snake.length *= 2  # Double the snake's length

# Main Game Loop
player_snake = Snake()  # Create an initial snake
while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_PAGEUP:
                # Pressing Page Up buys more health
                purchase_health(player_snake)
            elif event.key == pygame.K_PAGEDOWN:
                # Pressing Page Down buys a bigger snake
                purchase_size(player_snake)

    # Check if the player's snake hurts the other snake
    # This is a simplified example; you'd replace this with your actual game logic
    if pygame.sprite.collide_rect(player_snake, other_snake):
        # Hurt the other snake and reward money
        other_snake.health -= 10
        player_snake.money += HURT_REWARD

    # Additional game logic goes here...

    # Draw money counter
    font = pygame.font.Font(None, 36)
    money_text = font.render(f"Money: ${player_snake.money}", True, WHITE)
    surface.blit(money_text, (10, HEIGHT - 40))

    pygame.display.update()
    clock.tick(FPS)
