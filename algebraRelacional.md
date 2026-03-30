# Álgebra Relacional

## Conceitos Básicos

A **Álgebra Relacional** é uma linguagem de consulta procedural fundamental para Bancos de Dados Relacionais. Ela define operações sobre relações (tabelas) que produzem novas relações como resultado.

### Características Principais

- **Operações fechadas**: O resultado de uma operação é sempre uma relação
- **Composição**: Operações podem ser combinadas (aninhadas)
- **Base teórica**: Forma a base para linguagens como SQL
- **Formal e matemática**: Usa notação baseada em teoria de conjuntos

## Propriedades das Operações

Adicionamos a operação de Junção (Join) à tabela para completar o quadro de propriedades matemáticas.

| Operação | Comutativa | Associativa | Idempotente |
|----------|-----------|-----------|-----------|
| **Union (∪)** | Sim | Sim | Sim |
| **Intersection (∩)** | Sim | Sim | Sim |
| **Difference (−)** | Não | Não | Não |
| **Selection (σ)** | Sim | Sim | Sim |
| **Projection (π)** | Não | Sim | Não |
| **Cartesian Product (×)** | Sim | Sim | Não |
| **Natural/Inner Join (⊲⊳)**| Sim | Sim | Não |

*Nota: Outer Joins (Left, Right, Full) e Division (÷) não são comutativos nem associativos.*

## Resumo das Operações

A tabela foi expandida para incluir as operações de Interseção, Divisão, Projeção Generalizada e Funções de Agregação, garantindo que todas as ferramentas da Álgebra Relacional estejam mapeadas.

| Operação | Tipo | Entrada | Saída | Uso Principal |
|----------|------|---------|-------|---------------|
| **Selection (σ)** | Unária | 1 relação | Subset de linhas | Filtrar registros baseados em condições |
| **Projection (π)** | Unária | 1 relação | Subset de colunas | Selecionar colunas específicas e remover duplicatas |
| **Cartesian Product (×)** | Binária | 2 relações | Todas as combinações | Base teórica para a criação de joins |
| **Join (⊲⊳)** | Binária | 2 relações | Combinações com condição | Relacionar dados de tabelas diferentes |
| **Union (∪)** | Binária | 2 relações | Tuplas únicas de ambas | Combinar resultados de esquemas compatíveis |
| **Intersection (∩)** | Binária | 2 relações | Tuplas presentes em ambas | Encontrar dados em comum entre conjuntos |
| **Difference (−)** | Binária | 2 relações | Tuplas em R mas não em S | Excluir dados ou encontrar exceções |
| **Division (÷)** | Binária | 2 relações | Tuplas de R ligadas a todas de S| Consultas que envolvem "todos" ou "cada" |
| **Rename (ρ)** | Unária | 1 relação | Mesmos dados, nomes novos | Claridade, organização e self-joins |
| **Assignment (←)**| Unária | Qualquer expressão| Variável temporária | Modularizar e simplificar consultas complexas |
| **Gen. Projection** | Unária | 1 relação | Novas colunas computadas | Realizar cálculos aritméticos por linha |
| **Agregação (ℑ)** | Unária | 1 relação | Valores resumidos/agrupados| Obter estatísticas (Count, Sum, Avg, Max, Min) |

## Equivalências Importantes (Regras de Otimização)

As equivalências abaixo são a base do processo de **Otimização de Consultas (Query Optimization)** feito pelos SGBDs. Elas permitem que o banco de dados reescreva a consulta para executá-la da forma mais rápida possível, processando menos dados.

### Equivalências Básicas e de Definição

- **Intersection em termos de Difference**: `R ∩ S = R − (R − S)`
- **Join e Cartesian Product**: `R ⊲⊳ R.id = S.id S = σ R.id = S.id (R × S)`
- **Division em termos de outras operações**: `R ÷ S = π atributos_R (R) − π atributos_R ( (π atributos_R (R) × S) − R )`

### Equivalências de Seleção (σ)

- **Comutatividade de Selection**: `σ p1 (σ p2 (R)) = σ p2 (σ p1 (R))`
- **Cascata de Selection (Desmembramento)**: `σ p1 AND p2 (R) = σ p1 (σ p2 (R))`
- **Distribuição de Selection sobre Union**: `σ p (R ∪ S) = σ p (R) ∪ σ p (S)`
- **Distribuição de Selection sobre Join**: `σ p (R ⊲⊳ S) = σ p (R) ⊲⊳ S` *(Válido apenas se a condição 'p' envolver apenas atributos da tabela R. Isso reduz o tamanho da tabela antes de fazer o join, economizando processamento).*

### Equivalências de Projeção (π)

- **Cascata de Projection**: `π a (π a,b (R)) = π a (R)` *(Apenas a projeção mais externa importa).*
- **Comutatividade de Projection e Selection**: `π a (σ p (R)) = σ p (π a (R))` *(Válido apenas se a condição de seleção 'p' utilizar apenas os atributos que estão sendo projetados em 'a').*

## Operações Fundamentais

1. Selection
2. Projection
3. Union
4. Set Difference
5. Cartesian Product
6. Rename

---

### 1. **Selection (Seleção) - σ (sigma)**

**Definição**: Seleciona linhas (tuplas) de uma relação que satisfazem uma condição específica.

**Notação**: `σ<condição>(R)`

**Sintaxe no Relax**: `σ condição (R)`

**Exemplo concreto**:

- Problema: Queremos encontrar todos os funcionários com salário maior que 3000.
- Tabelas:
  - `Funcionarios(id, nome, salario, depto_id)`

``` sql
σ salario > 3000 (Funcionarios)
```

Retorna todos os funcionários com salário maior que 3000.

**Características**:

- Operação unária (trabalha com uma relação)
- Reduz o número de linhas
- Preserva todas as colunas
- Condições podem usar operadores: =, <, >, ≤, ≥, ≠, AND, OR, NOT

---

### 2. **Projection (Projeção) - π (pi)**

**Definição**: Seleciona colunas (atributos) específicas de uma relação, eliminando colunas não desejadas e duplicatas.

**Notação**: `π<atributos>(R)`

**Sintaxe no Relax**: `π atributos (R)`

**Exemplo concreto**:

- Problema: Queremos gerar uma lista contendo apenas os nomes e os salários dos funcionários, ignorando seus IDs e departamentos.
- Tabelas:
  - `Funcionarios(id, nome, salario, depto_id)`

``` sql
π nome, salario (Funcionarios)
```

Retorna apenas as colunas nome e salário.

**Características**:

- Operação unária
- Reduz o número de colunas
- Remove duplicatas automaticamente
- Preserva todas as linhas originais (após remoção de duplicatas)

---

### 3. **Union (União) - ∪ (union)**

**Definição**: Combina tuplas de duas relações, retornando todas as tuplas que aparecem em qualquer uma das relações, sem duplicatas.

**Notação**: `R ∪ S`

**Sintaxe no Relax**: `R ∪ S`

**Exemplo concreto**:

- Problema: Queremos uma lista única com os nomes de todos os funcionários e de todos os prestadores de serviço terceirizados.
- Tabelas:
  - `Funcionarios(nome)`
  - `Contratados(nome)`

``` sql
π nome (Funcionarios) ∪ π nome (Contratados)
```

Retorna todos os nomes de funcionários e contratados.

**Características**:

- Operação binária
- Ambas as relações devem ter o mesmo esquema (compatibilidade)
- Remove duplicatas automaticamente
- Número de tuplas ≤ |R| + |S|

---

### 4. **Set Difference (Diferença de Conjuntos) - − (difference)**

**Definição**: Retorna tuplas que aparecem na primeira relação mas não aparecem na segunda.

**Notação**: `R − S`

**Sintaxe no Relax**: `R - S`

**Exemplo concreto**:

- Problema: Descobrir quais funcionários continuam ativos na empresa.
- Tabelas:
  - `Funcionarios(id)`
  - `Demitidos(id)`

``` sql
π id (Funcionarios) − π id (Demitidos)
```

Retorna IDs de funcionários ativos (não demitidos).

**Características**:

- Operação binária
- Relações devem ter esquemas compatíveis
- NÃO é comutativa: R − S ≠ S − R
- Resultado contém apenas tuplas exclusivas de R

---

### 5. **Cartesian Product (Produto Cartesiano) - × (cartesian product)**

**Definição**: Combina cada tupla de uma relação com cada tupla de outra relação, criando todas as combinações possíveis.

**Notação**: `R × S` ou `R ⊗ S`

**Sintaxe no Relax**: `R * S` ou `R x S`

**Exemplo concreto**:

- Problema: Queremos ver todas as combinações possíveis de atribuição entre departamentos e funcionários, independentemente de onde eles realmente trabalham.
- Tabelas:
  - `Departamentos(nome_depto)`
  - `Funcionarios(nome_func)`

``` sql
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

### 6. **Rename (Renomeação) - ρ (rho)**

**Definição**: Renomeia uma relação ou seus atributos para clareza ou para permitir operações recursivas.

**Notação**: `ρ<novo_nome>(R)` ou `ρ<novo_nome>(antigo_atributo1 → novo_atributo1, ...)(R)`

**Sintaxe no Relax**: `ρ novo_nome (R)` ou `ρ antigo_nome -> novo_nome (R)`

**Exemplo concreto**:

- Problema: Queremos isolar apenas os gerentes e chamar a coluna de identificação de "gerente_id".
- Tabelas:
  - `Funcionarios(id, nome, cargo)`

``` sql
ρ (nome → nome_gerente, id → gerente_id) (σ cargo = 'Gerente' (Funcionarios))
```

Retorna uma tabela com atributos renomeados focada apenas nos gerentes.

**Características**:

- Operação unária
- Não altera os dados, apenas os nomes
- Útil para self-joins (junção de uma relação com ela mesma)
- Permite clareza em consultas complexas

---

## Operações Derivadas

1. Joins (Junções)
2. Assignment (Atribuição)
3. Division (Divisão)
4. Generalized Projection (Projeção Generalizada)
5. Aggregate Functions (Funções de Agregação)

### 1. **Joins (Junções) - ⊲⊳ (join)**

Para tornar os objetivos dos diferentes Joins mais evidentes, usaremos o seguinte cenário para todos os exemplos abaixo:

- **Tabelas de Referência**:
  - `Funcionarios(id, nome, depto_id)`
    - `(1, 'Alice', 10)`
    - `(2, 'Bob', 20)`
    - `(3, 'Carlos', NULL)` *(Carlos é terceirizado e não tem departamento)*
  - `Departamentos(depto_id, nome_depto)`
    - `(10, 'TI')`
    - `(20, 'RH')`
    - `(30, 'Vendas')` *(Não há funcionários alocados em Vendas)*

#### 1.1 **Inner Join (θ-join) / Junção Interna**

**Definição**: Retorna apenas tuplas que satisfazem a condição em ambas as relações.
**Notação**: `R ⊲⊳<condição> S`
**Sintaxe no Relax**: `R ⊲⊳ condição S`
**Exemplo concreto**:

- Problema: Queremos apenas os funcionários que estão lotados em um departamento existente.

```sql
Funcionarios ⊲⊳ Funcionarios.depto_id = Departamentos.depto_id (Departamentos)
```

*Resultado*: Alice (TI) e Bob (RH). Carlos e o departamento de Vendas são ignorados.

#### 1.2 **Natural Join (Junção Natural)**

**Definição**: Join automático nos atributos com nomes iguais. Remove os atributos duplicados.
**Notação**: `R ⊲⊳ S`
**Sintaxe no Relax**: `R ⊲⊳ S`
**Exemplo concreto**:

- Problema: Combinar dados cruzando a coluna `depto_id` (que existe nas duas tabelas) sem escrevê-la duas vezes no resultado.

```sql
Funcionarios ⊲⊳ Departamentos
```

*Resultado*: Alice (TI) e Bob (RH), mas a coluna `depto_id` aparece uma única vez.

#### 1.3 **Left Outer Join (Junção Externa à Esquerda) - ⟕**

**Definição**: Inclui todas as tuplas da relação esquerda. Preenche com NULL as colunas da direita quando não há correspondência.
**Notação**: `R ⟕ S`
**Exemplo concreto**:

- Problema: Obter a lista de **todos os funcionários**, mostrando o nome do departamento deles quando houver.

```sql
Funcionarios ⟕ Departamentos
```

*Resultado*: Alice (TI), Bob (RH) e **Carlos (NULL)**.

#### 1.4 **Right Outer Join (Junção Externa à Direita) - ⟖**

**Definição**: Inclui todas as tuplas da relação direita. Preenche com NULL as colunas da esquerda quando não há correspondência.
**Notação**: `R ⟖ S`
**Exemplo concreto**:

- Problema: Obter a lista de **todos os departamentos**, mostrando quem trabalha neles quando houver funcionários.

```sql
Funcionarios ⟖ Departamentos
```

*Resultado*: Alice (TI), Bob (RH) e **NULL (Vendas)**.

#### 1.5 **Full Outer Join (Junção Externa Completa) - ⟗**

**Definição**: Inclui todas as tuplas de ambas as relações. Preenche com NULL quando não há correspondência.
**Notação**: `R ⟗ S`
**Exemplo concreto**:

- Problema: Relatório completo com absolutamente todos os dados, tanto funcionários sem setor quanto setores vazios.

```sql
Funcionarios ⟗ Departamentos
```

*Resultado*: Alice (TI), Bob (RH), **Carlos (NULL)** e **NULL (Vendas)**.

---

### 2. **Assignment (Atribuição) - ←**

**Definição**: Atribui o resultado de uma operação de álgebra relacional a uma relação temporária ou permanente.

**Notação**: `variável ← expressão`

**Sintaxe no Relax**: `var = expressao`

**Exemplo concreto**:

- Problema: Queremos isolar os gerentes em um passo e cruzar os dados deles com departamentos no passo seguinte, para não escrever tudo numa linha enorme.
- Tabelas:
  - `Funcionarios(id, nome, salario, depto_id)`
  - `Departamentos(id, nome)`

``` sql
Temp ← σ salario > 5000 (Funcionarios)
Gerentes ← π nome, salario (Temp ⊲⊳ depto_id = id (Departamentos))
```

**Características**:

- Não é tecnicamente uma operação relacional
- Permite quebrar consultas complexas em passos
- Facilita legibilidade
- Cria variáveis que podem ser reutilizadas

---

### 3. **Division (Divisão) - ÷**

**Definição**: Retorna as tuplas de uma relação (dividendo) que estão associadas a **todas** as tuplas de outra relação (divisor). Útil para responder perguntas que envolvem "todos" ou "cada".

**Notação**: `R ÷ S`

**Exemplo concreto**:

- Problema: Encontrar os IDs dos funcionários que trabalham em **todos** os projetos da empresa.
- Tabelas:
  - `Alocacoes(func_id, proj_id)` (Dividendo)
  - `Projetos(proj_id)` (Divisor)

```sql
Alocacoes ÷ Projetos
```

Retorna apenas os `func_id` que possuem ligação com absolutamente todos os `proj_id` listados na tabela de Projetos.

### 4. **Generalized Projection (Projeção Generalizada) - π**

**Definição**: Uma extensão da projeção que permite a criação de novas colunas através da aplicação de expressões aritméticas sobre atributos existentes.

**Notação**: `π<F1, F2...>(R)` (onde F são expressões matemáticas).

**Exemplo concreto**:

- Problema: Descobrir o ganho anual de cada funcionário.
- Tabelas:
  - `Funcionarios(nome, salario_mensal)`

```sql
π nome, (salario_mensal * 12) → salario_anual (Funcionarios)
```

### 5. **Aggregate Functions (Funções de Agregação) - ℑ (Gama)**

**Definição**: Aplica funções matemáticas (Count, Sum, Avg, Min, Max) a conjuntos de valores de uma relação para gerar valores resumidos, podendo agrupá-los por características em comum.

**Notação**: `<atributos_agrupamento> ℑ <função_agregação>(R)`

**Exemplo concreto**:

- Problema: Descobrir qual é o salário médio pago por cada departamento.
- Tabelas:
  - `Funcionarios(id, depto_id, salario)`

```sql
depto_id ℑ avg(salario) → media_salarial (Funcionarios)
```

Retorna uma lista agrupada por `depto_id` mostrando a `media_salarial` em cada um.

---

### Update

**Definição**: Permite modificar os dados existentes em uma relação, como atualizar valores, inserir novas tuplas ou deletar tuplas.
**Notação**: Varia dependendo da operação (UPDATE, INSERT, DELETE)
**Exemplo concreto**:

- Problema: Aumentar o salário de todos os funcionários do departamento de TI em 10%.
- Tabelas:
  - `Funcionarios(id, nome, salario, depto_id)`

```sql

```

## Valores Nulos e desconhecidos
