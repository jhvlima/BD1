# Álgebra Relacional

## Conceitos Básicos

A **Álgebra Relacional** é uma linguagem de consulta procedural fundamental para Bancos de Dados Relacionais. Ela define operações sobre relações (tabelas) que produzem novas relações como resultado.

### Características Principais

- **Operações fechadas**: O resultado de uma operação é sempre uma relação
- **Composição**: Operações podem ser combinadas (aninhadas)
- **Base teórica**: Forma a base para linguagens como SQL
- **Formal e matemática**: Usa notação baseada em teoria de conjuntos

---

## Operações Fundamentais

### 1. **Selection (Seleção) - σ**

**Definição**: Seleciona linhas (tuplas) de uma relação que satisfazem uma condição específica.

**Notação**: `σ<condição>(R)`

**Exemplo**:

```
σ salario > 3000 (Funcionarios)
```

Retorna todos os funcionários com salário maior que 3000.

**Características**:

- Operação unária (trabalha com uma relação)
- Reduz o número de linhas
- Preserva todas as colunas
- Condições podem usar operadores: =, <, >, ≤, ≥, ≠, AND, OR, NOT

---

### 2. **Projection (Projeção) - π**

**Definição**: Seleciona colunas (atributos) específicas de uma relação, eliminando colunas não desejadas e duplicatas.

**Notação**: `π<atributos>(R)`

**Exemplo**:

```
π nome, salario (Funcionarios)
```

Retorna apenas as colunas nome e salário.

**Características**:

- Operação unária
- Reduz o número de colunas
- Remove duplicatas automaticamente
- Preserva todas as linhas originais (após remoção de duplicatas)

---

### 3. **Cartesian Product (Produto Cartesiano) - ×**

**Definição**: Combina cada tupla de uma relação com cada tupla de outra relação, criando todas as combinações possíveis.

**Notação**: `R × S` ou `R ⊗ S`

**Exemplo**:

```
Departamentos × Funcionarios
```

Se Departamentos tem 5 tuplas e Funcionarios tem 10 tuplas, o resultado terá 50 tuplas.

**Características**:

- Operação binária (trabalha com duas relações)
- Número de tuplas = |R| × |S|
- Número de colunas = colunas(R) + colunas(S)
- Raramente usado isoladamente (muito dados)
- Base para operações de jointura

---

### 4. **Join (Junção) - ⊲⊳**

**Definição**: Combina tuplas de duas relações baseado em uma condição de junção, retornando apenas as combinações que satisfazem a condição.

**Notação**: `R ⊲⊳<condição> S` ou `R ⊲⊳ S on condição`

**Exemplo**:

```
Funcionarios ⊲⊳ depto_id = id (Departamentos)
```

**Tipos de Join**:

#### **Inner Join (θ-join)**

- Retorna apenas tuplas que satisfazem a condição em ambas as relações
- Exemplo: `R ⊲⊳ R.id = S.depto_id S`

#### **Left Outer Join**

- Inclui todas as tuplas da relação esquerda
- Preenche com NULL as colunas da direita quando não há correspondência

#### **Right Outer Join**

- Inclui todas as tuplas da relação direita
- Preenche com NULL as colunas da esquerda quando não há correspondência

#### **Full Outer Join**

- Inclui todas as tuplas de ambas as relações
- Preenche com NULL quando não há correspondência

#### **Natural Join (⊲⊳)**

- Join automático nos atributos com nomes iguais
- Remove os atributos duplicados
- Exemplo: `Funcionarios ⊲⊳ Departamentos`

---

### 5. **Union (União) - ∪**

**Definição**: Combina tuplas de duas relações, retornando todas as tuplas que aparecem em qualquer uma das relações, sem duplicatas.

**Notação**: `R ∪ S`

**Exemplo**:

```
π nome (Funcionarios) ∪ π nome (Contratados)
```

Retorna todos os nomes de funcionários e contratados.

**Características**:

- Operação binária
- Ambas as relações devem ter o mesmo esquema (compatibilidade)
- Remove duplicatas automaticamente
- Número de tuplas ≤ |R| + |S|

---

### 6. **Set Difference (Diferença de Conjuntos) - −**

**Definição**: Retorna tuplas que aparecem na primeira relação mas não aparecem na segunda.

**Notação**: `R − S`

**Exemplo**:

```
π id (Funcionarios) − π id (Demitidos)
```

Retorna IDs de funcionários ativos (não demitidos).

**Características**:

- Operação binária
- Relações devem ter esquemas compatíveis
- NÃO é comutativa: R − S ≠ S − R
- Resultado contém apenas tuplas exclusivas de R

---

### 7. **Rename (Renomeação) - ρ**

**Definição**: Renomeia uma relação ou seus atributos para clareza ou para permitir operações recursivas.

**Notação**: `ρ<novo_nome>(R)` ou `ρ<novo_nome>(antigo_atributo1 → novo_atributo1, ...)(R)`

**Exemplo 1 - Renomear relação**:

```
ρ Gerentes (σ cargo = 'Gerente' (Funcionarios))
```

**Exemplo 2 - Renomear atributos**:

```
ρ (nome → nome_gerente, id → gerente_id)(Funcionarios)
```

**Características**:

- Operação unária
- Não altera os dados, apenas os nomes
- Útil para self-joins (junção de uma relação com ela mesma)
- Permite clareza em consultas complexas

---

### 8. **Assignment (Atribuição) - ←**

**Definição**: Atribui o resultado de uma operação de álgebra relacional a uma relação temporária ou permanente.

**Notação**: `variável ← expressão`

**Exemplo**:

```
Temp ← σ salario > 5000 (Funcionarios)
Gerentes ← π nome, salario (Temp ⊲⊳ depto_id = id (Departamentos))
```

**Características**:

- Não é tecnicamente uma operação relacional
- Permite quebrar consultas complexas em passos
- Facilita legibilidade
- Cria variáveis que podem ser reutilizadas

---

## Expressões Complexas

### Combinando Operações

**Exemplo 1**: Encontrar nomes de funcionários que trabalham no departamento "TI" com salário > 3000:

```
π nome (σ salario > 3000 ∧ depto_nome = 'TI' (Funcionarios ⊲⊳ depto_id = id (Departamentos)))
```

**Exemplo 2**: Funcionários que nunca faltaram:

```
π nome (Funcionarios) − π nome_funcionario (π nome_funcionario (Faltas))
```

---

## Propriedades das Operações

| Operação | Comutativa | Associativa | Idempotente |
|----------|-----------|-----------|-----------|
| Union (∪) | Sim | Sim | Sim |
| Intersection (∩) | Sim | Sim | Sim |
| Difference (−) | Não | Não | Não |
| Selection (σ) | Sim | Sim | Sim |
| Projection (π) | Não | Sim | Não |
| Cartesian Product (×) | Sim | Sim | Não |

---

## Resumo das Operações

| Operação | Tipo | Entrada | Saída | Uso |
|----------|------|---------|-------|-----|
| Selection | Unária | 1 relação | Subset de linhas | Filtrar linhas |
| Projection | Unária | 1 relação | Subset de colunas | Selecionar colunas |
| Cartesian Product | Binária | 2 relações | Todas as combinações | Base para joins |
| Join | Binária | 2 relações | Combinações com condição | Relacionar tabelas |
| Union | Binária | 2 relações | Tuplas únicas de ambas | Combinar resultados |
| Difference | Binária | 2 relações | Tuplas em R mas não em S | Excluir dados |
| Rename | Unária | 1 relação | Mesmos dados, nomes novos | Claridade e self-joins |
| Assignment | Unária | Qualquer expressão | Variável | Modularizar consultas |

---

## Equivalências Importantes

- **Intersection em termos de Difference**: R ∩ S = R − (R − S)
- **Join e Cartesian Product**: R ⊲⊳ R.id = S.id S = σ R.id = S.id (R × S)
- **Comutatividade de Selection**: σ p1 (σ p2 (R)) = σ p2 (σ p1 (R))
- **Cascata de Projection**: π a (π a,b (R)) = π a (R)

---

## Observações Finais

A **Álgebra Relacional** é fundamental para entender:

- Como SQL é traduzido internamente
- Otimização de consultas
- Planejamento e execução de queries
- Validação de expressões equivalentes

Dominar essas operações permite escrever consultas mais eficientes e entender melhor o comportamento de bancos de dados relacionais.
