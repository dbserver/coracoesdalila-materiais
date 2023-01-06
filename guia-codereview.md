# **Guia para Code Review**

## *Observações gerais*

- [ ]  Descrição do PR (Link do card, link da US ou descrição da issue/bug, comentários do desenvolvedor)

- [ ] O nome da branch segue o padrão acordado pelo time

- [ ] O destino do PR é a branch de desenvolvimento

## *Código*

- [ ] Nome de variáveis/métodos em bom entendimento e em português

- [ ] Procurar prints e consoleLogs de debug

- [ ] Imports adicionados que não são usados

- [ ] [FRONT] Erros no console

- [ ] Comentários desnecessários ao longo do código (cuidado: comentários explicativos são bem vindos)

- [ ] Verificar os testes no front e backend (se estão passando)

- [ ] Todos os métodos/variáveis adicionados ao código são usados?

- [ ] Existem substituições de código que podem melhorar a eficiência do sistema?

- [ ] Existem substituições que podem melhorar o código no sentido do clean code? (Métodos muito longos que podem ser “quebrados”, código muito extenso que pode ser mais clean, etc.)

## *Testes*

- [ ] [Novas funcionalidades] Foram adicionados todos os testes unitários? Se não, o time está ciente?

- [ ] Os testes estão abordando tanto caminhos felizes quanto caminhos infelizes?

## *Finalização*

- [ ] Existem conflitos com a dev?

- [ ] Existem alterações no banco de dados a serem confirmados antes de dar o merge para a dev?