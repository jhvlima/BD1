# Structured Query Language - SQL

## Equivalência Álgebra Relacional e SQL

| Operação               | Símbolo | Equivalente em SQL              | Exemplo SQL                                               |
| ---------------------- | ------- | ------------------------------- | --------------------------------------------------------- |
| **Selection**          | σ       | `WHERE`                         | `SELECT * FROM alunos WHERE idade > 18;`                  |
| **Projection**         | π       | `SELECT` (colunas específicas)  | `SELECT nome, idade FROM alunos;`                         |
| **Cartesian Product**  | ×       | `CROSS JOIN`                    | `SELECT * FROM A CROSS JOIN B;`                           |
| **Join**               | ⊲⊳      | `INNER JOIN`, `LEFT JOIN`, etc. | `SELECT * FROM A INNER JOIN B ON A.id = B.id;`            |
| **Union**              | ∪       | `UNION`                         | `SELECT nome FROM A UNION SELECT nome FROM B;`            |
| **Intersection**       | ∩       | `INTERSECT`                     | `SELECT nome FROM A INTERSECT SELECT nome FROM B;`        |
| **Difference**         | −       | `EXCEPT` / `MINUS`              | `SELECT nome FROM A EXCEPT SELECT nome FROM B;`           |
| **Division**           | ÷       | Subquery com `NOT EXISTS`       | *(ver abaixo)*                                            |
| **Rename**             | ρ       | `AS` (alias)                    | `SELECT nome AS aluno_nome FROM alunos;`                  |
| **Assignment**         | ←       | `WITH` (CTE)                    | `WITH temp AS (SELECT * FROM alunos) SELECT * FROM temp;` |
| **Proj. Generalizada** | —       | Expressões no `SELECT`          | `SELECT salario * 1.1 AS novo_salario FROM funcionarios;` |
| **Agregação**          | ℑ       | `GROUP BY` + funções            | `SELECT COUNT(*) FROM alunos;`                            |
