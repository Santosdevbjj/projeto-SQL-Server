## Montando Consultas Relacionais no SQL Server 

![Screenshot_20250617-153050](https://github.com/user-attachments/assets/f4cb15ea-1bb3-446d-8b76-3b659dc6d97f)


## Bootcamp WEX - End to End Engineering. Ministrado pela DIO.

---


## Desafio de projeto: 

Para este desafio, você precisará usar seus conhecimentos adquiridos no módulo de banco de dados, da trilha .NET da DIO.

## Contexto: 
Você é responsável pelo banco de dados de um site de filmes, onde são armazenados dados sobre os filmes e seus atores. Sendo assim, foi solicitado para que você realize uma consulta no banco de dados com o objetivo de trazer alguns dados para análises.

## Proposta:

Você precisará realizar 12 consultas ao banco de dados, cada uma retornando um tipo de informação. 

**O seu banco de dados é modelado da seguinte maneira:**

![diagramaWexDesafioBD](https://github.com/user-attachments/assets/1ee10904-5b2c-46a3-b7df-f2f4d65ccbf1)



**As tabelas são descritas conforme a seguir:**

**Filmes**

Tabela responsável por armazenar informações dos filmes.

**Atores**

Tabela responsável por armazenar informações dos atores.

**Generos**

Tabela responsável por armazenar os gêneros dos gêneros.

**ElencoFilme**

Tabela responsável por representar um relacionamento do tipo muitos para muitos entre filmes e atores, ou seja, um ator pode trabalhar em muitos filmes, e filmes podem ter muitos atores.

**FilmesGenero**

Tabela responsável por representar um relacionamento do tipo muitos para muitos entre filmes e gêneros, ou seja, um filme pode ter mais de um gênero, e um gênero pode fazer parte de muitos filmes. 


**Objetivo:**

Você deverá criar diversas consultas, com o objetivo de retornar os dados a seguir. Abaixo de cada pedido tem o retorno esperado. O seu retorno deve ser igual ao da imagem.


**1 - Buscar o nome e ano dos filmes**

![AnoENomeWex](https://github.com/user-attachments/assets/bae28d6f-1f73-44b2-bccd-73250e1c5ec2)


**2 - Buscar o nome e ano dos filmes ordenados por ordem crescente pelo ano**

![AnoNomeFilmeWex](https://github.com/user-attachments/assets/cb677ce2-2f72-4c6e-8f9c-3b96e71317c1)


**3 - Buscar pelo filme de volta para o futuro, trazendo o nome, ano e a duração**

![NomeAnoDura](https://github.com/user-attachments/assets/bb1390f5-a4cc-4520-b168-146b7dd1280f)


**4 - Buscar os filmes lançados em 1997**

![lanca1997](https://github.com/user-attachments/assets/a96623cf-dbf8-4057-93e0-e78c4d6e039e) 



**5 - Buscar os filmes lançados APÓS o ano 2000**

![filmesApos2000](https://github.com/user-attachments/assets/37b6230c-13c3-4287-971e-45ca5bf6643f)


**6 - Buscar os filmes com a duração maior que 100 e menor que 150, ordenando pela duração em ordem crescente**

![filmeMaior100Menor150](https://github.com/user-attachments/assets/431a40aa-390f-4904-834b-072dfae385b2)


**7 - Buscar a quantidade de filmes lançados no ano, agrupando por ano, ordenando pela duração em ordem decrescente**

![filmeAgrupadoAno](https://github.com/user-attachments/assets/f95dd279-d0e4-4039-a685-4c55793b0e35)



**8 - Buscar os Atores do gênero masculino, retornando o PrimeiroNome, UltimoNome**

![atorGeneroPrimeroNome](https://github.com/user-attachments/assets/b2356c2a-b884-4b3d-928f-14e54bf96f0e)


**9 - Buscar os Atores do gênero feminino, retornando o PrimeiroNome, UltimoNome, e ordenando pelo PrimeiroNome**

![atorGeneroFemino](https://github.com/user-attachments/assets/228ca7a5-2187-4819-84fe-5c6a3cc5b5b4)


**10 - Buscar o nome do filme e o gênero** 

![nomeFilmeGenero](https://github.com/user-attachments/assets/b9218797-a27e-4aaa-8edd-9df792eedec9)



**11 - Buscar o nome do filme e o gênero do tipo "Mistério"**

![filmeGeneroMisterio](https://github.com/user-attachments/assets/995ee85a-96b5-4611-bb6f-3deee3e38fd7)


**12 - Buscar o nome do filme e os atores, trazendo o PrimeiroNome, UltimoNome e seu Papel**

![filmeAtorPrimeroUltimoNome](https://github.com/user-attachments/assets/b58c7838-8af5-4960-b926-094e96a32300)



---

**Contato:**


[![Portfólio Sérgio Santos](https://img.shields.io/badge/Portfólio-Sérgio_Santos-111827?style=for-the-badge&logo=githubpages&logoColor=00eaff)](https://santosdevbjj.github.io/portfolio/)
[![LinkedIn Sérgio Santos](https://img.shields.io/badge/LinkedIn-Sérgio_Santos-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/santossergioluiz) 


---


