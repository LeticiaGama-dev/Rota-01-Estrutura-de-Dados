
# Rota 01 – Estrutura de Dados

##  Introdução
O projeto **Rota 01 – Estrutura de Dados** tem como objetivo construir um sistema tabular relacional para a empresa **Super Store**, aplicando o processo **ETL (Extrair, Transformar e Carregar)**.  

A proposta é criar um conjunto de tabelas de fatos e dimensões que permita armazenar e consultar informações de forma eficiente, facilitando a análise de dados e a tomada de decisões estratégicas.  

O trabalho visa também introduzir os conceitos de estruturas de dados e modelagem dimensional, utilizando técnicas de ETL e organização em banco de dados relacional.

##  Tecnologias Utilizadas
- **Google BigQuery** – para criação e consulta das tabelas de fatos e dimensões  
- **Planilhas Google** – para extração e transformação de dados externos  
- **Python / Google Colab** – para web scraping (BeautifulSoup)  
- **SQL** – linguagem utilizada nas consultas e criação das tabelas  

##  Como Rodar o Projeto
1. Acesse o **Google BigQuery** e conecte-se ao dataset da **Super Store**.  
2. Importe a tabela de vendas da Super Store conforme o roteiro.  
3. Identifique valores nulos, duplicados e inconsistências conforme as etapas do processo ETL.  
4. Realize a pesquisa de concorrentes via **Planilhas Google (IMPORTHTML)** ou **Python (BeautifulSoup)**.  
5. Modele as tabelas de **fatos** e **dimensões**, utilizando identificadores únicos (IDs).  
6. Crie as tabelas no **BigQuery**, estruturando o modelo relacional conforme planejado.  
7. Projete o **pipeline de atualização de dados**, determinando a sequência de atualização entre as tabelas.  

### Observações
- Dados provenientes do dataset da Super Store e de fontes públicas (Wikipedia).  
- A limpeza realizada foi **conceitual**, voltada à padronização e estruturação dos dados para consultas.  
- Não foram inseridos dados novos nem removidos valores nulos — apenas identificados e tratados para padronização.  

##  Estrutura e Etapas do Projeto

### 1. Extração
- Importação dos dados originais da **Super Store**.  
- Pesquisa e extração de dados de concorrentes utilizando **IMPORTHTML** (Planilhas Google) ou **BeautifulSoup** (Python).

### 2. Transformação
- Identificação e tratamento de:
  - Valores **nulos** (identificados, mas não removidos);
  - **Duplicatas** (identificadas para evitar redundâncias nas tabelas);
  - **Inconsistências categóricas** (padronização de texto, formatação e uniformização);
  - **Inconsistências numéricas** (verificação de tipos incorretos).  
- Padronização dos dados antes da criação das tabelas de fatos e dimensões.

### 3. Carga
- Criação do modelo relacional no **BigQuery** com tabelas de **fatos** e **dimensões**:  
  - **Tabelas de Dimensões:** Contêm informações únicas e descritivas (clientes, produtos, regiões, modos de envio etc.).  
  - **Tabela de Fatos:** Contém métricas de negócio (vendas, lucro, quantidade, descontos) referenciadas pelos IDs das dimensões.  
- Escolha do **modelo estrela (Star Schema)**, que facilita consultas e análises no contexto de Business Intelligence.

### 4. Pipeline de Atualização
- Definição da sequência de atualização das tabelas, considerando dependências:
  - As tabelas **dimensão** devem ser atualizadas **antes** da tabela **fato**.  
  - Projeção de um **pipeline conceitual** de atualização diária dos dados.  

##  Consultas SQL Utilizadas
Foram criadas e executadas consultas SQL no **BigQuery** para:
- Identificação de nulos e duplicatas;
- Padronização de variáveis categóricas;
- Criação das tabelas de dimensões e da tabela fato;
- Relacionamento entre as tabelas utilizando IDs.  

*(As queries completas estão documentadas na ficha técnica.)*

##  Estrutura do Repositório
Rota-01-Estrutura-de-Dados/  
│  
├─ datasets/               # Bases de dados utilizadas (Super Store e concorrentes)  
├─ dashboards_screenshots/ # Capturas de tela da modelagem ou esquema relacional  
├─ README.md               # Documento explicativo do projeto  
├─ .gitignore (opcional)   # Arquivos a serem ignorados no Git  

## Autor
**Leticia Gama de Souza**  
[LinkedIn](https://www.linkedin.com/in/leticia-gama-code) | [GitHub](https://github.com/LeticiaGama-dev)
