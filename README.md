# game-codeimport pygame
import random

# Initialize pygame
pygame.init()

# Set up the screen
screen_width = 800
screen_height = 600
screen = pygame.display.set_mode((screen_width, screen_height))
pygame.display.set_caption("Catch Game")

# Set up colors
WHITE = (255, 255, 255)
RED = (255, 0, 0)

# Set up the clock
clock = pygame.time.Clock()

# Define classes
class Player(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.Surface((50, 50))
        self.image.fill(RED)
        self.rect = self.image.get_rect()
        self.rect.center = (screen_width // 2, screen_height - 50)
    
    def update(self):
        self.rect.x, _ = pygame.mouse.get_pos()

class FallingObject(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.Surface((30, 30))
        self.image.fill(WHITE)
        self.rect = self.image.get_rect()
        self.rect.x = random.randint(0, screen_width - self.rect.width)
        self.rect.y = -self.rect.height
        self.speedy = random.randint(3, 7)
    
    def update(self):
        self.rect.y += self.speedy
        if self.rect.top > screen_height:
            self.rect.x = random.randint(0, screen_width - self.rect.width)
            self.rect.y = -self.rect.height
            self.speedy = random.randint(3, 7)

# Create sprite groups
all_sprites = pygame.sprite.Group()
objects = pygame.sprite.Group()

# Create player object
player = Player()
all_sprites.add(player)

# Main game loop
running = True
while running:
    # Keep loop running at the right speed
    clock.tick(60)
    
    # Process input (events)
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
    
    # Update
    all_sprites.update()
    
    # Spawn a falling object
    if random.random() < 0.02:
        falling_object = FallingObject()
        all_sprites.add(falling_object)
        objects.add(falling_object)
    
    # Check for collisions
    hits = pygame.sprite.spritecollide(player, objects, True)
    for hit in hits:
        print("Caught object!")
    
    # Draw / render
    screen.fill((0, 0, 0))
    all_sprites.draw(screen)
    
    # After drawing everything, flip the display
    pygame.display.flip()

pygame.quit()
