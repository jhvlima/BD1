# Introdução a Banco de Dados

## O que é um Banco de Dados?

Um **Banco de Dados** é um conjunto organizado de dados correlatos armazenados de forma estruturada, permitindo acesso, atualização e gerenciamento eficiente.

**Características**:

- Dados organizados e persistentes
- Acesso controlado
- Redundância mínima
- Integridade e segurança
- Múltiplos usuários simultâneos

---

## Modelagem Entidade-Relacionamento (MER)

### Conceito

A **Modelagem Entidade-Relacionamento** é um modelo conceitual que descreve a estrutura de um banco de dados através de **entidades**, **atributos** e **relacionamentos**. É o passo intermediário entre a realidade do negócio e a implementação física no banco.

---

## Componentes Principais

### 1. **Entidades**

**Definição**: Um objeto ou conceito no mundo real que pode ser identificado de forma única.

**Características**:

- Representa uma coisa, pessoa, evento ou conceito
- Tem propriedades (atributos)
- Múltiplas instâncias podem existir
- Representadas em retângulos no diagrama ER

**Exemplos**:

- Aluno, Professor, Disciplina, Departamento
- Cliente, Projeto, Produto
- Filme, Ator, Diretor

---

### 2. **Atributos**

**Definição**: Propriedades ou características que descrevem uma entidade.

**Tipos de Atributos**:

#### **Atributo Simples**

- Não pode ser subdivido
- Exemplo: idade, salário, CPF

#### **Atributo Composto**

- Pode ser subdividido em componentes menores
- Exemplo: Endereço (rua, número, cidade, CEP)

#### **Atributo Univalorado**

- Possui um único valor
- Exemplo: data de nascimento

#### **Atributo Multivalorado**

- Pode ter múltiplos valores
- Exemplo: telefones de um contato (residencial, comercial, celular)
- Representado com linha dupla

#### **Atributo Derivado**

- Seu valor pode ser calculado a partir de outros atributos
- Exemplo: idade (calculada a partir de data de nascimento)
- Representado com linha tracejada

---

### 3. **Chaves (Keys)**

**Definição**: Um atributo ou conjunto de atributos que identifica de forma única uma entidade.

#### **Chave Primária (Primary Key - PK)**

- Atributo ou conjunto de atributos que identifica UNICAMENTE cada tupla
- Não pode ser nula (NOT NULL)
- Deve ser única
- Apenas uma por entidade
- Exemplo: `CPF` em Aluno

#### **Chave Super**

- Qualquer atributo ou conjunto que identifica unicamente uma entidade
- Inclui a chave primária e outras combinações
- Exemplo: {CPF}, {Email}, {CPF, Nome}

#### **Chave Candidata**

- Atributo ou conjunto que PODE ser chave primária
- Identifica unicamente as tuplas
- Pode haver várias por entidade
- A mais apropriada é escolhida como primária
- Exemplo: CPF, Email (ambos únicos em Pessoa)

#### **Chave Estrangeira (Foreign Key - FK)**

- Atributo que faz referência à chave primária de outra entidade
- Cria relacionamento entre entidades
- Permite manter integridade referencial
- Exemplo: `depto_id` em Funcionário (referencia Departamento.id)

#### **Chave Composta**

- Combinação de dois ou mais atributos que forma a chave
- Exemplo: (matricula, ano_semestre) em Inscrição

---

### 4. **Relacionamentos**

**Definição**: Associação ou vínculo entre duas ou mais entidades.

**Exemplo**: Um Aluno **se inscreve em** uma Disciplina

#### **Tipos de Relacionamento**

- **1:1** (Um para Um)
- **1:N** (Um para Muitos)
- **N:M** (Muitos para Muitos)

---

## Cardinalidade (Relações de Multiplicidade)

### **1:1 (Um para Um)**

**Definição**: Uma instância em A está associada a NO MÁXIMO uma instância em B, e vice-versa.

**Notação**: Linha com "1" em ambas as extremidades

**Exemplos**:

- Um Pessoa tem um CPF
- Um Presidente governa um País
- Um Funcionário tem um Crachá

**Implementação**:

```
Pessoa (id_pessoa: PK, nome, cpf_fk: FK)
CPF (id_cpf: PK, numero_cpf, id_pessoa_fk: FK UNIQUE)
```

---

### **1:N (Um para Muitos)**

**Definição**: Uma instância em A pode estar associada a MUITAS instâncias em B, mas cada instância em B está associada a apenas UMA instância em A.

**Notação**: Linha com "1" em um lado e "N" no outro (ou (0,1) e (0,N))

**Exemplos**:

- Um Departamento tem MUITOS Funcionários
- Um Professor ministra MUITAS Disciplinas
- Um Cliente faz MUITOS Pedidos
- Uma Disciplina tem MUITOS Alunos

**Implementação**:

```
Departamento (id_depto: PK, nome)
Funcionario (id_func: PK, nome, depto_id_fk: FK)
```

---

### **N:M (Muitos para Muitos)**

**Definição**: Uma instância em A pode estar associada a MUITAS instâncias em B, e vice-versa.

**Notação**: Linha com "N" em ambas as extremidades

**Exemplos**:

- Um Aluno estuda MUITAS Disciplinas
- Uma Disciplina tem MUITOS Alunos
- Um Ator participa em MUITOS Filmes
- Um Filme tem MUITOS Atores
- Um Projeto utiliza MUITOS Recursos
- Um Recurso é utilizado em MUITOS Projetos

**Implementação** (deve criar tabela intermediária):

```
Aluno (id_aluno: PK, nome)
Disciplina (id_disciplina: PK, nome)

Inscricao (id_aluno_fk: FK, id_disciplina_fk: FK, data_inscricao, nota)
// Ambos os FKs formam a chave primária composta
```

---

## Notação de Cardinalidade

### **Notação (min, max)**

Especifica cardinalidade mínima e máxima:

- **(0,1)**: Opcional, no máximo 1
- **(1,1)**: Obrigatório, exatamente 1
- **(0,N)**: Opcional, múltiplos
- **(1,N)**: Obrigatório, múltiplos

### **Exemplos**

- Pessoa → (1,1) tem (1,1) ← CPF (cada pessoa tem um CPF obrigatório)
- Departamento → (1,N) contém (1,1) ← Funcionário (departamento tem vários funcionários obrigatoriamente)
- Aluno → (0,N) inscreve-se (0,N) ← Disciplina (aluno pode se inscrever em várias disciplinas, opcionalmente)

---

## Exemplo Prático: Sistema de Universidade

### **Entidades**

- Pessoa (id_pessoa: PK, nome, data_nascimento)
- Aluno (id_aluno: PK, matricula: UNIQUE, id_pessoa_fk: FK)
- Professor (id_professor: PK, matricula_prof: UNIQUE, id_pessoa_fk: FK)
- Departamento (id_depto: PK, nome: UNIQUE)
- Disciplina (id_disc: PK, codigo: UNIQUE, nome, id_depto_fk: FK)

### **Relacionamentos e Cardinalidades**

- 1 Pessoa → (1,1) é (0,1) → Aluno OU Professor (uma pessoa pode ser um aluno ou professor)
- 1 Professor → (1,N) ministra (1,1) → Disciplina (um professor ministra várias disciplinas)
- 1 Aluno → (0,N) inscreve-se (0,N) ← Disciplina (aluno pode se inscrever em várias disciplinas)
- 1 Departamento → (1,N) oferta (1,1) → Disciplina (departamento oferta várias disciplinas)

---

## Resumo das Chaves

| Tipo | Definição | Quantidade | Pode ser NULL |
|------|-----------|-----------|----------------|
| **Primária** | Identifica unicamente a tupla | 1 por entidade | Não |
| **Candidata** | Pode ser primária | Várias | Não |
| **Super** | Qualquer identificador único | Várias | Não |
| **Estrangeira** | Referencia chave primária de outra entidade | Várias | Sim (depende) |

---

## Resumo da Cardinalidade

| Cardinalidade | Notação | Significado | Exemplo |
|--------------|---------|-----------|---------|
| **1:1** | 1 — 1 | Um para um | Pessoa ↔ CPF |
| **1:N** | 1 — N | Um para muitos | Departamento → Funcionários |
| **N:M** | N — N | Muitos para muitos | Alunos ↔ Disciplinas |

---

## Dicas Importantes

✓ **Para identificar entidades**: Procure por substantivos nas especificações
✓ **Para identificar atributos**: Procure por adjetivos e características
✓ **Para identificar relacionamentos**: Procure por verbos de ação
✓ **Para determinar cardinalidade**: Faça perguntas como "Um X pode ter quantos Y?" e "Um Y pertence a quantos X?"
✓ **Use diagramas ER**: Visualize os relacionamentos para melhor compreensão
✓ **Normalize o modelo**: Reduza redundâncias seguindo as formas normais (1NF, 2NF, 3NF)

---

## Próximas Etapas

1. **Modelo Lógico**: Transformar MER em tabelas relacionais
2. **Modelo Físico**: Implementar em um SGBD (SQL Server, MySQL, PostgreSQL, etc.)
3. **Normalização**: Aplicar regras de normalização (1NF, 2NF, 3NF, BCNF)
4. **SQL**: Criar as tabelas e relacionamentos
