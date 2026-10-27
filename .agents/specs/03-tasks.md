# 03 — Plano de Tarefas Executáveis (`TERMO-MIPS`)

Tarefas dimensionadas para **uma sessão de trabalho cada**, organizadas em ordem de dependência e alinhadas aos quatro marcos oficiais do cronograma (Seção 4 do enunciado).
- Marcador **`[P]`**: tarefa que pode ser executada em paralelo após suas dependências diretas.
- **Regra de ouro:** após cada tarefa de código, realizar commits descritivos no GitHub (Seção 4 — Fase 2) e registrar a sessão em `.agents/logs/ai-log.md`.

---

## Marco 1 — Submissão da Ideia e Repositório Inicial (Prazo: **26/10**)

### T-01 — Fechamento da Proposta Fase 1 e Criação do Repositório GitHub
- **Responsável:** `[PREENCHER]`
- **Dependências:** Nenhuma
- **Descrição:** Preencher os dados do grupo em `05-proposta-fase1.md`, criar o repositório no GitHub (com estrutura mínima `src/`, `docs/`, `README.md` inicial e `docs/uso-ia.md` inicial), conceder acesso à professora (se privado) e submeter o formulário da Fase 1.
- **Arquivos afetados:** `.agents/specs/05-proposta-fase1.md`, `README.md`, `docs/uso-ia.md`
- **Critério de pronto:** Formulário enviado até 26/10 com link válido do GitHub e estrutura de pastas criada no primeiro commit.
- **Como verificar com evidência concreta:** Abrir o link do repositório em aba anônima (ou confirmar convite de colaborador) e verificar presença das pastas `src/`, `docs/` e `.agents/`.

---

## Marco 2 — Núcleo Funcional (MVP) e Demonstração 1 em Laboratório (Prazo: **11/11**)

### T-02 — Esqueleto `.data`, Menu Principal (`main`) e Eco de String com Saída Limpa
- **Responsável:** `[PREENCHER]`
- **Dependências:** `T-01`
- **Descrição:** Criar `src/termips.asm` com as variáveis `.word` alinhadas no topo do `.data`, `buf_entrada` (64 B), strings básicas, laço do menu principal (`1`, `2`, `0`, opção inválida), leitura e eco simples de uma string via `syscall 8` e encerramento limpo via `syscall 10` ao digitar `0`.
- **Arquivos afetados:** `src/termips.asm`, `.agents/logs/ai-log.md`
- **Critério de pronto:** Programa monta sem *warnings* no MARS, rejeita opção `9` no menu, lê e ecoa uma string nas opções `1`/`2` e encerra sem erro ao escolher `0`.
- **Como verificar com evidência concreta:**
  - *Rastreamento manual vs MARS:* Antes da `syscall 8`, anotar `$a0` (endereço de `buf_entrada`, ex.: `0x10010074`) e `$a1 = 64`. Digitar `"teste"` + Enter e inspecionar o *Data Segment* (modo ASCII): confirmar os bytes `'t' 'e' 's' 't' 'e' '\n' '\0'` (`0x74 0x65 0x73 0x74 0x65 0x0a 0x00`). Confirmar no *Data Segment* que todas as `.word` iniciais começam em endereços múltiplos de 4 (`...0`, `...4`, `...8`, `...c`).

### T-03 — Procedimento `ler_e_validar_tentativa` (Tamanho, Caixa e Alfabeto)
- **Responsável:** `[PREENCHER]`
- **Dependências:** `T-02`
- **Descrição:** Implementar `ler_e_validar_tentativa` com prólogo/epílogo salvando `$s0–$s1` em `$sp` (8 bytes): detecção de comando `'0'` (`$v0 = 2`), cálculo de comprimento útil ignorando `\n`/`\r`, rejeição de tamanho `!= 5` (`$v0 = 1`), conversão `'a'..'z' → 'A'..'Z'` (`subi 32`), rejeição fora de `'A'..'Z'` (`$v0 = 1`) e gravação em `tentativa_atual` (`$v0 = 0`).
- **Arquivos afetados:** `src/termips.asm`, `.agents/logs/ai-log.md`
- **Critério de pronto:** Entradas `""`, `"sol"`, `"bananas"` e `"ca5a!"` retornam `$v0 = 1` com mensagem clara; `"0"` retorna `$v0 = 2`; `"termo"` grava `"TERMO\0"` em `tentativa_atual` e retorna `$v0 = 0`, preservando `$sp`, `$s0` e `$s1` idênticos aos valores antes do `jal`.
- **Como verificar com evidência concreta:**
  - *Rastreamento manual vs MARS:* Carregar valores sentinela em `$s0 = 0x11111111` e `$s1 = 0x22222222` antes de `jal ler_e_validar_tentativa`, anotar `$sp` (ex.: `0x7fffeffc`). Executar passo a passo (*Step*) com entrada `"tErMo\n"`: verificar decremento de `$sp` em `-8`, conversão do byte `'t'` (`0x74` → `0x54`) no registrador temporário, retorno `$v0 = 0` e restauração exata de `$s0`, `$s1` e `$sp` após `jr $ra`.

### T-04 — Comparação Simples (Passada 1) e Impressão Formatada (`imprimir_linha_tentativa`)
- **Responsável:** `[PREENCHER]`
- **Dependências:** `T-03`
- **Descrição:** Fixar provisoriamente `segredo_atual = "TERMO"`, implementar a Passada 1 em `avaliar_tentativa` (marcando `status[i] = 2` para exatos e `0` para demais, contando `$v0`) e implementar `imprimir_linha_tentativa` (salvando `$s0–$s2` em `$sp`, 12 bytes) exibindo `[1/6] PALAV -> [X]  .  ...`.
- **Arquivos afetados:** `src/termips.asm`, `.agents/logs/ai-log.md`
- **Critério de pronto:** Para segredo `"TERMO"` e entrada `"TRAMO"`, imprime `[1/6] TRAMO -> [T]  .   .  [M] [O]` e retorna `$v0 = 3`.
- **Como verificar com evidência concreta:**
  - *Rastreamento manual vs MARS:* Calcular à mão `status_atual` esperado (`[2, 0, 0, 2, 2]`) para `"TRAMO"` vs `"TERMO"`. Rodar com *Breakpoint* ao final de `avaliar_tentativa` e conferir no *Data Segment* os 5 bytes `0x02, 0x00, 0x00, 0x02, 0x02` em `status_atual` e `$v0 == 3`.

### T-05 — Tratamento Completo de Letras Repetidas (Passada 2 em `avaliar_tentativa`)
- **Responsável:** `[PREENCHER]`
- **Dependências:** `T-04`
- **Descrição:** Completar `avaliar_tentativa` adicionando a cópia inicial para `segredo_temp`, o consumo `segredo_temp[i] = '*'` na Passada 1 e o laço aninhado da Passada 2 (`status[i] = 1`, consumo `segredo_temp[j] = '*'`, incremento de `$v1` e `break` do laço `j`), além da formatação `(X)` em `imprimir_linha_tentativa`.
- **Arquivos afetados:** `src/termips.asm`, `.agents/logs/ai-log.md`
- **Critério de pronto:** Contra `"TERMO"`, `"MORRO"` produz `(M)  .  [R]  .  [O]` (`$v0=2, $v1=1`) e `"ARARA"` produz ` .  (R)  .   .   . ` (`$v0=0, $v1=1`); contra `"CARTA"`, `"ARARA"` produz `(A) (R) (A)  .   . ` (`$v0=0, $v1=3`).
- **Como verificar com evidência concreta:**
  - *Rastreamento manual vs MARS:* Rastrear manualmente `segredo_temp` para `"MORRO"` vs `"TERMO"`: antes = `"TERMO"`, fim da Passada 1 = `"TE*M*"`, fim da Passada 2 = `"TE***"`. Executar no MARS com *Breakpoint* entre a Passada 1 e a Passada 2 e no `jr $ra`, conferindo byte a byte `segredo_temp` (`0x54 0x45 0x2a 0x2a 0x2a`), `status_atual` (`1, 0, 2, 0, 2`), `$v0 = 2` e `$v1 = 1` (e que `segredo_atual` permaneceu intacto `"TERMO"`).

### T-06 `[P]` — Banco de 200 Palavras, Sorteio (`syscall 42`) e Modo Teste (`selecionar_segredo`)
- **Responsável:** `[PREENCHER]`
- **Dependências:** `T-02` *(pode rodar em paralelo a `T-03..T-05`)*
- **Descrição:** Popular `banco_palavras` com 200 palavras `.asciiz` de 5 letras ASCII maiúsculas sem acento e sem duplicatas (`indice 0 = "TERMO"`, `indice 1 = "CARTA"`), adicionar rótulo `fim_banco`, e implementar `selecionar_segredo` suportando modo sorteio (`$a0 = 0` → `syscall 42` lendo o índice retornado em **`$a0`**) e Modo Teste (`$a0 = 1` → lê `indice_teste`), copiando `banco_palavras + 6*i` para `segredo_atual`.
- **Arquivos afetados:** `src/termips.asm`, `.agents/logs/ai-log.md`
- **Critério de pronto:** `fim_banco - banco_palavras == 1200` bytes exatos (`0x4B0`); Modo Teste carrega sempre `"TERMO"`; modo sorteio carrega palavras válidas de 5 letras em múltiplas execuções (verificando também o item 1 de `open-questions.md` sobre semente no MARS).
- **Como verificar com evidência concreta:**
  - *Rastreamento manual vs MARS:* Conferir na tabela *Labels* do MARS que `addr(fim_banco) - addr(banco_palavras) == 0x000004B0` (`1200` decimal). No modo sorteio, colocar *Breakpoint* logo após `syscall` (código 42), ler o índice em `$a0` (ex.: `i = 37`), calcular à mão `addr(banco_palavras) + 6*37` (`+ 222 = +0xDE`), inspecionar esse endereço no *Data Segment* e confirmar que após o laço de cópia os mesmos 5 caracteres estão em `segredo_atual`.

### T-07 — Histórico `6x6`, Limite de Tentativas e Fim de Partida (`executar_partida` + `exibir_tabuleiro`)
- **Responsável:** `[PREENCHER]`
- **Dependências:** `T-05`, `T-06`
- **Descrição:** Implementar `registrar_tentativa`, `exibir_tabuleiro` (não-folha com pilha de 12 B salvando `$ra, $s0, $s1`) e `executar_partida` (não-folha com pilha de 16 B salvando `$ra, $s0, $s1, $s2`), integrando o laço de até 6 tentativas válidas, vitória imediata quando `$v0 == 5` (inclusive na `6/6`), derrota após `6/6` revelando `segredo_atual` e retorno ao menu principal.
- **Arquivos afetados:** `src/termips.asm`, `.agents/logs/ai-log.md`
- **Critério de pronto:** MVP 100% funcional ponta a ponta nos casos `CT-01`, `CT-02` e `CT-03`, pronto para a **Demonstração 1 em laboratório (11/11)**.
- **Como verificar com evidência concreta:**
  - *Rastreamento manual vs MARS:* Executar `CT-03` (6 palpites incorretos no Modo Teste) monitorando `$sp` na entrada e na saída de `executar_partida` (devem coincidir exatamente em `0x7fffeffc`), `tentativas_usadas` evoluindo de `0` a `6` (sem subir em entradas inválidas) e os 36 bytes de `historico_palavras` contendo as 6 strings gravadas em `base + 0, +6, +12, +18, +24, +30`.

### T-08 `[P]` — Documentação Contínua Pré-Demo 1 (`docs/testes.md` e `docs/uso-ia.md` parciais)
- **Responsável:** `[PREENCHER]`
- **Dependências:** `T-07`
- **Descrição:** Criar `docs/testes.md` com os resultados preliminares observados de `CT-01`, `CT-02` e `CT-03` e atualizar `docs/uso-ia.md` com a principal sugestão/verificação do MVP (ex.: algoritmo de duas passadas ou `syscall 42` em `$a0`), garantindo repositório organizado já na Demonstração 1 (Seção 4 — Fase 2).
- **Arquivos afetados:** `docs/testes.md`, `docs/uso-ia.md`
- **Critério de pronto:** Documentos versionados no GitHub antes de **11/11** e roteiro rápido de demonstração alinhado entre o grupo.
- **Como verificar com evidência concreta:** Outro integrante (que não escreveu `T-07`) clona o repositório, abre `src/termips.asm` no MARS e reproduz `CT-01` usando apenas as instruções escritas.

---

## Marco 3 — Polimento, Documentação Final, Vídeo e Entrega (Prazo: **07/12**)

### T-09 — Ajustes Pós-Demonstração 1 e Avaliação de 1 Variação Criativa Opcional
- **Responsável:** `[PREENCHER]`
- **Dependências:** `T-07`, `T-08` (e feedback de laboratório em 11/11)
- **Descrição:** Registrar e aplicar eventuais ajustes sugeridos pela professora na Demonstração 1. Se o cronograma estiver folgado, implementar **no máximo uma** variação criativa de baixo custo de `01-spec.md` (preferencialmente `EXT-01` Painel de Alfabeto `A–Z` ou `EXT-02` Patente/Placar de Sessão) e revisar comentários do `.asm`.
- **Arquivos afetados:** `src/termips.asm`, `.agents/logs/ai-log.md`
- **Critério de pronto:** Código congelado (*code freeze*), auditado com `prompts/03-reviewer-mips.md`, sem instruções inúteis e com >= 100 linhas úteis confirmadas.
- **Como verificar com evidência concreta:** Contar as linhas úteis do código-fonte congelado (excluindo comentários, brancos, rótulos isolados, diretivas e `.data`) e reexecutar `CT-01`, `CT-02` e `CT-03` sem regressões.

### T-10 `[P]` — Consolidação Final de `docs/testes.md` e `docs/uso-ia.md`
- **Responsável:** `[PREENCHER]`
- **Dependências:** `T-09`
- **Descrição:** Finalizar `docs/testes.md` (entradas exatas, esperado, observado copiado do console do MARS e justificativa técnica para os 3 casos) e `docs/uso-ia.md` (ferramentas, finalidade, sugestão relevante aproveitada/alterada e evidência concreta do rastreamento manual vs MARS — Seção 7).
- **Arquivos afetados:** `docs/testes.md`, `docs/uso-ia.md`
- **Critério de pronto:** Ambos os arquivos atendem integralmente às tabelas das Seções 6 e 7 do enunciado.
- **Como verificar com evidência concreta:** Reexecutar do zero no MARS cada um dos 3 testes copiando literalmente as entradas descritas em `docs/testes.md` e confrontar caractere a caractere com a coluna "Resultado observado".

### T-11 `[P]` — Redação Final de `docs/ficha-tecnica.md` e `README.md`
- **Responsável:** `[PREENCHER]`
- **Dependências:** `T-09`
- **Descrição:** Redigir `docs/ficha-tecnica.md` (concisa, equivalente a **até 2 páginas**, sem duplicar o código: objetivo, lógica, estruturas de dados, procedimentos, localização exata dos RTs com linhas/rótulos, contagem real de instruções úteis, contribuição individual de cada integrante e síntese do feedback da Demo 1) e finalizar `README.md` (descrição, integrantes, versão/configuração do MARS, como rodar normal e Modo Teste, comandos, limitações conhecidas e espaço/link do vídeo).
- **Arquivos afetados:** `docs/ficha-tecnica.md`, `README.md`
- **Critério de pronto:** Ficha técnica cabe em <= 2 páginas na visualização de impressão/PDF e `README.md` contém todos os itens da tabela da Seção 6.
- **Como verificar com evidência concreta:** Exportar/visualizar impressão de `docs/ficha-tecnica.md` conferindo paginação (<= 2 páginas) e conferir que todos os nomes de procedimentos e rótulos citados batem exatamente com `src/termips.asm`.

### T-12 — Gravação do Vídeo Final (<= 4 min) e Conferência Geral de Entrega (Seção 10)
- **Responsável:** `[PREENCHER]`
- **Dependências:** `T-10`, `T-11`
- **Descrição:** Gravar tela com narração (máx. **4:00 min**, Seção 5) cobrindo: (1) ideia e funcionalidade principal (~40s); (2) uso normal + tratamento de entrada inválida (~1m00s); (3) *Step-by-step* de `avaliar_tentativa` no MARS mostrando registradores, `segredo_temp` na memória e uma decisão relevante (~1m30s); (4) escolha criativa e limitação conhecida (~30s). Inserir link no `README.md`, marcar versão final (*tag/release*) e validar a checklist de `04-traceability.md`.
- **Arquivos afetados:** `README.md` (link do vídeo)
- **Critério de pronto:** Vídeo com duração `<= 4:00`, áudio audível, links do vídeo e do GitHub acessíveis externamente e submissão da Fase 3 realizada até **07/12**.
- **Como verificar com evidência concreta:** Testar abertura do link do vídeo e do repositório em janela anônima (ou conta externa sem privilégio de dono) e cronometrar duração total do vídeo.

---

## Marco 4 — Estudo Coletivo para a Prova Individual (Prazo: **09/12**)

### T-13 — Revisão Coletiva e Simulado Sem Consulta do Código do Grupo
- **Responsável:** Todos os integrantes (`[PREENCHER]` moderador da sessão)
- **Dependências:** `T-09`, `T-11`
- **Descrição:** Reunião obrigatória de estudo (sem consulta ao MARS na segunda metade) onde cada integrante explica: (1) cálculo `banco_palavras + 6*i` e `lb`/`sb`; (2) todos os laços e desvios; (3) quadros de pilha (`$sp`, `$ra`, `$s0–$s2`) de `executar_partida` e `exibir_tabuleiro`; (4) simulação escrita das duas alterações propostas na Seção 10 de `02-plan.md` sobre `avaliar_tentativa` (caso concreto + estado inicial + efeito original vs alterado + justificativa instrução a instrução).
- **Arquivos afetados:** Nenhum arquivo de entrega (estudo para avaliação de 2,0 pts individuais — Seções 8 e 9)
- **Critério de pronto:** Cada integrante responde por escrito, sem consulta, a 1 variação simulada da questão aberta atingindo os 4 critérios da Seção 9 (caso concreto, comportamento original, comportamento alterado e justificativa por execução).
- **Como verificar com evidência concreta:** Correção cruzada das respostas escritas usando o MARS ao final do simulado para comprovar que o efeito previsto à mão bate com a execução alterada.
