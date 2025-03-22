# Test
Prueba

import pygame
import random
import sys
import tkinter as tk
from tkinter import messagebox

# Inicializar pygame
pygame.init()

# Definir constantes
MAPA_ANCHO = 20
MAPA_ALTO = 9
ANCHO, ALTO = 1200, 800
TAMANO_CELDA = min(ANCHO // MAPA_ANCHO, ALTO // MAPA_ALTO) #

pantalla = pygame.display.set_mode((MAPA_ANCHO * TAMANO_CELDA, MAPA_ALTO * TAMANO_CELDA)) #crear la ventana
pygame.display.set_caption("pacmannnnnnn")


#------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
# Cargar imágenes de Pac-Man y Fantasma
#pacman_img = pygame.image.load('pacman.png')
# = pygame.transform.scale(pacman_img, (TAMANO_CELDA, TAMANO_CELDA))  # Ajustar tamaño

#fantasm_img = pygame.image.load('fantasm.png')
#fantasm_img = pygame.transform.scale(fantasm_img, (TAMANO_CELDA, TAMANO_CELDA))  # Ajustar tamaño
#------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
# Definir colores
NEGRO = (0, 0, 0)
AZUL = (0, 0, 255)
AMARILLO = (255, 255, 0)
BLANCO = (255, 255, 255)
ROJO = (255, 0, 0)

# Definir variables del juego
pac_x, pac_y = 1, 1
fan_x, fan_y = 10, 5
puntos = 0
velocidad = 100
#------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
def reiniciar_juego():
    """Restablece el estado del juego a sus valores iniciales."""
    global pac_x, pac_y, puntos

    #reiniciar la posicion del jugador
    pac_x, pac_y = 1, 1
    fan_x, fan_y = 10, 5
    puntos = 0
    contador_fantasma = 0  # Reiniciar el contador para ralentizar el fantasma
    mapa = [[0] * MAPA_ANCHO for _ in range(MAPA_ALTO)]
    mapa[5][5] = 2
#------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
mapa = [
    [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
    [1, 3, 3, 3, 3, 2, 3, 1, 3, 3, 2, 3, 3, 3, 2, 3, 3, 3, 3, 1],
    [1, 3, 1, 1, 3, 1, 3, 1, 3, 1, 1, 1, 3, 1, 1, 1, 3, 1, 3, 1],
    [1, 3, 3, 1, 3, 3, 3, 1, 3, 3, 3, 1, 3, 3, 2, 1, 3, 1, 3, 1],
    [1, 1, 3, 1, 1, 1, 3, 1, 1, 1, 2, 1, 1, 1, 3, 1, 3, 1, 3, 1],
    [1, 3, 2, 3, 3, 2, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 2, 1],
    [1, 3, 1, 1, 3, 1, 1, 1, 3, 1, 1, 1, 3, 1, 1, 1, 3, 1, 3, 1],
    [1, 3, 3, 3, 3, 2, 3, 1, 3, 3, 3, 3, 3, 3, 2, 3, 3, 3, 3, 1],
    [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]
]
    # Definir el mapa del juego (1: pared, 3: camino vacío, 2: punto)
    # Reiniciar la posición del jugador a la coordenada inicial
    # Restablecer el puntaje a cero
    # Volver a cargar el mapa con sus elementos originales
#------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
# Función para mover el personaje principal
def mover_pacman(dx, dy): #se define la funcion 
    global pac_x, pac_y, puntos
    nuevo_x = pac_x + dx
    nuevo_y = pac_y + dy
    if 0 <= nuevo_x < MAPA_ANCHO and 0 <= nuevo_y < MAPA_ALTO and mapa[nuevo_y][nuevo_x] != 1:
        pac_x, pac_y = nuevo_x, nuevo_y
        if mapa[pac_y][pac_x] == 2:
            puntos += 10
            mapa[pac_y][pac_x] = 0

    """Mueve el Pac-Man en la dirección dada y actualiza la recolección de puntos."""
    # Verificar que el movimiento no atraviese paredes
    # Si hay un punto en la nueva posición, incrementar el puntaje
    # Actualizar la posición del jugador en el mapa

def mostrar_puntaje():
    font = pygame.font.Font(None, 36)  # Fuente del texto
    texto = font.render(f"Puntos: {puntos}", True, (255, 255, 255))  # Texto blanco
    pantalla.blit(texto, (20, 20))  # Posición del puntaje en la pantalla    
#------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
# Función para mover el fantasma
def mover_fantasma():
    global fan_x, fan_y
    movimientos = [(0, 1), (0, -1), (1, 0), (-1, 0)]  # Abajo, Arriba, Derecha, Izquierda
    random.shuffle(movimientos)  # Mezcla los movimientos para que sea más aleatorio

    for dx, dy in movimientos: #se calcula las nuevas coordenadas basandose en el actual 
        nuevo_x = fan_x + dx
        nuevo_y = fan_y + dy

        # Verifica que el nuevo movimiento no atraviese paredes (1 = pared)
        if 0 <= nuevo_x < MAPA_ANCHO and 0 <= nuevo_y < MAPA_ALTO and mapa[nuevo_y][nuevo_x] != 1:
            fan_x, fan_y = nuevo_x, nuevo_y
            break  # Sale del bucle cuando encuentra un movimiento válido

    """Mueve el fantasma de forma aleatoria en direcciones permitidas."""
    # Seleccionar una dirección aleatoria entre arriba, abajo, izquierda o derecha
    # Comprobar que el movimiento no atraviese paredes
    # Actualizar la posición del fantasma en el mapa
    
#------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
# Función para mostrar la pantalla de Game Over
def mostrar_game_over():
    pantalla.fill(NEGRO)
    font = pygame.font.Font(None, 74)
    texto = font.render("GAME OVER", True, ROJO)
    pantalla.blit(texto, (ANCHO // 2 - 100, ALTO // 2 - 50))
    pygame.display.flip()
    pygame.time.delay(2000)
    preguntar_volver_a_jugar("Game Over. ¿Quieres jugar de nuevo?")

    """Muestra la pantalla de Game Over y pregunta si el jugador quiere reiniciar."""
    # Dibujar el mensaje de Game Over en la pantalla
    # Esperar un tiempo y preguntar al usuario si desea reiniciar el juego

#función para mostrar que has ganado
def mostrar_you_won():
    pantalla.fill(NEGRO)
    font = pygame.font.Font(None, 74)
    texto = font.render("Has ganado", True, ROJO)
    pantalla.blit(texto, (ANCHO // 2 - 100, ALTO // 2 - 50))
    pygame.display.flip()
    pygame.time.delay(20)
    preguntar_volver_a_jugar("¿Quieres jugar de nuevo?")
#------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
# Función para preguntar si el jugador quiere volver a jugar
def preguntar_volver_a_jugar(mensaje):
    root = tk.Tk()
    root.withdraw()
    respuesta = messagebox.askyesno("Fin del juego", mensaje)
    if respuesta:
        reiniciar_juego()
    else:
        pygame.quit()
        sys.exit()

    """Muestra un cuadro de diálogo preguntando si el jugador quiere volver a jugar."""
    # Crear una ventana emergente con tkinter para mostrar un mensaje de juego terminado
    # Preguntar al usuario si desea jugar nuevamente con opciones "Sí" o "No"
    # Si el usuario elige "Sí", reiniciar el juego
    # Si el usuario elige "No", cerrar pygame y terminar el programa
#------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
#TERMINAR
   # Bucle principal
ejecutando = True
while ejecutando:
    for evento in pygame.event.get():
        if evento.type == pygame.QUIT:
            ejecutando = False

    teclas = pygame.key.get_pressed()
    if teclas[pygame.K_LEFT]:
        mover_pacman(-1, 0)
    if teclas[pygame.K_RIGHT]:
        mover_pacman(1, 0)
    if teclas[pygame.K_UP]:
        mover_pacman(0, -1)
    if teclas[pygame.K_DOWN]:
        mover_pacman(0, 1)

    contador_fantasma = 0  # Nuevo contador para controlar la velocidad del fantasma
    velocidad_fantasma = 5  # El fantasma se mueve cada 3 ciclos
    # Hacer que el fantasma se mueva más lento usando el contador
    contador_fantasma += 5
    if contador_fantasma >= velocidad_fantasma:
        mover_fantasma()
        contador_fantasma = 0  # Reinicia el contador después de moverse

    #vericar si el jugador perdio
    if (pac_x, pac_y) == (fan_x, fan_y):
        mostrar_game_over()

    #verificar si ya no quedan puntos
    if not any(2 in fila for fila in mapa):
        mostrar_you_won()

    #mapa en pantalla
    pantalla.fill(NEGRO)
    for y in range(MAPA_ALTO):
        for x in range(MAPA_ANCHO):
            if mapa[y][x] == 1:
                pygame.draw.rect(pantalla, AZUL, (x * TAMANO_CELDA, y * TAMANO_CELDA, TAMANO_CELDA, TAMANO_CELDA))
            elif mapa[y][x] == 2:
                pygame.draw.circle(pantalla, BLANCO, (x * TAMANO_CELDA + TAMANO_CELDA // 2, y * TAMANO_CELDA + TAMANO_CELDA // 2), TAMANO_CELDA // 4)
            
   
    #dibujar principal y fantasma
    
    pygame.draw.circle(pantalla, AMARILLO, (pac_x* TAMANO_CELDA + TAMANO_CELDA // 2, pac_y * TAMANO_CELDA + TAMANO_CELDA // 2), TAMANO_CELDA // 2)
    pygame.draw.circle(pantalla, ROJO, (fan_x* TAMANO_CELDA + TAMANO_CELDA // 2, fan_y * TAMANO_CELDA + TAMANO_CELDA // 2), TAMANO_CELDA // 2)

    # Dibujar Pac-Man (Jugador)
    #pantalla.blit(pacman_img, (pac_x * TAMANO_CELDA, pac_y * TAMANO_CELDA))

    # Dibujar Fantasma
    #pantalla.blit(fantasm_img, (fan_x * TAMANO_CELDA, fan_y * TAMANO_CELDA))
    
    mostrar_puntaje()  # Dibujar el puntaje en la pantalla

    pygame.display.flip()
    pygame.time.delay(velocidad)
