# Exercícios de Regex

---

## Exercício 1 — Três linguagens sobre alfabetos pequenos

### 1) Sobre {0,1}: palavras que terminam em 00

**Linguagem:** todas as cadeias binárias cujos dois últimos símbolos são `0` e `0` (a cadeia toda pode ter qualquer coisa antes).

**Regex:**
```
(0|1)*00
```

**Justificativa:**
- `(0|1)*` — qualquer prefixo, incluindo vazio
- `00` — obrigatoriamente termina com dois zeros

**Exemplos aceitos:** `00`, `100`, `1100`, `0000`

**Exemplos rejeitados:** `0`, `10`, `101`, `001` (termina em `01`), `110` (termina em `10`)

---

### 2) Sobre {a,b}: palavras com exatamente dois "a"

**Linguagem:** cadeias com qualquer quantidade de `b`, mas exatamente dois `a` no total.

**Regex:**
```
b*ab*ab*
```

**Justificativa:**
- `b*` antes, entre e depois dos dois `a`'s obrigatórios
- Cada `a` aparece exatamente uma vez de forma literal → total de 2 "a"s garantido

**Exemplos aceitos:** `aa`, `aba`, `baab`, `bbabba`, `abab`

**Exemplos rejeitados:** `a` (só um a), `aaa` (três a's), `b` (nenhum a), `aaab` (três a's)

---

### 3) Identificador: duas maiúsculas + três algarismos + uma minúscula opcional

**Regex:**
```
[A-Z]{2}[0-9]{3}[a-z]?
```

**Justificativa:**
- `[A-Z]{2}` — exatamente duas letras maiúsculas
- `[0-9]{3}` — exatamente três algarismos
- `[a-z]?` — zero ou uma letra minúscula no final

**Exemplos aceitos:** `AB123`, `XY999a`, `ZZ000z`

**Exemplos rejeitados:** `A123` (só uma maiúscula), `AB12` (só dois algarismos), `AB123ab` (duas minúsculas), `ab123` (minúsculas no lugar errado)

---

## Desafio: Matrícula Acadêmica

**Formato:** `CURSO-ANO-NÚMERO-TURNO`

| Regra | Valor |
|---|---|
| CURSO | CCO, ESW ou SIS |
| ANO | 2024 a 2029 |
| NÚMERO | exatamente quatro algarismos |
| TURNO | M, T ou N |
| Separador | hífen entre os blocos, sem caracteres extras |

### Regex completa

```
^(CCO|ESW|SIS)-202[4-9]-\d{4}-[MTN]$
```

### Justificativa por bloco

| Bloco | Regra | Trecho da regex | Por quê |
|---|---|---|---|
| Âncoras | Não permitir caracteres extras antes/depois | `^` ... `$` | Sem âncoras, a regex aceitaria `XXCCO-2024-0001-M` ou `CCO-2024-0001-MYY`, pois faria match parcial |
| CURSO | Apenas CCO, ESW ou SIS | `(CCO\|ESW\|SIS)` | Escolha fechada — não pode ser qualquer sigla de 3 letras, senão aceitaria "ABC" |
| Hífen | Separador obrigatório | `-` (literal) | Faz parte do formato exigido |
| ANO | 2024 a 2029 | `202[4-9]` | Os três primeiros dígitos são fixos (`202`), o último varia só entre 4 e 9 — evita aceitar 2030, 2020 etc. |
| Hífen | Separador | `-` | — |
| NÚMERO | Exatamente 4 algarismos | `\d{4}` | Quantificador exato evita 3 ou 5 dígitos |
| Hífen | Separador | `-` | — |
| TURNO | M, T ou N | `[MTN]` | Classe fechada de caracteres, exatamente 1 caractere |

### Tabela de testes

| Entrada | Esperado | Resultado | Motivo |
|---|---|---|---|
| `CCO-2024-0001-M` | ✅ Aceita | ✅ | Todos os blocos válidos |
| `ESW-2029-9999-N` | ✅ Aceita | ✅ | Limite superior do ano, válido |
| `SIS-2024-0000-T` | ✅ Aceita | ✅ | Número com zeros à esquerda é válido (são 4 algarismos) |
| `ABC-2024-0001-M` | ❌ Rejeita | ❌ | Curso "ABC" não está na lista fechada |
| `CCO-2030-0001-M` | ❌ Rejeita | ❌ | Ano fora do intervalo (2030 > 2029) |
| `CCO-2023-0001-M` | ❌ Rejeita | ❌ | Ano fora do intervalo (2023 < 2024) |
| `CCO-2024-001-M` | ❌ Rejeita | ❌ | Número com apenas 3 algarismos |
| `CCO-2024-00011-M` | ❌ Rejeita | ❌ | Número com 5 algarismos |
| `CCO-2024-0001-X` | ❌ Rejeita | ❌ | Turno "X" não é M, T ou N |
| `CCO_2024_0001_M` | ❌ Rejeita | ❌ | Usa underline em vez de hífen |
| `CCO-2024-0001-m` | ❌ Rejeita | ❌ | Minúscula não prevista em `[MTN]` |
| ` CCO-2024-0001-M` (espaço antes) | ❌ Rejeita | ❌ | Âncora `^` barra caractere extra no início |
| `CCO-2024-0001-M ` (espaço depois) | ❌ Rejeita | ❌ | Âncora `$` barra caractere extra no final |
| `CCOESW-2024-0001-M` | ❌ Rejeita | ❌ | Tentativa de burlar a alternância concatenando dois cursos — falha porque nenhuma opção do grupo bate com "CCOESW" |

### Casos de fronteira interessantes

- **`CCO-2024-0001-MT`** → rejeitada, pois `[MTN]` aceita só 1 caractere e a âncora `$` não permite sobra.
- **`CCO-02024-0001-M`** → rejeitada, o "0" extra antes de "2024" quebra o padrão `202[4-9]`.
- **`sis-2024-0001-m`** (tudo minúsculo) → rejeitada, pois a regex é case-sensitive por padrão; se quisesse aceitar minúsculas, seria preciso adicionar a flag `i` ou incluir `[a-z]` nas classes — o enunciado não pede isso, então mantemos case-sensitive.
