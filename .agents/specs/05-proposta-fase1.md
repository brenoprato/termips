# 05 — Proposta do Projeto — Fase 1 (Submissão da Ideia em 26/10)

> Espelho idêntico a `docs/proposta-fase1.md`, escrito de forma clara e direta para preenchimento dos campos `[PREENCHER]` e envio no formulário da Fase 1.

---

## 1. Nome Provisório, Grupo e Integrantes

- **Nome provisório do produto:** TerMIPS
- **Integrantes:**
  - Breno Porto Pinheiro do Prado
  - Felipe Pereira Silva
  - Guilherme Bento Ramos
  - José Rubens Freitas Pinheiro
  - Luis Fernando Giaquinto Zólio

---

## 2. Objetivo, Público Previsto e Interação Principal

- **Objetivo:** Desenvolver o **TerMIPS**, um jogo de adivinhação de palavras no estilo *Termo / Wordle* em Assembly MIPS executado no MARS. O objetivo do jogador é descobrir uma palavra secreta de 5 letras em até 6 tentativas utilizando as pistas exibidas após cada jogada.
- **Público previsto:** Estudantes, professora e monitores de Arquitetura e Organização de Computadores (ICT-Unifesp), além de usuários interessados em jogos rápidos de raciocínio lógico e palavras no terminal.
- **Interação principal:** Ao iniciar o programa, o usuário encontra um menu principal com três opções: **Jogar** (`1`), **Tutorial** (`2`, que exibe um texto explicativo de como o jogo e os símbolos de dica funcionam) e **Sair** (`0`). Ao iniciar uma partida, o programa sorteia uma palavra secreta e, a cada rodada, o jogador digita um palpite de 5 letras pelo teclado. O programa valida a entrada, mostra o histórico de todas as tentativas já feitas na partida indicando letras na posição exata, letras em outra posição e letras ausentes, e segue até a vitória ou o fim das 6 tentativas (revelando a palavra secreta ao final).

---

## 3. Entradas, Processamento, Saídas e Exemplo de Uso

- **Entradas:** Escolha de opção no menu inicial (`1` para jogar, `2` para ler o tutorial, `0` para sair) e palavras de 5 letras digitadas no console a cada rodada (aceitando letras maiúsculas ou minúsculas, além de `0` caso o jogador queira encerrar antes do fim).
- **Processamento:**
  1. Sorteio de uma palavra secreta a partir de uma lista interna de palavras de 5 letras.
  2. Verificação da entrada: confere se tem exatamente 5 caracteres, se todos são letras de `A` a `Z`, converte minúsculas em maiúsculas e recusa entradas inválidas sem descontar tentativas do jogador.
  3. Comparação entre o palpite e a palavra secreta em duas etapas para lidar corretamente com letras repetidas: primeiro marca as letras que já estão na posição exata e depois verifica, entre as letras que sobraram, quais existem em outra posição.
  4. Armazenamento da jogada no histórico da partida e controle do limite de 6 rodadas.
- **Saídas:** Texto do tutorial (quando solicitado), painel de tentativas acumuladas em texto (`[X]` para posição certa, `(X)` para letra presente em outra posição e ` . ` para letra ausente), mensagens claras de aviso para entradas inválidas e mensagem final de vitória ou fim de tentativas.

### Exemplo de Uso (Palavra secreta da rodada: `TERMO`)

```text
Menu: [1] Jogar  [2] Tutorial  [0] Sair
Opcao: 1

Tentativa 1/6 (ou 0 para sair): sol
Aviso: digite exatamente 5 letras. A tentativa nao foi descontada.

Tentativa 1/6 (ou 0 para sair): morro
[1/6] MORRO -> (M)  .  [R]  .  [O]

Tentativa 2/6 (ou 0 para sair): termo
[1/6] MORRO -> (M)  .  [R]  .  [O]
[2/6] TERMO -> [T] [E] [R] [M] [O]

Parabens! Voce descobriu a palavra em 2/6 tentativas!
```

---

## 4. Como os Requisitos de MIPS Serão Incorporados

- **Vetores, matrizes e strings:** Um banco de cerca de 200 palavras de 5 letras será armazenado na memória de dados, junto com estruturas para guardar a palavra secreta da rodada e o histórico das até 6 tentativas e seus resultados.
- **Decisões e repetições:** Laços e desvios condicionais controlarão o menu, o limite de 6 tentativas, a varredura caractere a caractere na validação da entrada e a lógica de comparação de letras (incluindo o tratamento de letras repetidas).
- **Procedimentos, registradores e pilha:** O código será dividido em procedimentos curtos e bem definidos (exibição de menu/tutorial, sorteio da palavra, leitura e validação, avaliação da tentativa, gravação e impressão do histórico), passando parâmetros e retornos pelos registradores adequados e usando a pilha (`$sp`) para preservar endereços de retorno e registradores em chamadas aninhadas.
- **Tratamento de erros e encerramento:** Opções inexistentes no menu ou palpites vazios, curtos, longos ou com números/símbolos serão barrados com aviso amigável sem gastar rodada, e a opção `0` permitirá encerrar o programa de forma limpa a qualquer momento.

---

## 5. Escopo Mínimo, Divisão Inicial de Tarefas e Link do GitHub

- **Escopo mínimo (MVP):** Jogo completo em texto no console do MARS com menu (`Jogar`, `Tutorial` e `Sair`), palavras de 5 letras sem acento, até 6 tentativas, banco interno de ~200 palavras com sorteio (e uma constante interna configurável no código para fixar a palavra durante os testes reproduzíveis), validação de entradas, regra explícita para letras repetidas e exibição do histórico da partida. *(Ideias extras, como exibir um painel das letras do alfabeto já testadas ou ler palavras de arquivo `.txt`, ficam apenas como extensões opcionais se sobrar tempo).*
- **Divisão inicial de tarefas (dividida em 5 frentes entre os integrantes):**
  1. **Parte 1:** Menu principal (`Jogar`, `Tutorial`, `Sair`), organização dos dados em memória, banco de palavras e sorteio.
  2. **Parte 2:** Leitura pelo teclado, conversão de minúsculas para maiúsculas e validação de entradas inválidas.
  3. **Parte 3:** Lógica de comparação das palavras e regra em duas etapas para letras repetidas.
  4. **Parte 4:** Armazenamento do histórico de jogadas, exibição do tabuleiro no console e condições de vitória/derrota.
  5. **Parte 5:** Bateria de testes reproduzíveis no MARS, documentação (`ficha-tecnica.md`, `testes.md`, `uso-ia.md`), gravação do vídeo e revisão conjunta de todo o código para a prova individual.
- **Link do repositório no GitHub:** `https://github.com/brenoprato/termips`
