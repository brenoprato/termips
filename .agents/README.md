# Guia de Agentes e Fluxo SDD — CRIATHON MIPS (TERMO-MIPS)

Este diretório (`.agents/`) concentra todos os artefatos de **Spec-Driven Development (SDD)** gerados com apoio de Inteligência Artificial para planejar, rastrear e auditar o desenvolvimento do jogo **TERMO-MIPS** (*Detetive de Códigos*) na disciplina de *Arquitetura e Organização de Computadores* (ICT-Unifesp — Profa. Denise Stringhini).

> **Regra de separação de pastas:** Artefatos internos de planejamento de IA vivem em `.agents/`. Os entregáveis oficiais exigidos pela Seção 6 do enunciado (`README.md`, `src/`, `docs/ficha-tecnica.md`, `docs/testes.md`, `docs/uso-ia.md`) serão construídos e mantidos na raiz do repositório nas tarefas correspondentes de `specs/03-tasks.md`, alimentados pelos documentos daqui.

---

## 1. Estrutura do Diretório `.agents/`

```text
.agents/
├── README.md                  # Este guia: fluxo SDD, papéis de agentes, convenções e mapeamento
├── specs/
│   ├── 00-constitution.md     # Princípios e restrições inegociáveis do projeto
│   ├── 01-spec.md             # Especificação de produto (O QUÊ e POR QUÊ)
│   ├── 02-plan.md             # Arquitetura MIPS, layout .data, contratos, pseudocódigo e testes (COMO)
│   ├── 03-tasks.md            # Tarefas atômicas ordenadas por marcos do cronograma
│   ├── 04-traceability.md     # Matriz Enunciado → Requisito → Procedimento → Tarefa → Teste + Checklist Seção 10
│   └── 05-proposta-fase1.md   # Rascunho pronto para submissão do formulário da Fase 1 (26/10)
├── decisions/                 # Registros curtos de decisões arquiteturais (ADR-001 a ADR-006)
├── logs/
│   └── ai-log.md              # Registro cronológico auditável de todas as sessões com IA
├── prompts/                   # Histórico do prompt inicial e templates para os próximos agentes
│   ├── 01-spec-planner.md     # Prompt original usado nesta etapa de especificação
│   ├── 02-coder-mips.md       # Prompt padrão para implementar uma tarefa MIPS por vez
│   ├── 03-reviewer-mips.md    # Prompt padrão para auditar código .asm e convenções MIPS
│   └── 04-documenter.md       # Prompt padrão para gerar os entregáveis finais em docs/ e README.md
└── open-questions.md          # Dúvidas não bloqueantes, itens [PREENCHER] e suposições assumidas
```

---

## 2. Ordem de Uso dos Agentes (Workflow)

1. **Alinhamento Humano Inicial (Hoje — Fase 1):**
   - Revisar `open-questions.md` e preencher os campos `[PREENCHER]` em `specs/05-proposta-fase1.md`.
   - Submeter a proposta da Fase 1 (26/10) e criar o repositório no GitHub com commits incrementais (Seção 4).
2. **Implementação Incremental por Tarefa (`prompts/02-coder-mips.md`):**
   - Executar **uma tarefa por vez** seguindo a ordem de `specs/03-tasks.md`.
   - O agente *Coder MIPS* lê `00-constitution.md`, `02-plan.md` e a tarefa `T-XX`, escreve apenas o trecho correspondente em `src/termips.asm` e fornece o roteiro de verificação.
3. **Verificação Humana no MARS (Obrigatória — Seção 7):**
   - **Nenhum código vai para commit definitivo sem teste no MARS.**
   - O integrante responsável executa passo a passo no MARS, confere os registradores/memória indicados na tarefa e preenche o campo **Verificação feita pelo humano** em `logs/ai-log.md`.
4. **Revisão de Qualidade e Convenção MIPS (`prompts/03-reviewer-mips.md`):**
   - Antes dos marcos (Demonstração 1 em 11/11 e Entrega Final em 07/12), o agente *Reviewer MIPS* audita alinhamento de `.data`, preservação de `$s0–$s7`/`$ra` em `$sp`, ausência de instruções sem efeito e aderência aos contratos de `02-plan.md`.
5. **Geração e Consolidação da Documentação Oficial (`prompts/04-documenter.md`):**
   - Gera/atualiza `README.md`, `docs/ficha-tecnica.md` (máx. 2 páginas), `docs/testes.md` e `docs/uso-ia.md` a partir dos artefatos validados em `.agents/` e dos resultados reais observados no MARS.

---

## 3. Convenções Obrigatórias para Todos os Agentes

> **Nota importante sobre skills globais do ambiente:** Caso o ambiente de execução possua *skills* genéricas que peçam diretório `.context/` ou código estritamente em inglês, **a Constituição deste projeto (`specs/00-constitution.md`) prevalece**: os artefatos de SDD ficam em `.agents/` e os rótulos, identificadores e comentários em `src/termips.asm` devem ser escritos em **português claro (ASCII, sem acentos nos rótulos)** para facilitar o estudo e a prova individual sem consulta (Seção 8).

- **Fonte da verdade:** `CRIATHON_MIPS_Enunciado.md` vence qualquer divergência.
- **Rótulos MIPS:** `snake_case` em português sem acentos (ex.: `ler_e_validar_tentativa`, `avaliar_tentativa`, `laco_passada1`).
- **Comentários em `.asm`:** Explicar o papel de cada registrador no cabeçalho do procedimento e o *porquê* de cada desvio/acesso à memória (não narrar literalmente `soma 1 em $t0`).
- **Registro de IA:** Toda sessão que gere ou altere código ou documentação deve ganhar uma entrada datada em `logs/ai-log.md`.

---

## 4. Mapeamento `.agents/` → Entregáveis Oficiais do Enunciado (Seção 6)

| Entregável Oficial (Seção 6) | Artefatos-Fonte em `.agents/` |
| :--- | :--- |
| `README.md` (raiz) | `specs/01-spec.md` (visão, comandos, limitações), `specs/02-plan.md` (configuração MARS e modo teste), `open-questions.md` (integrantes e links) |
| `src/termips.asm` | `specs/00-constitution.md`, `specs/02-plan.md` (layout `.data`, tabela de procedimentos, pseudocódigo), `decisions/ADR-001` a `ADR-006` |
| `docs/ficha-tecnica.md` | `specs/01-spec.md` (objetivo), `specs/02-plan.md` (estruturas, procedimentos, localização dos RTs e contagem de linhas), `specs/04-traceability.md` |
| `docs/testes.md` | `specs/02-plan.md` (seção de 3 casos calculados à mão: CT-01, CT-02, CT-03) + observações reais colhidas no MARS |
| `docs/uso-ia.md` | `logs/ai-log.md`, `decisions/ADR-*.md` e `prompts/*.md` |
| Formulário Fase 1 (Seção 4) | `specs/05-proposta-fase1.md` |
