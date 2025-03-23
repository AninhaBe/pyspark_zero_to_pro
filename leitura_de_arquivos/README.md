## 📥 Leitura de Dados (arquivos) no PySpark

Nesta seção, organizei tudo que aprendi até agora sobre como **ler dados no PySpark**, incluindo os principais métodos, opções de configuração, erros comuns e boas práticas — tudo com exemplos reais no ambiente Databricks.

---

## 📝 Índice

- [🧠 O que é DBFS?](#-o-que-é-dbfs)  
- [📁 Explorando os arquivos com DBFS](#-explorando-os-arquivos-com-dbfs)  
- [📄 Lendo dados CSV](#-lendo-dados-csv)  
- [🧪 Erros comuns na leitura](#-erros-comuns-na-leitura)  
- [🧾 Lendo dados JSON](#-lendo-dados-json)  
- [🧠 Curiosidades sobre `.csv()` e `.json()`](#-curiosidades-sobre-csv-e-json)  
- [✅ Resumo](#-resumo)

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

### 📁 Explorando os arquivos com DBFS

Antes de iniciar a leitura de arquivos com Spark, é importante saber onde eles estão localizados. O Databricks oferece o **DBFS (Databricks File System)**, uma camada de abstração sobre o armazenamento em nuvem.

```python
dbutils.fs.ls("/FileStore/")
```

Esse comando é **específico do ambiente Databricks** e **não funciona fora dele** (como em notebooks locais ou ambientes Python puros).

#### 🔍 O que ele faz?

Ele **lista os arquivos e pastas** que estão armazenados no DBFS — é o equivalente ao `ls` no terminal Linux ou ao que fazemos no Windows Explorer ao abrir uma pasta.

#### 🔍 Como ele é estruturado?

```python
dbutils     # módulo interno do Databricks
   .fs      # funcionalidade específica para interagir com o File System (DBFS)
      .ls() # função que lista os arquivos e pastas dentro de um caminho
```

> **Ou seja**: estamos dizendo ao Databricks “use o utilitário de sistema de arquivos e me diga o que tem nessa pasta”.

#### 🧠 Para que ele serve na prática?

Usei o `dbutils.fs.ls()` principalmente para:
- Verificar se o arquivo que quero carregar com o Spark está realmente no DBFS
- Saber o **caminho completo** do arquivo (o `.path`) que vou passar para `.load()`
- Validar se o upload do arquivo foi feito com sucesso

---

### 📄 Lendo dados CSV

```python
df = (
    spark.read.format('csv')
    .option('inferSchema', True)
    .option('header', True)
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
    .option('multiLine', False)
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


### ✅ Resumo

| Ação                        | Comando                             |
|-----------------------------|-------------------------------------|
| Listar arquivos no DBFS     | `dbutils.fs.ls("/FileStore/")`      |
| Ler CSV                     | `.read.format('csv').load(...)`     |
| Ler JSON                    | `.read.format('json').load(...)`    |
| Evitar erro de indentação   | Use parênteses ao quebrar linhas    |
| Ver opções disponíveis      | `help(spark.read.csv)`              |

---

### PS: O notebook desenvolvido e os arquivos que foram usados no databricks pode ser encontrado dentro dessa pasta.
