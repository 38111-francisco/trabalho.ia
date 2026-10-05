# Documentação da API de Roupas

API RESTful completa para gerenciamento e consulta de roupas, desenvolvida em Node.js com Express e MongoDB (Mongoose), pronta para execução local e deploy na Vercel.

---

## 1. URL Base

- **Local:** `http://localhost:3000`
- **Produção (Vercel):** `https://SEU-BACKEND.vercel.app` (substituir pela URL gerada após deploy)

Todas as rotas da API estão disponíveis sob o prefixo:
```http
/api/roupas
```

---

## 2. Estrutura do Objeto `Roupa`

Cada roupa possui o seguinte formato JSON:

```json
{
  "id": "67a3f81e2b5c001234567890",
  "marca": "Nike",
  "modelo": "Camiseta Sportswear Club Fleece",
  "preco": 149.90,
  "foto": "https://images.unsplash.com/photo-1521572267360-ee0c2909d518?w=800",
  "createdAt": "2026-10-05T11:30:00.000Z",
  "updatedAt": "2026-10-05T11:30:00.000Z"
}
```

### Regras de Validação:
- `marca` (string): Obrigatória, não pode ser vazia.
- `modelo` (string): Obrigatório, não pode ser vazio.
- `preco` (number): Obrigatório, deve ser um número estritamente maior que zero (`preco > 0`).
- `foto` (string): Opcional. Quando informada, deve ser uma URL válida (`http://` ou `https://`).

---

## 3. Códigos de Status HTTP

| Código | Descrição |
|---|---|
| `200 OK` | Operação de consulta, atualização ou exclusão realizada com sucesso |
| `201 Created` | Roupa cadastrada com sucesso |
| `400 Bad Request` | Dados inválidos fornecidos no corpo da requisição ou formato de ID inválido |
| `404 Not Found` | Roupa não encontrada para o ID fornecido |
| `500 Internal Server Error` | Erro interno no processamento do servidor |

---

## 4. Endpoints

### 4.1. Listar todas as roupas
Retorna a lista completa de roupas cadastradas em ordem decrescente de criação.

- **Método:** `GET`
- **Rota:** `/api/roupas`
- **Parâmetros:** Nenhum
- **Resposta Sucesso (`200 OK`):**
```json
[
  {
    "id": "67a3f81e2b5c001234567890",
    "marca": "Nike",
    "modelo": "Camiseta Sportswear Club Fleece",
    "preco": 149.90,
    "foto": "https://images.unsplash.com/photo-1521572267360-ee0c2909d518?w=800",
    "createdAt": "2026-10-05T11:30:00.000Z",
    "updatedAt": "2026-10-05T11:30:00.000Z"
  }
]
```

**Exemplo cURL:**
```bash
curl -X GET http://localhost:3000/api/roupas
```

---

### 4.2. Buscar roupa por ID
Retorna uma única roupa correspondente ao ID informado.

- **Método:** `GET`
- **Rota:** `/api/roupas/:id`
- **Parâmetros de URL:** `id` (string de 24 caracteres hexadecimais do MongoDB ObjectId)
- **Resposta Sucesso (`200 OK`):**
```json
{
  "id": "67a3f81e2b5c001234567890",
  "marca": "Nike",
  "modelo": "Camiseta Sportswear Club Fleece",
  "preco": 149.90,
  "foto": "https://images.unsplash.com/photo-1521572267360-ee0c2909d518?w=800",
  "createdAt": "2026-10-05T11:30:00.000Z",
  "updatedAt": "2026-10-05T11:30:00.000Z"
}
```
- **Respostas de Erro:**
  - `400 Bad Request` (formato de ID inválido):
    ```json
    { "erro": "ID inválido." }
    ```
  - `404 Not Found` (não encontrada):
    ```json
    { "erro": "Roupa não encontrada." }
    ```

**Exemplo cURL:**
```bash
curl -X GET http://localhost:3000/api/roupas/67a3f81e2b5c001234567890
```

---

### 4.3. Criar nova roupa
Cadastra uma nova roupa com os dados validados.

- **Método:** `POST`
- **Rota:** `/api/roupas`
- **Headers:** `Content-Type: application/json`
- **Corpo da Requisição (Body):**
```json
{
  "marca": "Adidas",
  "modelo": "Jaqueta Corta-Vento Essentials",
  "preco": 299.90,
  "foto": "https://images.unsplash.com/photo-1544441893-675973e31985?w=800"
}
```
- **Resposta Sucesso (`201 Created`):**
```json
{
  "id": "67a3f8202b5c001234567891",
  "marca": "Adidas",
  "modelo": "Jaqueta Corta-Vento Essentials",
  "preco": 299.90,
  "foto": "https://images.unsplash.com/photo-1544441893-675973e31985?w=800",
  "createdAt": "2026-10-05T11:35:00.000Z",
  "updatedAt": "2026-10-05T11:35:00.000Z"
}
```
- **Respostas de Erro (`400 Bad Request`):**
  - Marca ausente:
    ```json
    { "erro": "A marca é obrigatória." }
    ```
  - Modelo ausente:
    ```json
    { "erro": "O modelo é obrigatório." }
    ```
  - Preço ausente:
    ```json
    { "erro": "O preço é obrigatório." }
    ```
  - Preço inválido (<= 0):
    ```json
    { "erro": "O preço deve ser maior que zero." }
    ```
  - URL de foto inválida:
    ```json
    { "erro": "A foto deve ser uma URL válida." }
    ```

**Exemplo cURL:**
```bash
curl -X POST http://localhost:3000/api/roupas \
  -H "Content-Type: application/json" \
  -d '{
    "marca": "Adidas",
    "modelo": "Jaqueta Corta-Vento Essentials",
    "preco": 299.90,
    "foto": "https://images.unsplash.com/photo-1544441893-675973e31985?w=800"
  }'
```

---

### 4.4. Atualizar roupa existente
Atualiza um ou mais campos de uma roupa previamente cadastrada.

- **Método:** `PUT`
- **Rota:** `/api/roupas/:id`
- **Headers:** `Content-Type: application/json`
- **Corpo da Requisição (Body):**
```json
{
  "preco": 259.90,
  "modelo": "Jaqueta Corta-Vento Essentials V2"
}
```
- **Resposta Sucesso (`200 OK`):**
```json
{
  "id": "67a3f8202b5c001234567891",
  "marca": "Adidas",
  "modelo": "Jaqueta Corta-Vento Essentials V2",
  "preco": 259.90,
  "foto": "https://images.unsplash.com/photo-1544441893-675973e31985?w=800",
  "createdAt": "2026-10-05T11:35:00.000Z",
  "updatedAt": "2026-10-05T11:40:00.000Z"
}
```
- **Respostas de Erro:**
  - `400 Bad Request` se campos enviados forem vazios ou inválidos.
  - `404 Not Found` se o ID não existir.

**Exemplo cURL:**
```bash
curl -X PUT http://localhost:3000/api/roupas/67a3f8202b5c001234567891 \
  -H "Content-Type: application/json" \
  -d '{"preco": 259.90}'
```

---

### 4.5. Excluir roupa
Remove uma roupa do banco de dados pelo seu ID.

- **Método:** `DELETE`
- **Rota:** `/api/roupas/:id`
- **Resposta Sucesso (`200 OK`):**
```json
{
  "mensagem": "Roupa excluída com sucesso.",
  "id": "67a3f8202b5c001234567891"
}
```
- **Respostas de Erro:**
  - `400 Bad Request` se o ID for inválido.
  - `404 Not Found` se o ID não for encontrado ou já foi excluído:
    ```json
    { "erro": "Roupa não encontrada." }
    ```

**Exemplo cURL:**
```bash
curl -X DELETE http://localhost:3000/api/roupas/67a3f8202b5c001234567891
```

---

## 5. Exemplos de Consumo via JavaScript (Fetch API)

```javascript
const API_URL = 'http://localhost:3000/api/roupas';

// 1. Listar roupas
async function listarRoupas() {
  const res = await fetch(API_URL);
  const roupas = await res.json();
  console.log(roupas);
}

// 2. Cadastrar roupa
async function cadastrarRoupa(dados) {
  const res = await fetch(API_URL, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(dados)
  });
  const data = await res.json();
  if (!res.ok) throw new Error(data.erro);
  return data;
}

// 3. Atualizar roupa
async function atualizarRoupa(id, dados) {
  const res = await fetch(`${API_URL}/${id}`, {
    method: 'PUT',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(dados)
  });
  const data = await res.json();
  if (!res.ok) throw new Error(data.erro);
  return data;
}

// 4. Excluir roupa
async function excluirRoupa(id) {
  const res = await fetch(`${API_URL}/${id}`, {
    method: 'DELETE'
  });
  const data = await res.json();
  if (!res.ok) throw new Error(data.erro);
  return data;
}
```
