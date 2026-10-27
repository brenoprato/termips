# **CRIATHON MIPS**

## *Programa criativo em Assembly MIPS*

*Arquitetura e Organização de Computadores*  
**Profa. Denise Stringhini** • ICT Unifesp

# **1 Objetivo e organização**

Em grupos de até cinco estudantes, desenvolver um pequeno programa interativo e criativo em Assembly MIPS, executado no MARS. O trabalho integra a criação de um produto com a aplicação dos conteúdos de programação e organização de computadores estudados na disciplina.

O prazo de desenvolvimento será de aproximadamente um mês e meio. Todos os integrantes devem compreender o código e participar de uma prova específica individual ao final.

# **2 O produto**

O programa pode ser um jogo, uma ferramenta, uma simulação de robótica ou um modelo de sistema embarcado. Deve receber ações do usuário, processá-las e apresentar resultados compreensíveis. A lógica principal deverá ser implementada em MIPS. Recursos gráficos são opcionais, embora recomendáveis. 

Escolham uma ideia e definam sua versão mínima. A proposta deverá indicar as ações do usuário, os dados armazenados, as regras de processamento e a condição de encerramento. Os exemplos podem ser adaptados ou combinados, mantendo um escopo viável para o prazo disponível. Criatividade também pode aparecer nas regras, na narrativa e na apresentação dos resultados. Abaixo estão algumas **sugestões**.

| Ideia | Funcionamento e versão mínima |
| :---- | :---- |
| **Robô explorador de Marte** | O usuário movimenta um robô em uma grade com obstáculos, amostras e uma base. Cada movimento consome bateria. O programa armazena o mapa, controla posição e energia e registra as coletas. A missão termina ao retornar à base ou esgotar a bateria. Versão mínima: mapa fixo e um tipo de amostra. |
| **Robô de resgate** | O usuário conduz um robô por uma área com caminhos bloqueados e pessoas aguardando atendimento. O robô transporta uma pessoa por vez até um ponto seguro. O programa valida movimentos, controla a carga e contabiliza os resgates. Versão mínima: mapa pequeno, pessoas em posições fixas e movimentos por turnos. |
| **Robô aspirador** | Uma matriz representa paredes, sujeira e espaços livres. O usuário comanda o robô, que limpa a posição ocupada e consome energia. O programa atualiza o mapa, a bateria e a quantidade de células limpas. Versão mínima: controle manual. Uma rotina automática simples pode ser uma extensão. |
| **Braço robótico de classificação** | Peças chegam com tipo, tamanho e indicação de defeito. O usuário informa os dados e o programa decide qual recipiente deve recebê-las, respeitando sua capacidade. Armazenem contadores e estados dos recipientes e tratem peças inválidas. Versão mínima: três tipos de peça e três recipientes, sem animação física. |
| **Estufa espacial** | O usuário fornece leituras simuladas de temperatura, umidade e luminosidade. O programa decide quando ativar ventilação, irrigação e iluminação, exibe seus estados e registra leituras. Versão mínima: limites fixos e execução em rodadas. Como extensão, permitam alterar limites e apresentem um resumo do consumo simulado. |
| **Elevador com prioridades** | O usuário registra chamadas em diferentes andares. O programa armazena solicitações, escolhe a próxima parada por uma regra explícita e atualiza andar e direção. Versão mínima: quatro andares, um elevador e uma política simples, como atender na direção atual. Não é necessário encontrar uma rota ótima. |
| **Semáforo inteligente** | O usuário simula chegada de veículos, pedido de pedestre e passagem de um veículo de emergência. O programa controla fases em passos discretos e atende pedidos segundo prioridades, evitando sinais conflitantes. Versão mínima: um cruzamento pequeno, temporizadores representados por contadores e transições acionadas pelo usuário. |
| **Programe o robô** | O jogador monta e edita um vetor de comandos, como avançar, girar e coletar. O programa executa a sequência em uma grade, mostra cada mudança de estado e verifica se a missão foi cumprida. Versão mínima: mapa fixo e limite de comandos. O desafio pode premiar soluções com menos movimentos. |
| **Duelo de robôs por turnos** | Dois robôs possuem energia, defesa e ações disponíveis. A cada turno, os jogadores escolhem atacar, defender ou recarregar. O programa valida a ação, calcula os efeitos e atualiza os atributos até o encerramento. Versão mínima: dois jogadores locais e regras determinísticas, sem adversário inteligente ou ação em tempo real. |
| **Fábrica de robôs defeituosos** | O jogador escolhe testes para descobrir a falha de um robô e depois seleciona um reparo. Cada teste tem um custo. O programa consulta sintomas, controla orçamento e verifica o diagnóstico e o reparo. Versão mínima: três falhas com regras fixas. As pistas devem depender dos testes, e não apenas de perguntas prontas. |
| **Missão cooperativa de dois robôs** | O jogador alterna entre dois robôs em um mapa pequeno. Um aciona interruptores e o outro transporta um objeto. O programa mantém as duas posições, os estados de portas e o inventário, verificando condições para cada ação. Versão mínima: uma porta, um objeto e um destino, com movimentos por turnos. |
| **Escape room de uma nave** | O jogador precisa restaurar sistemas para escapar de uma nave. Suas ações alteram energia, equipamentos e portas. O programa controla inventário e condições de acesso, calcula efeitos e libera etapas. Versão mínima: três ambientes e dois desafios interligados. Evitem uma sequência de perguntas cujas respostas apenas estejam armazenadas. |
| **Batalha naval com sonar** | Dois jogadores posicionam embarcações em tabuleiros pequenos e escolhem coordenadas para atacar. Um sonar limitado informa uma pista sobre uma região. O programa valida posições, registra tiros, detecta acertos e controla recursos. Versão mínima: tabuleiros fixos para teste e poucas embarcações; não é necessário um adversário automático. |
| **Detetive de códigos** | O jogador tenta descobrir uma sequência secreta. A cada tentativa, o programa calcula quantos valores estão corretos e quantos estão na posição certa, tratando valores repetidos conforme regras explícitas. Armazenem segredo e tentativas e controlem o limite de jogadas. Versão mínima: segredo fixo configurável e quatro posições. |
| **Base espacial com recursos limitados** | Durante um número fixo de turnos, o jogador distribui energia entre oxigênio, reparos e pesquisa. O programa calcula consequências, atualiza recursos e verifica sobrevivência e conclusão da missão. Versão mínima: três recursos e eventos predefinidos. O histórico de rodadas pode permitir comparar duas estratégias de distribuição. |

Nas simulações de robótica e sistemas embarcados, sensores e atuadores serão representados por dados e estados do programa. Não é necessário utilizar hardware físico: entradas podem vir do teclado ou de sequências de valores previamente definidas.

# **3 Requisitos técnicos obrigatórios**

* Executar no MARS, com instruções claras de configuração e uso.  
* Receber entradas do usuário e produzir saídas relacionadas às suas ações.  
* Utilizar decisões e repetições com função real na lógica do programa.  
* Armazenar e manipular dados em pelo menos um vetor, matriz ou string.  
* Organizar o código em pelo menos três procedimentos com responsabilidades claras.  
* Utilizar parâmetros, retornos e preservação de registradores conforme boas práticas discutidas.  
* Empregar a pilha de forma justificada, por exemplo, para preservar dados em chamadas de procedimentos.  
* Tratar entradas inválidas e oferecer uma forma clara de encerrar a execução.

# **Critério de complexidade mínima**

O produto deve combinar pelo menos duas ações relacionadas, manter um estado que se altera durante a execução e manipular uma estrutura de dados. Por exemplo: um robô recebe comandos, verifica obstáculos e atualiza sua posição; um sistema de sensores armazena leituras, calcula um indicador e decide se deve emitir um alerta.

Como referência mínima de implementação, o código deverá conter pelo menos 100 linhas úteis de instruções MIPS, somadas entre os arquivos do projeto (se existir mais de um). Contar uma instrução por linha de origem, incluindo pseudoinstruções como uma única instrução. Não contar linhas em branco, comentários, rótulos isolados, diretivas ou declarações de dados.

O mínimo deve ser atingido pela lógica necessária ao produto. Repetições artificiais, instruções sem efeito e trechos não utilizados não serão considerados. Rotinas de entrada e saída podem compor a contagem, mas o programa deve também realizar processamento, controle de estado e acesso a dados. Ultrapassar o mínimo não gera pontuação adicional por si só.

# **Referência de escopo em C**

Pensem em um programa que, em C, teria uma rotina principal, pelo menos três funções auxiliares, vetor ou matriz, laço de interação e tratamento de entradas inválidas. Uma solução compacta desse tipo poderia ocupar aproximadamente 40 a 60 linhas úteis de C, dependendo do produto e do estilo de programação. Essa faixa é apenas uma referência de escopo, e não uma equivalência fixa entre linhas de C e instruções MIPS.

Não é necessário entregar uma versão em C. A avaliação considerará a combinação do tamanho mínimo com os comportamentos e requisitos técnicos exigidos. Na ficha técnica, informem a contagem de instruções e indiquem quais procedimentos realizam as ações principais e atualizam o estado do produto.

# **4 Fases do trabalho**

As datas e o canal de submissão serão informados pela professora. O cronograma abaixo considera quatro semanas de desenvolvimento e uma prova após a entrega final.

| Fase | Data |
| :---- | :---: |
| Submissão da ideia | **26/10** |
| Demonstração 1 em laboratório | **11/11** |
| Demonstração 2 (a confirmar), vídeo e repositório final | **07/12** |
| Prova individual | **09/12** |

# **Fase 1 • Submissão da ideia**

Enviar uma proposta via formulário, contendo:

* Nome provisório do produto, identificação do grupo e integrantes.  
* Objetivo, público previsto e descrição da interação principal.  
* Entradas, processamento e saídas esperadas, com um exemplo de uso.  
* Como os requisitos de MIPS serão incorporados ao programa.  
* Escopo mínimo, divisão inicial de tarefas e link do repositório no GitHub.

A proposta deverá permitir verificar sua adequação ao trabalho e sua viabilidade no prazo. Iniciem o estudo e a implementação do núcleo previsto enquanto eventuais ajustes são discutidos no canal da disciplina.

# **Fase 2 • Desenvolvimento e acompanhamento**

Atualizar o repositório ao longo do desenvolvimento, com mensagens de commit que descrevam as alterações. Não deixar o envio de todo o código apenas para a entrega final.

Demonstrar em laboratório na data indicada no Cronograma.

# **Fase 3 • Entrega final**

Enviar o link do vídeo final e o link do repositório organizado. Verificar previamente se a professora consegue acessar o repositório e o vídeo. O repositório pode ser público ou privado, desde que o acesso necessário à avaliação esteja concedido. A publicação pública não é requisito do trabalho.

# **5 Vídeo final**

O vídeo deverá ter até quatro minutos. A gravação de tela com narração é suficiente; não é necessário realizar edição elaborada. Incluam:

* Apresentação breve da ideia e demonstração da funcionalidade principal.  
* Um exemplo de uso normal e o tratamento de uma entrada inválida.  
* Execução passo a passo de um trecho central no MARS, explicando valores em registradores ou memória e uma decisão relevante.  
* Indicação breve de uma escolha criativa e de uma limitação conhecida.

# **6 Organização do GitHub**

O repositório deverá permitir que outra pessoa encontre o código, execute o programa e confira os testes sem ajuda dos autores. Usem a seguinte organização mínima:

| Arquivo ou pasta | Conteúdo esperado |
| :---- | :---- |
| **README.md** | Descrição do produto, integrantes, instruções de execução, versão e configurações do MARS utilizadas, comandos de interação, limitações e link do vídeo. |
| **src/** | Arquivos .asm e outros recursos necessários. Identifiquem o arquivo principal. Comentem os procedimentos e decisões que precisem de explicação. |
| **docs/ficha-tecnica.md** | Objetivo, organização da lógica, estruturas de dados, procedimentos e localização dos requisitos técnicos no código. Incluam contribuição de cada integrante e síntese do feedback recebido. |
| **docs/testes.md** | Três casos reproduzíveis: uso normal, entrada inválida e situação limite. Para cada um, informem entradas ou ações, resultado esperado, resultado observado e justificativa. |
| **docs/uso-ia.md** | Ferramentas utilizadas, finalidade, exemplo relevante de uso e verificação realizada. Caso não utilizem IA, declarem isso e documentem uma decisão técnica verificada. |

A ficha técnica deverá ser concisa, com extensão equivalente a até duas páginas. Não é necessário duplicar no texto o código já disponível no repositório.

# **Testes reproduzíveis**

Definam uma sequência precisa de ações para cada teste. A situação limite deve ser pertinente ao produto, como atingir a borda do mapa, preencher a última posição ou esgotar tentativas. Se houver aleatoriedade, ofereçam uma configuração com dados fixos para repetir os testes e expliquem como ativá-la.

# **7 Uso de inteligência artificial e outras fontes**

É permitido usar IA para discutir ideias, apoiar a programação e investigar erros. O grupo é responsável pela correção do código entregue e deve compreender os trechos produzidos com apoio dessas ferramentas.

Documentem uma sugestão relevante da IA, o que foi aproveitado ou alterado e como sua correção foi verificada. A verificação deve apresentar evidência concreta, como rastreamento manual comparado com a execução. Não é necessário entregar o histórico completo das conversas.

Identifiquem as fontes de códigos e recursos reutilizados e descrevam as adaptações feitas pelo grupo. Todos os integrantes devem estudar a implementação completa para a avaliação individual.

# **8 Prova final individual**

Após a entrega, será aplicada uma prova curta, individual e sem consulta com base nos trabalhos entregues. Não será permitido utilizar o MARS, o repositório, materiais de apoio ou ferramentas de IA durante a prova.

A prova terá quatro questões de múltipla escolha e uma questão aberta. As objetivas avaliarão a interpretação de pequenos trechos MIPS relacionados aos requisitos comuns do trabalho: acesso à memória, decisões e repetições, procedimentos e pilha.

A questão aberta apresentará um trecho central do programa do próprio grupo e uma alteração proposta pela professora. O(a) estudante deverá prever o efeito da alteração, apresentar um caso concreto e justificar a diferença por meio da execução das instruções. O trecho necessário será fornecido no enunciado; não será exigida memorização do código.

Uma descrição genérica do produto não será suficiente. A resposta aberta deverá indicar entradas ou estado inicial, comportamento previsto e justificativa coerente com o código fornecido.

# **9 Critérios de avaliação**

| Componente | Pontos |
| :---- | :---: |
| Funcionamento, aplicação de MIPS e complexidade mínima | **4,0** |
| Criatividade e clareza da interação no MARS | **2,0** |
| Processo e entrega com demonstração, testes, GitHub, documentação, vídeo e registro de IA | **2,0** |
| Prova final individual | **2,0** |
| **Total** | **10,0** |

Os três primeiros critérios somam até 8,0 pontos e são avaliados por grupo. A prova acrescenta até 2,0 pontos individuais: 0,25 por questão objetiva e 1,0 pela questão aberta. A nota de cada estudante será a soma da nota do grupo com sua nota na prova.

Na questão aberta, serão considerados: caso que evidencie o efeito da alteração, previsão correta do comportamento original, previsão correta do comportamento alterado e justificativa baseada na execução. Cada aspecto valerá até 0,25 ponto.

# **10 Conferência antes da entrega**

* O programa executa a funcionalidade principal, atende aos requisitos técnicos e cumpre o critério de complexidade mínima.  
* O README permite configurar e utilizar o produto sem ajuda do grupo.  
* Os três testes podem ser repetidos e têm resultados documentados.  
* O vídeo tem até quatro minutos e demonstra o código em execução.  
* O repositório contém código, documentação, fontes e registro de uso de IA.  
* Os links estão acessíveis e a versão final está identificada.  
* Todos os integrantes conseguem analisar o código para a prova individual obrigatória.