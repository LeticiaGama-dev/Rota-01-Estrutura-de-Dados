
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

### 1. BigQuery – Criação de Tabelas e ETL
1. Acesse o **Google BigQuery** e conecte-se ao dataset da **Super Store**.  
2. Importe a tabela de vendas da Super Store.  
3. Execute consultas SQL para:
   - Identificar valores nulos e duplicados;  
   - Padronizar variáveis categóricas;  
   - Validar consistência de variáveis numéricas.  
4. Modele e crie as **tabelas de dimensões** (clientes, produtos, regiões, modos de envio, etc.) com IDs únicos.  
5. Crie a **tabela fato** contendo métricas de vendas, lucro, quantidade e desconto, referenciando os IDs das dimensões.  
6. Projete o **pipeline conceitual de atualização de dados**, definindo a ordem de atualização entre as tabelas (dimensões antes da fato).

### 2. Python / Google Colab – Web Scraping
1. Abra o notebook em **Google Colab** disponível na pasta `notebooks/`.  
2. Execute o script Python para extrair dados de concorrentes da Wikipedia utilizando **BeautifulSoup**.  
3. Os dados extraídos serão transformados em CSV e importados para BigQuery, integrando-se à estrutura de tabelas criada.

### Observações
- A tabela da Super Store é fictícia, enquanto a tabela de concorrentes foi obtida via web scraping do site wikipedia.  
- Todas as transformações realizadas foram **conceituais**, voltadas à padronização e organização dos dados.  
- Para reproduzir o projeto, basta seguir a sequência acima no BigQuery e executar o notebook de Python.  


### Observações
- Dados provenientes do dataset fictício da Super Store e de fontes públicas (Wikipedia).  
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
- Definição da sequência de atualização das tabelas, considerando dependências;  
  - Projeção de um **pipeline conceitual** de atualização diária dos dados.  

---

##  Códigos / Queries Relevantes

### 1. SQL – Criação das Tabelas de Dimensões

* **O que faz:** cria tabelas únicas para cada dimensão (clientes, produtos, regiões, modos de envio) com IDs exclusivos.

```sql
-- Exemplo: tabela dimensão Cliente
CREATE TABLE dimensao_cliente AS
SELECT DISTINCT
    customer_id AS cliente_id,
    customer_name,
    segment,
    city,
    state,
    country
FROM superstore_vendas;
```

### 2. SQL – Criação da Tabela Fato

* **O que faz:** cria tabela fato de vendas, referenciando os IDs das dimensões e agregando métricas de interesse.

```sql
-- Exemplo: tabela fato Vendas
CREATE TABLE fato_vendas AS
SELECT
    f.order_id,
    f.order_date,
    c.cliente_id,
    p.produto_id,
    r.regiao_id,
    f.quantity,
    f.sales,
    f.profit,
    f.discount
FROM superstore_vendas f
JOIN dimensao_cliente c ON f.customer_id = c.cliente_id
JOIN dimensao_produto p ON f.product_id = p.produto_id
JOIN dimensao_regiao r ON f.region = r.regiao_nome;
```

### 3. Python / Colab – Web Scraping de Concorrentes

* **O que faz:** extrai dados de concorrentes da Wikipedia e gera CSV para integração no BigQuery.

```python
import requests
from bs4 import BeautifulSoup
import pandas as pd

url = "https://pt.wikipedia.org/wiki/Lista_de_multinacionais"
resposta = requests.get(url)
soup = BeautifulSoup(resposta.text, 'html.parser')

# Localiza tabela e transforma em DataFrame
tabela = soup.find('table', {'class':'wikitable'})
df_concorrentes = pd.read_html(str(tabela))[0]

# Salva como CSV
df_concorrentes.to_csv('concorrentes.csv', index=False)
```

##  Visualizações / Dashboards

### Dashboard 1 – Visão Geral de Vendas

* **O que mostra:** resumo das vendas da Super Store por **categoria**, **subcategoria** e **região**.
* **Objetivo:** permite análise rápida do desempenho por produto e região, facilitando a identificação de padrões de vendas.
* **Link do print:** [Dash1-Rota 01](dashboards_screenshots/Dash1-Rota%2001.jpg)

### Dashboard 2 – Tabelas de Dimensões

* **O que mostra:** exemplos das tabelas de **dimensões** criadas, incluindo **clientes**, **produtos**, **regiões** e **modos de envio**.
* **Objetivo:** demonstra a organização dos dados em tabelas separadas para consultas eficientes, evitando duplicidade e facilitando o ETL.
* **Link do print:** [Dash2-Rota 01](dashboards_screenshots/Dash2-Rota%2001.jpg)

---

### Observações

* As queries SQL são exemplos representativos do processo de **criação das tabelas** e integração dos dados.
* O notebook Python contém o script completo para web scraping dos concorrentes.

---

## Autor
**Leticia Gama de Souza**  
[LinkedIn](https://www.linkedin.com/in/leticia-gama-code) | [GitHub](https://github.com/LeticiaGama-dev)
