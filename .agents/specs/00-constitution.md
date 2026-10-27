# 00 — Constituição do Projeto (TERMO-MIPS)

Este documento estabelece os princípios e restrições **inegociáveis** do projeto para o trabalho **CRIATHON MIPS** (ICT-Unifesp). Nenhum agente de IA nem integrante do grupo pode violar estas regras sem atualizar explicitamente esta constituição e verificar compatibilidade com o enunciado oficial.

---

## Artigo 1 — Hierarquia de Autoridade
1. **`CRIATHON_MIPS_Enunciado.md` é a fonte suprema da verdade.** Se qualquer especificação, plano, tarefa ou sugestão de IA conflitar com o enunciado oficial, o enunciado vence obrigatoriamente e o conflito deve ser sinalizado ao grupo.
2. As decisões arquiteturais consolidadas em `.agents/decisions/` detalham *como* o grupo cumpre o enunciado sem extrapolá-lo.

## Artigo 2 — Escopo Enxuto e MVP Primeiro
1. **Segurança de nota acima de ambição técnica:** O núcleo mínimo viável (MVP) em console texto deve estar 100% funcional, testado e compreendido por todos antes de qualquer extensão opcional (como leitura de `.txt` via syscalls 13/14/16 ou variações extras).
2. Quando um requisito puder ser atendido de duas formas corretas, **prefere-se sempre a mais simples, curta e legível**.

## Artigo 3 — Honestidade de Código e Proibição de "Gordura Artificial"
1. O critério mínimo de **100 linhas úteis de instruções MIPS** (Seção 3, *Critério de complexidade mínima*) deve emergir naturalmente da lógica real do jogo (menu, sorteio/modo fixo, leitura/validação, comparação em duas passadas, histórico e exibição).
2. **São terminantemente proibidos:** instruções sem efeito (`nop`, `add $t0, $t0, $zero`), desenrolamento artificial de laços (*loop unrolling* só para inflar linhas), código morto ou repetições artificiais — o enunciado declara expressamente que não serão contabilizados (Seção 3).

## Artigo 4 — Disciplina Estrita de Arquitetura MIPS e Pilha (`$sp`)
1. **Convenção de registradores (ABI MIPS):**
   - `$a0–$a3`: passagem de parâmetros (podem ser alterados por `syscall` e `jal`).
   - `$v0–$v1`: retorno de valores de procedimentos e códigos de `syscall`.
   - `$t0–$t9`: temporários (*caller-saved* — o procedimento chamado pode alterá-los livremente; quem chama não pode assumir preservação após `jal`).
   - `$s0–$s7`: salvos (*callee-saved* — todo procedimento que modificar um `$sX` **deve** salvá-lo em `$sp` no prólogo e restaurá-lo no epílogo).
   - `$ra`: endereço de retorno — **deve** ser salvo em `$sp` em todo procedimento não-folha (que executa `jal`).
2. **Uso justificado da pilha (`$sp`):** A pilha só é alocada quando houver motivo real (preservação de `$ra` em chamadas aninhadas ou preservação de `$s0–$s7` através de chamadas/`syscall`). Procedimentos folha puros que usam apenas `$t0–$t9` não alocam pilha desnecessariamente (Seção 3).
3. **Precisão no MARS:**
   - `syscall 8` mantém `\n` quando cabe no buffer; validar comprimento útil antes de `\n`/`\r`/`\0`.
   - `syscall 42` retorna o inteiro sorteado em **`$a0`** (e não em `$v0`).
   - Diretivas `.word` devem vir antes de declarações de bytes/strings ou precedidas de `.align 2` para evitar falhas de alinhamento.
   - Todas as palavras do banco em `.data` devem conter **exatamente 5 bytes ASCII (`A-Z`) + `\0` (6 bytes)**, sem caracteres UTF-8 acentuados.

## Artigo 5 — Código Pensado para a Prova Individual Sem Consulta (Seção 8)
1. **Compreensão coletiva obrigatória:** Haverá prova individual sem consulta valendo 2,0 pontos sobre interpretação de MIPS (memória, decisões/laços, procedimentos, pilha) e previsão de alteração em um trecho central do próprio código (Seção 8 e Seção 9).
2. Procedimentos devem ser curtos (~15 a 50 instruções úteis), com única responsabilidade, fluxo linear claro e **rótulos/comentários em português** explicando o contrato de registradores e o motivo das decisões.
3. Evitar truques obscuros de manipulação de bits quando uma comparação aritmética/lógica legível resolver o problema com clareza.

## Artigo 6 — Rastreabilidade, Testes Reproduzíveis e Transparência no Uso de IA
1. Toda decisão técnica relevante gera um ADR curto em `.agents/decisions/`.
2. Todo uso de IA deve ser registrado em `.agents/logs/ai-log.md` e posteriormente sintetizado em `docs/uso-ia.md`, contendo obrigatoriamente **verificação humana com evidência concreta** (rastreamento manual de registradores comparado à execução real no MARS — Seção 7).
3. O **Modo Teste** determinístico (palavra fixa configurável) é requisito obrigatório do MVP para garantir reprodutibilidade dos três casos de teste exigidos (Seção 6).
