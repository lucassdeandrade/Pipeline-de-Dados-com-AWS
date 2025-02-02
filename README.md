# Pipeline de Dados com AWS

### Este projeto consiste em um pipeline de processamento de dados na AWS, utilizando uma arquitetura de 3 camadas (dados, lógica e aplicação) e em diferentes zonas de armazenamento (Landing, Processing, Curated) para organizar e processar dados de forma estruturada. O processamento é realizado usando um cluster no Amazon EMR com um job escrito em PySpark. O cluster Spark lê os dados brutos da Landing Zone, realiza transformações e agregações, e salva os resultados nas zonas subsequentes.


![Capa-aws](https://github.com/user-attachments/assets/1d28e6cc-b046-4665-8e5f-9f741866b4dc)

# Arquitetura

###  Usaremos a arquitetura 3 camadas (dados, lógica, aplicação) que nos dá a praticidade para utilizar as tecnologias que preferimos em cada camada, sem a necessidade de concentrar de forma monolítica.

## Landing Zone
Aqui é a camada de **dados**: a área onde faço o armazenamento dos dados raw (brutos) extraindo da minha fonte de dados. aqui geralmente não faço grandes transformações, apenas o armazenamento mesmo.


- **Características:**
Contém dados no formato original, como foram extraídos. Pode ter dados estruturados, semiestruturados (JSON, XML) ou não estruturados (imagens, vídeos, logs). Utilizada para garantir uma cópia fiel do dado original, preservando a integridade.

## Processing Zone

Essa é a camada de **Lógica**: Aqui, os dados são processados e transformados. Isso inclui limpeza, normalização, agregação ou qualquer tipo de tratamento necessário.

- **Características**: Dados começam a ser estruturados e organizados. Pode envolver processos de deduplicação, correção de erros, conversão de formatos e enriquecimento.

## Curated Zone

Camada de **Aplicação/Apresentação**: Essa é a camada onde os dados processados e organizados são armazenados de forma otimizada para serem consumidos por relatórios ou análises.

- **Características:** Dados preparados para relatórios, análises e machine learning. Estruturados em formatos prontos para consulta, como tabelas SQL ou datasets otimizados (Parquet, ORC).

# Tecnologias Utilizadas

- **AWS S3**: Armazenamento de dados nas zonas Landing, Processing e Curated.

- **Amazon EMR**: Execução do job Spark.

- **Apache Spark**: Processamento distribuído de dados.

- **PySpark**: API de Python utilizada para o job Spark.

- **Spark History Server**: Visualização do histórico de execução dos jobs Spark.

- **AWS Step Functions**: Orquestração da Pipeline

# Guia Prático do projeto

### 1° Inicio criando os buckets no Amazon S3 para o processamento dos dados

- Landing.zo
- Processing.zo
- Curated.zo

**Desmarque o bloqueio ao acesso público. Por padrão, a AWS vai perguntar se você realmente quer fazer isso por questões de segurança. Mas, como esse bucket será usado somente para esse projeto e não para um ambiente de produção, não tem problema confiar e marcar a opção**.
