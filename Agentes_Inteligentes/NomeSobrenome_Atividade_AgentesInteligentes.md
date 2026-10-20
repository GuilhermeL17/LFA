# NomeSobrenome_Atividade_AgentesInteligentes

## Identificação

| Campo | Informação |
|---|---|
| Nome completo | [preencher] |
| Matrícula | [preencher] |
| Curso / Turma | [preencher] |
| Disciplina | Inteligência Artificial |
| Professora | [preencher] |
| Atividade | A2 (1,0 ponto): Agentes inteligentes |
| Material-base | Capítulo 2, Agentes inteligentes (aula de 20/10/2026) |
| Prazo e formato de envio | Conforme orientação da professora |

**Conteúdo:** Item 2.3 (nove afirmações) · Item 2.4 (sete casos com PEAS e classificação) · Item 2.5 (dez definições) · Síntese com autômatos · Conferência final · Referências.

---

## Item 2.3: Verdadeiro ou falso, com justificativa e exemplo/contraexemplo

**Hipóteses gerais.**
- **Agente racional:** para cada sequência de percepções possível, escolhe a ação que maximiza o valor **esperado** da medida de desempenho, dadas as percepções e o conhecimento prévio do agente.
- Racionalidade não é onisciência, nem clarividência, nem sucesso garantido. Ela é avaliada *ex ante* (pela informação disponível), não *ex post* (pelo resultado).
- **Função de agente:** mapeamento abstrato de sequências de percepções para ações. **Programa de agente:** implementação concreta, executada em uma arquitetura com recursos finitos.
- **Leitura dos quantificadores:** "todo" e "nunca" são universais, e **um único contraexemplo** os refuta; "existe", "existem" e "é possível" são existenciais, e **um único exemplo** os prova; "não pode" afirma impossibilidade em todos os casos, e um caso contrário a refuta.

### a. Um agente que detecta apenas informações parciais sobre o estado não pode ser perfeitamente racional.

**Julgamento: F**

**Justificativa:** A racionalidade é definida sobre a *sequência de percepções*, não sobre o estado completo. A observabilidade parcial pode reduzir o desempenho *alcançável*, mas não impede que o agente seja ótimo dentro do que é possível. Para a afirmação ser verdadeira, a observabilidade parcial teria de tornar a racionalidade impossível em todos os casos; basta um caso contrário para refutá-la.

**Contraexemplo:** Aspirador de pó de dois quadrados (A e B) com sensor apenas de posição e de sujeira local. Condições: desempenho = +1 por quadrado limpo a cada passo; ações = Esquerda, Direita, Aspirar, NoOp; geografia conhecida; ações executadas corretamente. A regra "se sujo, aspira; senão, vai ao outro quadrado" é racional, embora o agente não veja o quadrado vizinho. No pôquer, o agente também nunca vê as cartas dos oponentes e pode decidir de forma ótima.

### b. Existem ambientes de tarefa nos quais nenhum agente reativo puro pode comportar-se racionalmente.

**Julgamento: V**

**Justificativa:** "Existem" exige apenas **um** ambiente. O agente reativo puro decide só com a percepção atual, sem memória. Quando a ação ótima depende do histórico e a percepção atual é a mesma em situações que pedem ações diferentes, nenhum mapeamento percepção→ação acerta em todas.

**Exemplo:** Agente que deve sair de um labirinto desconhecido com percepção local idêntica em vários pontos (por exemplo, "corredor livre à frente"). A ação correta depende do caminho já percorrido (não repetir ciclos), informação que o agente reativo não possui. Hipótese: a medida de desempenho recompensa sair do labirinto em poucos passos.

### c. Existe um ambiente de tarefa em que todo agente é racional.

**Julgamento: V**

**Justificativa:** "Existe" pede um único exemplo. Basta um ambiente em que todas as ações tenham o mesmo desempenho esperado máximo.

**Exemplo:** Ambiente em que a medida de desempenho é constante, independentemente das ações (ou em que só há uma ação possível). Qualquer agente obtém o desempenho máximo possível e, portanto, é racional. Condição: o desempenho não depende das ações do agente.

### d. A entrada para o programa de agente é a mesma que a entrada para a função de agente.

**Julgamento: F**

**Justificativa:** A função de agente recebe a **sequência completa de percepções**. O programa de agente recebe apenas a **percepção atual**, vinda dos sensores; se precisar do histórico, deve armazená-lo em memória própria.

**Contraexemplo:** Agente de tabela de consulta: a função é definida sobre sequências inteiras, mas o programa recebe uma percepção por vez e acumula a sequência internamente antes de consultar a tabela. Num agente reativo simples, o programa nem guarda histórico, embora a função correspondente seja definida sobre sequências.

### e. Toda função de agente é implementável por uma combinação de programa/máquina.

**Julgamento: F**

**Justificativa:** "Toda" é universal; basta uma função não implementável. Funções de agente são mapeamentos arbitrários, incluindo os não computáveis, e há infinitas sequências de percepções possíveis, enquanto a máquina tem memória e tempo finitos.

**Contraexemplo:** Função cuja ação é "Sim" se o programa descrito na percepção para e "Não" caso contrário (problema da parada), não computável por nenhuma máquina. Também inviável: uma tabela de consulta completa para o xadrez, com mais entradas que átomos no universo. Hipótese: máquina = computador com recursos finitos.

### f. Suponha que um agente selecione sua ação uniformemente ao acaso do conjunto de ações possíveis. Existe um ambiente de tarefa determinista em que esse agente é racional.

**Julgamento: V**

**Justificativa:** "Existe" pede um caso. O agente aleatório é racional se todas as ações levam ao mesmo desempenho esperado máximo; o determinismo do ambiente não impede isso.

**Exemplo:** Mundo do aspirador determinista em que todos os quadrados já estão limpos e permanecem limpos, com desempenho = número de quadrados limpos por passo e **sem penalidade por movimento**. Qualquer ação (Esquerda, Direita, Aspirar, NoOp) mantém o desempenho máximo. Se houvesse penalidade por movimento, o agente aleatório deixaria de ser racional.

### g. É possível para um dado agente ser perfeitamente racional em dois ambientes de tarefa distintos.

**Julgamento: V**

**Justificativa:** "É possível" exige só um caso. A racionalidade depende do par (agente, ambiente de tarefa); um mesmo agente pode ser ótimo em dois ambientes cujas diferenças não alteram a ação ótima.

**Exemplo:** O aspirador reativo simples ("se sujo, aspira; senão, vai ao outro quadrado") é racional no mundo de dois quadrados e também em uma variante com outra distribuição inicial de sujeira, mantidas a mesma medida de desempenho e a mesma geografia. Caso trivial: o ambiente do item (c) e qualquer variante dele.

### h. Todo agente é racional em um ambiente não observável.

**Julgamento: F**

**Justificativa:** "Todo" é universal; basta um agente irracional. Sem observação, ainda existem ações melhores e piores em valor esperado, e escolher as piores é irracional.

**Contraexemplo:** Aspirador sem sensores, com sujeira inicial desconhecida e probabilidade positiva em cada quadrado, desempenho = quadrados limpos ao longo do tempo. Um agente que fica em NoOp para sempre tem desempenho esperado pior que um que alterna Aspirar e Mover (e limpa os dois quadrados). O primeiro não é racional.

### i. Um agente jogador de pôquer perfeitamente racional nunca perde.

**Julgamento: F**

**Justificativa:** "Nunca" é absoluto. A racionalidade maximiza o desempenho **esperado**, não garante o resultado. O pôquer tem informação oculta e aleatoriedade na distribuição das cartas; mesmo a melhor decisão pode perder numa mão específica.

**Contraexemplo:** O agente aposta com uma mão que vence em 95% dos casos; em 5% o oponente completa uma sequência e o agente perde. A decisão foi racional (valor esperado positivo), embora o resultado tenha sido negativo.

| Item | Julgamento | Termo-chave |
|---|---|---|
| a | F | "não pode": basta um caso racional |
| b | V | "existem": basta um ambiente |
| c | V | "existe": ambiente de desempenho constante |
| d | F | programa: percepção atual; função: sequência |
| e | F | "toda": funções não computáveis/inviáveis |
| f | V | "existe": todas as ações equivalentes |
| g | V | "é possível": basta um caso |
| h | F | "todo": basta um agente ruim |
| i | F | "nunca": racional ≠ garantia de ganho |

---

## Item 2.4: PEAS e classificação dos ambientes

**Critérios gerais.**
- O agente é a entidade que decide e age; regras, objetos e outros participantes pertencem ao ambiente.
- **Estocástico:** o próximo estado não é totalmente determinado pelo estado atual e pela ação do agente (inclui ruído físico, falhas e decisões imprevisíveis de outros agentes).
- **Estático/dinâmico/semidinâmico:** refere-se ao que ocorre enquanto o agente delibera.
- As classificações dependem das hipóteses declaradas em cada caso e mudariam se elas mudassem.

### Caso 1: Jogar futebol

**Agente analisado:** um jogador de linha (humano ou robô). **Objetivo e hipóteses:** partida oficial 11 x 11 com árbitro, regras da FIFA e clima normal; colegas e adversários são outros agentes.

- **P (desempenho):** gols marcados menos gols sofridos, vitória da equipe, poucas faltas e cartões, uso eficiente da energia.
- **E (ambiente):** campo, bola, traves, colegas, adversários, árbitro, gramado, clima, torcida (ruído).
- **A (atuadores):** pernas e pés (correr, chutar, driblar, desarmar), cabeça, tronco, voz e gestos.
- **S (sensores):** visão (campo limitado), audição (gritos, apito), tato e propriocepção.

| Propriedade | Classificação | Justificativa |
|---|---|---|
| Observabilidade | Parcialmente observável | Não vê o que está atrás de si nem as intenções dos outros jogadores. |
| Número de agentes | Multiagente: cooperativo (colegas) e competitivo (adversários) | Colegas têm o mesmo objetivo; adversários, o oposto. |
| Previsibilidade | Estocástico | Ruído no quique, vento, erro de execução e adversários imprevisíveis. |
| Dependência entre decisões | Sequencial | Posicionamento, passes e cansaço afetam jogadas futuras. |
| Mudança durante a decisão | Dinâmico | Bola e jogadores se movem enquanto o agente decide. |
| Natureza dos elementos | Combinação | Posição, velocidade e tempo são contínuos; placar e eventos de jogo, discretos. |
| Conhecimento do funcionamento | Conhecido | Regras e física são conhecidas; o que falta é saber a estratégia dos adversários, o que é incerteza sobre o estado, não sobre as regras. |

### Caso 2: Explorar os oceanos subterrâneos de Titã (cenário hipotético)

**Agente analisado:** robô submarino autônomo lançado em um oceano sob a crosta de gelo. **Objetivo e hipóteses:** não há vida inteligente nem outros robôs; a comunicação com a Terra tem atraso de dezenas de minutos ou mais, então o robô decide sozinho.

- **P:** quantidade e qualidade de dados científicos (mapas, química, possíveis bioassinaturas), integridade do robô, uso eficiente de energia, risco mínimo.
- **E:** oceano de água líquida, pressão e temperatura extremas, gelo, correntes, fundo desconhecido, o robô e a base na superfície.
- **A:** propulsores e lemes, lastro, derretedor térmico ou broca, braço de amostragem, aquecedores, transmissor de dados.
- **S:** sonar, câmeras, sensores de pressão, temperatura, salinidade e composição química, unidade inercial (IMU), medidor de bateria, receptor de comandos.

| Propriedade | Classificação | Justificativa |
|---|---|---|
| Observabilidade | Parcialmente observável | Percebe só o entorno imediato; topografia e correntes à frente são desconhecidas. |
| Número de agentes | Agente único | Pela hipótese de ausência de vida e de outros robôs. |
| Previsibilidade | Estocástico | Correntes, ruído dos sensores, falhas e dinâmica do gelo não são totalmente previsíveis. |
| Dependência entre decisões | Sequencial | Energia, posição e riscos se acumulam; uma decisão ruim pode encerrar a missão. |
| Mudança durante a decisão | Dinâmico | Correntes e o deslocamento do robô continuam durante a deliberação. |
| Natureza dos elementos | Contínuos (predominantemente) | Posição, pressão, temperatura, velocidade e tempo são contínuos; os comandos de alto nível são discretos. |
| Conhecimento do funcionamento | Desconhecido (parcialmente) | A física geral é conhecida, mas o oceano de Titã nunca foi explorado; o agente precisa aprender o modelo. |

### Caso 3: Comprar livros usados de IA na Internet

**Agente analisado:** agente de software comprador que age por um usuário. **Objetivo e hipóteses:** orçamento e lista de livros definidos pelo usuário; lojas e marketplaces acessíveis por API ou página web; pagamento eletrônico autorizado.

- **P:** custo total mínimo (preço + frete), conformidade com a lista, condição adequada do livro, prazo de entrega, confiabilidade do vendedor, respeito ao orçamento.
- **E:** sites e marketplaces, anúncios, vendedores, outros compradores, avaliações, sistema de pagamento, transportadoras.
- **A (saídas digitais):** buscar, filtrar, comparar, enviar mensagem ao vendedor, adicionar ao carrinho, pagar, avaliar a compra.
- **S (entradas digitais):** páginas e respostas de API (preço, descrição, fotos, avaliações, estoque), e-mails de confirmação, rastreamento.

| Propriedade | Classificação | Justificativa |
|---|---|---|
| Observabilidade | Parcialmente observável | O estado real do livro, a honestidade do vendedor e o estoque atual não são totalmente visíveis. |
| Número de agentes | Multiagente: competitivo (outros compradores) e parcialmente cooperativo (negociação com o vendedor) | Compradores disputam o mesmo exemplar; vendedor quer preço alto, agente quer preço baixo. |
| Previsibilidade | Estocástico | Anúncios somem, preços mudam, envios atrasam e fraudes ocorrem. |
| Dependência entre decisões | Sequencial | Orçamento, estoque único e reputação ligam as compras entre si. |
| Mudança durante a decisão | Dinâmico | Preços e estoque mudam enquanto o agente pesquisa. |
| Natureza dos elementos | Discretos | Itens, ações, estados de pedido e preços (em centavos) são discretos; o tempo pode ser tratado como discreto. |
| Conhecimento do funcionamento | Conhecido (regras da compra) | Os procedimentos são conhecidos; a incerteza está nas informações, não nas regras. |

### Caso 4: Jogar uma partida de tênis

**Agente analisado:** um tenista (humano ou robô). **Objetivo e hipóteses:** partida de simples, regras oficiais, quadra ao ar livre com vento fraco.

- **P:** vencer a partida (pontos, games, sets), minimizar erros não forçados, preservar energia e evitar lesões.
- **E:** quadra, rede, bola, raquete, adversário, árbitro, vento, piso.
- **A:** pernas (deslocamento), braço e raquete (golpes, saque), tronco, voz.
- **S:** visão (bola e adversário), audição (contato da bola), propriocepção e tato.

| Propriedade | Classificação | Justificativa |
|---|---|---|
| Observabilidade | Parcialmente observável | Não vê a intenção do adversário nem percebe com precisão o efeito (spin) e a velocidade da bola. |
| Número de agentes | Multiagente competitivo | O adversário tem objetivo oposto (em duplas haveria também cooperação). |
| Previsibilidade | Estocástico | Ruído motor, vento, quique irregular e estratégia imprevisível do adversário. |
| Dependência entre decisões | Sequencial | Posicionamento, fadiga e placar ligam um ponto ao seguinte; dentro do ponto, cada golpe define o próximo. |
| Mudança durante a decisão | Dinâmico | Bola e adversário se movem enquanto o jogador decide. |
| Natureza dos elementos | Combinação | Física, posição e tempo são contínuos; placar e regras, discretos. |
| Conhecimento do funcionamento | Conhecido | O estilo do adversário é desconhecido, mas as regras e a física do jogo são conhecidas. |

### Caso 5: Praticar tênis contra uma parede

**Agente analisado:** o tenista em treino. **Objetivo e hipóteses:** parede lisa e plana, sem vento, um único jogador.

- **P:** número de rebatidas consecutivas, precisão do golpe e do ponto de retorno, regularidade da técnica.
- **E:** parede, bola, piso, raquete, espaço de treino.
- **A:** pernas, braço e raquete, tronco.
- **S:** visão, audição e propriocepção.

| Propriedade | Classificação | Justificativa |
|---|---|---|
| Observabilidade | Parcialmente observável (em grau pequeno) | Bola, parede e posição própria são visíveis, mas o spin e a velocidade exata da bola são difíceis de perceber. Numa idealização, seria completamente observável. |
| Número de agentes | Agente único | A parede é um objeto: não tem objetivos nem adapta seu comportamento. |
| Previsibilidade | Estocástico (em grau pequeno) | A parede é previsível, mas a execução motora tem ruído e a bola tem pequenas irregularidades. Numa idealização, seria determinístico. |
| Dependência entre decisões | Sequencial | Cada golpe define a próxima chegada da bola. |
| Mudança durante a decisão | Dinâmico | A bola continua em movimento enquanto o jogador decide. |
| Natureza dos elementos | Combinação | Física e posição são contínuas; a contagem de rebatidas é discreta. |
| Conhecimento do funcionamento | Conhecido | A física e o comportamento da parede são bem conhecidos. |

### Caso 6: Realizar um salto de altura

**Agente analisado:** o atleta saltador. **Objetivo e hipóteses:** uma tentativa de salto, técnica Fosbury, pista, sarrafo e colchão padrão; os rivais não interferem na tentativa.

- **P:** transpor o sarrafo na altura-alvo (ou maximizar a altura transposta), sem derrubá-lo e sem se lesionar.
- **E:** pista de aproximação, sarrafo e suportes, colchão, vento, juízes.
- **A:** pernas (corrida e impulsão), braços, tronco e cabeça (rotação e arqueamento do corpo).
- **S:** visão (na corrida), sistema vestibular e propriocepção (orientação no ar), tato.

| Propriedade | Classificação | Justificativa |
|---|---|---|
| Observabilidade | Parcialmente observável | No arco, de costas para o sarrafo, o atleta não o vê e depende da propriocepção. |
| Número de agentes | Agente único | Em cada tentativa os rivais não interagem com o atleta. Na competição completa, só a escolha das alturas envolve estratégia entre competidores. |
| Previsibilidade | Estocástico | Erros de passada e de impulsão, vento e variações de execução. |
| Dependência entre decisões | Episódico (por tentativa) | Cada tentativa pode ser avaliada separadamente. Fadiga e número limitado de tentativas tornariam a competição completa sequencial. |
| Mudança durante a decisão | Dinâmico | O corpo está em movimento e as decisões ocorrem em frações de segundo. |
| Natureza dos elementos | Contínuos | Posição, ângulo, velocidade e tempo são contínuos. |
| Conhecimento do funcionamento | Conhecido | A mecânica do salto e as regras são conhecidas, embora a execução exija prática. |

### Caso 7: Participar de um leilão de um item, realizando lances

**Agente analisado:** licitante (software ou humano). **Objetivo e hipóteses:** leilão inglês ascendente de um único item, com relógio e prazo; o agente tem um valor máximo privado para o item.

- **P:** excedente = valor atribuído ao item menos preço pago, se ganhar; 0 se não ganhar; nunca exceder o orçamento.
- **E:** leiloeiro, item, lances atuais, outros licitantes, regras do leilão, relógio.
- **A:** dar um lance (com valor), definir um lance máximo automático, desistir.
- **S (digitais):** lance atual, histórico de lances, tempo restante, número de participantes, descrição do item.

| Propriedade | Classificação | Justificativa |
|---|---|---|
| Observabilidade | Parcialmente observável | Os lances são públicos, mas os valores máximos e as estratégias dos outros licitantes não são. |
| Número de agentes | Multiagente competitivo | Os licitantes disputam o mesmo item (conluio seria cooperação, mas é ilegal). |
| Previsibilidade | Estocástico | A resposta dos outros a um lance é imprevisível, o que gera incerteza estratégica. |
| Dependência entre decisões | Sequencial | Cada lance influencia o preço, as reações dos outros e as opções futuras. |
| Mudança durante a decisão | Dinâmico | Outros lances e o relógio continuam enquanto o agente decide. |
| Natureza dos elementos | Discretos (predominantemente) | Lances em incrementos fixos e decisões em rodadas. O tempo real é contínuo, mas modelável como discreto. |
| Conhecimento do funcionamento | Conhecido (regras); valores alheios desconhecidos | As regras do leilão são conhecidas; a incerteza está nas preferências dos adversários. |

### Comparação final

**Tênis contra uma pessoa e contra a parede.** Contra uma pessoa, o ambiente é multiagente competitivo: o adversário tem objetivos próprios, adapta a estratégia e esconde intenções, o que torna o ambiente mais estocástico e parcialmente observável, e a medida de desempenho passa a ser a vitória. Contra a parede, o ambiente é de agente único e quase determinístico: a parede reage sempre do mesmo modo, o foco é a técnica e a consistência, e o desempenho é medido por rebatidas consecutivas e precisão. O treino é mais fácil de modelar, mas não desenvolve a leitura do adversário.

**Informações incompletas na compra de livros usados.** O agente só vê o anúncio. Não vê o estado real do livro (marcas de uso, páginas faltando), não sabe se o vendedor é honesto, se o item ainda existe ou se o envio será pontual. Fotos e avaliações são sinais parciais e podem ser manipulados. Por isso as decisões ocorrem sob incerteza e exigem estimar probabilidades e riscos (por exemplo, pagar mais por um vendedor bem avaliado).

**Influência de outros agentes no futebol e no leilão.** No futebol, colegas e adversários determinam o estado a cada instante: os colegas permitem cooperação (passes, marcação em conjunto) e os adversários exigem antecipação e engano. O resultado de uma ação depende da reação dos outros, o que torna o ambiente estocástico para o agente. No leilão, o preço final e o vencedor dependem dos lances alheios; o agente precisa inferir o valor máximo dos rivais pelos lances observados e decidir quando parar. Nos dois casos a racionalidade exige considerar o comportamento estratégico dos outros, o que o agente único (parede, salto, Titã) não precisa fazer.

---

## Item 2.5: Definições de termos

**Agente**

- **Definição:** Entidade que percebe o ambiente por sensores e age sobre ele por atuadores, escolhendo ações para cumprir algum objetivo. Pode ser humano, robô ou software.
- **Exemplo:** Termostato inteligente que lê a temperatura da sala (sensor) e liga ou desliga o aquecedor (atuador).

**Função de agente**

- **Definição:** Descrição abstrata do comportamento do agente: um mapeamento de cada sequência possível de percepções para uma ação. Diz *o que* o agente faz, não *como*.
- **Exemplo:** No mundo do aspirador de dois quadrados, a tabela que associa a sequência [A, Sujo] à ação Aspirar e [A, Limpo] à ação Direita.

**Programa de agente**

- **Definição:** Implementação concreta da função de agente, executada numa arquitetura física. Recebe a percepção atual e devolve uma ação, usando memória e recursos finitos.
- **Exemplo:** Código em Python que recebe (posição, estado) e retorna "Aspirar" se o quadrado estiver sujo; caso contrário, move o aspirador.

**Racionalidade**

- **Definição:** Propriedade do agente que, para cada sequência de percepções, escolhe a ação que maximiza o valor esperado da medida de desempenho, dados seu conhecimento prévio e suas percepções. Não exige onisciência nem garante sucesso em cada tentativa.
- **Exemplo:** Quem leva guarda-chuva porque há 70% de chance de chuva é racional, mesmo que o dia termine seco.

**Autonomia**

- **Definição:** Grau em que o agente decide com base na própria experiência e percepções, e não apenas no conhecimento embutido pelo projetista. Um agente autônomo aprende a compensar conhecimento prévio parcial ou incorreto.
- **Exemplo:** Aspirador robô que aprende, com o uso, quais cômodos e horários acumulam mais sujeira e ajusta sua rotina sem novas instruções.

**Agente reativo**

- **Definição:** Agente que escolhe a ação usando apenas a percepção atual, por regras condição-ação, sem memória do passado nem previsão do futuro. Funciona bem quando o ambiente é completamente observável.
- **Exemplo:** Freio automático que aciona quando o sensor detecta obstáculo a menos de 2 metros.

**Agente baseado em modelo**

- **Definição:** Agente que mantém um estado interno, atualizado pelas percepções e por um modelo de como o mundo evolui e de como suas ações o afetam. Isso permite lidar com observabilidade parcial.
- **Exemplo:** Carro autônomo que continua "sabendo" que há um veículo no ponto cego, mesmo sem vê-lo no momento, porque rastreou sua posição e velocidade anteriores.

**Agente baseado em objetivos**

- **Definição:** Agente que, além do modelo do mundo, tem um objetivo explícito e escolhe ações por aproximarem-no dele, usando busca ou planejamento. Mudar o objetivo muda o comportamento sem reescrever regras.
- **Exemplo:** Aplicativo de navegação que planeja a rota até o destino e a recalcula se uma via for bloqueada.

**Agente baseado em utilidade**

- **Definição:** Agente que avalia os estados com uma função de utilidade (grau de "satisfação") e escolhe a ação de maior utilidade esperada. Permite comparar objetivos conflitantes e tratar incerteza e trocas entre benefícios.
- **Exemplo:** Aplicativo de rotas que pondera tempo, pedágio, segurança e consumo para recomendar o trajeto de melhor equilíbrio, e não apenas o que chega ao destino.

**Agente com aprendizagem**

- **Definição:** Agente que melhora o desempenho com a experiência. O elemento de aprendizagem usa o retorno de um crítico para modificar o elemento de desempenho, e um gerador de problemas sugere novas experiências.
- **Exemplo:** Programa de xadrez que joga muitas partidas, analisa as derrotas e ajusta a avaliação de posições para jogar melhor nas seguintes.

---

## Síntese com autômatos

**Cenário escolhido:** Caso 3 do item 2.4, comprar livros usados de IA na Internet. **Hipóteses:** o agente busca um único livro de IA com orçamento fixo, avalia um anúncio por vez e só compra de vendedores com reputação aceitável. O autômato é uma simplificação do problema real.

**Estados internos (Q)**

| Estado | Significado |
|---|---|
| q0, Buscando | Ainda não há oferta aceitável registrada. |
| q1, Oferta candidata | Existe um anúncio que cumpre preço e reputação, guardado como melhor opção até agora. |
| q2, Comprado | O pedido foi feito; a tarefa terminou (estado final). |

**Percepções (Σ)**

| Percepção | Significado |
|---|---|
| p1, Anúncio bom | Preço dentro do orçamento e vendedor bem avaliado. |
| p2, Anúncio ruim | Preço acima do orçamento, vendedor mal avaliado ou condição inaceitável. |
| p3, Anúncio indisponível | A oferta guardada sumiu ou foi vendida a outro comprador. |

**Ações (A)**

| Ação | Significado |
|---|---|
| a1, Continuar busca | Consultar o próximo anúncio. |
| a2, Reservar oferta | Adicionar ao carrinho e registrar a oferta como candidata. |
| a3, Efetuar compra | Pagar e confirmar o pedido. |

**Regras de transição** (formato: estado atual + percepção → próximo estado, ação)

1. **q0 + p1 → q1, a2.** Na busca, ao perceber um anúncio bom, o agente passa a "oferta candidata" e reserva o item.
2. **q1 + p1 → q2, a3.** Com uma oferta candidata, ao perceber que ela continua boa (reconfirmação), o agente passa a "comprado" e paga.

Regras complementares, para completar o autômato: q0 + p2 → q0, a1 (anúncio ruim, continua buscando); q1 + p3 → q0, a1 (a oferta guardada sumiu, volta à busca).

**Como o estado interno representa informação passada.** O agente não guarda a lista de anúncios vistos; o estado resume o que importa do histórico. q0 codifica "até agora nada serviu", q1 codifica "já vi uma oferta boa e a reservei" e q2 codifica "a compra foi feita". Assim, a mesma percepção p1 produz ações diferentes conforme o passado: em q0 leva a reservar (a2) e em q1, a comprar (a3). Um agente reativo puro responderia sempre igual a p1 e não distinguiria a primeira oferta boa da oferta já reservada. O estado funciona como memória compacta, útil em ambiente parcialmente observável.

**Limitação da simplificação.** Com três estados, o autômato não registra quantidade, preço ou qualidade das ofertas já vistas: guarda uma só oferta candidata, não compara várias nem lembra que um vendedor foi ruim. Trata "bom" e "ruim" como categorias binárias, ignorando frete, graus de preço e risco de fraude, que exigiriam função de utilidade e mais estados. Além disso, é determinista e não representa a incerteza sobre a honestidade do vendedor nem a mudança de preço durante a decisão.

---

## Conferência final

| Verificação | Situação |
|---|---|
| Nove afirmações do item 2.3 (a a i), cada uma com V/F, justificativa e exemplo ou contraexemplo | Atendido |
| Quantificadores (todo, nenhum, existe, nunca) analisados e hipóteses explícitas | Atendido |
| Sete casos do item 2.4 com agente, hipóteses, PEAS e sete propriedades justificadas | Atendido |
| Comparação final (tênis, livros usados, futebol e leilão) | Atendido |
| Dez termos do item 2.5 com definição de 2 a 4 linhas e exemplo coerente | Atendido |
| Síntese: 3 estados, 3 percepções, 3 ações, 2 regras, explicação do estado e limitação | Atendido |
| Identificação (nome, matrícula, turma, professora) | **Pendente: preencher no topo** |
| Formato e prazo de envio | **Pendente: seguir a orientação da professora** |
| Referências conferidas com o material da aula | **Pendente: confirmar edição e páginas** |

---

## Referências

RUSSELL, Stuart; NORVIG, Peter. **Inteligência Artificial: uma abordagem moderna**. Capítulo 2: Agentes inteligentes. [confirmar edição, editora e ano da versão usada na disciplina]

Material-base da disciplina: Capítulo 2, Agentes inteligentes, disponibilizado para a aula de 20/10/2026.

Nota: as definições e exemplos foram redigidos com palavras próprias a partir dos conceitos do Capítulo 2 (agente, função e programa de agente, racionalidade, ambientes de tarefa e PEAS, tipos de agente). Se outras fontes forem consultadas, registrá-las aqui.
