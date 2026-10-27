# Registro Cronológico de Uso de IA (`ai-log.md`)

Este arquivo registra todas as sessões de trabalho assistidas por Inteligência Artificial no projeto **TERMO-MIPS**, servindo como fonte auditável para a redação de `docs/uso-ia.md` (Seção 6 e Seção 7 de `CRIATHON_MIPS_Enunciado.md`).

> **Regra obrigatória (Seção 7):** O campo **Verificação feita pelo humano** nunca deve ser preenchido de forma genérica ("parece certo"). Registre sempre uma evidência concreta (conferência contra seção do enunciado, cálculo manual de offsets/status ou valores observados em registradores/memória no MARS).

---

## Sessão 01 — Especificação e Planejamento SDD (Fase 1)

- **Data/hora · agente · objetivo:**
  `2026-10-26 11:48–12:55 (-03:00)` · Agente SDD (Spec & Plan — Gemini) · Ler integralmente `CRIATHON_MIPS_Enunciado.md`, confrontar as decisões iniciais do grupo com o enunciado oficial e produzir toda a árvore de especificação, arquitetura MIPS, tarefas, rastreabilidade, ADRs e rascunho da proposta Fase 1 dentro de `.agents/` (sem código `.asm`).

- **Entradas usadas (arquivos, prompt):**
  - `CRIATHON_MIPS_Enunciado.md` (enunciado oficial da Profa. Denise Stringhini — ICT-Unifesp).
  - Prompt inicial arquivado integralmente em `.agents/prompts/01-spec-planner.md`.

- **Saídas geradas (arquivos criados/alterados):**
  - `.agents/README.md`
  - `.agents/open-questions.md`
  - `.agents/specs/00-constitution.md`
  - `.agents/specs/01-spec.md`
  - `.agents/specs/02-plan.md`
  - `.agents/specs/03-tasks.md`
  - `.agents/specs/04-traceability.md`
  - `.agents/specs/05-proposta-fase1.md`
  - `.agents/decisions/ADR-001.md` a `ADR-006.md`
  - `.agents/prompts/01-spec-planner.md` a `04-documenter.md`
  - `.agents/logs/ai-log.md`

- **Sugestão relevante da IA e o que foi aproveitado ou alterado:**
  1. **Armazenamento explícito do histórico de tentativas (`ADR-003`):** A IA identificou que a Seção 2 do enunciado (*Detetive de códigos*, linha 35) pede literalmente *"Armazenem segredo e tentativas"* e a Seção 3 exige *"manter um estado que se altera durante a execução e manipular uma estrutura de dados"*. Em vez de apenas sobrescrever um único buffer de entrada a cada rodada, foi incorporado o armazenamento das até 6 tentativas e 6 vetores de status em matrizes linearizadas `6x6` (`historico_palavras` e `historico_status`) em `.data`, reimprimindo o tabuleiro acumulado.
  2. **Buffer de leitura largo (`64` bytes) na `syscall 8` + saída universal com `'0'` (`ADR-005`):** Evita que tentativas inválidas com mais de 5 letras (ex.: `"BANANAS\n"`) sejam truncadas prematuramente sem `\n`, e define `'0'` como comando de saída inequívoco que não colide com palpites alfabéticos.
  3. **Alinhamento de `.word` no topo de `.data` e sentinela `fim_banco` (`ADR-001`, `02-plan.md`):** Previne exceção `Address error` no MARS e permite verificar matematicamente (`fim_banco - banco_palavras == 1200`) que nenhuma das 200 palavras contém acento UTF-8 de 2 bytes.
  4. **Caso de teste `"MORRO"` contra `"TERMO"` (`CT-01`):** Projetado e calculado à mão para exercitar em uma única palavra as duas direções críticas de letras repetidas (`'O'` duplicado antes do acerto exato e `'R'` duplicado após o acerto exato).
  - *Decisão do grupo sobre aproveitamento/alteração:* `[PREENCHER PELO GRUPO — ex.: aproveitado integralmente / ajustado item X]`.

- **Verificação feita pelo humano (campo a preencher: evidência concreta):**
  - `[PREENCHER PELO GRUPO — ex.: Conferimos manualmente o cálculo das duas passadas de "MORRO" contra "TERMO" (Passada 1 gera status [0,0,2,0,2] e segredo_temp "TE*M*"; Passada 2 gera [1,0,2,0,2] -> "(M)  .  [R]  .  [O]"), revisamos os itens da Seção 3 e Seção 10 de CRIATHON_MIPS_Enunciado.md contra 04-traceability.md e preenchemos os dados do grupo em 05-proposta-fase1.md.]`

---

## Sessão 02 — Organização do Repositório e Entrega da Fase 1 (26/10)

- **Data/hora · agente · objetivo:**
  `2026-10-26 13:01–13:09 (-03:00)` · Agente SDD / Documenter · Adotar o nome **TerMIPS**, simplificar a linguagem da proposta da Fase 1 para foco explicativo e organizar a estrutura oficial do repositório (`README.md`, `src/termips.asm`, `docs/`, `.gitignore` e `git init`).

- **Entradas usadas (arquivos, prompt):**
  - Escolhas do usuário: nome `TerMIPS`, `.agents/` ignorado via `.gitignore` e marcadores `[PREENCHER]` mantidos.
  - `CRIATHON_MIPS_Enunciado.md` (Seções 4 e 6) e `.agents/specs/05-proposta-fase1.md`.

- **Saídas geradas (arquivos criados/alterados):**
  - Criados na raiz/repositório oficial: `.gitignore`, `README.md`, `src/termips.asm` (esqueleto inicial), `docs/proposta-fase1.md`, `docs/ficha-tecnica.md`, `docs/testes.md`, `docs/uso-ia.md`.
  - Atualizados em `.agents/` para manter sincronismo com `src/termips.asm` e `TerMIPS`.

- **Sugestão relevante da IA e o que foi aproveitado ou alterado:**
  - Sugestão de nomes iniciais levou à escolha de **TerMIPS** pelo usuário; proposta reescrita sem jargão de baixo nível (mantendo apenas o resumo de alto nível de MIPS exigido na Fase 1) e estrutura inicial completa da Seção 6 preparada para o primeiro commit.

- **Verificação feita pelo humano (campo a preencher: evidência concreta):**
  - `[PREENCHER PELO GRUPO — ex.: Preenchimento dos integrantes e link do GitHub, conferência de git status garantindo que .agents/ está ignorado e envio do formulário da Fase 1 em 26/10.]`

---

## Sessão 03 — Refinamento de Identidade Autoral (`TerMIPS`) e Novo Menu com Tutorial

- **Data/hora · agente · objetivo:**
  `2026-10-26 13:16–13:20 (-03:00)` · Agente SDD · Remover referências à sugestão *"Detetive de códigos"* (apresentando o **TerMIPS** diretamente como ideia do grupo) e substituir o menu principal para `[1] Jogar`, `[2] Tutorial` (texto explicativo das regras) e `[0] Sair`, movendo a configuração de palavra fixa para uma constante interna em `.data` (`usar_palavra_fixa`).

- **Entradas usadas (arquivos, prompt):**
  - Solicitação do usuário no chat para retirar menções a *"Detetive de códigos"* e adotar menu `Jogar / Tutorial / Sair`.

- **Saídas geradas (arquivos criados/alterados):**
  - Atualizados: `README.md`, `docs/proposta-fase1.md`, `docs/ficha-tecnica.md`, `docs/testes.md`, `docs/uso-ia.md` e documentos correspondentes em `.agents/specs/`.

- **Sugestão relevante da IA e o que foi aproveitado ou alterado:**
  - O usuário melhorou a usabilidade trocando a opção de menu `Modo Teste` por `Tutorial`; a IA preservou a exigência de reprodutibilidade da Seção 6 do enunciado através da constante interna `usar_palavra_fixa` em `.data`, sem poluir a interface do jogador nem a proposta da Fase 1.

- **Verificação feita pelo humano (campo a preencher: evidência concreta):**
  - `[PREENCHER PELO GRUPO]`
