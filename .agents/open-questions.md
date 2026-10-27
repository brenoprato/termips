# Questões em Aberto, Itens `[PREENCHER]` e Suposições Assumidas

Este arquivo registra (1) dados administrativos que dependem exclusivamente do grupo, (2) ambiguidades não bloqueantes com as suposições padrão adotadas na especificação e (3) detalhes técnicos do simulador MARS que devem ser confirmados experimentalmente na primeira tarefa de código.

---

## 1. Dúvidas Bloqueantes

- **Nenhuma dúvida bloqueante identificada.** Todas as decisões centrais de escopo, regras, entrada/saída e arquitetura já foram definidas pelo grupo ou possuem suposição padrão segura abaixo, permitindo concluir integralmente a etapa de especificação e planejamento.

---

## 2. Dados Administrativos a Preencher pelo Grupo (`[PREENCHER]`)

| ID | Item | Onde impacta | Status |
| :--- | :--- | :--- | :--- |
| **ADM-01** | Nome definitivo do grupo / número da equipe | `05-proposta-fase1.md`, `README.md`, `docs/ficha-tecnica.md` | `[PREENCHER]` |
| **ADM-02** | Nomes completos dos 5 integrantes | `05-proposta-fase1.md`, `README.md`, `docs/ficha-tecnica.md` | **Preenchido** (Breno, Felipe, Guilherme, José Rubens, Luis Fernando) |
| **ADM-03** | Divisão inicial de tarefas da Fase 1 | `05-proposta-fase1.md`, `docs/proposta-fase1.md` | **Preenchido** (dividida em 5 frentes sem atrelar nomes fixos) |
| **ADM-04** | URL do repositório GitHub | `05-proposta-fase1.md`, `README.md` | **Preenchido** (`https://github.com/brenoprato/termips`) |
| **ADM-05** | Versão exata do MARS adotada pelo grupo | `README.md`, `docs/ficha-tecnica.md` | **Confirmado** (MARS 4.6U / 4.5+) |
| **ADM-06** | Nome definitivo do produto | `05-proposta-fase1.md`, `README.md` | **Decidido: `TerMIPS`** |

---

## 3. Ambiguidades Não Bloqueantes e Suposições Padrão Adotadas

| ID | Questão / Ambiguidade | Suposição Padrão Adotada | Onde está documentada | Como reverter se o grupo preferir outra opção |
| :--- | :--- | :--- | :--- | :--- |
| **OQ-01** | **Armazenamento de tentativas anteriores:** A descrição de "Detetive de códigos" na Seção 2 diz *"Armazenem segredo e tentativas"*, mas o briefing mencionou apenas o banco e o buffer de leitura. | **Armazenar o histórico das até 6 tentativas e seus status em `.data`** (`historico_palavras` e `historico_status`) e reexibir o tabuleiro acumulado a cada jogada. Garante cumprimento inequívoco das Seções 2 e 3 com custo mínimo (~30 instruções). | `ADR-003`, `RF-06`, `02-plan.md` | Basta confirmar se o grupo concorda (altamente recomendado manter por segurança de nota). |
| **OQ-02** | **Como ativar o Modo Teste:** Constante alterada em `.data` antes de montar ou opção explícita no menu inicial em tempo de execução? | **Ambos combinados:** O menu inicial oferece `[1] Jogar (sorteio)` e `[2] Modo Teste (palavra fixa)`, sendo que o índice usado na opção `[2]` vem da word `indice_teste` em `.data` (padrão `0` → `"TERMO"`). Assim o avaliador testa sem precisar editar `.asm` e também pode mudar `indice_teste` se quiser. | `RF-01`, `RF-02`, `02-plan.md` | Se o grupo preferir ocultar do menu e usar apenas flag em `.data`, remover opção `2` em `main`. |
| **OQ-03** | **Sentido de "sem repetição" no banco de 200 palavras:** Refere-se a não repetir a mesma palavra duas vezes no banco ou a palavras sem letras repetidas internamente? | **Sem palavras duplicadas no banco** (200 palavras distintas de 5 letras). Palavras com letras repetidas (ex.: `CARTA`, `PAPEL`, `MORRO`) são permitidas no banco porque o algoritmo de duas passadas já foi exigido especificamente para tratar letras repetidas no segredo e no palpite. Índice `0` (`"TERMO"`) tem 5 letras distintas para clareza didática nos testes base, e incluímos palavra com letra repetida no índice `1` (`"CARTA"`) para testes extras. | `ADR-001`, `01-spec.md`, `02-plan.md` | Caso o grupo queira apenas isogramas (5 letras distintas) no banco, basta filtrar a lista na tarefa `T-06`. |
| **OQ-04** | **Comando para encerrar a execução a qualquer momento (Seção 3, item 8):** Qual entrada usar durante uma partida sem colidir com tentativas válidas? | **Digitar `0` + Enter** tanto no menu inicial quanto durante qualquer tentativa. Como entradas válidas exigem 5 letras alfabéticas (`A-Z`), `'0'` jamais colide com um palpite válido. | `ADR-005`, `RF-03`, `RF-09` | Pode ser trocado por `SAIR` (4 letras) se preferirem comando textual. |
| **OQ-05** | **Fluxo ao terminar uma partida (vitória ou 6ª tentativa):** Encerrar imediatamente o programa ou voltar ao menu principal? | **Voltar ao menu principal** (`[1] Jogar`, `[2] Modo Teste`, `[0] Sair`), permitindo jogar outra rodada na mesma execução ou encerrar com `0`. Se o jogador digitar `0` no meio da partida, volta ao menu ou encerra direto (definimos encerrar limpo via `syscall 10` ao escolher `0` no menu e cancelar partida atual voltando ao menu — ou saindo direto — ver `RF-03`/`RF-09`: padronizamos `0` na partida encerrando a partida e voltando ao menu, e `0` no menu chamando `syscall 10`, ou `0` encerrando imediatamente; optamos por **`0` encerrar imediatamente o programa via `syscall 10` em qualquer ponto** para máxima simplicidade e previsibilidade nos testes!). | `ADR-005`, `RF-09`, `02-plan.md` | Ajustável em 2 linhas no desvio de retorno de `executar_partida` em `main`. |
| **OQ-06** | **Formato exato da saída sem cores:** Apenas `.` solto perde a referência visual de qual letra foi digitada. | Formato alinhado por linha: `[n/6] PALAV -> [X] (Y)  .   .  (Z)` (mostra a palavra tentada à esquerda e as 5 células de largura fixa de 3 caracteres à direita: `[X]`, `(X)`, ` . `). | `ADR-004`, `RF-07`, `02-plan.md` | Ajustável apenas em `imprimir_linha_tentativa`, sem tocar na lógica de `avaliar_tentativa`. |
| **OQ-07** | **Idioma de rótulos e comentários em `.asm` vs skills globais do ambiente:** | **Português claro** (rótulos ASCII sem acento, comentários em português) para maximizar compreensão do grupo na prova individual sem consulta (Seção 8). | `00-constitution.md`, `ADR-006` | — |

---

## 4. Detalhes Técnicos Marcados como "A Verificar no MARS"

Estes itens devem ser validados experimentalmente no MARS durante as tarefas `T-02` e `T-06` e registrados em `logs/ai-log.md`:

1. **`syscall 42` e semente entre execuções (*Reset* no MARS):** Confirmar se chamar `syscall 42` com `$a0 = 0` e `$a1 = 200` (sem chamar `syscall 40`) já produz índices variados a cada clique em *Reset + Go* no MARS 4.5, ou se é recomendável inicializar a semente uma vez em `main` usando `syscall 30` (tempo do sistema em `$a0`) seguido de `syscall 40` (`$a0 = 0`, `$a1 = tempo`).
2. **Comportamento do console Run I/O da `syscall 8` em linhas muito longas (> 63 caracteres):** Com `$a1 = 64`, qualquer tentativa de até 62 caracteres + `\n` + `\0` é consumida inteira em uma única leitura. Verificar no MARS se digitar > 64 caracteres numa única linha deixa bytes residuais para a próxima `syscall 8` (e registrar como limitação conhecida no `README.md` se aplicável).
3. **Contagem de instruções expandidas vs linhas de código-fonte:** O enunciado (Seção 3, linha 55) explicita: *"Contar uma instrução por linha de origem, incluindo pseudoinstruções como uma única instrução."* Confirmar na aba *Execute / Text Segment* vs arquivo `.asm` a contagem exata ao preencher `docs/ficha-tecnica.md`.
