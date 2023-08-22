# Tetris

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