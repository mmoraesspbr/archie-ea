# Operação do Ambiente Archie EA

## Ambiente

Diretório base:

```text
C:\EA\archie-poc
```

Diretório da aplicação:

```text
C:\EA\archie-poc\source
```

Entrar na pasta:

```powershell
cd C:\EA\archie-poc\source
```

---

## Verificar Ambiente

## Ver todos os containers

```powershell
docker compose ps
```

## Ver todos os containers inclusive parados

```powershell
docker compose ps -a
```

## Ver imagens

```powershell
docker images
```

## Ver volumes

```powershell
docker volume ls
```

---

## Iniciar Ambiente

## Subir tudo

```powershell
docker compose up -d
```

## Subir apenas servidor

```powershell
docker compose up -d server
```

## Subir PostgreSQL

```powershell
docker compose up -d postgres
```

## Subir Redis

```powershell
docker compose up -d redis
```

---

## Parar Ambiente

## Parar todos os containers

```powershell
docker compose stop
```

## Derrubar ambiente

```powershell
docker compose down
```

## Derrubar ambiente removendo volumes

ATENÇÃO: apaga banco e dados.

```powershell
docker compose down -v
```

---

## Reiniciar Ambiente

```powershell
docker compose restart
```

## Reiniciar apenas o servidor

```powershell
docker compose restart server
```

---

## Logs

## Logs gerais

```powershell
docker compose logs
```

## Seguir logs em tempo real

```powershell
docker compose logs -f
```

## Logs do servidor

```powershell
docker compose logs server
```

## Logs PostgreSQL

```powershell
docker compose logs postgres
```

## Logs Redis

```powershell
docker compose logs redis
```

## Últimas 100 linhas do servidor

```powershell
docker compose logs server --tail 100
```

---

## Health Check

## Verificar saúde

Abrir:

```text
http://localhost:5000/health
```

Resultado esperado:

```json
{
  "status":"healthy"
}
```

---

## URLs do Ambiente

## Archie EA

```text
http://localhost:5000
```

## Health

```text
http://localhost:5000/health
```

## Adminer

```text
http://localhost:8080
```

---

## PostgreSQL

## Entrar no banco

```powershell
docker compose exec postgres psql -U postgres
```

## Listar bases

```sql
\l
```

## Conectar na base Archie

```sql
\c archie
```

## Listar tabelas

```sql
\dt
```

## Sair

```sql
\q
```

---

## Acesso ao Container

## Servidor

```powershell
docker compose exec server bash
```

## PostgreSQL (container)

```powershell
docker compose exec postgres bash
```

## Redis

```powershell
docker compose exec redis sh
```

---

## Backup

## Backup SQL simples

```powershell
docker compose exec postgres pg_dump -U postgres archie > C:\EA\archie-poc\backup\baseline.sql
```

## Backup com data

```powershell
docker compose exec postgres pg_dump -U postgres archie > C:\EA\archie-poc\backup\archie_$(Get-Date -Format yyyyMMdd).sql
```

---

## Backup Compactado (Recomendado)

## Gerar snapshot

```powershell
docker compose exec postgres pg_dump -U postgres -Fc archie > C:\EA\archie-poc\backup\archie-clean-install.backup
```

## Snapshot da PoC

```powershell
docker compose exec postgres pg_dump -U postgres -Fc archie > C:\EA\archie-poc\backup\archie-poc.backup
```

---

## Restore

## Restore SQL

```powershell
docker compose exec -T postgres psql -U postgres archie < C:\EA\archie-poc\backup\baseline.sql
```

## Restore Backup Compactado

```powershell
docker compose exec -T postgres pg_restore -U postgres -d archie < C:\EA\archie-poc\backup\archie-poc.backup
```

---

## Administração

## Criar administrador

```powershell
docker compose exec server python create_admin.py
```

---

## Atualização do Projeto

## Verificar branch

```powershell
git branch
```

## Atualizar código

```powershell
git pull
```

## Rebuild

```powershell
docker compose build
```

## Recriar ambiente

```powershell
docker compose up -d --force-recreate
```

---

## Comandos de Emergência

## Status completo

```powershell
docker compose ps -a
```

## Reiniciar tudo

```powershell
docker compose down
docker compose up -d
```

## Remover tudo

ATENÇÃO:

```powershell
docker compose down -v
docker system prune -a
```

---

## Estrutura da PoC

```text
C:\EA\archie-poc
│
├── backup
│
├── data
│
├── docs
│   ├── INSTALACAO-PoC-ARCHIE-EA.md
│   └── OPERACAO-ARCHIE-EA.md
│
└── source
```

---

## Checklist

## Instalação

- [x] Docker Desktop
- [x] PostgreSQL
- [x] Redis
- [x] Adminer
- [x] Archie EA
- [x] Admin criado

## Operação

- [ ] Backup inicial
- [ ] Importar Capacidades
- [ ] Importar Aplicações
- [ ] Importar Processos
- [ ] Importar Roadmap

## Avaliação

- [ ] Application Portfolio
- [ ] Capability Map
- [ ] Rationalization
- [ ] Technology Portfolio
- [ ] Roadmap
