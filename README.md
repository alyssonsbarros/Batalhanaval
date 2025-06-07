# Batalhanaval

#include <stdio.h>

#define TAM 10
#define TAM_HABILIDADE 5
#define NAVIO 3
#define HABILIDADE 5

// Função para exibir o tabuleiro com elementos distintos
void exibirTabuleiro(int tabuleiro[TAM][TAM]) {
    printf("Tabuleiro:\n");
    for (int i = 0; i < TAM; i++) {
        for (int j = 0; j < TAM; j++) {
            printf("%d ", tabuleiro[i][j]);
        }
        printf("\n");
    }
}

// Cria matriz cone (formato de pirâmide apontando para baixo)
void criarMatrizCone(int matriz[TAM_HABILIDADE][TAM_HABILIDADE]) {
    for (int i = 0; i < TAM_HABILIDADE; i++) {
        for (int j = 0; j < TAM_HABILIDADE; j++) {
            int centro = TAM_HABILIDADE / 2;
            if (j >= centro - i && j <= centro + i)
                matriz[i][j] = 1;
            else
                matriz[i][j] = 0;
        }
    }
}

// Cria matriz cruz
void criarMatrizCruz(int matriz[TAM_HABILIDADE][TAM_HABILIDADE]) {
    for (int i = 0; i < TAM_HABILIDADE; i++) {
        for (int j = 0; j < TAM_HABILIDADE; j++) {
            if (i == TAM_HABILIDADE / 2 || j == TAM_HABILIDADE / 2)
                matriz[i][j] = 1;
            else
                matriz[i][j] = 0;
        }
    }
}

// Cria matriz octaedro (losango)
void criarMatrizOctaedro(int matriz[TAM_HABILIDADE][TAM_HABILIDADE]) {
    for (int i = 0; i < TAM_HABILIDADE; i++) {
        for (int j = 0; j < TAM_HABILIDADE; j++) {
            int centro = TAM_HABILIDADE / 2;
            if (abs(i - centro) + abs(j - centro) <= centro)
                matriz[i][j] = 1;
            else
                matriz[i][j] = 0;
        }
    }
}

// Aplica a matriz de habilidade sobre o tabuleiro
void aplicarHabilidade(int tabuleiro[TAM][TAM], int habilidade[TAM_HABILIDADE][TAM_HABILIDADE], int origemLinha, int origemColuna) {
    int offset = TAM_HABILIDADE / 2;

    for (int i = 0; i < TAM_HABILIDADE; i++) {
        for (int j = 0; j < TAM_HABILIDADE; j++) {
            int linhaTab = origemLinha - offset + i;
            int colTab = origemColuna - offset + j;

            if (linhaTab >= 0 && linhaTab < TAM && colTab >= 0 && colTab < TAM) {
                if (habilidade[i][j] == 1 && tabuleiro[linhaTab][colTab] == 0)
                    tabuleiro[linhaTab][colTab] = HABILIDADE;
            }
        }
    }
}

// Posiciona navios no tabuleiro
void posicionarNavios(int tabuleiro[TAM][TAM]) {
    // Horizontal
    for (int i = 0; i < 3; i++) tabuleiro[0][i] = NAVIO;

    // Vertical
    for (int i = 0; i < 3; i++) tabuleiro[i][5] = NAVIO;

    // Diagonal principal \
    for (int i = 0; i < 3; i++) tabuleiro[6 + i][2 + i] = NAVIO;

    // Diagonal secundária /
    for (int i = 0; i < 3; i++) tabuleiro[1 + i][8 - i] = NAVIO;
}

int main() {
    int tabuleiro[TAM][TAM] = {0};

    // Inicializa as matrizes de habilidade
    int cone[TAM_HABILIDADE][TAM_HABILIDADE];
    int cruz[TAM_HABILIDADE][TAM_HABILIDADE];
    int octaedro[TAM_HABILIDADE][TAM_HABILIDADE];

    criarMatrizCone(cone);
    criarMatrizCruz(cruz);
    criarMatrizOctaedro(octaedro);

    // Posiciona os navios
    posicionarNavios(tabuleiro);

    // Define pontos de origem das habilidades no tabuleiro
    aplicarHabilidade(tabuleiro, cone, 2, 2);       // Cone em (2,2)
    aplicarHabilidade(tabuleiro, cruz, 5, 5);       // Cruz em (5,5)
    aplicarHabilidade(tabuleiro, octaedro, 8, 7);   // Octaedro em (8,7)

    // Exibe o tabuleiro final
    exibirTabuleiro(tabuleiro);

    return 0;
}
