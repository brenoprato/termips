# 04 — Matriz de Rastreabilidade e Checklist de Entrega (`TERMO-MIPS`)

Este documento cruza cada exigência de `CRIATHON_MIPS_Enunciado.md` com os requisitos (`RF`/`RT`/`RNF`), procedimentos planejados em `02-plan.md`, tarefas em `03-tasks.md` e casos de teste.

---

## 1. Matriz de Rastreabilidade Ponta a Ponta

| Origem no Enunciado | Requisito(s) | Procedimento(s) / Artefato Alvo | Tarefa(s) | Verificação / Caso de Teste |
| :--- | :--- | :--- | :--- | :--- |
| **Seção 1** — Compreensão por todos os integrantes para prova individual | `RNF-01` | Todos os procedimentos (`src/termips.asm`) | `T-09`, `T-13` | Simulado sem consulta (`T-13`) |
| **Seção 2 (linha 18)** — Ações do usuário, dados armazenados, regras e condição de encerramento | `RF-01` a `RF-09` | Fluxo completo (`main` → `executar_partida`) | `T-01` a `T-07` | `CT-01`, `CT-02`, `CT-03` |
| **Seção 2 (linha 35 — Detetive de códigos)** — Sequência secreta, contagem/posição certa e repetidas por regra explícita | `RF-02`, `RF-05`, `RF-07` | `selecionar_segredo`, `avaliar_tentativa`, `imprimir_linha_tentativa` | `T-04`, `T-05`, `T-06` | `CT-01` (palpites `"MORRO"` e `"TRAMO"`) |
| **Seção 2 (linha 35)** — Armazenar segredo e tentativas e controlar limite de jogadas | `RF-06`, `RF-08`, `RT-09` | `registrar_tentativa`, `exibir_tabuleiro`, `executar_partida` (`historico_*`, `tentativas_usadas`) | `T-07` | `CT-01` (4 jogadas) e `CT-03` (limite de 6 jogadas) |
| **Seção 3 (item 1)** — Executar no MARS com instruções claras | `RT-01` | `src/termips.asm`, `README.md` | `T-02`, `T-11` | Execução limpa no MARS 4.5 (`T-08`, `T-10`) |
| **Seção 3 (item 2)** — Receber entradas do usuário e produzir saídas relacionadas | `RT-02`, `RF-01`, `RF-03`, `RF-07` | `main`, `ler_e_validar_tentativa`, `imprimir_linha_tentativa` | `T-02`, `T-03`, `T-04` | `CT-01`, `CT-02` |
| **Seção 3 (item 3)** — Decisões e repetições com função real | `RT-03`, `RF-04`, `RF-05`, `RF-08` | Laços e desvios em `executar_partida`, `ler_e_validar_tentativa`, `avaliar_tentativa`, `exibir_tabuleiro` | `T-03`, `T-05`, `T-07` | `CT-01`, `CT-02`, `CT-03` |
| **Seção 3 (item 4)** — Armazenar/manipular em vetor, matriz ou string | `RT-04`, `RF-02`, `RF-06`, `RNF-03` | `banco_palavras`, `historico_palavras`, `historico_status`, `segredo_temp`, `status_atual` | `T-03` a `T-07` | Inspeção do *Data Segment* + `CT-01`, `CT-03` |
| **Seção 3 (item 5)** — Pelo menos 3 procedimentos com responsabilidades claras | `RT-05` | 7 procedimentos + `main` (Tabela da Seção 3 de `02-plan.md`) | `T-03` a `T-07` | Auditoria arquitetural (`T-09`, `docs/ficha-tecnica.md`) |
| **Seção 3 (item 6)** — Parâmetros, retornos e preservação de registradores | `RT-06` | Contratos `$a0–$a2`, `$v0–$v1` e preservação de `$s0–$s2` | `T-03` a `T-07` | Teste com sentinelas em `$s0–$s2` no MARS (`T-03`, `T-07`) |
| **Seção 3 (item 7)** — Uso justificado da pilha (`$sp`) | `RT-07` | `executar_partida` (16 B), `exibir_tabuleiro` (12 B), `ler_e_validar_tentativa` (8 B), `imprimir_linha_tentativa` (12 B) | `T-03`, `T-04`, `T-07` | Inspeção de `$sp` e memória da pilha no MARS (`T-03`, `T-07`) |
| **Seção 3 (item 8)** — Tratar entradas inválidas e forma clara de encerrar | `RT-08`, `RF-03`, `RF-04`, `RF-09` | `main`, `ler_e_validar_tentativa` (`0` → `syscall 10`) | `T-02`, `T-03` | `CT-02` (vazia, `<5`, `>5`, não-letra, opção inválida e saída `0`) |
| **Seção 3 (Complexidade mínima — estado)** — >= 2 ações relacionadas + estado mutável + estrutura de dados | `RT-09`, `RF-05`, `RF-06` | `executar_partida` + `avaliar_tentativa` + `registrar_tentativa` | `T-05`, `T-07` | `CT-01`, `CT-03` |
| **Seção 3 (Complexidade mínima — tamanho)** — >= 100 linhas úteis de instruções MIPS sem repetição artificial | `RT-10` | `src/termips.asm` (~217–266 instruções estimadas) | `T-07`, `T-09`, `T-11` | Contagem auditada documentada em `docs/ficha-tecnica.md` (`T-09`, `T-11`) |
| **Seção 4** — Fases: Proposta (26/10), Commits contínuos, Demo 1 (11/11), Entrega (07/12) | `RNF-04` | `05-proposta-fase1.md`, histórico Git, repositório completo | `T-01`, `T-07`, `T-08`, `T-12` | Histórico de commits distribuídos e marcos cumpridos |
| **Seção 5** — Vídeo final de até 4 minutos com os 4 elementos exigidos | `RNF-04` | Vídeo gravado + link em `README.md` | `T-12` | Cronometragem `<= 4:00` e checagem de roteiro (`T-12`) |
| **Seção 6** — Organização GitHub (`README.md`, `src/`, `ficha-tecnica.md` <= 2 pág., `testes.md`, `uso-ia.md`) | `RNF-02`, `RNF-04` | Raiz do repositório e pasta `docs/` | `T-01`, `T-08`, `T-10`, `T-11` | Conferência de estrutura e acesso em aba anônima (`T-12`) |
| **Seção 6 (Testes reproduzíveis)** — 3 casos (normal, inválida, limite) + dados fixos para aleatoriedade | `RNF-02`, `RF-01`, `RF-02` | Modo Teste (`opção 2` / `indice_teste`) + `docs/testes.md` | `T-06`, `T-08`, `T-10` | Reprodução exata de `CT-01`, `CT-02` e `CT-03` (`T-10`) |
| **Seção 7** — Registro e verificação concreta do uso de IA e fontes | `RNF-04` | `.agents/logs/ai-log.md` → `docs/uso-ia.md` | Todas (`T-01` a `T-10`) | Evidência manual vs MARS documentada (`T-10`) |
| **Seção 8** — Prova final individual (objetivas + questão aberta sobre trecho do grupo) | `RNF-01` | Foco especial em `avaliar_tentativa` e quadros de pilha | `T-13` | Simulado escrito individual sem consulta (`T-13`) |

---

## 2. Checklist de Conferência Antes da Entrega (Espelho da Seção 10 do Enunciado)

Cada item abaixo reproduz fielmente a **Seção 10 ("Conferência antes da entrega", linhas 157–165)** de `CRIATHON_MIPS_Enunciado.md` e aponta a tarefa responsável por comprová-lo:

- [ ] **Item 10.1:** *"O programa executa a funcionalidade principal, atende aos requisitos técnicos e cumpre o critério de complexidade mínima."*
  - **Cumpriremos em:** `T-07` (integração do MVP) e `T-09` (auditoria dos requisitos `RT-01` a `RT-10` e contagem confirmada >= 100 linhas úteis sem instruções artificiais).
- [ ] **Item 10.2:** *"O README permite configurar e utilizar o produto sem ajuda do grupo."*
  - **Cumpriremos em:** `T-08` (teste cruzado por outro integrante) e `T-11` (finalização do `README.md` com versão do MARS, passo a passo de abertura/montagem/execução, comandos, Modo Teste e limitações).
- [ ] **Item 10.3:** *"Os três testes podem ser repetidos e têm resultados documentados."*
  - **Cumpriremos em:** `T-06` (Modo Teste determinístico com `indice_teste`) e `T-10` (`docs/testes.md` com entradas, resultado esperado, resultado observado e justificativa para `CT-01`, `CT-02` e `CT-03`).
- [ ] **Item 10.4:** *"O vídeo tem até quatro minutos e demonstra o código em execução."*
  - **Cumpriremos em:** `T-12` (gravação `<= 4:00` contendo ideia, uso normal, entrada inválida, *step-by-step* de `avaliar_tentativa` no MARS com registradores/memória, escolha criativa e limitação conhecida).
- [ ] **Item 10.5:** *"O repositório contém código, documentação, fontes e registro de uso de IA."*
  - **Cumpriremos em:** `T-01`, `T-10` e `T-11` (`README.md`, `src/termips.asm`, `docs/ficha-tecnica.md`, `docs/testes.md`, `docs/uso-ia.md` alimentado por `.agents/logs/ai-log.md`).
- [ ] **Item 10.6:** *"Os links estão acessíveis e a versão final está identificada."*
  - **Cumpriremos em:** `T-12` (verificação de permissões do repositório GitHub e do link do vídeo + criação de tag/identificação da versão final de entrega em 07/12).
- [ ] **Item 10.7:** *"Todos os integrantes conseguem analisar o código para a prova individual obrigatória."*
  - **Cumpriremos em:** `T-13` (sessão coletiva e simulado sem consulta cobrindo acesso à memória, laços/decisões, pilha e alterações em `avaliar_tentativa` antes de 09/12).
