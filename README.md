API Pedidos v0.4 - Documentação
📋 Visão Geral
A API Pedidos v0.4 é uma solução completa para gerenciamento de pedidos, implementada com Node.js, Express, JWT para autenticação e Firebase Realtime Database.

URL Base: https://apidelibery.onrender.com

🚀 Características Principais
🔐 Autenticação JWT para endpoints protegidos

🗄️ Firebase Realtime Database com organização automática por data

🚚 Suporte a Entrega e Retirada com campo tipoPedido

✅ Validação robusta de dados de entrada

📱 API RESTful com respostas padronizadas

🔐 Autenticação
A API utiliza autenticação baseada em JWT (JSON Web Tokens):

Faça login usando /login para obter um token

Inclua o token no cabeçalho Authorization das requisições

Formato do cabeçalho:

http
Authorization: Bearer SEU_TOKEN_JWT_AQUI
Validade do token: 10 horas

📡 Endpoints
🔍 Health Check
http
GET /
Público - Retorna status da API

Resposta:

json
{
  "ok": true,
  "message": "API Pedidos v0.4 rodando — JWT + Firebase",
  "pastaHoje": "PEDIDOS_MANUAIS_26112025",
  "timestamp": "2025-11-26T10:30:00.000Z"
}
🔑 Login
http
POST /login
Público - Autentica usuário e retorna token JWT

Body:

json
{
  "usuario": "admin",
  "senha": "123"
}
Resposta de Sucesso:

json
{
  "ok": true,
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
📦 Criar Pedido
http
POST /pedido
Protegido - Cria um novo pedido manual

Headers:

http
Authorization: Bearer SEU_TOKEN_JWT_AQUI
Content-Type: application/json
Body:

json
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
Resposta de Sucesso (201):

json
{
  "ok": true,
  "firebase_id": "-Nr98h12ad9",
  "pasta": "PEDIDOS_MANUAIS_26112025",
  "pedido": {
    "cliente": "João Silva",
    "endereco": { ... },
    "estimatedDeliveryMinutes": 30,
    "id": "1732618800000",
    "tipoPedido": "Entrega",
    "motoboy": { ... },
    "pagamento": "Pix",
    "status": "pendente",
    "taxa": 5,
    "telefone": "11999999999",
    "valor_total": 42.9,
    "itens": { ... }
  }
}
📋 Listar Pedidos do Dia
http
GET /pedidos
Protegido - Retorna todos os pedidos do dia atual

Headers:

http
Authorization: Bearer SEU_TOKEN_JWT_AQUI
📅 Listar Pedidos por Data
http
GET /pedidos/:data
Protegido - Retorna pedidos de uma data específica

Parâmetro: data (formato DDMMAAAA)

Exemplo:

http
GET /pedidos/26112025
🗃️ Esquemas de Dados
📝 Esquema do Pedido
Campo	Tipo	Obrigatório	Descrição
cliente	string	✅	Nome do cliente
telefone	string	❌	Telefone do cliente
pagamento	string	❌	Pix, Cartão, Dinheiro, Outros
taxa	number	❌	Taxa de entrega
valor_total	number	❌	Valor total do pedido
tipoPedido	string	❌	"Entrega" ou "Retirada" (padrão: "Entrega")
endereco	object	✅	Endereço de entrega
endereco.rua	string	❌	Nome da rua
endereco.numero	string	❌	Número do endereço
endereco.bairro	string	❌	Bairro
endereco.referencia	string	❌	Ponto de referência
itens	object	❌	Itens do pedido
motoboy	object	❌	Informações do motoboy
💡 Exemplos de Uso
🔐 Autenticação e Criação de Pedido (JavaScript)
javascript
// 1. Fazer login para obter token
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

// 2. Criar pedido de entrega
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
console.log(resultado);
🖥️ Exemplos cURL
bash
# Obter token
TOKEN=$(curl -s -X POST https://apidelibery.onrender.com/login \
  -H "Content-Type: application/json" \
  -d '{"usuario":"admin","senha":"123"}' | jq -r '.token')

# Listar pedidos do dia
curl -H "Authorization: Bearer $TOKEN" \
  https://apidelibery.onrender.com/pedidos

# Listar pedidos por data
curl -H "Authorization: Bearer $TOKEN" \
  https://apidelibery.onrender.com/pedidos/26112025
📦 Criar Pedido de Retirada
javascript
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
📊 Códigos de Status HTTP
Código	Descrição
200	OK - Requisição bem-sucedida
201	Created - Recurso criado com sucesso
400	Bad Request - Dados da requisição inválidos
401	Unauthorized - Token inválido ou não fornecido
500	Internal Server Error - Erro no servidor
🆕 Novidades na v0.4
✅ Campo tipoPedido para definir entrega ou retirada

✅ Melhor organização de código

✅ Validações aprimoradas

✅ Respostas de erro mais detalhadas

🔗 Links Úteis
URL da API: https://apidelibery.onrender.com

Repositório: [Link para o GitHub]

Documentação Completa: [Link para documentação HTML]

📅 Última atualização: Novembro 2025
🔄 Versão: 0.4
