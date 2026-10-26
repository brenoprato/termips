# TerMIPS

Jogo interativo de adivinhação de palavras em **Assembly MIPS** (simulador MARS), no estilo *Termo / Wordle*, desenvolvido pelos integrantes do grupo para o **CRIATHON MIPS** — *Arquitetura e Organização de Computadores* (ICT-Unifesp • Profa. Denise Stringhini).

---

## Sobre o Projeto

No **TerMIPS**, o programa sorteia uma palavra secreta de **5 letras** em português (sem acentos) e o jogador tem até **6 tentativas** para descobri-la pelo console de texto do MARS.

No menu inicial, o usuário pode iniciar uma partida, abrir o **Tutorial** (com a explicação completa das regras e símbolos) ou sair do jogo.

A cada tentativa válida, o programa analisa letra por letra — tratando corretamente casos de letras repetidas — e mostra o histórico das jogadas usando três símbolos simples:

- `[X]` — a letra `X` está na palavra e na **posição exata**;
- `(X)` — a letra `X` existe na palavra, mas em **outra posição**;
- ` . ` — a letra **não existe** na palavra secreta (ou suas ocorrências já foram preenchidas).

### Exemplo Rápido (Palavra secreta: `TERMO`)

```text
Menu: [1] Jogar  [2] Tutorial  [0] Sair
Opcao: 1

Tentativa 1/6 (ou 0 para sair): casas
[1/6] CASAS ->  .   .   .   .   . 

Tentativa 2/6 (ou 0 para sair): morro
[1/6] CASAS ->  .   .   .   .   . 
[2/6] MORRO -> (M)  .  [R]  .  [O]

Tentativa 3/6 (ou 0 para sair): termo
[1/6] CASAS ->  .   .   .   .   . 
[2/6] MORRO -> (M)  .  [R]  .  [O]
[3/6] TERMO -> [T] [E] [R] [M] [O]

PARABENS! Voce descobriu a palavra em 3/6 tentativas!
```

---

## Integrantes do Grupo

- **Grupo:** `[PREENCHER — Nome ou Número do Grupo]`
- `[PREENCHER — Nome Completo 1 — RA]`
- `[PREENCHER — Nome Completo 2 — RA]`
- `[PREENCHER — Nome Completo 3 — RA]`
- `[PREENCHER — Nome Completo 4 — RA]`
- `[PREENCHER — Nome Completo 5 — RA]`

---

## Estrutura do Repositório

```text
.
├── README.md               # Apresentação geral, instruções de uso e execução
├── src/
│   └── termips.asm         # Arquivo principal em Assembly MIPS
└── docs/
    ├── proposta-fase1.md   # Proposta inicial do projeto (Fase 1 — 26/10)
    ├── ficha-tecnica.md    # Organização lógica, estruturas e procedimentos
    ├── testes.md           # Casos de teste reproduzíveis (normal, inválido e limite)
    └── uso-ia.md           # Registro transparente de ferramentas de apoio e verificação
```

---

## Como Executar no MARS

1. Abra o simulador **MARS 4.5** (`Mars4_5.jar`).
2. No menu superior, clique em **File → Open...** e selecione `src/termips.asm`.
3. Monte o código em **Run → Assemble** (atalho `F3`).
4. Inicie a execução em **Run → Go** (atalho `F5`) e interaja pela aba **Run I/O** na parte inferior.

### Comandos Principais
- `1` no menu inicial: inicia uma nova partida.
- `2` no menu inicial: exibe o **Tutorial** explicando como o jogo e as dicas funcionam.
- `0` (no menu ou durante qualquer tentativa): encerra o programa imediatamente.
- Letras maiúsculas e minúsculas são aceitas normalmente; entradas com tamanho diferente de 5 letras ou com símbolos/números exibem aviso e **não descontam tentativa**.

---

## Limitações Conhecidas (Escopo Mínimo)

- Interação integralmente em modo texto no console padrão do MARS (sem cores ANSI).
- Palavras restritas a 5 letras do alfabeto de `A` a `Z` sem acentuação gráfica.
- Qualquer combinação de 5 letras é aceita como palpite na versão mínima.

---

## Vídeo de Demonstração

- **Link do vídeo (até 4 min):** `[A adicionar na Entrega Final — 07/12]`
