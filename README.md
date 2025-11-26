
# 🚀 API Pedidos v0.4 - Documentação Completa

## 📋 Visão Geral

A **API Pedidos v0.4** é uma solução completa para gerenciamento de pedidos, implementada com **Node.js, Express, JWT** para autenticação e **Firebase Realtime Database** como banco de dados.

**🌐 URL Base:** `https://apidelibery.onrender.com`

### ✨ Características Principais

| Recurso | Descrição |
|---------|-----------|
| 🔐 **Autenticação JWT** | Endpoints protegidos com tokens válidos por 10 horas |
| 🗄️ **Firebase Database** | Armazenamento em tempo real com organização automática por data |
| 🚚 **Tipos de Pedido** | Suporte a **Entrega** e **Retirada** com campo `tipoPedido` |
| ✅ **Validação Robusta** | Verificação completa dos dados de entrada |
| 📱 **API RESTful** | Padrão REST com respostas consistentes |

---

## 🔐 Autenticação

### 📌 Como Funciona

1. **Faça login** usando o endpoint `/login` para obter um token JWT
2. **Inclua o token** no cabeçalho `Authorization` de todas as requisições protegidas

### 🔑 Formato do Header

```http
Authorization: Bearer SEU_TOKEN_JWT_AQUI
```

**⏰ Validade do token:** 10 horas

---

## 📡 Endpoints da API

### 1. 🔍 Health Check
```http
GET /
```
**🔓 Público** - Retorna status da API

**✅ Resposta:**
```json
{
  "ok": true,
  "message": "API Pedidos v0.4 rodando — JWT + Firebase",
  "pastaHoje": "PEDIDOS_MANUAIS_26112025",
  "timestamp": "2025-11-26T10:30:00.000Z"
}
```

### 2. 🔑 Login
```http
POST /login
```
**🔓 Público** - Autentica usuário e retorna token JWT

**📦 Body da Requisição:**
```json
{
  "usuario": "admin",
  "senha": "123"
}
```

**✅ Resposta de Sucesso:**
```json
{
  "ok": true,
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

**❌ Resposta de Erro:**
```json
{
  "erro": "Usuário ou senha incorretos"
}
```

### 3. 📦 Criar Pedido
```http
POST /pedido
```
**🔒 Protegido** - Cria um novo pedido manual no sistema

**📋 Headers:**
```http
Authorization: Bearer SEU_TOKEN_JWT_AQUI
Content-Type: application/json
```

**📦 Body da Requisição:**
```json
{
  "cliente": "João Silva",
  "telefone": "11999999999",
  "pagamento": "Pix",
  "taxa": 5,
  "valor_total": 42.90,
  "tipoPedido": "Entrega",
  "endereco": {
    "rua": "Rua das Flores",
    "numero": "123",
    "bairro": "Centro",
    "referencia": "Próximo à praça"
  },
  "itens": {
    "item1": {
      "nome": "Pizza Margherita",
      "qtd": 1,
      "preco": 42.90
    }
  },
  "motoboy": {
    "id": "mb001",
    "nome": "Carlos"
  }
}
```

**✅ Resposta de Sucesso (201 Created):**
```json
{
  "ok": true,
  "firebase_id": "-Nr98h12ad9",
  "pasta": "PEDIDOS_MANUAIS_26112025",
  "pedido": {
    "cliente": "João Silva",
    "endereco": {
      "bairro": "Centro",
      "numero": "123",
      "referencia": "Próximo à praça",
      "rua": "Rua das Flores"
    },
    "estimatedDeliveryMinutes": 30,
    "id": "1732618800000",
    "tipoPedido": "Entrega",
    "motoboy": {
      "id": "mb001",
      "nome": "Carlos"
    },
    "pagamento": "Pix",
    "status": "pendente",
    "taxa": 5,
    "telefone": "11999999999",
    "valor_total": 42.9,
    "itens": {
      "item1": {
        "nome": "Pizza Margherita",
        "qtd": 1,
        "preco": 42.9
      }
    }
  }
}
```

### 4. 📋 Listar Pedidos do Dia
```http
GET /pedidos
```
**🔒 Protegido** - Retorna todos os pedidos do dia atual

**📋 Headers:**
```http
Authorization: Bearer SEU_TOKEN_JWT_AQUI
```

### 5. 📅 Listar Pedidos por Data
```http
GET /pedidos/:data
```
**🔒 Protegido** - Retorna pedidos de uma data específica

**📌 Parâmetro:** `data` (formato DDMMAAAA)

**🔗 Exemplo:**
```http
GET /pedidos/26112025
```

---

## 🗃️ Esquemas de Dados

### 📝 Esquema Completo do Pedido

| Campo | Tipo | Obrigatório | Descrição | Valor Padrão |
|-------|------|-------------|-----------|-------------|
| `cliente` | `string` | ✅ | Nome do cliente | - |
| `telefone` | `string` | ❌ | Telefone do cliente | `"-"` |
| `pagamento` | `string` | ❌ | Forma de pagamento | `"Outros"` |
| `taxa` | `number` | ❌ | Taxa de entrega | `0` |
| `valor_total` | `number` | ❌ | Valor total do pedido | `0` |
| `tipoPedido` | `string` | ❌ | **"Entrega"** ou **"Retirada"** | `"Entrega"` |
| `endereco` | `object` | ✅ | Endereço completo | - |
| `endereco.rua` | `string` | ❌ | Nome da rua | `""` |
| `endereco.numero` | `string` | ❌ | Número do endereço | `""` |
| `endereco.bairro` | `string` | ❌ | Bairro | `""` |
| `endereco.referencia` | `string` | ❌ | Ponto de referência | `""` |
| `itens` | `object` | ❌ | Itens do pedido | `{}` |
| `motoboy` | `object` | ❌ | Informações do motoboy | `{id: "", nome: ""}` |
| `status` | `string` | ❌ | Status do pedido | `"pendente"` |

---

## 💻 Exemplos de Uso

### 🔐 JavaScript/Node.js

#### 1. Autenticação e Criação de Pedido

```javascript
// 1. Fazer login para obter token
async function login() {
    const loginResponse = await fetch('https://apidelibery.onrender.com/login', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify({
            usuario: 'admin',
            senha: '123'
        })
    });

    const { token } = await loginResponse.json();
    return token;
}

// 2. Criar pedido de entrega
async function criarPedido(token) {
    const pedidoResponse = await fetch('https://apidelibery.onrender.com/pedido', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
            'Authorization': `Bearer ${token}`
        },
        body: JSON.stringify({
            cliente: 'João Silva',
            telefone: '11999999999',
            pagamento: 'Pix',
            tipoPedido: 'Entrega',
            taxa: 5,
            valor_total: 42.90,
            endereco: {
                rua: 'Rua das Flores',
                numero: '123',
                bairro: 'Centro',
                referencia: 'Próximo à praça'
            },
            itens: {
                item1: {
                    nome: 'Pizza Margherita',
                    qtd: 1,
                    preco: 42.90
                }
            }
        })
    });

    const resultado = await pedidoResponse.json();
    return resultado;
}

// Uso
const token = await login();
const pedidoCriado = await criarPedido(token);
console.log(pedidoCriado);
```

#### 2. Pedido de Retirada

```javascript
const pedidoRetirada = {
    cliente: 'Maria Santos',
    telefone: '11988888888',
    pagamento: 'Cartão',
    tipoPedido: 'Retirada',  // ← Tipo alterado para Retirada
    valor_total: 35.50,
    endereco: {
        rua: 'Av. Principal',
        numero: '456',
        bairro: 'Centro',
        referencia: ''
    },
    itens: {
        item1: {
            nome: 'Hambúrguer Artesanal',
            qtd: 2,
            preco: 35.50
        }
    }
};
```

#### 3. Listar Pedidos

```javascript
async function listarPedidos(token) {
    const response = await fetch('https://apidelibery.onrender.com/pedidos', {
        method: 'GET',
        headers: {
            'Authorization': `Bearer ${token}`
        }
    });

    const pedidos = await response.json();
    return pedidos;
}
```

### 🖥️ Exemplos cURL

#### 1. Autenticação

```bash
# Obter token
TOKEN=$(curl -s -X POST https://apidelibery.onrender.com/login \
  -H "Content-Type: application/json" \
  -d '{"usuario":"admin","senha":"123"}' | jq -r '.token')

echo "Token: $TOKEN"
```

#### 2. Criar Pedido

```bash
curl -X POST https://apidelibery.onrender.com/pedido \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "cliente": "João Silva",
    "telefone": "11999999999",
    "pagamento": "Pix",
    "tipoPedido": "Entrega",
    "taxa": 5,
    "valor_total": 42.90,
    "endereco": {
      "rua": "Rua das Flores",
      "numero": "123",
      "bairro": "Centro",
      "referencia": "Próximo à praça"
    },
    "itens": {
      "item1": {
        "nome": "Pizza Margherita",
        "qtd": 1,
        "preco": 42.90
      }
    }
  }'
```

#### 3. Listar Pedidos

```bash
# Pedidos do dia
curl -H "Authorization: Bearer $TOKEN" \
  https://apidelibery.onrender.com/pedidos

# Pedidos por data específica
curl -H "Authorization: Bearer $TOKEN" \
  https://apidelibery.onrender.com/pedidos/26112025
```

### 🐍 Python Example

```python
import requests

# 1. Login
login_url = "https://apidelibery.onrender.com/login"
login_data = {
    "usuario": "admin",
    "senha": "123"
}

response = requests.post(login_url, json=login_data)
token = response.json()["token"]

# 2. Criar pedido
pedido_url = "https://apidelibery.onrender.com/pedido"
headers = {
    "Authorization": f"Bearer {token}",
    "Content-Type": "application/json"
}

pedido_data = {
    "cliente": "João Silva",
    "telefone": "11999999999",
    "pagamento": "Pix",
    "tipoPedido": "Entrega",
    "taxa": 5,
    "valor_total": 42.90,
    "endereco": {
        "rua": "Rua das Flores",
        "numero": "123",
        "bairro": "Centro",
        "referencia": "Próximo à praça"
    }
}

response = requests.post(pedido_url, json=pedido_data, headers=headers)
print(response.json())
```

---

## 📊 Códigos de Status HTTP

| Código | Status | Descrição |
|--------|--------|-----------|
| `200` | **OK** | Requisição bem-sucedida |
| `201` | **Created** | Recurso criado com sucesso |
| `400` | **Bad Request** | Dados da requisição inválidos |
| `401` | **Unauthorized** | Token inválido ou não fornecido |
| `500` | **Internal Server Error** | Erro no servidor |

---

## 🆕 Novidades na Versão 0.4

### ✨ Melhorias Implementadas

- ✅ **Novo campo `tipoPedido`** - Define se é **Entrega** ou **Retirada**
- ✅ **Validações aprimoradas** - Verificação mais robusta dos dados
- ✅ **Organização de código** - Estrutura mais limpa e mantível
- ✅ **Respostas de erro** - Mensagens mais claras e detalhadas
- ✅ **Documentação completa** - Exemplos para múltiplas linguagens

### 🔄 Campos Obrigatórios

Para criar um pedido, apenas estes campos são obrigatórios:

```json
{
  "cliente": "string",
  "endereco": {
    "bairro": "string",
    "numero": "string", 
    "referencia": "string",
    "rua": "string"
  }
}
```

---

## ❌ Tratamento de Erros

### Exemplos de Respostas de Erro

**🔐 Token não enviado:**
```json
{
  "erro": "Token não enviado"
}
```

**🔐 Token inválido:**
```json
{
  "erro": "Token inválido ou expirado"
}
```
**📝 Dados inválidos:**
```json
{
  "erro": "cliente é obrigatório e deve ser uma string."
}
```

**📝 Endereço inválido:**
```json
{
  "erro": "endereco é obrigatório e deve ser um objeto."
}
```

---

## 🔗 Links e Recursos

- **🌐 URL da API:** https://apidelibery.onrender.com
- **📚 Documentação Completa:** [Link para documentação HTML]
- **🐙 Repositório:** [Link para o GitHub]

---

## 📞 Suporte

Em caso de problemas ou dúvidas:

1. Verifique se o token JWT é válido e não expirou
2. Confirme que todos os campos obrigatórios estão presentes
3. Valide o formato dos dados enviados
4. Verifique a conexão com a internet

---

**📅 Última atualização:** Novembro 2025  
**🔄 Versão da API:** 0.4  
**👨‍💻 Mantido por:** Diemgot

```
