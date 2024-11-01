# Pipeline de Dados do Telegram

Este projeto implementa um pipeline de dados completo para capturar, processar e analisar
mensagens enviadas a um chatbot no Telegram, utilizando a infraestrutura da AWS. O objetivo é
coletar dados de conversas para realizar análises que apoiem o aprimoramento do atendimento e
da interação automatizada com os usuários.

# Estrutura do Projeto

O pipeline é dividido em três etapas principais:

1. Ingestão de Dados: Mensagens capturadas pelo bot do Telegram são enviadas para a AWS via
webhook e armazenadas em um bucket S3 no formato original (JSON).

2 . ETL (Extract, Transform, Load): Diariamente, uma função AWS Lambda processa os dados
brutos, transformando-os para um formato orientado a colunas (Parquet) e os armazena em uma
camada enriquecida no S3.

3 . Apresentação dos Dados: Os dados transformados são disponibilizados para consulta via AWS
Athena, permitindo que analistas utilizem SQL para extrair insights diretamente da camada de
dados refinados.

# Tecnologias Utilizadas

- Telegram Bot API: Captura e envio de mensagens do Telegram.
  
- AWS API Gateway: Recepção de mensagens via webhook.
  
- AWS Lambda: Processamento e transformação de dados.
  
- AWS S3: Armazenamento de dados em camadas (raw e enriched).
  
- AWS Athena: Consulta e análise dos dados processados.

# Configuração do Ambiente

## 1. Criar o Bot no Telegram: 

- Inicie uma conversa com o BotFather no Telegram e crie um novo bot, salvando o token gerado.

## 2. Configuração da AWS:

- S3: Crie dois buckets no S3 (ex: telegram-raw e telegram-enriched) para armazenar os dados
brutos e processados.

- Lambda: Crie funções Lambda para a ingestão e o processamento (ETL) dos dados.
  
- API Gateway: Configure uma API REST que ative a função Lambda de ingestão para receber os
dados via webhook.

- Athena: Configure uma tabela externa apontando para o bucket telegram-enriched para consulta
SQL dos dados processados.

## 3. Configuração das Variáveis de Ambiente:

- AWS Lambda: Defina as variáveis AWS_S3_BUCKET (bucket de dados brutos) e TELEGRAM_CHAT_ID (ID do grupo Telegram) na função Lambda.

## 4. Permissões IAM: 

- Conceda permissões para que a função Lambda possa ler e gravar nos buckets S3.
   
## Estrutura do Código


- lambda_ingestao.py: Função Lambda que recebe e armazena as mensagens do Telegram no S3.
  
- lambda_etl.py: Função Lambda que realiza o ETL, transformando e movendo os dados para a camada enriched no S3.

- sql_queries.sql: Exemplo de consultas SQL para análise dos dados no AWS Athena.
  
- configuracoes_telegram.py: Configurações e métodos de interação com a API do Telegram, incluindo o webhook.


## Exemplo de Uso


1. Ingestão: Envie mensagens para o bot no Telegram e verifique se os dados são armazenados no bucket S3 na camada raw.

2. Processamento ETL: Execute a função Lambda de ETL para transformar os dados brutos e movê-los para a camada enriched.

3. Análise: Acesse o AWS Athena e use as consultas SQL para extrair insights dos dados processados.

## Consultas de Exemplo no Athena

- Quantidade de mensagens por dia:

  SELECT context_date, COUNT(1) AS message_count FROM telegram GROUP BY context_date
  ORDER BY context_date DESC;

- Média de tamanho das mensagens por usuário por dia:
  
  SELECT user_id, user_first_name, context_date, AVG(LENGTH(text)) AS avg_message_length
  FROM telegram GROUP BY user_id, user_first_name, context_date ORDER BY context_date
  DESC;

  ## Contribuição
  
Contribuições são bem-vindas! Sinta-se à vontade para abrir uma issue ou enviar um pull request.


