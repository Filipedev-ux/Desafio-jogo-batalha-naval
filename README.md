# Desafio-jogo-batalha-naval
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define TAMANHO 5
#define NAVIOS 3

void inicializarTabuleiro(char tabuleiro[TAMANHO][TAMANHO]) {
    for (int i = 0; i < TAMANHO; i++) {
        for (int j = 0; j < TAMANHO; j++) {
            tabuleiro[i][j] = '~';
        }
    }
}

void exibirTabuleiro(char tabuleiro[TAMANHO][TAMANHO]) {
    printf("\n  ");
    for (int i = 0; i < TAMANHO; i++) {
        printf("%d ", i);
    }
    printf("\n");

    for (int i = 0; i < TAMANHO; i++) {
        printf("%d ", i);
        for (int j = 0; j < TAMANHO; j++) {
            printf("%c ", tabuleiro[i][j]);
        }
        printf("\n");
    }
}

void posicionarNavios(char tabuleiro[TAMANHO][TAMANHO]) {
    srand(time(NULL));
    int naviosPosicionados = 0;
    while (naviosPosicionados < NAVIOS) {
        int x = rand() % TAMANHO;
        int y = rand() % TAMANHO;

        if (tabuleiro[x][y] == '~') {
            tabuleiro[x][y] = 'N';
            naviosPosicionados++;
        }
    }
}

int atirar(char tabuleiroEscondido[TAMANHO][TAMANHO], char tabuleiroVisivel[TAMANHO][TAMANHO], int x, int y) {
    if (tabuleiroEscondido[x][y] == 'N') {
        printf("\nFOGO! Voce acertou um navio!\n");
        tabuleiroVisivel[x][y] = 'X';
        tabuleiroEscondido[x][y] = 'X';
        return 1;
    } else {
        printf("\nAGUA! Voce errou.\n");
        tabuleiroVisivel[x][y] = 'O';
        return 0;
    }
}

int main() {
    char tabuleiroEscondido[TAMANHO][TAMANHO];
    char tabuleiroVisivel[TAMANHO][TAMANHO];
    int tentativas = 10;
    int acertos = 0;
    int x, y;

    inicializarTabuleiro(tabuleiroVisivel);
    inicializarTabuleiro(tabuleiroEscondido);
    posicionarNavios(tabuleiroEscondido);

    printf("--- Batalha Naval ---\n");
    printf("Voce tem %d tentativas para afundar %d navios.\n", tentativas, NAVIOS);

    while (tentativas > 0 && acertos < NAVIOS) {
        exibirTabuleiro(tabuleiroVisivel);
        printf("\nTentativas restantes: %d\n", tentativas);
        printf("Digite as coordenadas (linha e coluna): ");
        scanf("%d %d", &x, &y);

        if (x < 0 || x >= TAMANHO || y < 0 || y >= TAMANHO) {
            printf("\nCoordenadas invalidas! Tente novamente.\n");
            continue;
        }

        if (tabuleiroVisivel[x][y] != '~') {
            printf("\nVoce ja atirou nesta posicao! Tente novamente.\n");
            continue;
        }

        if (atirar(tabuleiroEscondido, tabuleiroVisivel, x, y)) {
            acertos++;
        }

        tentativas--;

        if (acertos == NAVIOS) {
            printf("\nParabens! Voce afundou todos os navios! 🏆\n");
            exibirTabuleiro(tabuleiroVisivel);
        } else if (tentativas == 0) {
            printf("\nFim de jogo! Voce ficou sem tentativas. 👎\n");
            printf("Os navios estavam em:\n");
            for(int i = 0; i < TAMANHO; i++){
                for(int j = 0; j < TAMANHO; j++){
                    if(tabuleiroEscondido[i][j] == 'N'){
                        tabuleiroVisivel[i][j] = 'N';
                    }
                }
            }
            exibirTabuleiro(tabuleiroVisivel);
        }
    }

    return 0;
}
