# **PySpark Zero to Pro**  
**Documentando meu aprendizado sobre Apache Spark e PySpark**  

Este repositório contém meus estudos e experimentos sobre **Apache Spark**, explicando sua **arquitetura, funcionamento e como ele processa grandes volumes de dados**. Aqui, estou registrando minha compreensão do assunto, aprimorando conceitos ao longo do caminho.  

---

## **Índice**
- [📌 O que é Apache Spark?](#-o-que-é-apache-spark)
- [⚙️ Arquitetura do Spark](https://github.com/AninhaBe/pyspark_zero_to_pro/blob/main/README.md#%EF%B8%8F-arquitetura-do-spark)
- [🚀 Fluxo de Execução do Spark](#-fluxo-de-execução-do-spark)
- [📚 Recursos para Estudo](#-recursos-para-estudo)
- [📌 Como Rodar este Repositório](#-como-rodar-este-repositório)

---

## **O que é Apache Spark?**
Apache Spark é um **framework open-source** que facilita o **processamento de dados em larga escala**.  

Ou seja, o Spark é um mecanismo de **computação distribuída** que permite **dividir e distribuir dados entre várias máquinas** para processá-los de forma **mais eficiente**.  

Imagine que você tenha **1GB de dados** para processar. Você pode:
1. **Rodar em uma única máquina** e tentar aumentar seus recursos (CPU, memória).  
2. **Dividir o trabalho entre várias máquinas** e conectá-las, formando um **cluster**.  

No segundo caso, cada máquina do cluster processa **uma parte dos dados**, tornando o processo **mais rápido e eficiente**.  

**Exemplo do mundo real**:  
Se você está na faculdade e tem **milhares de computadores disponíveis**, o Spark permite que você use **todas essas máquinas juntas**, aproveitando o poder de processamento delas.  

### **Por que usar Spark?**
✅ **Processamento Distribuído** → Permite trabalhar com **Big Data**.  
✅ **Alta Performance** → Pode ser **100x mais rápido** que outros frameworks.  
✅ **Processamento em Memória (In-Memory)** → Evita leituras e gravações no disco.  
✅ **Funciona com SQL, Machine Learning e Streaming de dados**.  

Se um código Python tradicional demoraria **horas** para processar um grande conjunto de dados, o Spark pode fazer isso **em minutos** usando múltiplas máquinas.  

---

## **Arquitetura do Spark**
A arquitetura do Spark é **baseada em clusters** e segue um modelo **Mestre-Escravo (Master-Slave)**.  

A imagem abaixo representa essa estrutura:  
![cluster-overview](https://github.com/user-attachments/assets/34ea3450-581b-4c2d-9c51-dcbb9fac7758)


### **1. Driver Program (Programa Principal)**
O **Driver Program** é onde tudo começa!  
- Ele **inicializa a sessão Spark**, criando o **SparkContext**, que será responsável por gerenciar os nós.  
- Ele **divide as tarefas** e **coordena o processamento** dentro do cluster.  

**Exemplo no PySpark**:
```python
from pyspark.sql import SparkSession

spark = SparkSession.builder.appName("MeuApp").getOrCreate()
```
**Aqui, o `SparkSession` cria o Driver Program.**

---

### **2. Cluster Manager (Gerenciador de Recursos)**
- O **Cluster Manager** é responsável por gerenciar os recursos do cluster.  
- Ele decide **quantos nós serão usados**, **quantos executores serão criados** e **quantos recursos cada executor pode ter**.  
- Ele recebe as **requisições do SparkContext** e distribui as tarefas.  

O Spark pode rodar com diferentes **gerenciadores de cluster**:
- **Standalone** (modo básico do próprio Spark).  
- **YARN** (Hadoop).  
- **Mesos**.  
- **Kubernetes** (gerenciador na nuvem).  

**O Cluster Manager distribui os recursos de acordo com a demanda do SparkContext!**

---

### **3. Worker Nodes (Nós Trabalhadores)**
Os **Worker Nodes** são as máquinas onde **o processamento realmente acontece**.  
Cada Worker contém:  
✔ **Executors** → Executam as tarefas que foram enviadas pelo Driver.  
✔ **Tasks** → Pequenas partes do código que serão rodadas em paralelo.  
✔ **Cache** → Armazena dados em memória para melhorar a performance.  

**Quanto mais Worker Nodes disponíveis, mais rápido será o processamento!** 🚀

---

## **Fluxo de Execução do Spark**
Agora que entendemos os componentes, veja **como o Spark processa os dados** passo a passo:

1️⃣ O **Driver Program** inicia a sessão Spark e cria um **SparkContext**.  
2️⃣ O **SparkContext** conversa com o **Cluster Manager** e solicita recursos.  
3️⃣ O **Cluster Manager** distribui os recursos e inicia os **Executors** nos **Worker Nodes**.  
4️⃣ O **Driver Program** envia **tarefas (Tasks)** para os **Executors** processarem os dados.  
5️⃣ Cada **Worker Node** processa suas tarefas **em paralelo** e pode armazenar dados em **cache**.  
6️⃣ Os **Executors** retornam os resultados ao **Driver**, que une tudo e gera a saída final.  

---

## **Recursos para Estudo**
🔹 [Documentação Oficial do Spark](https://spark.apache.org/docs/latest/)  
🔹 [Curso Gratuito de PySpark no Databricks](https://www.databricks.com/learn)  
🔹 [Livro: Learning Spark - O'Reilly](https://www.oreilly.com/library/view/learning-spark-2nd/9781492050032/)  

---

## **Como Rodar este Repositório**
Para rodar os exemplos de PySpark localmente, siga estes passos:

1️⃣ **Instale o PySpark**  
```sh
pip install pyspark
```

2️⃣ **Crie uma SparkSession no Python**
```python
from pyspark.sql import SparkSession

spark = SparkSession.builder.appName("MeuApp").getOrCreate()
```

3️⃣ **Rode um exemplo simples**
```python
data = [(1, "Alice"), (2, "Bob"), (3, "Charlie")]
df = spark.createDataFrame(data, ["id", "nome"])
df.show()
```


