# ☁️ AWS Data Lake & EMR Processing Pipeline

![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white) ![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54) ![Hadoop](https://img.shields.io/badge/Apache%20Hadoop-66CC00?style=for-the-badge&logo=apachehadoop&logoColor=white)

Este repositório contém a infraestrutura e o código para um pipeline de processamento distribuído de Big Data (MapReduce) utilizando **Amazon Elastic MapReduce (EMR)**, integrando armazenamento escalável via **Amazon S3** e scripts em **Python (MrJob)**.

Este projeto demonstra a capacidade de arquitetar soluções de dados em nuvem, desde a ingestão (S3) até o processamento massivo (Hadoop/EMR) gerenciado via Infraestrutura como Código (IaC dinâmico pela lib `mrjob`).

---

## 🏗️ Arquitetura do Projeto

1. **Storage Layer (S3):** Um Data Lake foi construído no Amazon S3, estruturado nas camadas padrão:
   - `/data`: Arquivos raw de input (Ex: datasets massivos de texto).
   - `/temp`: Área de staging temporário para o EMR.
   - `/output`: Resultados agregados finais gerados pelos clusters.
2. **Compute Layer (AWS EMR):** Clusters efêmeros gerenciados sob demanda para otimização de custos, executando o framework Hadoop MapReduce.
3. **Application Layer (Python/MrJob):** Script Map-Reduce para analisar as frequências léxicas de documentos massivos. O `mrjob` abstrai a criação e o encerramento do cluster EMR diretamente pelo terminal.

## 🛠️ Tecnologias Utilizadas

- **Cloud Provider:** Amazon Web Services (AWS)
- **Data Lake:** Amazon S3
- **Big Data Compute:** Amazon EMR (Elastic MapReduce), Apache Hadoop
- **Linguagem:** Python (Boto3, mrjob)

---

## ⚙️ Como Executar

### Pré-requisitos
1. Uma conta ativa na AWS com permissões para `S3`, `EMR` e `EC2`.
2. Credenciais de acesso configuradas (`aws configure`).
3. Uma Key Pair(`.pem`) do EC2 criada.

### Passo-a-Passo

1. **Clone o repositório e crie o ambiente virtual:**
   ```bash
   git clone https://github.com/michael-eng-ai/AWS-BigData.git
   cd AWS-BigData
   python -m venv venv
   source venv/bin/activate
   pip install boto3 mrjob
   ```

2. **Configure o seu Datalake no S3:**
   Crie um bucket (Ex: `s3://meu-datalake-senior`) e as pastas `/data`, `/temp` e `/output`. Faça o upload do seu dataset raw para a pasta `data/`.

3. **Inicie o Processamento (Cluster será provisionado e destruído automaticamente):**
   ```bash
   python dio-live-wordcount-test.py -r emr s3://meu-datalake-senior/data/SherlockHolmes.txt \
   --output-dir=s3://meu-datalake-senior/output/job1 \
   --cloud-tmp-dir=s3://meu-datalake-senior/temp/
   ```

4. **Análise de Resultados:**
   Os arquivos de contagem estarão particionados diremente na subpasta `output/job1` do S3.

---

> *Este projeto foi desenvolvido como demonstração de provisionamento elástico e gerenciamento analítico na plataforma AWS.*
