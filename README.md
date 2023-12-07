# Yigithan1import pygame
import sys
import random

# Pygame'yi başlat
pygame.init()

# Ekran boyutları
WIDTH, HEIGHT = 800, 600

# Renkler
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (255, 0, 0)

# Oyun hızı
SPEED = 5

# Araba özellikleri
car_width = 50
car_height = 100

# Pencereyi oluştur
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Araba Yarışı")

# Araba resmi yükle
car_img = pygame.image.load("car.png")

# Oyun döngüsü
def game_loop():
    car_x = WIDTH // 2 - car_width // 2
    car_y = HEIGHT - car_height - 10

    car_speed = 0

    obstacle_width = 50
    obstacle_height = 50
    obstacle_speed = 5

    obstacle_x = random.randint(0, WIDTH - obstacle_width)
    obstacle_y = -obstacle_height

    clock = pygame.time.Clock()

    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()

            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    car_speed = -SPEED
                elif event.key == pygame.K_RIGHT:
                    car_speed = SPEED

            if event.type == pygame.KEYUP:
                if event.key == pygame.K_LEFT or event.key == pygame.K_RIGHT:
                    car_speed = 0

        # Arabayı hareket ettir
        car_x += car_speed

        # Engeli hareket ettir
        obstacle_y += obstacle_speed

        # Engelin alt sınırına ulaştığında yeni bir engel oluştur
        if obstacle_y > HEIGHT:
            obstacle_y = -obstacle_height
            obstacle_x = random.randint(0, WIDTH - obstacle_width)

        # Çarpışma kontrolü
        if (
            car_x < obstacle_x + obstacle_width
            and car_x + car_width > obstacle_x
            and car_y < obstacle_y + obstacle_height
            and car_y + car_height > obstacle_y
        ):
            game_over()

        # Pencereyi temizle
        screen.fill(WHITE)

        # Arabayı çiz
        screen.blit(car_img, (car_x, car_y))

        # Engeli çiz
        pygame.draw.rect(screen, RED, (obstacle_x, obstacle_y, obstacle_width, obstacle_height))

        # Ekranı güncelle
        pygame.display.flip()

        # FPS ayarı
        clock.tick(30)

# Oyun bittiğinde çağrılan fonksiyon
def game_over():
    font = pygame.font.Font(None, 74)
    text = font.render("Game Over", True, BLACK)
    screen.blit(text, (WIDTH // 2 - 200, HEIGHT // 2 - 50))
    pygame.display.flip()
    pygame.time.wait(2000)  # 2 saniye bekle
    game_loop()  # Oyun döngüsünü yeniden başlat

# Oyun döngüsünü başlat
game_loop()
