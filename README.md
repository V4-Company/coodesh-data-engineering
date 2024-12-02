# Teste Prático para Engenheiro e Analista de Dados

### Cenário

Uma empresa está migrando dados de um sistema legado (representado por um banco de dados SQL) para sua nova arquitetura baseada em AWS. Você precisa criar um pipeline de dados que extraia informações do banco de dados, transforme-as e as carregue no Data Lake (S3).

### Tarefas

1. **Extração de Dados**
   - Escreva um script Python que se conecte a um banco de dados SQL (pode usar SQLite para simplificar) e extraia dados de uma tabela de vendas.

2. **Transformação**
   - Transforme os dados extraídos:
     - Converta as datas para o formato ISO.
     - Calcule o total de vendas por dia.
     - Remova quaisquer dados duplicados.

3. **Carregamento**
   - Simule o carregamento dos dados transformados para um "bucket S3" (pode ser um diretório local).
   - Organize os dados em uma estrutura de partições por ano/mês/dia.

4. **Consulta e Análise**
   - Escreva uma consulta SQL que poderia ser executada no Amazon Athena para analisar as vendas totais por mês.

5. **Visualização**
   - Crie um esboço de dashboard (pode ser um mock-up ou descrição detalhada) que apresentaria os insights chave dos dados de vendas.

6. **Bônus (se o tempo permitir)**
   - Proponha um modelo de machine learning simples que poderia ser implementado com Amazon SageMaker para prever vendas futuras.

### Entrega

- Código Python para extração, transformação e carregamento.
- Script SQL para a consulta no Athena.
- Descrição ou mock-up do dashboard.
- Documento explicando suas decisões de design e como sua solução se alinha com a arquitetura moderna de dados.

### Critérios de Avaliação

- Qualidade e eficiência do código.
- Habilidade em transformar e analisar dados.
- Conhecimento de serviços AWS relevantes.
- Capacidade de pensar em escalabilidade e manutenção.
- Clareza na comunicação de ideias e soluções.

### Dicas

- Priorize a funcionalidade sobre a perfeição. É melhor entregar uma solução funcional completa do que uma solução parcial perfeita.
- Comente seu código para explicar decisões importantes e suposições feitas.
- Se não tiver certeza sobre algum detalhe da implementação, declare suas suposições no documento de explicação.
- Lembre-se de considerar aspectos como tratamento de erros, logging e performance, mesmo que não os implemente completamente devido ao limite de tempo.

### Recursos Permitidos

- Você pode usar qualquer documentação online pública de Python, SQL, e serviços AWS.
- Não é permitido o uso de códigos pré-escritos ou gerados por IA para as tarefas específicas do teste.

# Modelo de Dados para Teste Prático

## Tabela: vendas

Esta tabela representa as transações de vendas da empresa.

| Coluna          | Tipo         | Descrição                                   |
|-----------------|--------------|---------------------------------------------|
| id              | INTEGER      | Identificador único da venda (Chave Primária)|
| data_venda      | DATE         | Data em que a venda foi realizada           |
| id_produto      | INTEGER      | Identificador do produto vendido            |
| id_cliente      | INTEGER      | Identificador do cliente que fez a compra   |
| quantidade      | INTEGER      | Quantidade de itens vendidos                |
| valor_unitario  | DECIMAL(10,2)| Preço unitário do produto                   |
| valor_total     | DECIMAL(10,2)| Valor total da venda (quantidade * valor_unitário) |
| id_vendedor     | INTEGER      | Identificador do vendedor responsável       |
| regiao          | VARCHAR(50)  | Região onde a venda foi realizada           |

## Dados de Exemplo

Aqui estão alguns registros de exemplo para a tabela `vendas`:

```sql
INSERT INTO vendas (id, data_venda, id_produto, id_cliente, quantidade, valor_unitario, valor_total, id_vendedor, regiao)
VALUES 
(1, '2023-01-15', 101, 1001, 2, 50.00, 100.00, 5, 'Sul'),
(2, '2023-01-16', 102, 1002, 1, 75.50, 75.50, 6, 'Sudeste'),
(3, '2023-01-16', 103, 1003, 3, 25.00, 75.00, 5, 'Sul'),
(4, '2023-01-17', 101, 1004, 1, 50.00, 50.00, 7, 'Norte'),
(5, '2023-01-17', 104, 1002, 2, 30.00, 60.00, 6, 'Sudeste'),
(6, '2023-01-18', 102, 1005, 1, 75.50, 75.50, 8, 'Nordeste'),
(7, '2023-01-18', 103, 1001, 2, 25.00, 50.00, 5, 'Sul'),
(8, '2023-01-19', 104, 1003, 4, 30.00, 120.00, 7, 'Norte'),
(9, '2023-01-19', 101, 1002, 1, 50.00, 50.00, 6, 'Sudeste'),
(10, '2023-01-20', 102, 1004, 2, 75.50, 151.00, 8, 'Nordeste');
```

## Notas para os Candidatos

1. Este modelo de dados representa uma versão simplificada de um sistema de vendas. Na prática, você poderia ter tabelas adicionais para produtos, clientes e vendedores.

2. Para o propósito deste teste, você pode assumir que esta é a única tabela que você precisa trabalhar.

3. Você pode usar este esquema e os dados de exemplo para criar seu banco de dados SQLite para a parte de extração do teste.

4. Lembre-se de que, ao transformar os dados, você precisará converter as datas para o formato ISO, calcular o total de vendas por dia e remover quaisquer duplicatas (embora não haja duplicatas neste conjunto de dados de exemplo).

5. Ao simular o carregamento para um "bucket S3", considere como você organizaria esses dados em uma estrutura de partições por ano/mês/dia.

# Massa de Dados Ampliada para Teste Prático

Este arquivo contém uma amostra maior de dados de vendas, com 500 registros. Os dados foram gerados aleatoriamente, mas seguem padrões realistas de vendas.

## Script Python para Geração dos Dados

Aqui está o script Python usado para gerar os dados:

```python
import random
from datetime import datetime, timedelta

def generate_sales_data(num_records=500):
    start_date = datetime(2023, 1, 1)
    end_date = datetime(2023, 12, 31)
    products = list(range(101, 111))  # 10 produtos
    regions = ['Norte', 'Sul', 'Leste', 'Oeste', 'Centro']
    
    sales_data = []
    for i in range(1, num_records + 1):
        sale_date = start_date + timedelta(days=random.randint(0, (end_date - start_date).days))
        product_id = random.choice(products)
        customer_id = random.randint(1001, 2000)
        quantity = random.randint(1, 10)
        unit_price = round(random.uniform(10.0, 100.0), 2)
        total_value = round(quantity * unit_price, 2)
        seller_id = random.randint(1, 20)
        region = random.choice(regions)
        
        sale = (i, sale_date.strftime('%Y-%m-%d'), product_id, customer_id, quantity, unit_price, total_value, seller_id, region)
        sales_data.append(sale)
    
    return sales_data

# Gerar 500 registros de vendas
sales_data = generate_sales_data(500)

# Imprimir os INSERTs SQL
print("INSERT INTO vendas (id, data_venda, id_produto, id_cliente, quantidade, valor_unitario, valor_total, id_vendedor, regiao) VALUES")
for i, sale in enumerate(sales_data):
    print(f"({sale[0]}, '{sale[1]}', {sale[2]}, {sale[3]}, {sale[4]}, {sale[5]:.2f}, {sale[6]:.2f}, {sale[7]}, '{sale[8]}')", end='')
    if i < len(sales_data) - 1:
        print(",")
    else:
        print(";")
```

## Dados de Exemplo (Primeiros 20 registros)

Aqui estão os primeiros 20 registros da massa de dados gerada. O conjunto completo contém 500 registros.

```sql
INSERT INTO vendas (id, data_venda, id_produto, id_cliente, quantidade, valor_unitario, valor_total, id_vendedor, regiao) VALUES
(1, '2023-07-13', 108, 1820, 8, 50.64, 405.12, 2, 'Centro'),
(2, '2023-07-30', 104, 1290, 8, 35.21, 281.68, 18, 'Oeste'),
(3, '2023-09-13', 102, 1078, 4, 31.20, 124.80, 7, 'Centro'),
(4, '2023-05-15', 101, 1958, 8, 63.33, 506.64, 14, 'Leste'),
(5, '2023-03-22', 107, 1891, 1, 45.58, 45.58, 7, 'Leste'),
(6, '2023-02-20', 101, 1733, 6, 66.55, 399.30, 3, 'Oeste'),
(7, '2023-11-24', 106, 1115, 6, 70.34, 422.04, 19, 'Centro'),
(8, '2023-04-11', 109, 1340, 9, 49.69, 447.21, 12, 'Norte'),
(9, '2023-10-08', 104, 1412, 1, 68.71, 68.71, 11, 'Oeste'),
(10, '2023-02-02', 106, 1471, 3, 14.93, 44.79, 6, 'Oeste'),
(11, '2023-04-21', 105, 1968, 8, 33.50, 268.00, 4, 'Oeste'),
(12, '2023-01-13', 107, 1615, 10, 81.50, 815.00, 17, 'Leste'),
(13, '2023-09-03', 106, 1140, 3, 26.72, 80.16, 10, 'Sul'),
(14, '2023-04-30', 105, 1306, 5, 96.39, 481.95, 20, 'Centro'),
(15, '2023-01-21', 108, 1772, 5, 39.25, 196.25, 1, 'Norte'),
(16, '2023-05-31', 110, 1226, 7, 79.47, 556.29, 14, 'Oeste'),
(17, '2023-08-20', 104, 1668, 3, 49.43, 148.29, 13, 'Leste'),
(18, '2023-09-23', 105, 1879, 1, 61.15, 61.15, 16, 'Norte'),
(19, '2023-03-05', 101, 1895, 2, 27.39, 54.78, 8, 'Centro'),
(20, '2023-05-24', 109, 1608, 8, 36.92, 295.36, 9, 'Sul'),
-- ... (480 registros adicionais)
```

## Notas para os Candidatos

1. Este conjunto de dados contém 500 registros de vendas gerados aleatoriamente para o ano de 2023.

2. Os dados incluem variações realistas em termos de datas, produtos, quantidades, preços e regiões.

3. Use este conjunto de dados para criar seu banco de dados SQLite para a parte de extração do teste.

4. Ao trabalhar com estes dados, você terá a oportunidade de demonstrar suas habilidades em lidar com um volume maior de informações, o que é mais próximo de cenários reais de engenharia e análise de dados.

5. Lembre-se de que, ao transformar os dados, você precisará converter as datas para o formato ISO, calcular o total de vendas por dia e estar preparado para lidar com possíveis duplicatas.

6. Ao simular o carregamento para um "bucket S3", considere como você organizaria eficientemente estes 500 registros em uma estrutura de partições por ano/mês/dia.

Boa sorte!


[LINK DA RESPOSTA]