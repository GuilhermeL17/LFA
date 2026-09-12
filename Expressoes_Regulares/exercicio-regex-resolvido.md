# Resolução — Expressões Regulares

---

## Exercício 1 — Sufixo 00

**Alfabeto:** Σ = {0,1}

**Blocos:** parte livre (qualquer sequência de 0s e 1s) + parte fixa final "00"

**Escolhas/ordem/repetições:** o prefixo pode ser vazio ou ter qualquer combinação de 0 e 1, repetida livremente (por isso usamos `(0|1)*` ou `[01]*`); o sufixo é fixo e literal.

**Válidos:** `00`, `100`, `0100`, `111100`

**Inválidos:** `0`, `010`, `001` (termina em 1), `""` (vazio, não termina em 00)

**Expressão:**
```
(0|1)*00
```

---

## Exercício 2 — Exatamente dois "a"

**Alfabeto:** Σ = {a,b}

**Blocos:** três regiões de b's (antes do 1º a, entre os dois a's, depois do 2º a), com exatamente dois a's fixos.

**Escolhas/ordem/repetições:** cada região de b's é `b*` (zero ou mais), e os dois a's aparecem exatamente uma vez cada, na ordem fixa.

**Válidos:** `aa`, `baab`, `bbabab`, `aabbb`

**Inválidos:** `a` (só um a), `aaa` (três a's), `abaaa` (três a's), `bbb` (nenhum a)

**Expressão:**
```
b*ab*ab*
```

---

## Exercício 3 — Identificador acadêmico

**Blocos:** duas letras (maiúsculas) + três algarismos + letra minúscula opcional.

**Escolhas/ordem/repetições:** as duas letras e os três algarismos são fixos em quantidade; a letra final é opcional (0 ou 1 ocorrência).

**Válidos:** `AB123`, `XY999z`, `CD000a`

**Inválidos:** `A123` (só uma letra), `AB1234` (4 algarismos), `AB12` (2 algarismos), `AB123AB` (letra extra no final)

**Expressão:**
```
^[A-Z]{2}[0-9]{3}[a-z]?$
```

---

## Desafio final — Código de matrícula acadêmica

**Blocos:**
1. Curso: `CCO`, `ESW` ou `SIS` — escolha entre 3 alternativas literais
2. Hífen literal
3. Ano: `2024`–`2029`
4. Hífen literal
5. Número: exatamente 4 algarismos
6. Hífen literal
7. Turno: `M`, `T` ou `N` — escolha entre 3 alternativas literais
8. Âncoras de início/fim para impedir partes extras

### Expressão regular

```
^(CCO|ESW|SIS)-202[4-9]-[0-9]{4}-[MTN]$
```

### Justificativa por blocos

- `(CCO|ESW|SIS)`: alternância exata, sem permitir minúsculas nem outros cursos (rejeita `ADS` e `esw`)
- `-`: hífen literal, obrigatório e único separador (rejeita `/`)
- `202[4-9]`: fixa "202" e restringe o último dígito a 4–9, cobrindo exatamente 2024–2029 (rejeita 2030)
- `[0-9]{4}`: exatamente quatro dígitos, nem mais nem menos (rejeita `123`)
- `[MTN]`: exatamente um caractere entre os três turnos válidos (rejeita `X`)
- `^...$`: âncoras que impedem prefixos ou sufixos extras

### Dois novos casos inválidos

1. `CCO-2024-00001-M` — cinco algarismos no número (viola `{4}` exato)
2. `CCO-2024-0001-m` — turno em minúscula (viola `[MTN]`, case-sensitive)

### Perguntas para justificar

**Qual subexpressão representa a escolha entre cursos?**
`(CCO|ESW|SIS)` — grupo de alternância.

**Como o intervalo de anos foi limitado sem aceitar 2030?**
Ao fixar os três primeiros dígitos como `202` e restringir o último a `[4-9]`, cobrimos exatamente 2024 a 2029; 2030 exigiria mudar o dígito das dezenas (de 2 para 3), o que a expressão não permite.

**Por que `{4}` é diferente de `+` no bloco numérico?**
`{4}` exige exatamente quatro repetições; `+` aceita uma ou mais, sem limite superior, então aceitaria indevidamente `12345` ou até `1`.

**Qual é a função das âncoras?**
`^` e `$` garantem que toda a cadeia — do início ao fim — corresponda ao padrão, impedindo que caracteres extras antes ou depois sejam aceitos.

**Sua expressão aceita alguma cadeia que viola as regras?**
Não, dentro dos testes considerados: cada bloco é restrito exatamente às regras (curso fechado, ano fechado em 6 valores, número com 4 dígitos exatos, turno fechado em 3 valores, separadores fixos, âncoras totais). Os casos de teste fornecidos (aceitos e rejeitados) são consistentes com essa expressão.

---

## Desafio extra — DFA equivalente

Estrutura conceitual do DFA (descrição, já que o desenho não pode ser reproduzido em texto):

- **Estados por progresso de blocos:** um "trilho" linear de estados representando: leitura do curso (3 sub-trilhos, um por palavra CCO/ESW/SIS, pois são palavras fixas letra a letra) → estado após 1º hífen → 4 estados para os dígitos do ano (cada um validando o dígito esperado, com ramificação apenas no último dígito entre 4–9) → estado após 2º hífen → 4 estados para os dígitos do número (cada um aceitando qualquer `[0-9]`) → estado após 3º hífen → 1 estado ramificando em M/T/N → **estado de aceitação**.
- **Ramificações:** ocorrem (a) na primeira letra do curso (C→CCO, E→ESW, S→SIS), (b) no último dígito do ano (4 a 9 seguem, senão sumidouro), e (c) no caractere de turno (M, T ou N levam ao estado de aceitação; qualquer outro vai ao sumidouro).
- **Único estado de aceitação:** o estado alcançado após ler um turno válido (M, T ou N) na última posição.
- **Estado sumidouro:** qualquer símbolo fora do esperado em qualquer posição (letra errada, dígito fora de [4-9] no ano, separador diferente de "-", turno inválido, ou qualquer símbolo além do fim esperado) leva a um estado sumidouro do qual não há saída de aceitação — isso garante que cadeias com caracteres extras ou fora de ordem sejam sempre rejeitadas.

---

## Perguntas de reflexão 

**1. Toda expressão regular formal representa uma linguagem regular?**
Sim, por definição — expressões regulares e linguagens regulares são equivalentes (teorema de Kleene).

**2. Toda linguagem regular pode ser representada por uma expressão regular?**
Sim, na mesma direção do teorema: linguagem regular ⟺ reconhecida por DFA/NFA ⟺ descrita por ER.

**3. Qual é a relação entre um ER, um NFA e um DFA?**
São três formalismos equivalentes em poder expressivo: toda ER pode ser convertida em um NFA (construção de Thompson), todo NFA pode ser convertido em um DFA (construção de subconjuntos), e todo DFA pode ser convertido de volta em uma ER. Todos descrevem exatamente a classe das linguagens regulares.

**4. Qual é a diferença entre uma expressão teórica regular e as extensões de motores de programação?**
A ER formal (teoria da computação) tem apenas união, concatenação e estrela de Kleene, com poder exatamente igual a autômatos finitos. Motores de regex de linguagens de programação (PCRE, etc.) adicionam recursos como backreferences e lookahead/lookbehind, que podem tornar o "regex" prático mais poderoso que autômatos finitos — deixando de ser, tecnicamente, "regular" no sentido formal.

**5. Por que um autômato finito reconhece paridade, mas não consegue contar arbitrariamente e comparar duas quantidades sem limite?**
Paridade exige apenas lembrar um bit de estado (par/ímpar), o que cabe em um número finito de estados. Contar e comparar quantidades ilimitadas exigiria memória proporcional ao tamanho da entrada, que cresce sem limite — mas um DFA tem um número fixo e finito de estados, não pode armazenar contagens arbitrariamente grandes.

**6. Por que {aⁿbⁿ | n≥0} não é regular?**
Pelo lema do bombeamento (pumping lemma): qualquer DFA hipotético teria um número finito de estados k; ao processar aᵏbᵏ, por Pigeonhole dois prefixos de a's levariam ao mesmo estado, permitindo "bombear" (repetir) uma subcadeia de a's sem repetir a correspondente de b's, quebrando a igualdade n=n — contradição.

**7. O que muda ao passarmos de linguagens regulares para linguagens livres de contexto?**
Ganha-se uma pilha (memória ilimitada, mas de acesso restrito a LIFO) — os autômatos correspondentes (autômatos de pilha, PDA) podem contar e comparar quantidades de símbolos, algo impossível para DFAs, que só têm memória finita (o próprio conjunto de estados).
