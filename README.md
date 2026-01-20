# fastapi-employees-api

API REST simples para cadastro de funcionários (CRUD), feita em **FastAPI**, com foco principal em **aprender e praticar testes**.

A ideia é construir o projeto em etapas, começando com uma implementação mínima e evoluindo com:
- testes unitários e de API
- validações de contrato
- tratamento de erros
- cobertura
- boas práticas de organização
- pipeline de qualidade (lint/format/type-check/CI)

---

## 🎯 Escopo

Entidade: **Funcionário (Employee)**

### Identificador
- O identificador do funcionário na API é o **document** (documento).
- O **document** é **único**.
- O **email** também é **único**.

### Rotas planejadas
- `POST /employees` → cria funcionário
- `GET /employees/{document}` → obtém funcionário pelo documento
- `PUT /employees/{document}` → atualiza funcionário pelo documento
- `DELETE /employees/{document}` → remove funcionário pelo documento

### Regras iniciais (para guiar os testes)
- `document` é obrigatório e deve ser único (duplicado → `409 Conflict`)
- `email` é obrigatório, deve ser válido e deve ser único (duplicado → `409 Conflict`)
- buscar/atualizar/deletar `document` inexistente → `404 Not Found`

---

## 🧱 Stack

- Python 3.11+
- FastAPI
- Pydantic
- Pytest
- HTTPX (para TestClient/requests em testes)


