# Instalação da PoC - Archie EA

## Objetivo

Implantar o Archie EA localmente utilizando:

- Docker Desktop
- VS Code
- PostgreSQL em Docker
- Redis em Docker
- Ambiente Windows

---

## Ambiente

## Estrutura de Diretórios

```text
C:\EA\archie-poc
│
├── backup
├── data
└── source
```

## Ferramentas

- Windows 11
- Docker Desktop
- VS Code
- Git
- PowerShell

---

## Clonagem do Projeto

Repositório:

https://github.com/Archiet-Ltd/archie-ea

Clone realizado em:

```text
C:\EA\archie-poc\source
```

---

## Configuração do Ambiente

Arquivo:

```text
source\.env
```

Criado a partir de:

```text
.env.example
```

Comando:

```powershell
copy .env.example .env
```

---

## Ajustes Realizados no .env

## Banco

```env
POSTGRES_PASSWORD=Postgres_Admin_2026

DATABASE_DEPLOY_PASSWORD=Deploy_2026

DATABASE_RUNTIME_PASSWORD=Runtime_2026
```

## Database URLs

```env
DATABASE_ADMIN_URL=postgresql://postgres:Postgres_Admin_2026@localhost:5432/postgres

DATABASE_DEPLOY_URL=postgresql://archie_deploy:Deploy_2026@localhost:5432/archie

DATABASE_URL=postgresql://archie_runtime:Runtime_2026@localhost:5432/archie
```

## Secret Key

```env
SECRET_KEY=<definido para o ambiente>
```

## Transformation Secret

Necessário utilizar HEX.

Comando utilizado:

```powershell
python -c "import secrets; print(secrets.token_hex(32))"
```

Exemplo:

```env
TRANSFORMATION_COMMAND_CAPABILITY_SECRET=4f1ecaaad3100d2d43527dfb19a2619711c70eb849ac51b6acce1e3c3dc9b657
```

## Admin

```env
ADMIN_EMAIL=mmoraes.sp.mm@gmail.com
```

---

## Problemas Encontrados

## 1. Banco archie não existia

Erro:

```text
ValueError: target database does not exist: archie
```

Correção:

```sql
CREATE DATABASE archie;
CREATE DATABASE archie_test;
```

Executado via:

```powershell
docker compose exec postgres psql -U postgres
```

---

## 2. Scripts Shell com Problemas

Arquivos corrigidos:

```text
scripts/database/init-databases.sh

scripts/database/deploy-schema.sh

scripts/database/backup_marker_to_manifest.sh
```

Ação:

Converter de:

```text
CRLF
```

para:

```text
LF
```

via VS Code.

---

## 3. Transformation Secret Inválido

Erro:

```text
TRANSFORMATION_COMMAND_CAPABILITY_SECRET must contain hexadecimal bytes
```

Correção:

Gerar chave hexadecimal:

```powershell
python -c "import secrets; print(secrets.token_hex(32))"
```

---

## 4. Porta da Digital Ocean

Erro:

```text
Cannot bind 10.106.0.6:5000
```

Origem:

```yaml
- '10.106.0.6:5000:5000'
```

Correção:

Removida a linha.

Configuração final:

```yaml
ports:
  - '5000:5000'
```

---

## Inicialização

Validar configuração:

```powershell
docker compose config
```

Build:

```powershell
docker compose build
```

Subir ambiente:

```powershell
docker compose up -d
```

---

## Sequência de Containers

```text
postgres

database-bootstrap

schema-deploy

database-acl

server
```

---

## Verificações

## Containers

```powershell
docker compose ps
```

Resultado esperado:

```text
source-postgres-1

source-redis-1

source-adminer-1

source-server-1
```

## Health Check

URL:

```text
http://localhost:5000/health
```

Resultado:

```json
{
  "status":"healthy"
}
```

---

## Acesso ao Banco de Dados

## PostgreSQL

Container:

```text
source-postgres-1
```

Porta:

```text
5432
```

## Adminer

URL:

```text
http://localhost:8080
```

Login:

```text
Servidor: postgres

Usuário: postgres

Senha: Postgres_Admin_2026
```

---

## Aplicação

URL:

```text
http://localhost:5000
```

---

## Usuário Administrador

Comando executado:

```powershell
docker compose exec server python create_admin.py
```

Resultado:

```text
Admin created: mmoraes.sp.mm@gmail.com
```

Organização:

```text
default
```

---

## Backup Inicial

Gerar baseline:

```powershell
docker compose exec postgres pg_dump -U postgres archie > C:\EA\archie-poc\backup\baseline.sql
```

---

## Estado Final

Checklist:

- [x] Docker funcionando
- [x] PostgreSQL funcionando
- [x] Redis funcionando
- [x] Adminer funcionando
- [x] Archie EA funcionando
- [x] Migrações executadas
- [x] Admin criado
- [x] Health Check OK
- [x] Ambiente pronto para PoC

---

## Próximos Passos

- Importar Capacidades
- Importar Aplicações
- Importar Processos
- Importar Relações Aplicação x Capacidade
- Importar Roadmap
- Validar dashboards
- Validar Rationalization
- Validar Capability Mapping
- Validar Roadmap de Transformação