# 01 — Especificação de Produto e Requisitos (`TerMIPS`)

## 1. Visão Geral, Público e Objetivo (Origem: Seções 1, 2 e 4 — Fase 1)

- **Nome do produto:** **TerMIPS** (*Jogo de Adivinhação de Palavras em Assembly MIPS*)
- **Ideia do grupo:** Jogo autoral no estilo *Termo / Wordle* para palavras de 5 letras em português, executado por console no simulador MARS.
- **Objetivo do produto:** Oferecer um jogo interativo de raciocínio lógico por console no simulador MARS em que o jogador deve descobrir uma palavra secreta de 5 letras em até 6 tentativas, recebendo pistas posicionais precisas a cada jogada (inclusive na presença de letras repetidas), consolidando conceitos de organização de memória, estruturas de controle, procedimentos e pilha em Assembly MIPS.
- **Público previsto:** Estudantes de Computação, professora e monitores da disciplina de *Arquitetura e Organização de Computadores* (ICT-Unifesp), além de qualquer usuário interessado em jogos de palavras em terminal.
- **Interação principal:** O jogador interage com um menu principal de três opções: `1` (**Jogar**), `2` (**Tutorial** — texto explicativo de como o jogo funciona) e `0` (**Sair**). Ao jogar, uma palavra de 5 letras é sorteada de um banco de ~200 palavras; a cada turno o jogador digita um palpite de 5 letras (ou `0` para encerrar); o programa valida a entrada, calcula acertos exatos e parciais em duas passadas, armazena a jogada no histórico e exibe o tabuleiro acumulado até a vitória (5 posições exatas) ou o esgotamento das 6 tentativas.

---

## 2. Histórias de Usuário

- **HU-01 (Partida e Tutorial):** Como jogador, quero consultar um tutorial rápido das regras no menu ou iniciar uma partida com palavra secreta sorteada entre cerca de 200 opções e até 6 tentativas para descobri-la.
- **HU-02 (Proteção contra Erros de Digitação):** Como jogador, quero que entradas com tamanho diferente de 5 letras ou com caracteres não alfabéticos sejam recusadas com mensagem explicativa **sem descontar** minhas tentativas, e que letras minúsculas sejam aceitas automaticamente.
- **HU-03 (Feedback Justo em Letras Repetidas):** Como jogador, quero que palpites com letras repetidas recebam indicações coerentes com a quantidade real daquela letra na palavra secreta, priorizando sempre acertos na posição exata.
- **HU-04 (Histórico e Encerramento):** Como jogador, quero visualizar todas as tentativas anteriores da partida atual a cada rodada e poder encerrar o programa a qualquer momento digitando `0`.
- **HU-05 (Avaliação Reproduzível):** Como avaliador(a) ou integrante testando o código, quero poder ativar uma flag simples em `.data` (`usar_palavra_fixa = 1`) para fixar a palavra secreta e repetir exatamente os casos de teste documentados (Seção 6).

---

## 3. Requisitos Funcionais (RF) e Critérios de Aceitação

### RF-01 — Menu Inicial e Tutorial *(Origem: Seção 2; Seção 3, itens 2 e 8)*
O sistema deve exibir um menu principal com três opções: `1` (Jogar), `2` (Tutorial — texto explicativo de como funciona o jogo) e `0` (Sair).
- **Critério 1.1:** **Dado** que o programa iniciou ou uma partida terminou, **Quando** o menu é exibido e o usuário digita `1`, **Então** inicia-se uma nova partida.
- **Critério 1.2:** **Dado** que o menu está ativo, **Quando** o usuário digita `2`, **Então** o sistema imprime o texto explicativo das regras e dos símbolos `[X]`, `(X)` e ` . ` e reapresenta o menu principal.
- **Critério 1.3:** **Dado** que o menu está ativo, **Quando** o usuário digita qualquer opção diferente de `0`, `1` ou `2`, **Então** o sistema exibe mensagem de opção inválida e reapresenta o menu sem travar.

### RF-02 — Seleção da Palavra Secreta no Banco `.data` *(Origem: Seção 2; Seção 3, item 4; Seção 6)*
O sistema deve manter em `.data` um banco contíguo de `N = 200` palavras em português (5 letras ASCII maiúsculas `A-Z`, sem acento, sem palavras duplicadas, terminadas em `\0`, ocupando 6 bytes cada, com endereço `banco_palavras + 6*i`) e copiar a palavra escolhida para `segredo_atual`. Para atender à exigência de testes reproduzíveis (Seção 6), uma flag interna `usar_palavra_fixa` em `.data` (`0` por padrão) permite usar `indice_fixo` sem alterar o menu do jogador.
- **Critério 2.1:** **Dado** que `usar_palavra_fixa == 0` (padrão), **Quando** `selecionar_segredo` executa `syscall 42` com limite superior `N`, **Então** o índice retornado em `$a0` (`0 <= i < N`) é usado para copiar os 5 caracteres de `banco_palavras + 6*i` para `segredo_atual`.
- **Critério 2.2:** **Dado** que `usar_palavra_fixa == 1` e `indice_fixo == 0` em `.data`, **Quando** `selecionar_segredo` executa, **Então** `segredo_atual` recebe deterministicamente `"TERMO"` (`banco_palavras + 0`).

### RF-03 — Leitura de Tentativa e Comando de Saída Imediata *(Origem: Seção 3, itens 2 e 8)*
A cada rodada, o sistema deve informar o número da tentativa atual (`1/6` a `6/6`) e ler a linha digitada via `syscall 8`, reconhecendo `0` como comando imediato de encerramento.
- **Critério 3.1:** **Dado** que o programa aguarda uma tentativa na rodada `k`, **Quando** o usuário digita `0` seguido de Enter, **Então** o programa exibe mensagem de encerramento e finaliza imediatamente via `syscall 10`.

### RF-04 — Validação de Comprimento, Alfabeto e Normalização de Caixa *(Origem: Seção 3, itens 3 e 8)*
O sistema deve validar a string lida antes de contabilizar a jogada: medir o comprimento até `\n` (ASCII 10), `\r` (ASCII 13) ou `\0` (ASCII 0); exigir exatamente 5 caracteres; converter letras minúsculas (`a–z`) em maiúsculas (`A–Z`); rejeitar qualquer caractere fora de `A–Z`. Qualquer sequência de 5 letras `A–Z` é aceita no MVP.
- **Critério 4.1:** **Dado** que o usuário está na tentativa `k/6`, **Quando** digita `"termo\n"` (minúsculas ou caixa mista `"TeRmO\n"`), **Então** a entrada é convertida para `"TERMO"` e aceita como tentativa válida.
- **Critério 4.2:** **Dado** que o usuário está na tentativa `k/6`, **Quando** digita linha vazia (`"\n"`), menos de 5 letras (`"SOL\n"`) ou mais de 5 letras (`"BANANA\n"`), **Então** o sistema exibe erro de tamanho inválido e **mantém** o contador na tentativa `k/6`.
- **Critério 4.3:** **Dado** que o usuário está na tentativa `k/6`, **Quando** digita 5 caracteres contendo número, espaço, símbolo ou byte acentuado (ex.: `"CA5A!\n"`), **Então** o sistema exibe erro de caractere não alfabético e **mantém** o contador na tentativa `k/6`.

### RF-05 — Comparação em Duas Passadas com Tratamento de Letras Repetidas *(Origem: Seção 2, linha 35; Seção 3, item 3)*
O sistema deve comparar a tentativa validada com uma cópia temporária mutável da palavra secreta (`segredo_temp`) em duas passadas explícitas:
1. **Passada 1 (Posição Exata):** Para `i = 0..4`, se `tentativa[i] == segredo_temp[i]`, marca posição `i` como **exata** (`2`) e consome `segredo_temp[i] = '*'`.
2. **Passada 2 (Letra Presente em Outra Posição):** Para cada `i = 0..4` não marcado como exato, procura `j = 0..4` tal que `tentativa[i] == segredo_temp[j]`; se encontrar, marca posição `i` como **presente** (`1`), consome `segredo_temp[j] = '*'` e encerra a busca para este `i`; caso contrário, mantém posição `i` como **ausente** (`0`).
- **Critério 5.1:** **Dado** segredo `"TERMO"` e tentativa `"MORRO"`, **Quando** a avaliação executa, **Então** a Passada 1 marca `[R]` (pos 2) e `[O]` (pos 4) consumindo ambos, e a Passada 2 marca `(M)` (pos 0) e `.` para o `'O'` extra (pos 1) e para o `'R'` extra (pos 3), resultando em `(M)  .  [R]  .  [O]`.
- **Critério 5.2:** **Dado** segredo `"CARTA"` (duas letras `'A'`) e tentativa `"ARARA"` (três letras `'A'`), **Quando** a avaliação executa, **Então** apenas duas ocorrências de `'A'` recebem marcação (`(A)` nas posições 0 e 2) e a terceira (pos 4) resulta em `.` (ausente).

### RF-06 — Armazenamento de Estado e Histórico de Tentativas *(Origem: Seção 2, linha 35; Seção 3, item 4 e Complexidade Mínima)*
A cada tentativa válida, o sistema deve gravar os 5 caracteres da tentativa e os 5 códigos de avaliação nos vetores/matrizes de histórico (`historico_palavras` e `historico_status`) e incrementar `tentativas_usadas`.
- **Critério 6.1:** **Dado** que o jogador realizou `k` tentativas válidas (`1 <= k <= 6`), **Quando** a memória `.data` é inspecionada, **Então** as `k` palavras e seus `k` vetores de status estão preservados sequencialmente no histórico da partida atual.

### RF-07 — Exibição Textual do Tabuleiro *(Origem: Seção 2; Seção 3, item 2)*
Após cada tentativa válida, o sistema deve exibir todas as linhas registradas até o momento no formato alinhado sem cores:
`[k/6] PALAV -> C0 C1 C2 C3 C4`
onde cada célula `Ci` ocupa 3 caracteres: `[X]` para posição exata, `(X)` para letra presente em outra posição e ` . ` para letra ausente.
- **Critério 7.1:** **Dado** segredo `"TERMO"` e 1ª tentativa `"TRAMO"`, **Quando** o tabuleiro é impresso, **Então** exibe `[1/6] TRAMO -> [T] (R)  .  [M] [O]`.

### RF-08 — Detecção de Vitória e Derrota por Limite de Tentativas *(Origem: Seção 2, linha 35; Seção 3, item 3)*
O sistema deve encerrar a partida imediatamente ao atingir 5 acertos exatos (vitória) ou após a 6ª tentativa válida sem 5 acertos exatos (derrota, revelando a palavra secreta), retornando ao menu inicial.
- **Critério 8.1 (Vitória):** **Dado** que a avaliação retornou `5` acertos exatos na tentativa `k` (`1 <= k <= 6`), **Quando** o tabuleiro é exibido, **Então** o sistema parabeniza o jogador informando `k/6` tentativas, não pede novas tentativas da mesma partida e retorna ao menu inicial.
- **Critério 8.2 (Derrota na 6ª tentativa):** **Dado** que o jogador concluiu a tentativa `6/6` com menos de 5 acertos exatos, **Quando** o tabuleiro é exibido, **Então** o sistema informa que as tentativas acabaram, **exibe a palavra secreta** (`segredo_atual`) e retorna ao menu inicial.

### RF-09 — Encerramento Limpo do Programa *(Origem: Seção 3, item 8)*
- **Critério 9.1:** **Dado** que o usuário digitou `0` no menu inicial ou durante a leitura de uma tentativa, **Quando** o comando é processado, **Então** o programa imprime mensagem final e executa `syscall 10` sem erros de execução no MARS.

---

## 4. Requisitos Técnicos Obrigatórios do Enunciado (RT) — Seção 3

Copiados e numerados diretamente da **Seção 3 ("Requisitos técnicos obrigatórios" e "Critério de complexidade mínima")** de `CRIATHON_MIPS_Enunciado.md`:

- **RT-01** *(Seção 3, linha 42)*: Executar no MARS, com instruções claras de configuração e uso.
- **RT-02** *(Seção 3, linha 43)*: Receber entradas do usuário e produzir saídas relacionadas às suas ações.
- **RT-03** *(Seção 3, linha 44)*: Utilizar decisões e repetições com função real na lógica do programa.
- **RT-04** *(Seção 3, linha 45)*: Armazenar e manipular dados em pelo menos um vetor, matriz ou string.
- **RT-05** *(Seção 3, linha 46)*: Organizar o código em pelo menos três procedimentos com responsabilidades claras.
- **RT-06** *(Seção 3, linha 47)*: Utilizar parâmetros, retornos e preservação de registradores conforme boas práticas discutidas.
- **RT-07** *(Seção 3, linha 48)*: Empregar a pilha de forma justificada, por exemplo, para preservar dados em chamadas de procedimentos.
- **RT-08** *(Seção 3, linha 49)*: Tratar entradas inválidas e oferecer uma forma clara de encerrar a execução.
- **RT-09** *(Seção 3, linha 53 — Complexidade mínima)*: Combinar pelo menos duas ações relacionadas, manter um estado que se altera durante a execução e manipular uma estrutura de dados.
- **RT-10** *(Seção 3, linhas 55–57 — Complexidade mínima)*: Conter pelo menos **100 linhas úteis de instruções MIPS** (uma instrução/pseudoinstrução por linha; sem contar linhas em branco, comentários, rótulos isolados, diretivas ou `.data`), atingidas pela lógica necessária ao produto (sem repetições artificiais, instruções sem efeito ou trechos não utilizados).

---

## 5. Requisitos Não Funcionais e de Processo (RNF)

- **RNF-01 — Legibilidade para Prova Individual *(Seções 1 e 8)*:** Código enxuto (~180–240 instruções úteis), com procedimentos curtos (<= 50 instruções cada) e comentários em português claro para estudo sem consulta.
- **RNF-02 — Reprodutibilidade Determinística *(Seção 6)*:** Flag interna `usar_palavra_fixa` em `.data` documentada em `docs/testes.md`, permitindo repetir fielmente os 3 casos de teste obrigatórios sem poluir o menu do jogador.
- **RNF-03 — Integridade ASCII do Banco *(Restrição técnica MIPS)*:** Nenhuma palavra ou entrada aceita caracteres acentuados multibyte UTF-8; cada entrada do banco possui estritamente 5 bytes `0x41..0x5A` + `0x00` (`6` bytes).
- **RNF-04 — Entregáveis de Repositório e Vídeo *(Seções 4, 5, 6 e 7)*:** Repositório GitHub com commits incrementais, `README.md`, `src/`, `docs/ficha-tecnica.md` (até ~2 páginas), `docs/testes.md`, `docs/uso-ia.md` e vídeo de até 4 minutos.

---

## 6. Entradas, Processamento, Saídas e Exemplos Completos de Uso

### Resumo E-P-S
- **Entradas:** Opção do menu (`1`, `2`, `0`) e tentativas de palavras via teclado (`syscall 8`), aceitando maiúsculas/minúsculas ou `0` para sair.
- **Processamento:** Sorteio (`syscall 42` em `$a0`) ou carga fixa no banco `banco + 6*i`; validação de tamanho (=5) e faixa alfabética com conversão `a-z → A-Z`; algoritmo em duas passadas consumindo letras casadas em `segredo_temp`; gravação nas matrizes de histórico e atualização do contador de jogadas.
- **Saídas:** Tabuleiro textual acumulado (`[X]` posição certa, `(X)` outra posição, ` . ` ausente), mensagens específicas de erro de entrada e mensagens de vitória/derrota (revelando o segredo ao perder).

### Exemplo Completo 1 — Uso Normal com Letras Repetidas e Vitória (Palavra secreta: `"TERMO"`)

```text
=== TerMIPS ===
[1] Jogar
[2] Tutorial
[0] Sair
Escolha uma opcao: 1

Partida iniciada! Descubra a palavra de 5 letras em ate 6 tentativas.

Tentativa 1/6 (ou 0 para sair): casas
[1/6] CASAS ->  .   .   .   .   . 

Tentativa 2/6 (ou 0 para sair): morro
[1/6] CASAS ->  .   .   .   .   . 
[2/6] MORRO -> (M)  .  [R]  .  [O]

Tentativa 3/6 (ou 0 para sair): TRAMO
[1/6] CASAS ->  .   .   .   .   . 
[2/6] MORRO -> (M)  .  [R]  .  [O]
[3/6] TRAMO -> [T] (R)  .  [M] [O]

Tentativa 4/6 (ou 0 para sair): termo
[1/6] CASAS ->  .   .   .   .   . 
[2/6] MORRO -> (M)  .  [R]  .  [O]
[3/6] TRAMO -> [T] (R)  .  [M] [O]
[4/6] TERMO -> [T] [E] [R] [M] [O]

PARABENS! Palavra descoberta em 4/6 tentativas!
```

### Exemplo Completo 2 — Tutorial, Entradas Inválidas e Saída Voluntária

```text
=== TerMIPS ===
[1] Jogar
[2] Tutorial
[0] Sair
Escolha uma opcao: 9
[ERRO] Opcao invalida. Digite 1, 2 ou 0.
Escolha uma opcao: 2

--- COMO JOGAR ---
Descubra a palavra secreta de 5 letras (sem acentos) em ate 6 tentativas.
Apos cada tentativa:
  [X] = a letra X esta na posicao exata
  (X) = a letra X existe na palavra, mas em outra posicao
   .  = a letra nao faz parte da palavra
Digite 0 a qualquer momento para encerrar.

Escolha uma opcao: 1

Partida iniciada! Descubra a palavra de 5 letras em ate 6 tentativas.

Tentativa 1/6 (ou 0 para sair): 
[ERRO] Digite exatamente 5 letras (voce digitou 0). Tentativa nao descontada.

Tentativa 1/6 (ou 0 para sair): sol
[ERRO] Digite exatamente 5 letras (voce digitou 3). Tentativa nao descontada.

Tentativa 1/6 (ou 0 para sair): bananas
[ERRO] Digite exatamente 5 letras (voce digitou 7). Tentativa nao descontada.

Tentativa 1/6 (ou 0 para sair): ca5a!
[ERRO] Use apenas letras de A a Z sem acentos. Tentativa nao descontada.

Tentativa 1/6 (ou 0 para sair): livro
[1/6] LIVRO ->  .   .   .  (R) [O]

Tentativa 2/6 (ou 0 para sair): 0
Encerrando TerMIPS. Ate logo!
```

---

## 7. Casos de Borda Especificados

1. **Última tentativa (`6/6`) com acerto:** Vitória deve ter precedência sobre o esgotamento — se na 6ª tentativa `exatos == 5`, o jogador **vence** (`6/6`), não perde.
2. **Última tentativa (`6/6`) com erro:** Encerra o laço imediatamente após imprimir as 6 linhas e revela `segredo_atual`.
3. **Entrada com mais de 5 letras (ex.: `"BANANAS\n"`):** Buffer de leitura dimensionado com 64 bytes (`$a1 = 64`) absorve toda a linha até o `\n`, permitindo contar tamanho > 5 sem deixar lixo para a próxima leitura (em entradas normais de até 62 caracteres).
4. **Caractere não alfabético ou acentuado:** Caracteres como `'0'–'9'`, espaço `' '`, hífen `'-'` ou bytes UTF-8 (`>= 0x80`, ex.: `'Ç'`, `'Ã'`) têm valor fora das faixas ASCII `65..90` e `97..122`, sendo rejeitados na validação tanto por tamanho (UTF-8 ocupa 2 bytes) quanto por faixa alfabética.
5. **Linha vazia (`"\n"`):** Primeiro byte útil já é `\n` (ASCII 10) → comprimento calculado `0` → rejeitado sem consumir tentativa.
6. **Letras repetidas antes e depois de posição exata (ex.: `"MORRO"` contra `"TERMO"`):** Se a letra repetida aparecer primeiro numa posição errada (índice 1: `'O'`) e depois numa posição exata (índice 4: `'O'`), a Passada 1 garante que o índice 4 receba `[O]` e o índice 1 receba `.` (e nunca `(O)`).
7. **Comando de saída (`"0\n"`) vs palavra inválida de 1 caractere (`"a\n"`):** Apenas `'0'` isolado (seguido de `\n`/`\r`/`\0`) dispara saída limpa; `"a\n"` ou `"00\n"` caem na regra normal de entrada inválida.

---

## 8. Escopo Mínimo (MVP), Variações Criativas Opcionais e Fora de Escopo

### 8.1 Escopo Mínimo Obrigatório (MVP)
- Menu em console texto (`1 = Jogar`, `2 = Tutorial`, `0 = Sair`).
- Banco de 200 palavras em `.data` (`banco + 6*i`) + sorteio via `syscall 42` + flag interna `usar_palavra_fixa` / `indice_fixo` em `.data` para testes reproduzíveis.
- Leitura via `syscall 8`, validação de 5 letras ASCII, conversão minúsculas → maiúsculas e saída com `0`.
- Comparação em duas passadas tratando letras repetidas.
- Histórico das até 6 tentativas em memória e reexibição alinhada `[X]`, `(X)`, ` . `.
- Controle de vitória, derrota na 6ª tentativa com revelação do segredo e retorno ao menu.

### 8.2 Três Variações Criativas de Baixo Custo *(Opcionais — apoio aos 2,0 pts de Criatividade e Clareza, Seções 2 e 9)*

| ID | Dimensão (Seção 2) | Descrição da Variação Criativa (Opcional) | Custo Estimado | Risco |
| :--- | :--- | :--- | :--- | :--- |
| **EXT-01** *(Recomendada #1)* | **Apresentação** | **Painel de Alfabeto Residual (`A–Z`):** Vetor `alfabeto_status[26]` atualizado a cada palpite e impresso abaixo do tabuleiro mostrando letras já confirmadas `[A]`, presentes `(A)`, descartadas `_` e não testadas `A`. Ajuda muito a jogabilidade em console sem cores e adiciona mais um vetor indexado por `letra - 'A'`. | Baixo (~25 instruções MIPS; +26 bytes em `.data`) | Muito baixo (isolado na exibição) |
| **EXT-02** *(Recomendada #2)* | **Narrativa** | **Modo "Agente Cripto-MARS" + Patente e Placar de Sessão:** Narrativa enxuta de interceptação de código com contadores de sessão (`partidas_jogadas`, `vitorias_sessao`) e classificação ao vencer (`1–2` tentativas: *Mestre Decifrador*; `3–4`: *Analista Sênior*; `5–6`: *Agente de Campo*). | Muito baixo (~18 instruções MIPS + strings em `.data`) | Quase nulo (apenas desvios finais e 2 contadores `.word`) |
| **EXT-03** | **Regras** | **Sonda de Frequência (`?`) com Custo de Tentativa:** Uma vez por partida (se restarem >= 2 tentativas), digitar `?` consome 1 tentativa do limite de 6 em troca de revelar a letra exata da primeira posição ainda não descoberta. Cria decisão estratégica real de risco × informação. | Baixo/Médio (~28 instruções MIPS + 1 flag `sonda_usada`) | Baixo (exige testar interação com o limite de 6 rodadas) |

### 8.3 Outras Extensões Técnicas Opcionais (Pós-MVP)
- **EXT-04 — Dicionário restrito:** Verificar por busca linear se a tentativa de 5 letras existe nas 200 palavras do banco (ou banco auxiliar) antes de aceitar (~20 instruções MIPS).
- **EXT-05 — Leitura de banco externo `.txt` (`syscalls 13/14/16`):** Carregar palavras de um arquivo `.txt` com *fallback* para o banco interno. **Riscos registrados:** (a) caminho relativo depende do diretório de trabalho onde o `Mars4_5.jar` foi iniciado; (b) quebra de linha Windows (`\r\n`, 2 bytes) vs Linux (`\n`, 1 byte) desloca offsets se lido com stride fixo; (c) falha de abertura (`$v0 < 0` na `syscall 13`) exige tratamento explícito.

### 8.4 Fora de Escopo (Não será feito)
- Interface gráfica Bitmap Display ou Keyboard MMIO (desnecessário para a nota máxima, alto risco de tempo e difícil de explicar na prova individual).
- Suporte a acentuação gráfica UTF-8 (`Ã`, `Ç`, `É`) ou palavras de tamanhos variáveis.
- Modo multiplayer em rede ou inteligência artificial resolvedora automática.
