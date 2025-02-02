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

![1-Create S3 bucket](https://github.com/user-attachments/assets/cf2c987a-d1e8-4957-82bc-1fc5deab380e)

### 2° Faço o upload dos arquivos que eu já extrair da nossa fonte de dados diretamente para o bucket **Landing.zo**

![2-upload file](https://github.com/user-attachments/assets/ebe14d4f-a7ea-4e52-b776-21b442110b65)

### 3° Crio um cluster no Amazon EMR
- **Name**: Coloco o nome do meu cluster 'pt-data-cluster'.
- **Amazon EMR release**:  Escolho a versão EMR 7.6 e deixo os pacotes de aplicação como default.
- **Cluster configuration**: Deixo selecionado o campo **Uniform instance** opção que oferece facilidade, estabilidade enquanto a **Flexible Instance Fleets** Permite otimizar custos e desempenho.
- **Primary**: Escolho a instância EC2 que irá atuar como nó mestre, responsável por gerenciar o cluster.
- **Core Instance**: Define os nós principais que processarão os dados. Esses nós armazenam e executam os workloads do cluster.
- **Cluster Scaling and Provisioning**: Seleciono **Set cluster size manually** que me permite definir o tamanho do cluster de forma manual, assim evitando escalonamento automático e custos inesperados.
- **Provisioning Configuration**: Defino 3 instâncias para o meu uso. Assim, aumentando minha escalabilidade, tolerância a falhas e desempenho.
- **Virtual Private Cloud**: Define a rede onde o cluster roda, controlando segurança, acesso e conectividade com outros serviços da AWS. Normalmente, já vem uma VPC padrão quando se cria uma conta na AWS, mas você pode criar uma que mais se encaixar no seu uso.
- **Security Configuration and EC2 Key Pair**: Serve para acessar as instâncias do cluster via SSH. 
- **Identity and Access Management (IAM)**: Seleciono **Create a service role** e depois, em **Security group**, coloco a vpc que eu já selecionei acima em **Networking**. E permito o acesso a todos os meus buckets.
- **EC2 instance profile for Amazon EMR**: Escolho a opção **Create an instance profile** O EMR criará automaticamente uma IAM Role nova com permissões padrão, facilitando para quem não quer configurar a Role manualmente.

### Lembrando que há diversas outras possibilidades de configurações no cluster, mas aqui coloquei somente as que eu fiz algum tipo de alteração.

- ### Após esses processo, nosso cluster está pronto para ser criado.

![3-Create cluster](https://github.com/user-attachments/assets/cda75f3d-66ca-48a3-b5bf-6b1ebbbe3b7b)









