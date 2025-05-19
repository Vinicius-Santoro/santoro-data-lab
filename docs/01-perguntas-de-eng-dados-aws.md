# **Perguntas de Engenharia de Dados na AWS**

## **1. Conceitos Fundamentais de Engenharia de Dados**

### **1.1 Como você lidaria com dados em tempo real vs. dados em batch?**

<span class="texto-verde">**Dados em Tempo Real**</span></span>

Esses dados são processados à medida que chegam, com baixa latência. Para isso, na AWS, podemos usar:

- **Amazon Kinesis Data Streams:** Para coletar e processar grandes fluxos de dados em tempo real, como logs de eventos de aplicativos ou transações financeiras.
- **AWS Lambda:** Para processamento serverless de eventos em tempo real, ideal para transformar dados conforme eles chegam.
- **Amazon DynamoDB Streams:** Para capturar mudanças em uma tabela do DynamoDB em tempo real e processá-las automaticamente.

Exemplo: gitsync e apisybc.

<span class="texto-verde">**Dados em Batch**</span>

Esses dados são processados em intervalos de tempo específicos. Na AWS, os principais serviços são:

- **Amazon S3 + AWS Glue**: Para armazenar grandes volumes de dados e preparar (ETL) esses dados para análise.
- **Amazon EMR (Elastic MapReduce)**: Para processar grandes volumes de dados usando frameworks como Apache Spark ou Hadoop.
- **Amazon Athena**: Para consultar diretamente dados armazenados no S3, sem necessidade de configuração de servidor.
- **Amazon RDS/Aurora**: Para processar e analisar dados armazenados em bancos de dados relacionais em horários específicos.
- **AWS Batch**: Para gerenciar e escalar automaticamente jobs em batch.

Exemplo prático: Em uma empresa de análise de marketing, você pode armazenar todos os dados de campanhas publicitárias em Amazon S3, usar o AWS Glue para transformar esses dados e o Amazon Athena para gerar relatórios diários de performance.

### **1.2 Qual a diferença do Amazon RDS para Amazon Redshift?**

<span class="texto-verde">**Amazon RDS**</span>

* **Tipo:** Banco de dados relacional gerenciado.
* **Uso:** Otimizado para transações OLTP (Online Transaction Processing).
* **Exemplos de Bancos Suportados:** MySQL, PostgreSQL, MariaDB, SQL Server, Oracle e Amazon Aurora.
* **Casos de Uso:** Aplicações web, sistemas de gestão empresarial, aplicativos móveis, e-commerce.

<span class="texto-verde">**Amazon Redshift**</span>

* **Tipo:** Data Warehouse gerenciado.
* **Uso:** Otimizado para consultas analíticas (OLAP - Online Analytical Processing).
* **Armazenamento:** Baseado em colunas, ideal para grandes volumes de dados.
* **Casos de Uso:** Análise de dados em larga escala, relatórios de BI (Business Intelligence).

## 2. Cloud Computing na AWS

### **2.1 Quais são os principais serviços da AWS para engenharia de dados?**

* **Amazon S3:** Armazenamento escalável de objetos.
* **AWS Glue:** Serviço de ETL gerenciado.
* **Amazon Redshift:** Data warehouse em nuvem.
* **Amazon Athena:** Consulta de dados em S3 usando SQL.
* **Amazon EMR:** Cluster gerenciado para Big Data.
* **Amazon Kinesis:** Ingestão e processamento de dados em tempo real.
* **AWS Lambda:** Execução de código sem servidor.

### **2.2 Quais são as diferenças entre o AWS Glue e o Amazon EMR na AWS para processamento de big data?**

Tanto o **AWS Glue** quanto o **Amazon EMR (Elastic MapReduce)** são serviços da AWS para **processamento de big data**, mas eles possuem características e casos de uso específicos.

<span class="texto-verde">**AWS Glue**</span>

* **Tipo:** Serviço de ETL (Extract, Transform, Load) totalmente gerenciado.
* **Interface:** Baseada em servidorless, sem necessidade de gerenciamento de infraestrutura.
* **Casos de Uso:** Transformação de dados, preparação de dados para análise, integração com Amazon S3 e Amazon Athena.
* **Linguagem Suportada:** PySpark (Python + Spark) e Scala.
* **Principais Funcionalidades:**

  * Glue Data Catalog para metadados e esquema dos dados.
  * Glue Studio com interface gráfica para criar pipelines ETL.
  * Glue Crawlers para descoberta automática de esquemas de dados.
* **Escalabilidade:** Automática (você não gerencia nós).
* **Modelo de Custo:** Paga-se por segundo de execução.

**Exemplo Prático:** Você precisa transformar arquivos JSON armazenados no Amazon S3 em tabelas Parquet otimizadas para consulta com Amazon Athena.

<span class="texto-verde">**Amazon EMR (Elastic MapReduce)**</span>

* **Tipo:** Cluster gerenciado para frameworks de big data (Hadoop, Spark, Presto, HBase, Flink, e mais).
* **Interface:** Controle completo do cluster (EC2 gerenciado) e configuração personalizada.
* **Casos de Uso:** Processamento intensivo de big data, machine learning distribuído, análise de logs, e pipelines de dados complexos.
* **Linguagem Suportada:** Python (Spark), Scala, Java, SQL, R, e mais, dependendo do framework.
* **Principais Funcionalidades:**

  * Suporte para múltiplos frameworks (Hadoop, Spark, Presto, etc.).
  * Controle total sobre o hardware (tipo de instância, número de nós, configuração de memória/CPU).
  * Suporte para workloads altamente personalizadas.
* **Escalabilidade:** Manual ou automática (Auto Scaling).
* **Modelo de Custo:** Paga-se por instância EC2 e armazenamento associado.

**Exemplo Prático:** Você precisa treinar um modelo de machine learning em Spark MLlib com um cluster de 100 nós para análise de logs de servidores.

<span class="texto-verde">**Quando usar cada um?**</span>

| Critério           |  **AWS Glue**                                     |  **Amazon EMR**                                                 |
| ------------------ | --------------------------------------------------- | ----------------------------------------------------------------- |
| **Simplicidade**   | Simples, serverless.                                | Complexo, com controle total do cluster.                          |
| **Custo**          | Custo controlado (serverless).                      | Pode ser mais caro (depende do tamanho do cluster).               |
| **Flexibilidade**  | Limitado a ETL (Spark, Scala, Python).              | Altamente flexível (vários frameworks).                           |
| **Casos de Uso**   | ETL, preparação de dados, integração com S3/Athena. | Processamento de big data, machine learning, pipelines complexos. |
| **Gerenciamento**  | Automático (serverless).                            | Manual (você gerencia os nós).                                    |
| **Escalabilidade** | Automática.                                         | Manual ou automática (Auto Scaling).                              |

<span class="texto-verde">**Como escolher?**</span>

* Use **AWS Glue** se você precisa de uma solução de ETL fácil de configurar e gerenciada para transformar e preparar dados.
* Use **Amazon EMR** se você precisa de controle total sobre o cluster e suporte para frameworks adicionais (como Hadoop, Spark MLlib, Presto).


### **2.3 Como você garantiria a segurança dos dados armazenados no S3?**

Para garantir a segurança dos dados armazenados no Amazon S3, podemos adotar as seguintes práticas:

<span class="texto-verde">**Controle de Acesso**</span>

* **Políticas do IAM:** Use políticas de controle de acesso refinadas para usuários e grupos.
* **Bucket Policies:** Defina políticas específicas de acesso ao bucket, limitando quem pode visualizar ou modificar os dados.
* **Access Control Lists (ACLs):** Configure permissões em nível de objeto, se necessário.

<span class="texto-verde">**Criptografia**</span>

* **Criptografia em repouso:** Habilite a criptografia automática com AWS Key Management Service (KMS) ou chaves gerenciadas pelo S3 (SSE-S3).
* **Criptografia em trânsito:** Use HTTPS (TLS) para transferir dados de forma segura.

<span class="texto-verde">**Monitoramento e Auditoria**</span>

* **Amazon CloudTrail:** Ative o CloudTrail para registrar todas as ações realizadas no S3, garantindo rastreabilidade.
* **Amazon Macie:** Identifique dados confidenciais e receba alertas sobre possíveis exposições.
* **S3 Access Logs:** Habilite o registro de acessos para analisar quem acessou o bucket.

<span class="texto-verde">**Proteção Contra Exclusão**</span>

* **Versionamento:** Ative o versionamento para manter cópias antigas dos objetos.
* **Bloqueio de Objetos:** Use o S3 Object Lock para proteger objetos contra exclusão acidental.
* **MFA Delete:** Exija autenticação multifator para exclusão de objetos críticos.

<span class="texto-verde">**Melhores Práticas Adicionais**</span>

* **Princípio de Menor Privilégio:** Conceda apenas as permissões necessárias para cada usuário.
* **Segregação de Buckets:** Mantenha dados sensíveis e públicos em buckets separados.
* **Avaliação Regular:** Realize auditorias periódicas para garantir que as permissões estão configuradas corretamente.

### **2.4 Como você configuraria um pipeline de ingestão de dados usando Glue e S3?**

* Configuraria uma tarefa do Glue para extrair dados de uma fonte (como RDS), transformá-los e carregá-los no S3.
* Definiria gatilhos para automação (horário, eventos S3).

## 3. Design e Arquitetura de Dados

### **3.1 Como você projetaria uma solução de ingestão de dados escalável na AWS?**

* Utilizaria Amazon S3 para armazenamento bruto.
* AWS Glue para transformação de dados.
* Amazon Kinesis para ingestão em tempo real.

### **3.2 Como você projetaria uma arquitetura para processar dados em tempo real?**

* Utilizaria Amazon Kinesis para ingestão.
* AWS Lambda para processamento em tempo real.
* Amazon DynamoDB ou S3 para armazenamento.

### **3.3 Quais são as boas práticas para particionar e organizar dados em um data lake no S3?**

* Usar prefixos de partição (por exemplo, ano/mês/dia).
* Definir políticas de ciclo de vida para arquivamento.
* Manter uma estrutura de pasta padronizada.

## 4. Performance e Otimização

### **4.1 Como otimizar uma consulta no Redshift?**

* Uso de sort keys e distribution keys.
* Compressão de colunas.
* Materialized views.

<span class="texto-verde">**O que são Sort Keys e Distribution Keys?**</span>

* **Sort Keys:** Colunas usadas para ordenar os dados fisicamente na tabela, otimizando a velocidade de leitura para consultas frequentes que filtram ou ordenam por essas colunas.

  * **Exemplo:** Se você usar uma coluna `anomesdia` como sort key e ordená-la de forma crescente, o Redshift organizará os dados fisicamente nessa ordem. Consultas filtrando por datas específicas serão muito mais rápidas, pois o Redshift lerá diretamente o intervalo necessário.

* **Distribution Keys:** Definem como os dados são distribuídos entre os nós do cluster Redshift, garantindo que consultas sejam mais eficientes, minimizando o movimento de dados entre nós.

<span class="texto-verde">**O que é Compressão de Colunas?**</span>

É a aplicação de algoritmos de compressão (como LZO, ZSTD) nas colunas da tabela, reduzindo o espaço de armazenamento e acelerando a leitura dos dados. Existem dois tipos principais.

<span class="texto-verde">**Compressão sem perda (Lossless Compression):**</span>

- Não perde nenhum dado durante a compressã
- Ideal para dados críticos (textos, códigos, tabelas).
- Exemplo: Algoritmo LZO (Lempel-Ziv-Oberhumer) e ZSTD (Zstandard), muito usados em bancos de dados como Redshift.

<span class="texto-verde">**Como Funciona:**</span>

- Identifica padrões repetidos nos dados.
- Substitui padrões por referências menores.
- Ao descomprimir, restaura os dados originais exatamente como eram.

!!! tip "Exemplo"

    **Dados Originais:** AAAAAABBBCCCCCC

    **Após Compressão:** A6B3C6

<span class="texto-verde">**Compressão com perda (Lossy Compression):**</span>

- Perde alguns dados durante a compressão.
- Usada em imagens, vídeos e áudio (onde uma pequena perda de qualidade é aceitável).
- Exemplo: JPEG (imagens), MP3 (áudio).

<span class="texto-verde">**O que são Materialized Views?**</span>

* São visualizações que armazenam os resultados de uma consulta em cache, permitindo acesso mais rápido aos dados processados, pois evitam a necessidade de reexecutar a consulta toda vez.


### **4.2 Como melhorar o desempenho de uma pipeline de ETL no Glue?**

* Utilizando "pushdown predicate" para filtrar dados.
* Habilitando a opção de paralelismo dinâmico.

!!! tip "pushdown predicate"

    **Pushdown Predicate** é uma técnica de otimização em pipelines de ETL que pode ser aplicado no Glue. Em vez de carregar todos os dados para processamento e aplicar filtros depois, o Pushdown Predicate aplica os filtros diretamente na fonte de dados. Isso reduz a quantidade de dados transferidos e processados, acelerando o ETL.

    O filtro sempre é feito, mas:

    - <span class="texto-vermelho">**Sem pushdown**</span>, o Glue poderia ler todos os dados da fonte e só depois aplicar o filtro, o que é custoso.

    - <span class="texto-verde">**Com pushdown**</span>, o filtro é “empurrado para baixo” até a fonte, ou seja, o banco de dados (ou sistema de arquivos) já retorna apenas os </span>dados filtrados.

### **4.3 O que é particionamento de dados e como ele ajuda no desempenho?**

* Particionamento é a divisão dos dados em segmentos menores.
* Melhora o desempenho de consultas ao permitir leitura seletiva.

## 5. Monitoramento e Segurança

### **5.1 Como você configuraria políticas de acesso para proteger seus dados no S3?**

* Usando políticas de bucket e políticas de IAM.
* Ativando o bloqueio de acesso público.

### **5.2 Quais são as melhores práticas para monitorar uma pipeline de dados na AWS?**

* Usando Amazon CloudWatch para métricas e alarmes.
* Habilitando o logging detalhado no Glue.

### **5.3 Como você implementaria controle de versão em uma pipeline de dados?**

* Usando versionamento em S3.
* Gerenciando versões de código com Git.

## 6. Perguntas Práticas e Situações de Problemas

### **6.1 Como você identificaria e corrigiria falhas em uma pipeline de dados em produção?**

<span class="texto-verde">**Identificação de Falhas**</span>

**Monitoramento Contínuo:**

   - Use **Amazon CloudWatch** para monitorar métricas e criar alarmes que notificam quando há falhas, lentidão ou comportamentos anormais na pipeline.
   - Habilite logs detalhados nos serviços envolvidos (por exemplo, logs do AWS Glue, AWS Lambda, Kinesis, etc.).
   - Utilize dashboards para visualizar o status da pipeline em tempo real.

**Alertas e Notificações:**

   - Configure alertas via SNS (Simple Notification Service) ou outras ferramentas para receber notificações imediatas sobre falhas ou erros.

**Logs e Auditoria:**

   - Analise os logs para identificar mensagens de erro, exceções e padrões que possam indicar a origem do problema.
   - Use o **AWS CloudTrail** para rastrear ações feitas na infraestrutura que possam ter causado falhas.

<span class="texto-verde">**Correção de Falhas**</span>

**Diagnóstico:**

   - Identifique exatamente onde a falha ocorreu (ex: no processo de ingestão, transformação, carregamento, etc.).
   - Verifique dependências externas, como conexões com bancos de dados, permissões e quotas.

**Resolução:**

   - Se for um erro temporário (ex: timeout, falha de rede), pode ser suficiente reiniciar o job ou a função.
   - Corrija erros de configuração ou código conforme identificado nos logs.
   - Atualize permissões ou políticas de acesso se necessário.

**Testes e Validação:**

   - Execute testes localmente ou em ambiente de staging para garantir que o problema foi resolvido.
   - Monitore a pipeline após a correção para garantir que a falha não se repita.

<span class="texto-verde">**Boas Práticas**</span>

- Implementar **retry logic** e tratamento de erros dentro da pipeline.
- Usar **versionamento** para código e configurações para facilitar rollback.
- Ter um plano de recuperação e comunicação claro para incidentes.