## 📥 Leitura de Dados (arquivos) no PySpark

Nesta seção, organizei tudo que aprendi até agora sobre como **ler dados no PySpark**, incluindo os principais métodos, opções de configuração, erros comuns e boas práticas — tudo com exemplos reais no ambiente Databricks.

---

## 📝 Índice

- [📁 Explorando os arquivos com DBFS](#-explorando-os-arquivos-com-dbfs)  
- [📄 Lendo dados CSV](#-lendo-dados-csv)  
- [🧪 Erros comuns na leitura](#-erros-comuns-na-leitura)  
- [🧾 Lendo dados JSON](#-lendo-dados-json)  
- [🧠 Curiosidades sobre `.csv()` e `.json()`](#-curiosidades-sobre-csv-e-json)  
- [🧠 O que é DBFS?](#-o-que-é-dbfs)  
- [✅ Resumo](#-resumo)

---

### 📁 Explorando os arquivos com DBFS

Antes de iniciar a leitura de arquivos com Spark, é importante saber onde eles estão localizados. O Databricks oferece o **DBFS (Databricks File System)**, uma camada de abstração sobre o armazenamento em nuvem.

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

📌 Detalhes:
- `'inferSchema': True` → o Spark tenta deduzir o tipo de cada coluna.
- `'header': True` → usa a primeira linha como nome das colunas.
- `display()` → função do Databricks para visualização rápida dos dados.

---

### 🧪 Erros comuns na leitura

⚠️ Problema que encontrei:

```python
.option('multiLine', False)\  # ❌ Comentário na mesma linha da barra invertida
```

📌 Solução correta:

```python
df = (
    spark.read.format('csv')
    .option('inferSchema', True)
    .option('header', True)
    .load('dbfs:/FileStore/arquivo.csv')
)
```

✅ **Use parênteses para evitar erros com quebras de linha.**

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

📌 Use `'multiLine': True` se o JSON estiver formatado em várias linhas.

---

### 🧠 Curiosidades sobre `.csv()` e `.json()`

- `.csv()` e `.json()` são **atalhos para `.format().load()`**
- Spark aceita dezenas de parâmetros como:
  - `sep`, `encoding`, `nullValue`, `timestampFormat`, etc.
- Você pode ver tudo com:

```python
help(spark.read.csv)
```

📚 [Documentação CSV](https://spark.apache.org/docs/latest/sql-data-sources-csv.html)  
📚 [Documentação JSON](https://spark.apache.org/docs/latest/sql-data-sources-json.html)

---

### 🧠 O que é DBFS?

- DBFS = **Databricks File System**
- Um sistema de arquivos virtual que roda sobre um storage na nuvem
- Você acessa com caminhos como:
  ```python
  'dbfs:/FileStore/arquivo.csv'
  ```

Pense nele como o “gerenciador de arquivos” dentro do seu ambiente Databricks.

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

### PS: O notebook desenvolvido e os arquivos que foram usados no databricks pode ser encontrado dentro dessa pasta
