# Solar System

Solar System with pygame


Mercury, Venus, Earth, Mars, Jupiter, and Saturn move around the Sun. 
Moon moves around the Earth and Titan around the Saturn.
Stars are twinkling.


## Source Code 

``` py 
import pygame
import os
import numpy as np

WINDOW_WIDTH = 1200
WINDOW_HEIGHT = 800

GRAY = (200, 200, 200)
BLACK = (0,0,0)
WHITE = (255,255,255)

CenterPos= WINDOW_WIDTH / 2 , WINDOW_HEIGHT /2

pygame.init()

pygame.display.set_caption("Solar System")

screen = pygame.display.set_mode((WINDOW_WIDTH, WINDOW_HEIGHT), pygame.FULLSCREEN)
surface = screen.convert_alpha()

clock = pygame.time.Clock()

current_path = os.path.dirname(__file__)
assets_path = os.path.join(current_path, 'assets')

Sunimg_upload= pygame.image.load(os.path.join(assets_path, 'sun.png'))
Sunimg=pygame.transform.scale(Sunimg_upload, (200,200))
Mercuryimg_upload= pygame.image.load(os.path.join(assets_path, 'mercury.png'))
Mercuryimg=pygame.transform.scale(Mercuryimg_upload, (7,7))
Venusimg_upload= pygame.image.load(os.path.join(assets_path, 'venus.png'))
Venusimg=pygame.transform.scale(Venusimg_upload, (18,18))
Earthimg_upload= pygame.image.load(os.path.join(assets_path, 'earth.png'))
Earthimg=pygame.transform.scale(Earthimg_upload, (20,20))
Marsimg_upload= pygame.image.load(os.path.join(assets_path, 'mars.png'))
Marsimg=pygame.transform.scale(Marsimg_upload, (10,10))
Jupiterimg_upload= pygame.image.load(os.path.join(assets_path, 'jupiter.png'))
Jupiterimg=pygame.transform.scale(Jupiterimg_upload, (100,100))
Saturnimg_upload= pygame.image.load(os.path.join(assets_path, 'saturn.png'))
Saturnimg=pygame.transform.scale(Saturnimg_upload, (141,141))
Moonimg_upload= pygame.image.load(os.path.join(assets_path, 'moon.png'))
Moonimg=pygame.transform.scale(Moonimg_upload, (7,7))
Titanimg_upload= pygame.image.load(os.path.join(assets_path, 'titan.png'))
Titanimg=pygame.transform.scale(Titanimg_upload, (15,15))
```

Upload images of planets.

``` py
class Planet(pygame.sprite.Sprite):
    def __init__(self, image, period, center, radius):
        pygame.sprite.Sprite.__init__(self)
        self.base=image
        self.position = pygame.Vector2(center)
        self.image= self.base
        self.period=period
        self.rot=0
        self.rot_speed = 2 * 3.14 * radius / period
        self.offset = pygame.Vector2(0,radius)
        self.offset_rotated = (0,0)
        
    def rotate(self):     
        self.offset_rotated = self.offset.rotate(self.rot)
        self.rect = self.image.get_rect(center = self.position - self.offset_rotated)
        
    def update(self):
        self.rot += self.rot_speed
        self.rotate()
    
    def getposition(self, scale):
        return self.position - pygame.Vector2(self.offset_rotated[0] *scale, self.offset_rotated[1] *scale)
       
```

Create Planet Class. 

``` py
class Star():
    def __init__(self):
        self.position = (np.random.randint(WINDOW_WIDTH), np.random.randint(WINDOW_HEIGHT))
        self.lightness = np.random.randint(5)
        self.r = np.random.randint(200,256)
        self.g = np.random.randint(200,256)
        self.b = np.random.randint(200, 256)
        self.a = np.random.randint(0, 256)
        self.size = np.random.random()*2
        self.color = (self.r, self.g, self.b, self.a)
    def update(self):
        if self.a+self.lightness > 255 or self.a+self.lightness < 0:
            self.lightness *= -1
        self.a+=self.lightness
        self.color = (self.r, self.g, self.b, self.a)
    def draw(self):
        pygame.draw.circle(surface, self.color, self.position, self.size) 
```

Create Star Class.

``` py
sun=Planet(Sunimg, 1, CenterPos, 0)
mercury=Planet(Mercuryimg, 880, CenterPos, 100) 
venus=Planet(Venusimg, 2247, CenterPos, 130) 
earth=Planet(Earthimg, 3652, CenterPos, 160)
mars=Planet(Marsimg, 6869, CenterPos, 200) 
jupiter=Planet(Jupiterimg, 43330, CenterPos, 250) 
saturn=Planet(Saturnimg, 107562, CenterPos, 350)


moon=Planet(Moonimg, 300, CenterPos, 30)
titan=Planet(Titanimg, 160, CenterPos, 50)

planets=pygame.sprite.Group()
planets.add(sun, mercury, venus, earth, mars, jupiter, saturn, moon, titan) 

stars=[]
for i in range(500):
    stars.append(Star())

done=False
while not done:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            done = True
        
    pygame.display.flip()
    moon.position=earth.getposition(1)
    titan.position = saturn.getposition(1)
    planets.update() 
    for i in stars:
        i.update()
    
    screen.fill(BLACK)
    surface.fill((0, 0, 0, 0))       
    planets.draw(screen)
    for i in stars:
        i.draw()
    
    screen.blit(surface, (0, 0))
    clock.tick(60)


pygame.quit()

```
   
