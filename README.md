# 💱 Currency Exchange Microservice (n8n)

![Status](https://img.shields.io/badge/Status-Functional%20%F0%9F%9F%A2-brightgreen)
![Stack](https://img.shields.io/badge/Stack-n8n%20%7C%20JSON%20%7C%20REST-blue)
![Education](https://img.shields.io/badge/Education-Rocketseat-8257e5)

## 📖 Visão Geral do Projeto

Este projeto consiste em um workflow desenvolvido no **n8n** que atua como um *middleware* (intermediário) de integração. O objetivo é fornecer uma API unificada e simplificada para consulta de cotações de moedas em tempo real, abstraindo a complexidade e a autenticação da API provedora (**AwesomeAPI**).

> 💡 **Contexto:** Este projeto foi desenvolvido e aprimorado como parte prática do curso de **n8n e Automação** da **Rocketseat**.

### ❓ Por que esta solução?
Em cenários corporativos, expor chaves de API de terceiros (*API Keys*) diretamente no front-end ou em aplicações clientes é uma falha grave de segurança.

Este workflow resolve isso **centralizando a autenticação no backend (n8n)** e entregando apenas os dados sanitizados ao solicitante.

---

![Preview do Workflow](images/workflow-preview.png)

## 🏗️ Arquitetura do Workflow

O fluxo de dados segue o padrão **Input -> Transform -> Request -> Response**:


1. **Webhook (Trigger):** Ponto de entrada da API (POST). Recebe a solicitação do cliente.

2. **Data Normalization (Set/Edit Fields):** Isola e valida a variável moeda recebida no corpo da requisição, garantindo que o fluxo seguinte receba dados limpos.

3. **External API Request (HTTP Node):** Realiza a comunicação com a API de economia, injetando o Token de autenticação de forma segura (server-side).

4. **Webhook Response:** Formata e devolve a resposta JSON da API externa para o cliente original.

## 🚀 Como Implementar

### ✅ Pré-requisitos
- Instância do **n8n** (Self-hosted ou Cloud)
- Chave de acesso (**Token**) da **AwesomeAPI**  
  - Opcional para testes simples  
  - **Obrigatório para uso em produção ou em escala**

---

### 📦 Instalação

#### 🔄 Importação do Workflow
1. Faça o download do arquivo `.json` deste workflow
2. No **n8n**, vá em: Menu > Import from File
3. 3. Selecione o arquivo `.json`

---

### 🔐 Configuração de Segurança (Crítico)

⚠️ **Importante:**  
O arquivo JSON foi **sanitizado** para remover qualquer credencial sensível.

#### Passos:
1. Abra o nó **HTTP Request**
2. Vá até **Query Parameters**
3. No campo `token`, substitua: SUA_CHAVE_API_AQUI pelo seu **token real da AwesomeAPI**

---

### ▶️ Ativação
- Clique no botão **Active** no canto superior direito da tela do n8n

---

### 📚 Documentação da API

Após ativado, o workflow expõe um endpoint para consumo externo.

---

#### 💱 Consultar Cotação
Retorna a cotação atual entre duas moedas ou de uma moeda para o **Real (BRL)**.

- **URL:**  https://{{seu-dominio-n8n}}/webhook/cotacao

- **Método:** `POST`

> Utilize o Postman para realizar a requisição

---

#### 📥 Corpo da Requisição (Body)

| Campo | Tipo   | Descrição                             | Exemplo              |
|------|--------|----------------------------------------|----------------------|
| moeda | string | Código da moeda ou par de moedas       | `"USD"` |

#### Exemplo (JSON)
```json
{
"moeda": "USD"
}
```

### Exemplo de Resposta (200 OK)
```json
{
  "USDBRL": {
    "code": "USD",
    "codein": "BRL",
    "name": "Dólar Americano/Real Brasileiro",
    "high": "5.15",
    "low": "5.10",
    "varBid": "0.02",
    "pctChange": "0.45",
    "bid": "5.14",
    "ask": "5.15",
    "timestamp": "1678999999",
    "create_date": "2024-01-04 10:00:00"
  }
}
```



