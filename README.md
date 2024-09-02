# testy_uklad_sloneczny
[Dokumentacja.docx](https://github.com/user-attachments/files/16839718/Dokumentacja.docx)
Kod źródłowy:
import pygame
import math

# ---- Inicjalizacja Pygame ---- 
pygame.init()

# ---- Ustawienia ekranu ---- 
width, height = 800, 800
screen = pygame.display.set_mode((width, height))
pygame.display.set_caption("Układ Słoneczny")

# ---- Kolory ---- 
black = (0, 0, 0)
white = (255, 255, 255)
yellow = (255, 255, 0)
blue = (100, 100, 255)
red = (255, 100, 100)
green = (100, 255, 100)
grey = (200, 200, 200)
darkblue = (0, 0, 139)   
orange = (255, 165, 0)   
# ---- Parametry orbit ---- 
orbits = [
    {"radius": 60, "speed": 4.74, "color": grey},     # Merkury
    {"radius": 100, "speed": 3.5, "color": orange},   # Wenus
    {"radius": 150, "speed": 2.98, "color": blue},    # Ziemia
    {"radius": 200, "speed": 2.41, "color": red},     # Mars
    {"radius": 250, "speed": 1.31, "color": yellow},  # Jowisz
    {"radius": 300, "speed": 0.97, "color": green},   # Saturn
    {"radius": 350, "speed": 0.68, "color": blue},    # Uran
    {"radius": 400, "speed": 0.54, "color": darkblue} # Neptun
]

# ---- Funkcja aktualizująca kąt planety ---- 
def update_planet_position(planet):
    planet["angle"] += planet["speed"]
    if planet["angle"] >= 360:
        planet["angle"] -= 360
    return planet

# ---- Parametry planet ---- 
planets = []
for orbit in orbits:
    planet = {
        "angle": 0,  
        "radius": orbit["radius"],
        "speed": orbit["speed"],
        "color": orbit["color"]
    }
    planets.append(planet)

# ---- Główna pętla programu ---- 
running = True
clock = pygame.time.Clock()

while running:
    screen.fill(black) 

    # ---- Rysowanie Słońca ---- 
    pygame.draw.circle(screen, yellow, (width // 2, height // 2), 30)

    # ---- Aktualizacja pozycji planet i ich rysowanie ---- 
    for planet in planets:
        planet = update_planet_position(planet)
        x = width // 2 + int(planet["radius"] * math.cos(math.radians(planet["angle"])))
        y = height // 2 + int(planet["radius"] * math.sin(math.radians(planet["angle"])))

        pygame.draw.circle(screen, planet["color"], (x, y), 10)

    # ---- Aktualizacja ekranu ---- 
    pygame.display.flip()

    # ---- Ustawienie liczby klatek na sekundę ---- 
    clock.tick(60)

    # ---- Obsługa zdarzeń ---- 
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

# ---- Zakończenie programu ---- 
pygame.quit()

# ---- Testy ---- 

def test_update_planet_position():
    planet = {"angle": 0, "radius": 100, "speed": 5, "color": (255, 255, 255)}
    updated_planet = update_planet_position(planet)
    assert updated_planet["angle"] == 5

    updated_planet = update_planet_position(updated_planet)
    assert updated_planet["angle"] == 10

    updated_planet["angle"] = 359
    updated_planet = update_planet_position(updated_planet)
    assert updated_planet["angle"] == 4  

def test_planet_initialization_and_movement():
    planets = []
    for orbit in orbits:
        planet = {
            "angle": 0,
            "radius": orbit["radius"],
            "speed": orbit["speed"],
            "color": orbit["color"]
        }
        planets.append(planet)

    for _ in range(10):  
        for planet in planets:
            angle_before = planet["angle"]
            planet = update_planet_position(planet)
            assert planet["angle"] > angle_before or planet["angle"] < angle_before

# ---- Uruchamianie testów tylko jeśli skrypt jest uruchamiany bezpośrednio ---- 
if __name__ == "__main__":
    test_update_planet_position()
    test_planet_initialization_and_movement()
    print("Wszystkie testy przeszły pomyślnie!")
