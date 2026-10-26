# Casos de Teste Reproduzíveis — TerMIPS

> *Plano inicial de testes (Fase 1). Para repetir exatamente os casos abaixo sem depender do sorteio aleatório, basta ativar a flag interna de configuração com palavra fixa (`TERMO`) no segmento `.data` de `src/termips.asm` antes de montar o código, conforme exigido pelo enunciado. Os resultados observados serão preenchidos na Fase 2.*

---

## Caso 1 — Uso Normal (Tutorial, Vitória e Letras Repetidas)
- **Objetivo:** Verificar abertura do tutorial, fluxo completo de uma partida vencida, conversão de minúsculas e pistas corretas para palavras com letras repetidas.
- **Entradas previstas:** Escolher `2` (Tutorial) → `1` (Jogar) → digitar `casas` → `morro` → `TRAMO` → `termo` → `0` (Sair).
- **Resultado esperado:**
  - `2` exibe as instruções de como jogar e retorna ao menu.
  - `casas` vira `CASAS` com saída ` .   .   .   .   . `
  - `morro` vira `MORRO` com saída `(M)  .  [R]  .  [O]`
  - `TRAMO` gera `[T] (R)  .  [M] [O]`
  - `termo` gera `[T] [E] [R] [M] [O]` e exibe mensagem de vitória em `4/6` tentativas.
- **Resultado observado no MARS:** `[A preencher na Fase 2]`
- **Justificativa:** Confirma funcionamento do menu, normalização de caixa, histórico acumulado e prioridade de posições exatas sobre letras repetidas.

---

## Caso 2 — Tratamento de Entradas Inválidas
- **Objetivo:** Garantir que entradas fora do padrão exibam aviso claro e **não consumam** tentativas do jogador.
- **Entradas previstas:** Menu `9` (opção inválida) → Menu `1` (Jogar) → Enter vazio → `sol` (3 letras) → `bananas` (7 letras) → `ca5a!` (número/símbolo) → `TERMO` → `0`.
- **Resultado esperado:** Todas as entradas inválidas exibem mensagem explicativa mantendo o contador em `1/6`; ao digitar `TERMO`, vence na tentativa `1/6`.
- **Resultado observado no MARS:** `[A preencher na Fase 2]`
- **Justificativa:** Comprova robustez da leitura e preservação do estado da partida diante de erros de digitação.

---

## Caso 3 — Situação Limite (Esgotamento da 6ª Tentativa)
- **Objetivo:** Verificar o encerramento exato ao atingir o limite máximo de 6 tentativas sem acertar a palavra secreta.
- **Entradas previstas:** Menu `1` (Jogar) → `CASAS` → `LIVRO` → `PUNHO` → `FESTA` → `MORRO` → `TRAMO` → `0`.
- **Resultado esperado:** Exibição das 6 linhas preenchidas no histórico (`1/6` a `6/6`), interrupção imediata das tentativas, mensagem de fim de jogo e **revelação da palavra secreta (`TERMO`)**.
- **Resultado observado no MARS:** `[A preencher na Fase 2]`
- **Justificativa:** Valida o controle de borda do contador de jogadas no limite superior (`6/6`).
