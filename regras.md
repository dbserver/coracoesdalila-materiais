# Guia de Regras para implementação - Corações da Lila

## 1. Comprar 1 carta
        link da api: jogada/comprarcarta


O jogador compra uma carta se possuir corações suficientes para comprá-la.

   Se a carta tiver bônus o jogador rola um dado e recebe de recompensa o resultado do respectivo bônus.

---

### Algoritmo

1. O front habilita para clicar apenas as cartas que o jogador pode comprar e as destaca.
    (levar em consideração os bônus que o jogador possui)

2. O jogador vai clicar na carta que deseja comprar dentre as que estão destacadas.

3. O front vai retirar a carta da lista da mesa e adicionar na lista do jogador.

4. O front vai enviar o status da sala para o back através da rota http específica deste tipo jogada.

5. O back valida a jogada:

    - verifica qual jogador tem o status de JOGANDO.
    - verifica se o jogador possui corações suficientes para realizar a compra.
    - adiciona os pontos da carta ao jogador.
    - verifica se a carta possui bônus, se sim roda o dado (funcao de random).
    - verifica qual o resultado do dado e ja aplica esse resultado ao estado do jogo.
    - altera o status do jogador para ESPERANDO e ja atualiza o próximo jogador como JOGANDO
        
6. O back finaliza a jogada

    - verifica se o jogador possui 8 pontos. (se possuir a sala deve alterar seu status para ULTIMA_RODADA).
    - verifica se o próximo jogador é quem começou o jogo (jogador que iniciou a sala) e se a sala está na ULTIMA_RODADA para finalizar o jogo.
    - Se o jogo deve ser [finalizado](#finalizar-o-jogo) o status da sala deve ser alterado para FINALIZADO.
    
7. Back atualiza a sala no banco.

8. Back devolve a sala para o front através do websocket.
   
9. verifica se a carta possui bonus.
    - Se a carta comprada tiver bônus o front pede para o usuario girar o dado que terá o resultado de acordo com o enviado pelo back.

10. O front atualiza o status da sala para todos
11. O front verifica se o jogo está FINALIZADO (como descrito [aqui](#finalizar-o-jogo))
12. Se não estiver finalizado o front verifica quem é o jogador com JOGANDO e libera a jogada para ele.

## 2. Comprar 1 carta Objetivo
        link da api: jogada/comprarobjetivo

O jogador compra uma carta objetivo se possuir 1 coração de qualquer tipo.

- - -
### Algoritmo
1. O front habilita o baralho de cartas objetivo apenas se o jogador atual (marcado com JOGANDO) possui 1 coração (de qualquer tipo).

2. O jogador clica no baralho de cartas objetivos

3. O front remove uma carta do baralho de objetivos e adiciona na lista de cartas objetivos do jogador.

4. O front envia para o back a alteração do status da sala no endereço http referente a esta jogada.

5. O back valida a jogada:
    - Verifica qual jogador tem o status JOGANDO.
    - Verifica se o jogador possui corações suficientes para comprar a carta objetivo.
    - Remove o numero de corações referente a compra de objetivos (priorizando os corações pequenos)
    - Altera o status do jogador para ESPERANDO e ja atualiza o próximo jogador como JOGANDO.

6. Altera a informação da sala no banco.

7. O back envia para o front a sala alterada através do websocket.

8. O front atualiza o status da sala para todos.

9. O front verifica se o jogo está FINALIZADO (como descrito [aqui](#finalizar-o-jogo))

10. Se não estiver finalizado o front verifica quem é o jogador com JOGANDO e libera a jogada para ele.


## 3. Comprar corações

O jogador pode optar por comprar 2 corações pequenos ou 1 coração grande se ele tiver menos de 5 corações de qualquer tipo.

---

1. O front habilita os corações para comprar se o jogador do turno tiver menos de 5 corações de qualquer tipo.

### 3.1 Comprar 1 coração grande
        link da api: jogada/comprarcoracaogrande

2. O jogador clica com o mouse no coração grande.

3. O front adiciona 1 coração grande para o jogador.

4. O front atualiza a sala com as alterações do jogador.

5. O front envia a sala alterada para o back através da requisição http na rota referente à jogada.

6. O back recebe o status da sala.

7. O back salva as alterações.

8. O back envia a sala salva para o front através do websocket.

9. O front recebe a sala salva.

10. O front atualiza o status da sala para todos.

11. O front verifica se o jogo está FINALIZADO (como descrito [aqui](#finalizar-o-jogo))

12. Se não estiver finalizado o front verifica quem é o jogador com JOGANDO e libera a jogada para ele.



### 3.2 Comprar 2 corações pequenos
        link da api: jogada/comprarcoracaopequeno
2. O jogador clica no icone de corações pequenos.

3. O front adiciona 2 corações pequenos para o jogador.

4. O front atualiza a sala com as alterações do jogador.

5. O front envia a sala alterada para o back através da requisição http na rota referente à jogada.

6. O back recebe o status da sala.

7. O back salva as alterações.

8. O back envia a sala salva para o front através do websocket.

9. O front recebe a sala salva.

10. O front atualiza o status da sala para todos.

11. O front verifica se o jogo está FINALIZADO (como descrito [aqui](#finalizar-o-jogo))

12. Se não estiver finalizado o front verifica quem é o jogador com JOGANDO e libera a jogada para ele.

## Finalizar o jogo
Quando alguém atingir 8 pontos de cartas, as jogadoras que faltam para completar o turno jogam mais uma vez. 

Os pontos dos objetivos são contados após o encerramento.

Após a contagem de pontos dos objetivos, o jogador com mais pontos é o vencedor.

- - -
## Back End
1. Verifica se a sala está com o status ULTIMA_RODADA.
2. Verifica se o próximo jogador é quem iniciou o jogo(o criador da sala).

3. ***Contabiliza os pontos da carta de objetivo***
    
4. Altera o status do jogo para FINALIZADO.

## Front End
1. O front verifica se o status do jogo é FINALIZADO

2. O front mostra um ranking com as pontuações dos jogadores (da maior para a menor).

Exemplo: 
```
    Placar final:
    Lila: 23
    José: 20
    ...
```