# Engenharia de Dados - Data streaming and batch processing in AWS services

Esse projeto tem como finalidade demonstrar conhecimentos adquiridos e replicar a arquitetura/pipeline de Streaming e Batch de dados adquiridos no curso: <br>
https://www.udemy.com/course/real-time ministrado pelo Prof. Fernado Amaral https://www.udemy.com/user/fernando-amaral-3


## Parte I: Streaming de dados em tempo real
Consumo de API que disponibiliza dados climáticos e emite alertas por email e SMS via AWS SNS <br> 
Duas aplicações python, uma para ingestão de dados no Broker (Kinesis Streaming) e outra para avaliação dos mesmos a fim de disparar eventos de acordo com parametros do clima estabelecidos previamente em variávis de ambiente de uma lambda function. 
Ao final, EventBridge é utilizado para schedular a entrega em periodos especificos

## Parte II: Batch de dados para Analytics
Disponibilza dados para posterior análise de eventos climáticos, consumidos
pelo broker e processados por uma aplicação python que particiona os dados brutos semi-estruturados e os armazena em um S3 <br>
Um crawler é usado para capturar o schema e gerar um data catalog e um processo de ETL é realizado para transformação dos mesmos em formato parquet. <br>
Ao final, um novo data catalog é disponibilizado e otimizado para consultas por meio do Athena ou qualquer outra ferramenta de BI 

## Arquitetura geral:

![0-arquitetura](https://github.com/user-attachments/assets/4e72251e-f218-4330-a27c-9d2fd866169e)


## Parte I
## Etapas

| Etapas | Serviço |
|--------|-----------|
| 1      | API: https://app.tomorrow.io/home |
| 2      | IAM Roles |
| 3      | Kinesis Data Stream (Broker) |
| 4      | Producer - Lambda + Python |
| 5      | Consumer - Lambda + Python |
| 6      | SNS |
| 7      | EventBridge |

## Tecnologias utilizadas:
![image](https://github.com/user-attachments/assets/eb97b2ed-a599-4b04-9b6e-7fa7f53fb654)
![image](https://github.com/user-attachments/assets/574117af-5a4d-41fc-be80-ebd2eebe3b23)
![image](https://github.com/user-attachments/assets/30805669-e48b-4326-bc49-ea56548963c6)
![image](https://github.com/user-attachments/assets/cf88b81f-08a1-4340-bd2f-341d34d56bd7)
![image](https://github.com/user-attachments/assets/82496d72-7daa-41ee-8452-25f29b000a43)


### Data Workflow:
- Produtores  : aplicação Python envolta em um Lambda requisita dados em tempo real para a API <p>
- Broker  : ingestão dos dados por meio do Kinesis Data Stream <p>
- Consumidores: aplicação Python envolta em um Lambda consulta registros do Broker e dispara eventos para tópicos do SNS segundo parametros do clima estipulados previamente <p>
- Scheduler: EventBridge estipula dia e hora para entrega de mensagens pontuais <p>
- Alerta: SNS tópicos de EMAIL e SMS <p>


## Imagens:<p>
- #### 1. Tomorrow API: <p>
![image](https://github.com/user-attachments/assets/a89c8ac6-24fd-44de-aea9-97d1cd80052b) <p>

- #### 1.1. API response: <p>
![image](https://github.com/user-attachments/assets/a0a9ffca-f7d4-424a-b7e5-2d20ac1c97a5) <p>

- #### 2. IAM Roles: <p>
![image](https://github.com/user-attachments/assets/3914a9bf-d090-4f2d-8ace-efacbbd63b34) <p>

- #### 3. Broker - Kinesis data stream: <p>
![image](https://github.com/user-attachments/assets/e8d75cbc-3e62-4f49-be39-57cb5070f43b) <p>

- #### 4. Producer - Lambda + Python: <p>
![image](https://github.com/user-attachments/assets/0c8a8c55-f8a8-4b97-8faf-59365ccbf4e1) <br>
![image](https://github.com/user-attachments/assets/201aed05-0c03-4d76-9aa8-c1c2d5abbcf6) <br>
![image](https://github.com/user-attachments/assets/fbd84b21-1e8f-4316-aa52-2e5ca0053c9e) <p>

 - #### 5. Registros Json: Kinesis data stream:: <p>
![image](https://github.com/user-attachments/assets/4fc1a462-86f4-4539-8809-88ae3121360f) <p>

 - #### 6. Consumer - Lambda + Python: <p>
![image](https://github.com/user-attachments/assets/2e802541-15a7-442a-ad6c-e31e4e2cb05f) <br>
![image](https://github.com/user-attachments/assets/147e8b64-194c-4694-8d20-9c51b48616c5) <p>

- #### 7. EventBridge: <p>
![image](https://github.com/user-attachments/assets/d289da8d-217d-468f-aeb6-576e1d055118) <p>

- #### 8. SNS - Alerta: <p>
![image](https://github.com/user-attachments/assets/e5eb35b3-f87a-4b0a-828e-a3a64cb79dd5) <br>
![image](https://github.com/user-attachments/assets/c7fcd26c-b946-42d3-b139-8f7e420b5c94) <p>

- #### 8.1. SNS - Email e SMS: <p>
![image](https://github.com/user-attachments/assets/a4f6c9c4-8cce-434a-a57f-87b45a6f1218) <br>
![image](https://github.com/user-attachments/assets/bfd741e5-4529-45c6-8e5c-68fd51b7b1fa) <br>
![image](https://github.com/user-attachments/assets/3d39d71d-7671-4f0c-a70b-8b0d8e02771c) <p>

<p>

## Parte II
## Etapas

| Etapas | Serviço |
|--------|-----------|
| 1      | S3 bucket |
| 2      | IAM Roles |
| 3      | Consumer: Lambda + Python |
| 4      | Kinesis Data Stream (Broker) |
| 5      | Glue Crawler | 
| 6      | Glue Job ETL |
| 7      | Parquet |
| 8      | Data Catalog |
| 9      | Athena |

## Tecnologias utilizadas:
![image](https://github.com/DieGit0/windfarm/assets/19257853/c1dd3296-eb91-456e-b66b-5677badd7a0b) 
![image](https://github.com/user-attachments/assets/9cf56027-9129-4b88-84ee-ce40a2a70427)
![image](https://github.com/user-attachments/assets/367272f8-cad8-4cfe-a0ad-512a687138bc) <p>

### Data Workflow:
- Produtores  : Workflow Parte I
- Consumidores: aplicação Python + Lambda function consulta dados disponiveis no Broker e persiste os mesmos em formato padrão particionados por ano/dia/hora em S3 pasta raw <p>
- Raw Crawler: reconhece schema e disponibiliza Data Catalog para raw data <p>
- ETL: realiza transformação dos dados "brutos" em formato parquet na pasta gold <p>
- Gold Crawler : gera um novo schema a partir dos arquivos parquet e disponibiliza Data Catalog otimizado para consultas <p>
- Analytics: Athena para consultas ad-hoc <p>

- #### 1. S3 bucket:
![image](https://github.com/user-attachments/assets/607c1692-b840-4526-91ff-c25651951ad8) <p>

- #### 2. Consumer batch:
![image](https://github.com/user-attachments/assets/34126eb5-c3d9-458f-a7e3-0c33b507f34f) <br>
![image](https://github.com/user-attachments/assets/84f4246e-16b6-49bc-a65c-bb79054017df) <br>
![image](https://github.com/user-attachments/assets/bbd71ea3-7653-4707-a0fb-cd33a04b57a8) <p>

- #### 3. Raw data (json):
![image](https://github.com/user-attachments/assets/9b5cb0e0-fa19-46ff-9885-b1e9257633a3) <p>

- #### 4. Crawlers:
![image](https://github.com/user-attachments/assets/5f6857f9-9077-4785-81a0-e6d9b5061484) <p>

- #### 5. Data Bases:
![image](https://github.com/user-attachments/assets/2dd3c837-7009-470c-a124-3f19db8af3f4) <p>

- #### 6. Raw data (Schema):
![image](https://github.com/user-attachments/assets/7d9967ae-d761-4a69-aadf-77653945c2df) <br>
![image](https://github.com/user-attachments/assets/b7d7ad66-ede5-4432-a610-61436618d571) <p>

- #### 7. ETL Job: <p>
![image](https://github.com/user-attachments/assets/1b16e20a-56e6-4d05-a634-bd6ab48d0eaf)
![image](https://github.com/user-attachments/assets/fa508902-6d13-4755-800a-087615a75359) <br>
![image](https://github.com/user-attachments/assets/a2ca8a34-5d59-4dea-a5ed-4cba29ec9f36) <p>

- #### 8. Parquet: <p>
![image](https://github.com/user-attachments/assets/4af370d5-34ef-4262-a5e3-9b49332a8e25) <p>

- #### 9. Gold Schema: <p>
![image](https://github.com/user-attachments/assets/a4b8dfd4-91f9-4c85-bfe8-2146aa728d52) <br>
![image](https://github.com/user-attachments/assets/f5164627-b01c-416c-ba6a-4f4e3015a7a0) <br>
![image](https://github.com/user-attachments/assets/5407b998-27b3-4373-a00a-d204700f9524) <p>

- #### 10. Athena: <p>
![image](https://github.com/user-attachments/assets/34b284ff-17ec-495d-a213-7ff1d35d423b)
![image](https://github.com/user-attachments/assets/8778b9c5-de89-41f1-bed5-f456f310e44d)
<p> 
<p>
