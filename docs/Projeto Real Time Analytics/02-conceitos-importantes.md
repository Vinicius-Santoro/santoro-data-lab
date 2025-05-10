# **Conceitos Importantes**

Nesta seção, são apresentados os principais conceitos necessários para o
desenvolvimento do projeto. Eles fornecem uma base teórica para compreensão dos
métodos e tecnologias utilizadas.

### **Processamento em Streaming**
- **Definição:** Processa dados em tempo real, à medida que são gerados. 
- **Uso:** Ideal para aplicações que exigem respostas rápidas, como
monitoramento de transações financeiras, detecção de fraudes e análise em
tempo real.

### **Processamento em Batch**
- **Definição:** Processa grandes volumes de dados em intervalos programados. 
- **Uso:** Recomendado para tarefas que não exigem resposta imediata, como
relatórios financeiros mensais, análises históricas e agregação de grandes
conjuntos de dados.

### **Quando Utilizar Cada Um**
- **Streaming:** Quando a latência é crítica e os dados precisam ser processados
em tempo real.
- **Batch:** Quando o volume de dados é alto e o processamento em tempo real não
é necessário.

---

### **Produtores, Consumidores e Brokers**
- **Produtores:** Entidades ou aplicações que geram e enviam dados para o
sistema de streaminga (Sensores IoT, Aplicações Web).
- **Consumidores:** Entidades ou aplicações que recebem e processam os dados do
sistema de streaming (Dashboards em Tempo Real, Sistemas de Alerta).
- **Brokers:** Sistemas intermediários que recebem dados dos produtores e os
distribuem aos consumidores (Apache Kafka, AWS Kinesis e RabbitMQ.).

---

## **Tipos de Janelas em Aplicações de Streaming**

### Janela de Tempo (Time Window)
- **Definição:** Agrupa eventos com base em intervalos de tempo fixos. 
- **Exemplo:** Uma janela de 5 minutos coleta todos os eventos que ocorrem nesse
período.

### Janela de Contagem (Count Window)
- **Definição:** Agrupa eventos com base em uma quantidade fixa de eventos. 
- **Exemplo:** Uma janela de contagem de 100 eventos processa os primeiros 100
eventos e então inicia uma nova janela.

### Janela Tumbling (Tumbling Window)
- **Definição:** Uma janela de tempo fixa e não sobreposta. Cada intervalo é
independente e não compartilha eventos com outras janelas.
- **Exemplo:** Uma janela tumbling de 10 segundos cria uma nova janela a cada 10
segundos, sem sobreposição.

---

## **Tipos de Garantia de Entrega em Streaming**

### At-Most-Once (No máximo uma vez)
- **Definição:** Uma mensagem pode ser entregue no máximo uma vez. Se ocorrer uma falha durante o envio, a mensagem pode ser perdida.
- **Uso:** Ideal para casos onde a perda de mensagens é aceitável e a latência é crítica, como monitoramento de métricas não críticas.

### At-Least-Once (No mínimo uma vez)
- **Definição:** Garante que uma mensagem seja entregue pelo menos uma vez, mesmo que isso signifique entregá-la mais de uma vez em casos de falha.
- **Uso:** Recomendado para cenários em que a perda de mensagens não é aceitável, mesmo que ocorra duplicidade, como em sistemas de faturamento.

### Exactly-Once (Exatamente uma vez)
- **Definição:** Garante que cada mensagem seja entregue exatamente uma vez, sem duplicidade e sem perda.
- **Uso:** Essencial para aplicações críticas onde a precisão é fundamental, como em processamento de pagamentos e transações financeiras.

---

## **Estratégias de Escalabilidade em Sistemas Distribuídos**

### Partitioning (Particionamento)
- **Definição:** Divide os dados em diferentes partições com base em uma chave de particionamento, permitindo que sejam distribuídos em diferentes nós.
- **Uso:** Comum em bancos de dados distribuídos e sistemas de filas, como Kafka e DynamoDB.
- **Vantagem:** Distribui a carga de trabalho, permitindo processamento paralelo e maior desempenho.
- **Desvantagem:** A escolha da chave de particionamento é crítica. Uma chave mal escolhida pode gerar partições desbalanceadas, causando gargalos.

### Sharding (Fragmentação)
- **Definição:** Uma forma específica de particionamento, onde os dados são divididos em "shards" (fragmentos), cada um armazenado em um nó separado.
- **Uso:** Comum em bancos de dados relacionais e NoSQL, como MongoDB e MySQL.
- **Vantagem:** Permite escalar horizontalmente o banco de dados, mantendo o desempenho.
- **Desvantagem:** A complexidade na gestão dos shards aumenta, especialmente na reconfiguração e migração de dados entre shards.

### Replicação
- **Definição:** Mantém múltiplas cópias dos dados em diferentes nós, garantindo alta disponibilidade e tolerância a falhas.
- **Uso:** Amplamente utilizado em bancos de dados distribuídos, como PostgreSQL e Cassandra.
- **Vantagem:** Melhora a disponibilidade e a resiliência do sistema, permitindo recuperação em caso de falhas.
- **Desvantagem:** Aumenta o consumo de armazenamento e o custo de rede. Sincronizar múltiplas cópias pode ser complexo.

### Auto Scaling
- **Definição:** Ajusta automaticamente os recursos de acordo com a demanda, aumentando ou diminuindo o número de instâncias ou capacidade de acordo com o uso.
- **Uso:** Comum em plataformas de nuvem, como AWS Auto Scaling, Kubernetes HPA (Horizontal Pod Autoscaler).
- **Vantagem:** Garante que os recursos sejam utilizados de forma eficiente, evitando custos desnecessários e mantendo o desempenho.
- **Desvantagem:** Pode gerar custos inesperados se não estiver configurado corretamente. Depende de métricas precisas para funcionar de forma eficaz.

---

## **Estratégias de Tolerância a Falhas em Sistemas Distribuídos**

### Replicação de Dados
- **Definição:** Mantém múltiplas cópias dos dados em diferentes nós ou regiões, garantindo que estejam disponíveis mesmo em caso de falha de um dos nós.
- **Uso:** Comum em bancos de dados distribuídos como Cassandra, MongoDB e sistemas de armazenamento em nuvem como Amazon S3.
- **Vantagem:** Melhora a disponibilidade e a resiliência dos dados.
- **Desvantagem:** Aumenta o custo de armazenamento e pode gerar inconsistências em sistemas que não possuem replicação síncrona.

### Checkpoints
- **Definição:** Armazena periodicamente o estado de um sistema, permitindo que ele seja restaurado a partir desse ponto em caso de falha.
- **Uso:** Comum em sistemas de processamento em streaming, como Apache Flink e Spark Streaming.
- **Vantagem:** Minimiza a perda de dados e o tempo de recuperação após uma falha.
- **Desvantagem:** Pode consumir recursos adicionais de armazenamento e processamento. A frequência dos checkpoints afeta o desempenho.

### Failover Automático
- **Definição:** Redireciona automaticamente o tráfego ou a carga de trabalho para uma instância de backup em caso de falha da instância primária.
- **Uso:** Comum em servidores de aplicação, bancos de dados distribuídos e sistemas de balanceamento de carga.
- **Vantagem:** Garante alta disponibilidade e continuidade do serviço sem intervenção manual.
- **Desvantagem:** A configuração inadequada pode resultar em tempo de inatividade. Requer monitoramento constante e infraestrutura adicional.

### Processamento Idempotente
- **Definição:** Garante que uma operação pode ser aplicada várias vezes sem alterar o resultado final, evitando duplicidade de processamento.
- **Uso:** Comum em sistemas de APIs, processamento de mensagens (como Kafka) e transações financeiras.
- **Vantagem:** Evita efeitos indesejados em caso de reenvio ou reprocessamento de mensagens.
- **Desvantagem:** Requer design cuidadoso das operações para garantir idempotência. Pode aumentar a complexidade do desenvolvimento.
