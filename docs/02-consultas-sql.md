# **Consultas SQL**
Esta seção tem como objetivo armazenar consultas SQL úteis, servindo tanto como material de estudo quanto referência prática para futuras necessidades.

## Extrair ID de ARN

**Objetivo:** Extrair os IDs dos recursos de API Gateway e nomes de funções Lambda presentes na tabela fictítica `base_cloudability`, identificando e categorizando os registros conforme o tipo.

<details>
<summary>Clique para visualizar a consulta de criação da tabela fictícia</summary>

```sql
CREATE TABLE base_cloudability (
    tipo_recurso VARCHAR(50),
    recurso TEXT
);

INSERT INTO base_cloudability (tipo_recurso, recurso) VALUES
('API Gateway', 'arn:aws:apigateway:us-east-1::/apis/aa11bb22cc/resources/abc123/method/GET'),
('API Gateway', 'arn:aws:apigateway:us-east-1::/apis/dd33ee44ff/resources/def456/method/POST'),
('API Gateway', 'arn:aws:apigateway:us-east-1::/restapis/gg55hh66ii/resources/ghi789/method/PUT'),
('API Gateway', 'arn:aws:apigateway:us-east-1::/restapis/jj77kk88ll/resources/jkl012/method/DELETE'),
('API Gateway', 'arn:aws:apigateway:us-east-1::/restapis/mm99nn00oo/resources/mno345/method/PATCH'),
('API Gateway', 'arn:aws:apigateway:us-east-1::/restapis/pp11qq22rr/resources/pqr678/method/GET'),
('API Gateway', 'arn:aws:apigateway:us-east-1::/restapis/ss33tt44uu/resources/stu901/method/POST'),
('API Gateway', 'arn:aws:apigateway:us-east-1::/restapis/vv55ww66xx/resources/vwx234/method/PUT'),
('API Gateway', 'arn:aws:apigateway:us-east-1::/restapis/yy77zz88aa/resources/yzb567/method/DELETE'),
('API Gateway', 'arn:aws:apigateway:us-east-1::/restapis/bb99cc00dd/resources/bcd890/method/PATCH'),

('Log API Gateway', 'arn:aws:logs:us-east-1:123456789012:log-group:API-Gateway-Execution-Logs_aa11bb22cc/prod'),
('Log API Gateway', 'arn:aws:logs:us-east-1:123456789012:log-group:API-Gateway-Execution-Logs_dd33ee44ff/prod'),
('Log API Gateway', 'arn:aws:logs:us-east-1:123456789012:log-group:API-Gateway-Execution-Logs_gg55hh66ii/prod'),
('Log API Gateway', 'arn:aws:logs:us-east-1:123456789012:log-group:API-Gateway-Execution-Logs_jj77kk88ll/prod'),
('Log API Gateway', 'arn:aws:logs:us-east-1:123456789012:log-group:API-Gateway-Execution-Logs_mm99nn00oo/prod'),
('Log API Gateway', 'arn:aws:logs:us-east-1:123456789012:log-group:api-gateway-access-logs_pp11qq22rr_prod'),
('Log API Gateway', 'arn:aws:logs:us-east-1:123456789012:log-group:api-gateway-access-logs_ss33tt44uu_prod'),
('Log API Gateway', 'arn:aws:logs:us-east-1:123456789012:log-group:api-gateway-access-logs_vv55ww66xx_prod'),
('Log API Gateway', 'arn:aws:logs:us-east-1:123456789012:log-group:api-gateway-access-logs_yy77zz88aa_prod'),
('Log API Gateway', 'arn:aws:logs:us-east-1:123456789012:log-group:api-gateway-access-logs_bb99cc00dd_prod'),

('Lambda Authorizer', 'arn:aws:lambda:us-east-1:123456789012:function:valida-token-api1'),
('Lambda Authorizer', 'arn:aws:lambda:us-east-1:123456789012:function:valida-token-api2'),
('Lambda Authorizer', 'arn:aws:lambda:us-east-1:123456789012:function:valida-token-api3'),
('Lambda Authorizer', 'arn:aws:lambda:us-east-1:123456789012:function:valida-token-api4'),
('Lambda Authorizer', 'arn:aws:lambda:us-east-1:123456789012:function:valida-token-api5'),
('Lambda Authorizer', 'arn:aws:lambda:us-east-1:123456789012:function:valida-token-api6'),
('Lambda Authorizer', 'arn:aws:lambda:us-east-1:123456789012:function:valida-token-api7'),
('Lambda Authorizer', 'arn:aws:lambda:us-east-1:123456789012:function:valida-token-api8'),
('Lambda Authorizer', 'arn:aws:lambda:us-east-1:123456789012:function:valida-token-api9'),
('Lambda Authorizer', 'arn:aws:lambda:us-east-1:123456789012:function:valida-token-api10'),

('Log Lambda Authorizer', 'arn:aws:logs:us-east-1:123456789012:log-group:/aws/lambda/valida-token-api1'),
('Log Lambda Authorizer', 'arn:aws:logs:us-east-1:123456789012:log-group:/aws/lambda/valida-token-api2'),
('Log Lambda Authorizer', 'arn:aws:logs:us-east-1:123456789012:log-group:/aws/lambda/valida-token-api3'),
('Log Lambda Authorizer', 'arn:aws:logs:us-east-1:123456789012:log-group:/aws/lambda/valida-token-api4'),
('Log Lambda Authorizer', 'arn:aws:logs:us-east-1:123456789012:log-group:/aws/lambda/valida-token-api5'),
('Log Lambda Authorizer', 'arn:aws:logs:us-east-1:123456789012:log-group:/aws/lambda/valida-token-api6'),
('Log Lambda Authorizer', 'arn:aws:logs:us-east-1:123456789012:log-group:/aws/lambda/valida-token-api7'),
('Log Lambda Authorizer', 'arn:aws:logs:us-east-1:123456789012:log-group:/aws/lambda/valida-token-api8'),
('Log Lambda Authorizer', 'arn:aws:logs:us-east-1:123456789012:log-group:/aws/lambda/valida-token-api9'),
('Log Lambda Authorizer', 'arn:aws:logs:us-east-1:123456789012:log-group:/aws/lambda/valida-token-api10');
```
</details>

### Consulta no PostgreSQL v17

```sql
WITH api_ids AS (
    SELECT
        recurso,
        COALESCE(
            substring(recurso FROM '/restapis/([a-z0-9]{10})'),
            substring(recurso FROM '/apis/([a-z0-9]{10})')
        ) AS id_arn,
        'API Gateway ID' AS tipo
    FROM base_cloudability
    WHERE recurso LIKE '%/restapis/%' OR recurso LIKE '%/apis/%'
),
lambda_names AS (
    SELECT
        recurso,
        COALESCE(
            substring(recurso FROM 'function:([^/]+)'),
            substring(recurso FROM '/aws/lambda/([^/]+)')
        ) AS id_arn,
        'Lambda Function Name' AS tipo
    FROM base_cloudability
    WHERE recurso LIKE '%function:%' OR recurso LIKE '%/aws/lambda/%'
)
SELECT * FROM api_ids
UNION ALL
SELECT * FROM lambda_names
```

### Consulta no AWS Athena

```sql
WITH api_ids AS (
    SELECT
        recurso,
        COALESCE(
            regexp_extract(recurso, '/restapis/([a-z0-9]{10})', 1),
            regexp_extract(recurso, '/apis/([a-z0-9]{10})', 1)
        ) AS id_arn,
        'API Gateway ID' AS tipo
    FROM base_cloudability
    WHERE recurso LIKE '%/restapis/%' OR recurso LIKE '%/apis/%'
),
lambda_names AS (
    SELECT
        recurso,
        COALESCE(
            regexp_extract(recurso, 'function:([^/]+)', 1),
            regexp_extract(recurso, '/aws/lambda/([^/]+)', 1)
        ) AS id_arn,
        'Lambda Function Name' AS tipo
    FROM base_cloudability
    WHERE recurso LIKE '%function:%' OR recurso LIKE '%/aws/lambda/%'
)
SELECT * FROM api_ids
UNION ALL
SELECT * FROM lambda_names
```