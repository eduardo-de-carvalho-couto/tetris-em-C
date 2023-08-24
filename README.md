# Tetris

## Desenvolvedores

- Eduardo de Carvalho Couto
- Ryan Ramon da Silva Bezerra
- Mayquias Sérgio Freitas da Costa

## Objetivo

O Tetris consiste em empilhar blocos que descem a tela, numa velocidade que cresce gradativamente enquanto o jogo evolui, de forma que completem linhas horizontais. Quando uma linha se forma, ela se desfaz, as camadas superiores caem, e o jogador ganha pontos até sua finalização seja por meio de pontos acumulados, tempo ou derrota do jogador.

## Menu Principal

O jogo apresenta um menu principal com as seguintes opções:

1. **Iniciar Jogo:** Inicia uma nova partida do Tetris.
2. **Classificação:** Mostra um ranking das melhores pontuações alcançadas por jogadores.
3. **Sair:** Encerra o jogo.

![09fa020c-4777-4dcb-98ef-d3c3d874f4ca](https://github.com/eduardo-de-carvalho-couto/tetris-em-C/assets/101133249/175ae1a3-fc3e-414b-9a62-82fe36788507)


O jogador escolhe entre essas opções usando o teclado.

![668b7886-6726-412c-9fbd-e11066853971](https://github.com/eduardo-de-carvalho-couto/tetris-em-C/assets/101133249/6855f2ac-4996-4d8e-9933-6bdf2f515ae6)


## Jogabilidade

### Movimento e Rotação das Peças

Ao iniciar o jogo, o jogador é apresentado a uma tela onde as peças tetris caem do topo da tela. As peças podem ser movidas para a esquerda ou direita usando as teclas 'a' e 'd', respectivamente. Pressionar 's' acelera a queda da peça. A tecla 'w' permite ao jogador girar a peça no sentido horário. A peça se move para baixo automaticamente em intervalos regulares.

![8f9abd7b-b33f-43b7-9d74-5b53840f7a4a](https://github.com/eduardo-de-carvalho-couto/tetris-em-C/assets/101133249/a796a0fe-28c5-49ad-b892-6b684e81d8a9)


## Eliminação de Linhas

Quando uma linha horizontal é preenchida com quadrados das peças, ela é eliminada do campo de jogo, e o jogador ganha pontos. Eliminar múltiplas linhas de uma vez concede mais pontos. As linhas acima da linha eliminada caem para preencher o espaço vazio.

## Aumento de Dificuldade

Conforme o jogador acumula uma pontuação de 1000 pontos, o nível de dificuldade aumenta. Isso torna as peças tetris mais rápidas na queda, desafiando o jogador a reagir mais rapidamente e a tomar decisões mais precisas.

## Fim do Jogo

O jogo termina quando uma nova peça tetris não pode ser acomodada no campo de jogo sem ultrapassar o topo da tela. Nesse momento, a partida é encerrada, e o jogador recebe informações sobre sua pontuação final e tempo de jogo.

## Classificação e Ranking

O jogo mantém um ranking das melhores pontuações dos jogadores. As pontuações são registradas junto com os nomes dos jogadores. Os jogadores podem visualizar o ranking acessando a opção "Ranking" no menu principal. O ranking exibe os jogadores com as pontuações mais altas, incentivando os jogadores a competirem por melhores resultados.

![2e3d47b5-9fd8-4ee8-85ed-c8f5d0042f25](https://github.com/eduardo-de-carvalho-couto/tetris-em-C/assets/101133249/f7f994a5-9006-4f3e-ba30-35bbaadd7710)

## Pontuação

A pontuação do jogador aumenta ao eliminar linhas completas no campo de jogo. A quantidade de pontos ganhos varia de acordo com o número de linhas eliminadas simultaneamente. Quanto mais linhas forem eliminadas ao mesmo tempo, mais pontos o jogador receberá. A pontuação é uma parte central da jogabilidade, incentivando os jogadores a buscarem formas eficientes de preencher as linhas.

## Código

#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>
#include <conio.h>
#include <time.h>
#include <windows.h>

#define MAX_JOGADORES 10

//Estrutura que representa o jogador
typedef struct {
    char nomeJogador[50];
    int pontuacao;
} Jogador;

void criarCampo(void);
void exibirCampo(void);
void jogar(void);
void posicionarPeca(void);
void atualizarCampo(bool p);
void moverHorizontalmente(char r);
void soltarPeca(void);
bool descerPeca(bool livre);
void controlarLinhas(void);
void rotacionar(void);
void atualizarTemporizador(void);
void moverAutomaticamente(void);
void salvarPontuacaoDoJogador(Jogador jogador);
void carregarPontuacoesDosJogadores(Jogador jogadores[], int *numJogadores);
void atualizarClassificacao(Jogador jogadores[], int numJogadores);
void exibirClassificacao(Jogador jogadores[], int numJogadores);
void exibirMenu(void);
void iniciarJogo(void);
void aumentarDificuldade(void);
void verificarVitoria(void);

_Bool campo[10][20];
bool vivo;
int estado;
int etapa;
int xPeca[4];
int yPeca[4];
bool peca;
int pecas[7][4][2] = {
    {{3, 1}, {4, 1}, {5, 1}, {6, 1}}, // pe�a reta
    {{3, 1}, {3, 2}, {4, 2}, {5, 2}}, // LL
    {{3, 2}, {4, 2}, {5, 2}, {5, 1}}, // RL
    {{4, 1}, {5, 1}, {4, 2}, {5, 2}}, // quadrado
    {{3, 1}, {4, 1}, {4, 2}, {5, 2}}, // LZ
    {{3, 2}, {4, 2}, {4, 1}, {5, 1}}, // RZ
    {{3, 2}, {4, 2}, {5, 2}, {4, 1}}  // pe�a carro
};

int rotacao[7][4][4][2] = {
    { // pe�a reta
        {{1, -1}, {0, 0}, {-1, 1}, {-2, 2}},
        {{-1, 1}, {0, 0}, {1, -1}, {2, -2}},
        {{1, -1}, {0, 0}, {-1, 1}, {-2, 2}},
        {{-1, 1}, {0, 0}, {1, -1}, {2, -2}}
    },
    { // LL
        {{2, 0}, {1, -1}, {0, 0}, {-1, 1}},
        {{0, 2}, {1, 1}, {0, 0}, {-1, -1}},
        {{-2, 0}, {-1, 1}, {0, 0}, {1, -1}},
        {{0, -2}, {-1, -1}, {0, 0}, {1, 1}}
    },
    { // RL
        {{1, -1}, {0, 0}, {-1, 1}, {0, 2}},
        {{1, 1}, {0, 0}, {-1, -1}, {-2, 0}},
        {{-1, 1}, {0, 0}, {1, -1}, {0, -2}},
        {{-1, -1}, {0, 0}, {1, 1}, {2, 0}}
    },
    { // quadrado
        {{0, 0}, {0, 0}, {0, 0}, {0, 0}},
        {{0, 0}, {0, 0}, {0, 0}, {0, 0}},
        {{0, 0}, {0, 0}, {0, 0}, {0, 0}},
        {{0, 0}, {0, 0}, {0, 0}, {0, 0}}
    },
    { // LZ
        {{2, 0}, {1, 1}, {0, 0}, {-1, 1}},
        {{-2, 0}, {-1, -1}, {0, 0}, {1, -1}},
        {{2, 0}, {1, 1}, {0, 0}, {-1, 1}},
        {{-2, 0}, {-1, -1}, {0, 0}, {1, -1}}
    },
    { // RZ
        {{1, -1}, {0, 0}, {1, 1}, {0, 2}},
        {{-1, 1}, {0, 0}, {-1, -1}, {0, -2}},
        {{1, -1}, {0, 0}, {1, 1}, {0, 2}},
        {{-1, 1}, {0, 0}, {-1, -1}, {0, -2}}
    },
    { // pe�a carro
        {{1, -1}, {0, 0}, {-1, 1}, {1, 1}},
        {{1, 1}, {0, 0}, {-1, -1}, {-1, 1}},
        {{-1, 1}, {0, 0}, {1, -1}, {-1, -1}},
        {{-1, -1}, {0, 0}, {1, 1}, {1, -1}}
    }
};

char nomeJogador[50];
time_t tempoInicio;
time_t tempoAtual;
int tempoDeJogo;
int nivel;
int pontuacao = 0;

// Função principal que inicia o jogo e exibe o menu
int main(void) {
    exibirMenu();
    iniciarJogo();

    return 0;
}

// Exibe o menu
void exibirMenu(void) {
    printf("===== Menu =====\n");
    printf("1. Iniciar Jogo\n");
    printf("2. Classificacao\n");
    printf("3. Sair\n");
    printf("================\n");

    int escolha;
    printf("Digite a opcao desejada: ");
    scanf("%d", &escolha);

    switch (escolha) {
        case 1:
            break;
        case 2:
            {
                Jogador jogadores[MAX_JOGADORES];
                int numJogadores = 0;
                carregarPontuacoesDosJogadores(jogadores, &numJogadores);
                exibirClassificacao(jogadores, numJogadores);
            }
            break;
        case 3:
            exit(0);
        default:
            printf("Opcao invalida. Tente novamente.\n");
            exibirMenu();
            break;
    }
}

// Função que inicia o jogo
void iniciarJogo(void) {
    pontuacao = 0;
    peca = false;
    nivel = 1;
    criarCampo();

    printf("Digite o nome do jogador: ");
    scanf("%49s", nomeJogador);

    posicionarPeca();
    tempoInicio = time(NULL);
    jogar();
}

// Função principal do jogo que controla a lógica do jogo em execução
void jogar(void) {
    vivo = true;
    char r = ' ';
    time_t ultimoMovimentoDescer = time(NULL);
    while(vivo) {
        if (_kbhit()) {
            r = _getch();
            switch(r) {
                case 'w': rotacionar(); break;
                case 's': soltarPeca(); break;
                case 'f':
                    descerPeca(true);
                    atualizarCampo(true);
                    break;
                case 'a': moverHorizontalmente(r); break;
                case 'd': moverHorizontalmente(r); break;
                case 'q': vivo = false; break;
                case 'p':
                    printf("\nJogo pausado. Pressione qualquer tecla para continuar...\n");
                    _getch();
                    break;
                case 27: // ESC
                    vivo = false;
                    break;
                default:
                    exibirCampo();
            }
        }

        time_t tempoAtual = time(NULL);
        if (difftime(tempoAtual, ultimoMovimentoDescer) >= 1.0) {
            moverAutomaticamente();
            ultimoMovimentoDescer = tempoAtual;
        }

        atualizarTemporizador();
        verificarVitoria();
    }

    tempoAtual = time(NULL);
    tempoDeJogo = difftime(tempoAtual, tempoInicio);
    printf("\nFim de Jogo!\nTempo de Jogo: %d segundos\nPontuacao: %d\n", tempoDeJogo, pontuacao);

    Jogador jogadorAtual;
    strcpy(jogadorAtual.nomeJogador, nomeJogador);
    jogadorAtual.pontuacao = pontuacao;

    salvarPontuacaoDoJogador(jogadorAtual);

    Jogador jogadores[MAX_JOGADORES];
    int numJogadores = 0;
    carregarPontuacoesDosJogadores(jogadores, &numJogadores);

    atualizarClassificacao(jogadores, numJogadores);
    exibirClassificacao(jogadores, numJogadores);
}

// Função que gira a peça
void rotacionar(void) {
    int xTemp[4];
    int yTemp[4];
    bool livre = true;
    for(int i = 0; i < sizeof(xPeca) / sizeof(int); i++) {
        xTemp[i] = xPeca[i];
        yTemp[i] = yPeca[i];
    }
    for(int i2 = 0; i2 < sizeof(xPeca) / sizeof(int); i2++) {
        if(xPeca[i2] + rotacao[estado][etapa][i2][0] < 0 ||
            xPeca[i2] + rotacao[estado][etapa][i2][0] >= sizeof(campo) / sizeof(campo[0]) ||
                yPeca[i2] + rotacao[estado][etapa][i2][1] < 0 ||
                    yPeca[i2] + rotacao[estado][etapa][i2][1] >= sizeof(campo[0])) {
                        livre = false;                    
        }
        if(livre) {
            for(int i = 0; i < sizeof(xPeca) / sizeof(int); i++) {
                campo[xTemp[i]][yTemp[i]] = 0;
            }
        }
        if(livre && 
            campo[xPeca[i2] + rotacao[estado][etapa][i2][0]][yPeca[i2] + rotacao[estado][etapa][i2][1]] != 1) {
                xPeca[i2] += rotacao[estado][etapa][i2][0];
                yPeca[i2] += rotacao[estado][etapa][i2][1];
        } else {
            livre = false;
        }
    }
    if(!livre) {  
        for(int i = 0; i < sizeof(xPeca) / sizeof(int); i++) {
            xPeca[i] = xTemp[i];
            yPeca[i] = yTemp[i];
            campo[xTemp[i]][yTemp[i]] = 1;
        }
    } else {
        etapa = etapa == 3 ? 0 : etapa + 1;
    }
    atualizarCampo(true);
}

// Função que controla a eliminação de linhas completas e atualiza a pontuação
void controlarLinhas(void) {
    int linhas = 0;
    for(int y = 0; y < sizeof(campo[0]); y++) {
        bool cheio = true;
        for(int x = 0; x < sizeof(campo) / sizeof(campo[0]); x++) {
            if(!campo[x][y]) {
                cheio = false;
                break;
            }
        }
        if(cheio) {
            linhas++;
            for(int x = 0; x < sizeof(campo) / sizeof(campo[0]); x++) {
                campo[x][y] = false;
            }
            for(int y2 = y; y2 > 0; y2--) {
                for(int x = 0; x < sizeof(campo) / sizeof(campo[0]); x++) {
                    campo[x][y2] = campo[x][y2 - 1];
                }
            }
        }
    }
    switch(linhas) {
        case 1: pontuacao += 10; break;
        case 2: pontuacao += 25; break;
        case 3: pontuacao += 55; break;
        case 4: pontuacao += 80; break;
    }
    if(linhas > 0) {
        exibirCampo();
    }
}

// Função que faz a peça cair rapidamente até o fundo do campo
void soltarPeca() {
    bool livre = true;
    while(livre) {
        livre = descerPeca(livre);
    }
}

// Função que faz a peça cair uma posição no campo e atualiza o campo
bool descerPeca(bool livre) {
    for(int i = 0; i < sizeof(xPeca) / sizeof(int); i++) {
        if(yPeca[i] + 1 >= sizeof(campo[0])) {
            livre = false;
            break;
        }
        if(campo[xPeca[i]][yPeca[i] + 1] && livre) {
            bool outra = true;
            for(int i2 = 0; i2 < sizeof(xPeca) / sizeof(int); i2++) {
                if(xPeca[i] == xPeca[i2] && yPeca[i] + 1 == yPeca[i2]) {
                    outra = false;
                    break;
                }
            }
            if(outra){
                livre = false;
                break;
            }
        }
    }
    if(livre) {
        for(int i = 0; i < sizeof(xPeca) / sizeof(int); i++) {
            campo[xPeca[i]][yPeca[i]] = false;
            yPeca[i] += 1;
        }
    } else {
        peca = false;
        atualizarCampo(false);
        controlarLinhas();
        posicionarPeca();
        aumentarDificuldade();
    }
    return livre;
}

// Função que move a peça horizontalmente
void moverHorizontalmente(char r) {
    int d = r == 'a' ? -1 : 1;
    bool permitido = true;
    bool novaPeca = false;
    for(int i = 0; i < sizeof(xPeca) / sizeof(int); i++) {
        if(xPeca[i] + d >= sizeof(campo) / sizeof(campo[0]) || xPeca[i] + d < 0) {
            permitido = false;
            break;
        }
        if(campo[xPeca[i] + d][yPeca[i]] && permitido) {
            bool outra = true;
            for(int i2 = 0; i2 < sizeof(xPeca) / sizeof(int); i2++) {
                if(xPeca[i] + d == xPeca[i2] && yPeca[i] == yPeca[i2]) {
                    outra = false;
                    break;
                }
            }
            if(outra) {
                permitido = false;
                novaPeca = true;
                break;
            }
        }
    }
    if(permitido) {
        for(int i = 0; i < sizeof(xPeca) / sizeof(int); i++) {
            campo[xPeca[i]][yPeca[i]] = false;
            xPeca[i] += d;
        }
    } else if(novaPeca) {
        peca = false;
    }
    atualizarCampo(true);
}

// Função que posiciona uma nova peça no topo do campo
void posicionarPeca(void) {
    srand(time(NULL));
    estado = rand() % 7;
    bool livre = true;
    for(int i = 0; i < sizeof(pecas[0]) / (2 * sizeof(int)); i++) {
        xPeca[i] = pecas[estado][i][0];
        yPeca[i] = pecas[estado][i][1];
        if(campo[xPeca[i]][yPeca[i]]) {
            livre = false;
            break;
        }
    }
    etapa = 0;
    peca = true;
    if(livre) {
        atualizarCampo(true);
    } else {
        vivo = false;
    }
}

// Função que atualiza o campo e exibe a visualização do jogo
void atualizarCampo(bool p) {
    for(int i = 0; i < sizeof(xPeca) / sizeof(int); i++) {
        campo[xPeca[i]][yPeca[i]] = true;
    }
    if(p) {
        exibirCampo();
    }
}

// Função que cria o campo de jogo
void criarCampo(void) {
    for(int y = 0; y < sizeof(campo[0]); y++) {
        for(int x = 0; x < sizeof(campo) / sizeof(campo[0]); x++) {
            campo[x][y] = false;
        }
    }
}

// Função que exibe o campo de jogo
void exibirCampo(void) {
    system("cls");
    
    // Exibe a borda superior do ret�ngulo
    for (int i = 0; i < (sizeof(campo) / sizeof(campo[0])) * 2 + 2; i++) {
        printf("#");
    }
    printf("\n");

    for (int y = 0; y < sizeof(campo[0]); y++) {
        printf("#"); // Lado esquerdo do ret�ngulo
        
        // Exibe o campo de jogo
        for (int x = 0; x < sizeof(campo) / sizeof(campo[0]); x++) {
            printf("%s", campo[x][y] ? "[]" : "  ");
        }
        
        printf("#"); // Lado direito do ret�ngulo
        
        // Exibe as informa��es do jogador (nome, pontua��o e temporizador) na lateral direita
        if (y == 0) {
            printf(" %s", nomeJogador);
        } else if (y == 1) {
            printf(" Pontuacao: %d", pontuacao);
        } else if (y == 2) {
            printf(" Tempo: %d segundos", tempoDeJogo);
        } else {
            printf(" ");
        }
        
        printf("\n");
    }
    
    // Exibe a borda inferior do ret�ngulo
    for (int i = 0; i < (sizeof(campo) / sizeof(campo[0])) * 2 + 2; i++) {
        printf("#");
    }
    printf("\n>");
}

// Função que atualiza o temporizador do jogo
void atualizarTemporizador(void) {
    tempoAtual = time(NULL);
    tempoDeJogo = difftime(tempoAtual, tempoInicio);
}

// Função que move automaticamente a peça para baixo
void moverAutomaticamente(void) {
    if (descerPeca(true)) {
        atualizarCampo(true);
    }
}

// Função que salva a pontuação do jogador em um arquivo
void salvarPontuacaoDoJogador(Jogador jogador) {
    FILE *arquivo = fopen("classificacao.txt", "a");
    if (arquivo == NULL) {
        printf("Erro ao abrir o arquivo.\n");
        return;
    }
    fprintf(arquivo, "%s %d\n", jogador.nomeJogador, jogador.pontuacao);
    fclose(arquivo);
}

// Função que carrega as pontuações dos jogadores a partir de um arquivo
void carregarPontuacoesDosJogadores(Jogador jogadores[], int *numJogadores) {
    FILE *arquivo = fopen("classificacao.txt", "r");
    if (arquivo == NULL) {
        printf("Erro ao abrir o arquivo.\n");
        return;
    }
    *numJogadores = 0;
    while (fscanf(arquivo, "%s %d", jogadores[*numJogadores].nomeJogador, &jogadores[*numJogadores].pontuacao) == 2) {
        (*numJogadores)++;
    }
    fclose(arquivo);
}

// Função que atualiza a classificação das pontuações dos jogadores
void atualizarClassificacao(Jogador jogadores[], int numJogadores) {
    for (int i = 0; i < numJogadores; i++) {
        for (int j = i + 1; j < numJogadores; j++) {
            if (jogadores[j].pontuacao > jogadores[i].pontuacao) {
                Jogador temp = jogadores[i];
                jogadores[i] = jogadores[j];
                jogadores[j] = temp;
            }
        }
    }
}

// Função que exibe a classificação das pontuações dos jogadores
void exibirClassificacao(Jogador jogadores[], int numJogadores) {
    printf("\nClassificacao de Pontuacoes:\n");
    for (int i = 0; i < numJogadores; i++) {
        printf("%d� - %s: %d\n", i + 1, jogadores[i].nomeJogador, jogadores[i].pontuacao);
    }
}

// Função que aumenta a dificuldade do jogo
void aumentarDificuldade(void) {
    if (pontuacao >= 1000) {
        nivel++;
    }
}

// Função que verifica se o jogador alcançou a pontuação de vitória
void verificarVitoria(void) {
    if (pontuacao >= 1000) {
        printf("\nParabens! Voce venceu o jogo!\n");
        vivo = false;
    }
}

