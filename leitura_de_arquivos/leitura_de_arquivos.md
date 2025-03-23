
---

## 📥 Leitura de Dados no PySpark

Nesta seção, organizei tudo que aprendi até agora sobre como **ler dados no PySpark**, incluindo os principais métodos, opções de configuração, erros comuns e boas práticas — tudo com exemplos reais no ambiente Databricks.

---

### 📁 Explorando os arquivos com DBFS

Antes de iniciar a leitura de arquivos com Spark, é importante saber onde eles estão localizados. O Databricks oferece o **DBFS (Databricks File System)**, uma camada de abstração sobre o armazenamento em nuvem.

Para listar arquivos e diretórios dentro do DBFS:

```python
dbutils.fs.ls("/FileStore/")
```

Isso funciona como um “Windows Explorer” em nuvem. Você pode verificar o nome dos arquivos e caminhos antes de carregá-los.

---

### 📄 Lendo dados CSV

```python
df = (
    spark.read.format('csv')
    .option('inferSchema', True)  # Infere automaticamente os tipos das colunas
    .option('header', True)       # Considera a primeira linha como cabeçalho
    .load('dbfs:/FileStore/BigMart_Sales__1_.csv')
)
df.display()
```

📌 Detalhes importantes:
- `'inferSchema': True` → o Spark analisa os dados e tenta entender automaticamente o tipo de cada coluna.
- `'header': True` → usa a primeira linha como nome das colunas (em vez de tratá-la como dado).
- O método `display()` é exclusivo do Databricks para exibir o DataFrame de forma visual.

---

### 🧪 Erros comuns na leitura

Durante meus testes, enfrentei um erro de indentação causado por **quebrar linhas com barra invertida (`\`) e adicionar comentários na mesma linha**:

```python
.option('multiLine', False)\  # ❌ Isso causa erro
```

✅ Solução recomendada: use **parênteses** ao redor de toda a expressão encadeada:

```python
df = (
    spark.read.format('csv')
    .option('inferSchema', True)
    .option('header', True)
    .load('dbfs:/FileStore/arquivo.csv')
)
```

---

### 🧾 Lendo dados JSON

```python
df_json = (
    spark.read.format('json')
    .option('inferSchema', True)
    .option('header', True)
    .option('multiLine', False)  # Nesse arquivo JSON, os dados estão em uma linha só
    .load('dbfs:/FileStore/drivers.json')
)
df_json.display()
```

📌 Explicando a opção:
- `'multiLine': False` → usado quando o JSON inteiro está em uma única linha. Se o JSON for formatado com quebras de linha, usamos `True`.

---

### 🧠 Curiosidades sobre `.csv()` e `.json()`

- Ambos os métodos (`.csv()` e `.json()`) são **atalhos** para `format().load()`, mas permitem configurações específicas.
- O Spark aceita **vários parâmetros adicionais**, como `sep`, `encoding`, `nullValue`, `timestampFormat`, entre outros.
- Você pode verificar todos os parâmetros disponíveis com:

```python
help(spark.read.csv)
```

Ou acessar a documentação:
🔗 https://spark.apache.org/docs/latest/sql-data-sources-csv.html  
🔗 https://spark.apache.org/docs/latest/sql-data-sources-json.html

---

### 🧠 O que é DBFS?

- **DBFS** = *Databricks File System*
- É o “disco virtual” do Databricks, onde seus arquivos ficam armazenados
- Pode ser acessado com caminhos como:
  ```python
  'dbfs:/FileStore/arquivo.csv'
  ```

É como o **gerenciador de arquivos do Databricks**, e pode ser acessado via código ou interface gráfica.

---

### ✅ Resumo

| Ação                        | Comando                             |
|-----------------------------|-------------------------------------|
| Listar arquivos no DBFS     | `dbutils.fs.ls("/FileStore/")`      |
| Ler CSV                     | `.read.format('csv').load(...)`     |
| Ler JSON                    | `.read.format('json').load(...)`    |
| Evitar erro de indentação   | Use parênteses ao quebrar linhas    |
| Ver opções disponíveis      | `help(spark.read.csv)`              |

---
