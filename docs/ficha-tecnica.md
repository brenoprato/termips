# Ficha Técnica — TerMIPS

> *Documento iniciado na Fase 1 (26/10). Os detalhes finais de implementação e contagem exata de instruções serão consolidados na Fase 2/3.*

## 1. Objetivo do Produto
O **TerMIPS** é um jogo interativo de adivinhação de palavras de 5 letras em até 6 tentativas, no estilo *Termo / Wordle*, executado por console no simulador MARS para a disciplina de Arquitetura e Organização de Computadores (ICT-Unifesp).

## 2. Organização Geral da Lógica (Prevista)
1. **Menu inicial:** Três opções simples — `1` para Jogar, `2` para abrir o Tutorial explicativo das regras e `0` para Sair.
2. **Entrada e validação:** Leitura da tentativa do usuário, verificação de tamanho exato de 5 letras (`A–Z`), conversão automática de minúsculas para maiúsculas e rejeição de entradas inválidas sem perda de tentativa.
3. **Avaliação em duas passadas:** Marcação prioritária de letras na posição exata (`[X]`) seguida da verificação de letras restantes presentes em outra posição (`(X)`), tratando repetições de letras sem duplicar pistas.
4. **Histórico e encerramento de partida:** Registro de cada jogada válida na memória e reexibição das tentativas acumuladas até a vitória ou o limite de 6 rodadas.

## 3. Estruturas de Dados e Procedimentos
- *A detalhar na Fase 2 conforme avanço de `src/termips.asm` (localização dos requisitos técnicos, uso de registradores/pilha e contagem de instruções úteis).*

## 4. Integrantes, Divisão de Tarefas e Feedback
- **Integrantes:**
  - Breno Porto Pinheiro do Prado
  - Felipe Pereira Silva
  - Guilherme Bento Ramos
  - José Rubens Freitas Pinheiro
  - Luis Fernando Giaquinto Zólio
- **Divisão de trabalho:** O desenvolvimento foi dividido em 5 frentes entre os integrantes (1. Menu, banco de palavras e sorteio; 2. Leitura e validação de entradas; 3. Avaliação das tentativas e letras repetidas; 4. Histórico, exibição e controle de partida; 5. Testes reproduzíveis, documentação e vídeo), com revisão cruzada de todo o código por todos os membros.
- **Síntese do feedback da Demonstração 1 (11/11):** `[A preencher após apresentação em laboratório]`
