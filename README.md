comando MoverEsquerda:
    se peça.podeMoverEsquerda():
        peça.moverEsquerda()
# Exemplo simplificado com pygame

import pygame

# Função para mover a peça para a esquerda
def mover_esquerda(peça):
    if peça.pode_mover_esquerda():
        peça.x -= 1  # Move a peça uma unidade para a esquerda

# Em algum lugar no loop principal do jogo:
for evento in pygame.event.get():
    if evento.type == pygame.KEYDOWN:
        if evento.key == pygame.K_LEFT:
            mover_esquerda(peça_atual)
import pygame
import random

# Inicializa o pygame
pygame.init()

# Configurações da tela
LARGURA_TELA, ALTURA_TELA = 300, 600
TELA = pygame.display.set_mode((LARGURA_TELA, ALTURA_TELA))
pygame.display.set_caption("Tetris")

# Cores
PRETO = (0, 0, 0)
BRANCO = (255, 255, 255)

# Configurações do tabuleiro
LINHAS, COLUNAS = 20, 10
TAMANHO_CELULA = 30
tabuleiro = [[0 for _ in range(COLUNAS)] for _ in range(LINHAS)]

# FPS
FPS = 10
clock = pygame.time.Clock()
# Definições das peças (tetrominós)
FORMAS = {
    'I': [[1, 1, 1, 1]],
    'O': [[1, 1], [1, 1]],
    'T': [[0, 1, 0], [1, 1, 1]],
    'S': [[0, 1, 1], [1, 1, 0]],
    'Z': [[1, 1, 0], [0, 1, 1]],
    'J': [[1, 0, 0], [1, 1, 1]],
    'L': [[0, 0, 1], [1, 1, 1]],
}

CORES = {
    'I': (0, 255, 255),
    'O': (255, 255, 0),
    'T': (128, 0, 128),
    'S': (0, 255, 0),
    'Z': (255, 0, 0),
    'J': (0, 0, 255),
    'L': (255, 165, 0),
}

# Classe da peça
class Pecas:
    def __init__(self, forma):
        self.forma = forma
        self.cor = CORES[forma]
        self.x = COLUNAS // 2 - len(FORMAS[forma][0]) // 2
        self.y = 0

    def girar(self):
        self.forma = [list(row) for row in zip(*self.forma[::-1])]
        
    def pode_mover(self, dx, dy):
        for i, linha in enumerate(self.forma):
            for j, valor in enumerate(linha):
                if valor:
                    x = self.x + j + dx
                    y = self.y + i + dy
                    if x < 0 or x >= COLUNAS or y >= LINHAS or (y >= 0 and tabuleiro[y][x]):
                        return False
        return True

    def mover(self, dx, dy):
        if self.pode_mover(dx, dy):
            self.x += dx
            self.y += dy
        elif dy:
            self.fixar()
            return True
        return False

    def fixar(self):
        for i, linha in enumerate(self.forma):
            for j, valor in enumerate(linha):
                if valor:
                    x = self.x + j
                    y = self.y + i
                    if y >= 0:
                        tabuleiro[y][x] = self.cor
        verificar_linhas()

def verificar_linhas():
    global tabuleiro
    novas_linhas = [linha for linha in tabuleiro if any(celula == 0 for celula in linha)]
    num_linhas_removidas = LINHAS - len(novas_linhas)
    tabuleiro = [[0 for _ in range(COLUNAS)] for _ in range(num_linhas_removidas)] + novas_linhas
def desenhar_tabuleiro():
    for y, linha in enumerate(tabuleiro):
        for x, cor in enumerate(linha):
            if cor:
                pygame.draw.rect(TELA, cor, (x * TAMANHO_CELULA, y * TAMANHO_CELULA, TAMANHO_CELULA, TAMANHO_CELULA))

def desenhar_peca(peca):
    for i, linha in enumerate(peca.forma):
        for j, valor in enumerate(linha):
            if valor
