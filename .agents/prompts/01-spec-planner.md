# Prompt 01 — Agente de Especificação e Planejamento (SDD)

> **Registro histórico:** Prompt utilizado na Sessão 01 (`2026-10-26`) para gerar toda a especificação, arquitetura, tarefas, rastreabilidade, ADRs e rascunho da Fase 1 em `.agents/`.

```markdown
@[CRIATHON_MIPS_Enunciado.md]
# PAPEL

Você é um agente de **Spec-Driven Development (SDD)**. Sua função nesta etapa é
APENAS especificar e planejar. **Não escreva código .asm nesta etapa.**
Outros agentes executarão as tarefas de código e de documentação depois, a
partir dos artefatos que você produzir. Por isso, tudo deve ser explícito,
rastreável e verificável.

# CONTEXTO

Anexei `CRIATHON_MIPS_Enunciado.md`, o enunciado oficial de um trabalho de
Arquitetura e Organização de Computadores (Unifesp). **Leia o arquivo inteiro
antes de qualquer coisa.** Ele é a fonte da verdade: se algo que eu disser aqui
conflitar com o enunciado, o enunciado vence e você deve me avisar.

Objetivo do grupo: **atingir a nota com segurança, com escopo enxuto**. Não
queremos ambição técnica desnecessária. Queremos cumprir todos os requisitos,
entregar tudo documentado e todos os integrantes entenderem o código (haverá
prova individual sem consulta sobre um trecho do nosso próprio programa).

# IDEIA JÁ DECIDIDA

Jogo no estilo **Termo/Wordle** ("Detetive de códigos"), em Assembly MIPS no MARS.

Decisões já tomadas (trate como requisitos, mas aponte riscos se houver):
- Palavra secreta de 5 letras, 6 tentativas, interação por console (texto).
- Banco com cerca de 200 palavras embutido no `.data`, **uma `.asciiz` por
  linha**, 6 bytes por palavra (5 letras + terminador). A palavra de índice `i`
  fica em `banco + 6*i`. Todas em maiúsculas, ASCII, sem acento, sem repetição.
- Sorteio via syscall 42 (o resultado volta em `$a0`, não em `$v0`).
- **Modo teste** com palavra fixa (índice fixo), para testes reproduzíveis.
- Entrada via syscall 8. O buffer mantém o `\n`; a validação deve checar
  tamanho 5, somente letras, e converter minúsculas em maiúsculas.
- Qualquer sequência de 5 letras é aceita como tentativa (verificar se a
  palavra existe no banco é extensão opcional).
- Saída em texto, sem cores: `[X]` posição certa, `(X)` letra existe em outra
  posição, `.` letra ausente (o formato exato fica a seu critério, justifique).
- **Letras repetidas** tratadas por regra explícita em duas passadas: (1)
  marcar posições exatas e consumir essas letras do segredo; (2) para as
  restantes, marcar "existe" só se ainda sobrar a letra, consumindo-a.
- Leitura de banco de palavras de arquivo `.txt` (syscalls 13/14/16) é
  **extensão opcional**, só depois do núcleo pronto. Registre os riscos
  (caminho relativo depende da pasta onde o MARS foi iniciado; `\r\n` vs `\n`).
- Dados do grupo (nome, integrantes, quantidade): deixe como `[PREENCHER]`.

# O QUE VOCÊ DEVE PRODUZIR

Crie a estrutura abaixo. **Tudo que for gerado por IA fica dentro de `.agents/`.**

.agents/
├── README.md                  # como os agentes funcionam, convenções, ordem de uso
├── specs/
│   ├── 00-constitution.md     # princípios e restrições inegociáveis
│   ├── 01-spec.md             # O QUÊ e POR QUÊ
│   ├── 02-plan.md             # COMO (arquitetura e contratos)
│   ├── 03-tasks.md            # lista ordenada de tarefas executáveis
│   ├── 04-traceability.md     # matriz enunciado → requisito → tarefa → teste
│   └── 05-proposta-fase1.md   # rascunho da proposta a enviar no formulário
├── decisions/                 # um ADR curto por decisão relevante (ADR-001.md...)
├── logs/
│   └── ai-log.md              # registro cronológico (formato abaixo)
├── prompts/                   # os prompts usados em cada agente (inclua este)
└── open-questions.md          # dúvidas bloqueantes e suposições assumidas

(... instruções detalhadas por arquivo e regras de trabalho 1 a 8 ...)
```
