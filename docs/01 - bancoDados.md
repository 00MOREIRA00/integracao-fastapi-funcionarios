# Banco de Dados (MVP) — Funcionários (PostgreSQL)

## Contexto
Este projeto será uma API de CRUD de funcionários com foco em operação real e testes.
Nesta etapa, o escopo é **apenas** definir o modelo de dados e regras no banco.

Escolhemos um modelo enxuto, mas com integridade suficiente para gerar cenários de teste:
- unicidade (documento e email)
- consistência de datas
- “deleção” via desativação (soft delete)

---

## Decisão: PostgreSQL
O banco escolhido para o MVP é **PostgreSQL**, pois:
- oferece constraints e índices robustos (unicidade, check constraints)
- é realista para produção
- facilita testes de integração (Docker, migrations, etc.)

---

## Tabela: `funcionarios`

### Campos
- `id` (UUID, PK)
- `nome` (texto)
- `documento` (texto) — normalmente CPF (pode vir com ou sem máscara)
- `email` (texto)
- `telefone` (texto, opcional)
- `data_nascimento` (date)
- `data_admissao` (date)
- `cargo` (texto)
- `ativo` (boolean, default true)
- `data_demissao` (date, opcional)
- `created_at` (timestamptz)
- `updated_at` (timestamptz)

---

## Regras no banco (integridade)

### 1) Unicidade
- `documento` deve ser único
- `email` deve ser único

### 2) Consistência de datas
- `data_nascimento` não pode ser futura
- `data_admissao` não pode ser anterior a `data_nascimento`

> Observação: validações com `CURRENT_DATE` em CHECK funcionam, mas podem ter implicações em testes/reprodutibilidade.
> Aqui adotamos CHECKs simples porque são úteis e reforçam integridade no banco.

### 3) Soft delete / desativação
- “Deletar” não remove o registro.
- O registro é desativado marcando `ativo=false`.
- Regras:
  - se `data_demissao` existir → `ativo` deve ser `false`
  - se `ativo` for `true` → `data_demissao` deve ser `null`

---

## Índices
- índices únicos para `documento` e `email`
- índice para `ativo` (listar ativos com performance)
- índice opcional para `cargo` (se houver filtros por cargo)

---

