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
