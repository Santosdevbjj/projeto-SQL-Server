# 🎬 Consultas Relacionais em SQL Server — Banco de Dados de Filmes

![Banner](https://github.com/user-attachments/assets/f4cb15ea-1bb3-446d-8b76-3b659dc6d97f)

> **Bootcamp WEX — End to End Engineering · DIO**

[![SQL Server](https://img.shields.io/badge/SQL_Server-Database-CC2927?style=for-the-badge&logo=microsoftsqlserver&logoColor=white)](https://www.microsoft.com/sql-server)
[![T-SQL](https://img.shields.io/badge/T--SQL-Queries-0078D7?style=for-the-badge&logo=microsoftsqlserver&logoColor=white)](https://learn.microsoft.com/sql/t-sql/)
[![Relacionamentos](https://img.shields.io/badge/Modelo-N:N_Relacionamentos-9B59B6?style=for-the-badge)]()
[![Status](https://img.shields.io/badge/Status-Concluído-00b300?style=for-the-badge)]()

---

## 1. Problema de Negócio

Plataformas de catálogo de filmes armazenam dados distribuídos em múltiplas tabelas relacionadas — filmes, atores, gêneros e elenco. O problema recorrente nesse cenário é que **dados isolados em tabelas separadas não respondem perguntas de negócio**. "Quais atores participaram de filmes de Mistério?" ou "Qual o papel de cada ator em cada filme?" são perguntas que só existem como resultado de cruzamento de tabelas.

Este projeto responde 12 perguntas analíticas reais sobre um banco de dados de filmes, utilizando consultas SQL que cobrem filtros simples, ordenação, agregação com `GROUP BY` e joins entre múltiplas tabelas com relacionamentos muitos-para-muitos — o padrão mais comum em bancos de dados relacionais de produção.

---

## 2. Contexto

O sistema foi desenvolvido como desafio de projeto no **Bootcamp WEX — End to End Engineering**, com foco em modelagem relacional e consultas T-SQL no SQL Server.

O banco de dados `Filmes` é composto por cinco tabelas com dois padrões de relacionamento:

- **`Filmes`** — catálogo com nome, ano de lançamento e duração
- **`Atores`** — cadastro com primeiro nome, último nome e gênero
- **`Generos`** — lista de gêneros cinematográficos
- **`ElencoFilme`** — tabela de junção N:N entre `Filmes` e `Atores`, armazenando também o papel de cada ator
- **`FilmesGenero`** — tabela de junção N:N entre `Filmes` e `Generos`

Os relacionamentos muitos-para-muitos são o coração do modelo: um ator pode trabalhar em vários filmes, um filme pode ter vários atores, e um filme pode pertencer a múltiplos gêneros. Navegar esses relacionamentos com `INNER JOIN` é o desafio central do projeto.

![Diagrama de Classes](https://github.com/user-attachments/assets/1ee10904-5b2c-46a3-b7df-f2f4d65ccbf1)

---

## 3. Premissas da Análise

- Todas as consultas foram executadas no contexto `USE [Filmes]` para garantir que operam sobre o banco correto
- Os aliases de tabela (`F`, `A`, `G`, `FG`, `EF`) foram padronizados de forma consistente em todas as consultas JOIN para facilitar leitura e manutenção
- A consulta 7 interpreta "ordenar pela duração em ordem decrescente" como ordenar pela **quantidade de filmes por ano** em ordem decrescente — a alternativa de ordenar por `SomaDuracao` está documentada no script como opção
- As comparações de gênero de ator usam `'M'` e `'F'` como valores literais, conforme o schema original
- Nenhuma das 12 consultas usa `SELECT *` — todas projetam apenas as colunas necessárias para a pergunta respondida, reduzindo transferência de dados desnecessária

---

## 4. Estratégia da Solução

As 12 consultas foram organizadas em três níveis crescentes de complexidade:

**Nível 1 — Consultas simples (queries 1 a 5)**
Projeção de colunas com `SELECT`, filtros com `WHERE` e ordenação com `ORDER BY`. Nenhum JOIN necessário — os dados respondem à pergunta diretamente da tabela `Filmes`.

**Nível 2 — Consultas com filtros compostos e agregação (queries 6 e 7)**
Filtros com operadores relacionais combinados (`AND`, `>`, `<`) e agrupamento com `GROUP BY` + `COUNT()`. A query 7 demonstra a diferença entre filtrar linhas (`WHERE`) e filtrar grupos (`HAVING`).

**Nível 3 — Consultas relacionais com múltiplos JOINs (queries 8 a 12)**
Cruzamento de duas ou três tabelas com `INNER JOIN`, navegando as tabelas de junção N:N para responder perguntas que envolvem entidades de domínios diferentes (filmes + gêneros, filmes + atores + papéis).

---

## 5. Insights Técnicos

A implementação das 12 consultas revela padrões recorrentes com impacto direto em performance e legibilidade:

**`INNER JOIN` vs resultado de produto cartesiano**
Sem `JOIN`, um `SELECT` com duas tabelas produz o produto cartesiano — cada linha de `Filmes` combinada com cada linha de `Atores`. Com 28 filmes e 23 atores, isso geraria 644 linhas irrelevantes. O `INNER JOIN` com a condição correta (`EF.IdFilme = F.Id`) filtra apenas as combinações com relacionamento real na tabela `ElencoFilme`.

**Tabelas de junção como entidades com dados próprios**
`ElencoFilme` não é apenas um conector — ela armazena `Papel`, um atributo do relacionamento que não pertence nem ao ator nem ao filme individualmente. Esse padrão é fundamental em modelagem relacional: quando um relacionamento N:N tem atributos próprios, ele se torna uma entidade de junção com coluna adicional.

**`COUNT(Id)` vs `COUNT(*)`**
A query 7 usa `COUNT(Id)` em vez de `COUNT(*)`. `COUNT(*)` conta todas as linhas incluindo NULLs; `COUNT(Id)` conta apenas linhas onde `Id` não é nulo. Como `Id` é `PRIMARY KEY`, o resultado é idêntico aqui — mas o hábito de referenciar a coluna explícita comunica intenção e é mais seguro em consultas com `LEFT JOIN`, onde NULLs podem aparecer.

**Aliases de coluna com `AS` nas queries de JOIN**
A query 10 retorna `F.Nome AS NomeFilme` para evitar ambiguidade: tanto `Filmes` quanto `Generos` poderiam ter uma coluna chamada `Nome`. Sem alias, o resultado seria ambíguo para o consumidor da query. Com `AS NomeFilme`, o contrato da consulta é explícito.

**Ordem dos JOINs e legibilidade**
As queries 10, 11 e 12 seguem o padrão: tabela principal primeiro (`Filmes`), depois tabela de junção, depois tabela de destino. Essa ordem espelha a navegação lógica do modelo relacional e facilita a leitura por outros desenvolvedores.

---

## 6. Resultados

As 12 consultas cobrem o espectro completo de operações analíticas sobre o banco:

| # | Consulta | Técnica SQL |
|---|---|---|
| 1 | Nome e ano de todos os filmes | `SELECT` + projeção de colunas |
| 2 | Filmes ordenados por ano crescente | `ORDER BY ASC` |
| 3 | Busca específica por título | `WHERE` com igualdade |
| 4 | Filmes lançados em 1997 | `WHERE` com filtro de ano |
| 5 | Filmes lançados após 2000 | `WHERE` com operador `>` |
| 6 | Filmes entre 100 e 150 min | `WHERE` com `AND` e faixa numérica |
| 7 | Quantidade de filmes por ano | `GROUP BY` + `COUNT()` + `ORDER BY DESC` |
| 8 | Atores do gênero masculino | `WHERE Genero = 'M'` |
| 9 | Atrizes ordenadas por primeiro nome | `WHERE` + `ORDER BY` |
| 10 | Filmes com seus gêneros | `INNER JOIN` duplo via `FilmesGenero` |
| 11 | Filmes do gênero Mistério | `INNER JOIN` duplo + `WHERE` no gênero |
| 12 | Filmes com elenco e papéis | `INNER JOIN` triplo via `ElencoFilme` |

Resultados visuais das consultas:

**Consulta 1 — Nome e Ano dos filmes**
![Q1](https://github.com/user-attachments/assets/bae28d6f-1f73-44b2-bccd-73250e1c5ec2)

**Consulta 2 — Filmes ordenados por ano**
![Q2](https://github.com/user-attachments/assets/cb677ce2-2F72-4c6e-8f9c-3b96e71317c1)

**Consulta 3 — De Volta para o Futuro**
![Q3](https://github.com/user-attachments/assets/bb1390f5-a4cc-4520-b168-146b7dd1280f)

**Consulta 4 — Filmes de 1997**
![Q4](https://github.com/user-attachments/assets/a96623cf-dbf8-4057-93e0-e78c4d6e039e)

**Consulta 5 — Filmes após 2000**
![Q5](https://github.com/user-attachments/assets/37b6230c-13c3-4287-971e-45ca5bf6643f)

**Consulta 6 — Duração entre 100 e 150 min**
![Q6](https://github.com/user-attachments/assets/431a40aa-390f-4904-834b-072dfae385b2)

**Consulta 7 — Quantidade de filmes por ano**
![Q7](https://github.com/user-attachments/assets/f95dd279-d0e4-4039-a685-4c55793b0e35)

**Consulta 8 — Atores masculinos**
![Q8](https://github.com/user-attachments/assets/b2356c2a-b884-4b3d-928f-14e54bf96f0e)

**Consulta 9 — Atrizes ordenadas**
![Q9](https://github.com/user-attachments/assets/228ca7a5-2187-4819-84fe-5c6a3cc5b5b4)

**Consulta 10 — Filmes e gêneros**
![Q10](https://github.com/user-attachments/assets/b9218797-a27e-4aaa-8edd-9df792eedec9)

**Consulta 11 — Filmes de Mistério**
![Q11](https://github.com/user-attachments/assets/995ee85a-96b5-4611-bb6f-3deee3e38fd7)

**Consulta 12 — Filmes com elenco e papéis**
![Q12](https://github.com/user-attachments/assets/b58c7838-8af5-4960-b926-094e96a32300)

---

## 7. Próximos Passos

- [ ] Criar **índices** nas colunas de chave estrangeira (`IdFilme`, `IdAtor`, `IdGenero`) nas tabelas de junção, reduzindo o custo de lookup nos JOINs
- [ ] Adicionar consultas com `LEFT JOIN` para identificar filmes sem gênero cadastrado e atores sem filmes associados — casos que `INNER JOIN` silenciosamente omite
- [ ] Implementar consultas com `HAVING` para filtrar grupos após agregação (ex: anos com mais de 3 filmes)
- [ ] Criar **Views** para as consultas mais frequentes (ex: `vw_FilmesComGenero`, `vw_ElencoCompleto`), abstraindo a complexidade dos JOINs para consumidores da base
- [ ] Modelar consultas de ranking com `ROW_NUMBER()` e `RANK()` para encontrar, por exemplo, o ator com mais filmes ou o gênero mais recorrente no catálogo
- [ ] Exportar os resultados para análise visual em **Power BI** conectado ao SQL Server via DirectQuery

---

## 🗂️ Estrutura do Projeto

```
projeto-SQL-Server/
├── SQLScriptFilmes.sql   # DDL completo: criação de tabelas, FK e INSERT de dados
└── respostasSQL.sql      # 12 consultas T-SQL documentadas com comentários
```

---

## 🛠️ Tecnologias Utilizadas

| Tecnologia | Versão | Papel no Projeto |
|---|---|---|
| SQL Server | 2019+ | Engine de banco de dados relacional |
| T-SQL | — | Linguagem de consulta e manipulação de dados |
| SSMS / Azure Data Studio | — | Interface de execução das queries |
| Git + GitHub | — | Versionamento dos scripts SQL |

---

## ▶️ Como Executar

**Pré-requisito:** SQL Server instalado localmente ou acesso a uma instância Azure SQL / SQL Server em container Docker.

```sql
-- Passo 1: Execute o script completo de criação do banco
-- Arquivo: SQLScriptFilmes.sql
-- Isso cria o banco [Filmes], as 5 tabelas, as constraints FK e insere todos os dados

-- Passo 2: Execute as consultas individualmente ou em bloco
-- Arquivo: respostasSQL.sql
USE [Filmes];
GO
-- Execute cada bloco de query para ver os resultados
```

Via Docker (SQL Server):

```bash
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=SuaSenha123!" \
  -p 1433:1433 --name sqlserver \
  -d mcr.microsoft.com/mssql/server:2019-latest
```

Conecte com SSMS ou Azure Data Studio em `localhost,1433` com usuário `sa`.

---

## 📐 Decisões Técnicas

**Por que tabelas de junção explícitas (`ElencoFilme`, `FilmesGenero`) em vez de arrays ou JSON?**
Relacionamentos N:N em bancos relacionais são modelados com tabelas de junção para manter integridade referencial via Foreign Key. Armazenar uma lista de IDs em JSON ou array quebraria a capacidade de fazer JOIN, indexar e garantir consistência com `FK`. A tabela de junção também permite adicionar atributos ao relacionamento — como `Papel` em `ElencoFilme` — sem alterar as tabelas de entidade.

**Por que `INNER JOIN` e não `LEFT JOIN` nas consultas de gênero e elenco?**
`INNER JOIN` retorna apenas filmes que têm relacionamento registrado em `FilmesGenero` ou `ElencoFilme`. `LEFT JOIN` retornaria todos os filmes, com `NULL` nas colunas de gênero/ator para os sem relacionamento. Para as perguntas de negócio propostas ("filmes com gênero", "filmes com elenco"), `INNER JOIN` é semanticamente correto — filmes sem dados de gênero ou elenco não devem aparecer no resultado.

**Por que aliases curtos (`F`, `A`, `G`) nas queries de JOIN?**
Queries com três ou mais tabelas se tornam ilegíveis quando o nome completo da tabela é repetido em cada condição de JOIN. Aliases de uma ou duas letras são convenção consolidada em T-SQL para consultas analíticas, desde que sejam consistentes dentro da mesma query.

---

## 🧠 Aprendizados

O maior aprendizado foi entender **quando usar `WHERE` e quando usar `HAVING`**. Ambos filtram resultados, mas em momentos diferentes da execução: `WHERE` filtra linhas antes do agrupamento, `HAVING` filtra grupos depois do `GROUP BY`. Na query 7, tentar filtrar por `COUNT()` com `WHERE` causaria erro — a contagem só existe após o agrupamento.

O desafio técnico mais relevante foi navegar os relacionamentos N:N da query 12, que exige três JOINs: `Filmes → ElencoFilme → Atores`. O ponto crítico é manter claro qual tabela conecta quais entidades, e qual coluna é usada em cada condição de join.

O que faria diferente: adicionaria `LEFT JOIN` como variação documentada das queries 10 a 12 para evidenciar quais filmes não têm gênero ou elenco cadastrado — casos que `INNER JOIN` silencia, mas que em produção representam dados incompletos que precisam de atenção.

---

## 🤝 Contribuição

Sugestões de novas consultas, otimizações com índices ou extensões do modelo de dados são bem-vindas. Abra uma issue ou envie um pull request.

---

## 📬 Contato

[![Portfólio](https://img.shields.io/badge/Portfólio-Sérgio_Santos-111827?style=for-the-badge&logo=githubpages&logoColor=00eaff)](https://portfoliosantossergio.vercel.app)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Sérgio_Santos-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/santossergioluiz)
[![GitHub](https://img.shields.io/badge/GitHub-Santosdevbjj-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Santosdevbjj)
