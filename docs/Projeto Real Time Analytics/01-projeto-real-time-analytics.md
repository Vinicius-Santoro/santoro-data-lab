# **Objetivos**

## **Objetivo Geral**
O objetivo desse projeto é desenvolver um pipeline que consome dados de uma API,
emite alertas em tempo real e armazena esses dados de maneira eficiente e
organizada. Utilizando um conjunto de serviços da AWS, será possível
integrar e orquestrar funções lambda, Kinesis, SNS, roles IAM, S3, Glue
(ETL, catálogo e crawler), Athena e CloudWatch para criar uma solução completa
e escalável de análise de dados em tempo real.

## **Objetivos Específicos**
- **Configuração e integração de funções lambda e Kinesis** para ingestão e
processamento de dados em tempo real.

- **Uso de SNS para alertas em tempo real**, garantindo que as informações
críticas cheguem rapidamente aos destinatários corretos.

- **Gerenciamento de roles IAM** para garantir segurança e controle de acesso.

- **Armazenamento de dados no S3** em duas áreas distintas: raw, para dados
brutos, e gold, para dados transformados e estruturados em formato parquet.

- **ETL com Glue, incluindo criação de catálogos e utilização de crawlers** para
organizar e transformar dados de maneira eficiente.

- **Consultas ad-hoc com Athena** para análise rápida e flexível dos dados
armazenados.

- **Monitoramento e logging com CloudWatch**, assegurando que todo o processo
seja monitorado e quaisquer problemas sejam rapidamente identificados e resolvidos.