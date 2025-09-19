# Chatwoot-e-Evolution-SaaS

> Plataforma SaaS baseada no **Chatwoot** e **Evolution API** para atendimento multicanal com automações, chatbots e integração em larga escala.

---

## Sumário

* [Sobre](#sobre)
* [Funcionalidades](#funcionalidades)
* [Requisitos](#requisitos)
* [Arquitetura](#arquitetura)
* [Instalação rápida (Docker)](#instalacao-rapida-docker)
* [Instalação em VPS](#instalacao-em-vps)
* [Configuração](#configuracao)
* [Endpoints principais](#endpoints-principais)
* [Exemplos de uso](#exemplos-de-uso)
* [Segurança](#seguranca)
* [Deploy / Produção](#deploy--producao)
* [Contribuição](#contribuicao)
* [Licença](#licenca)
* [Contato](#contato)

---

## Sobre

O **Chatwoot-e-Evolution-SaaS** é um projeto que combina o poder do **Chatwoot** (atendimento omnichannel open source) com a **Evolution API** para WhatsApp, possibilitando a criação de uma plataforma SaaS completa de atendimento, gestão de múltiplas empresas e automação.

---

## Funcionalidades

* Gerenciamento de múltiplos workspaces (multi-empresa)
* Atendimento omnichannel (WhatsApp via Evolution API, Facebook, Instagram, Telegram, E-mail, Webchat)
* API REST para criação/gestão de usuários, empresas e conexões
* Autenticação JWT / API Key
* Integração com Evolution API para WhatsApp Cloud e dispositivos
* Automação com fluxos e bots (n8n / Node-RED opcionais)
* Sistema de logs e métricas
* Configuração para deploy SaaS (multi-tenancy)

---

## Requisitos

* Docker >= 20.x
* Docker Compose >= 1.29 (ou Compose v2)
* PostgreSQL (banco principal do Chatwoot)
* Redis (filas e cache)
* Node.js 18+ (para serviços auxiliares)
* VPS com no mínimo 2GB RAM / 2 CPU (recomendado 4GB RAM para produção)
* Certificados SSL (Let's Encrypt recomendado)

---

## Arquitetura

Fluxo simplificado:

1. **Cliente** acessa a instância SaaS via navegador ou API.
2. **Chatwoot** gerencia usuários, workspaces e canais.
3. **Evolution API** conecta dispositivos WhatsApp e provê comunicação.
4. **PostgreSQL** armazena dados persistentes (usuários, conversas).
5. **Redis** gerencia filas e eventos em tempo real.
6. **Reverse Proxy (Nginx/Traefik)** fornece SSL e roteamento.

---

## Instalação rápida (Docker)

Clone o repositório e suba os serviços:

```bash
git clone https://github.com/seu-usuario/chatwoot-e-evolution-saas.git
cd chatwoot-e-evolution-saas
cp .env.example .env

# Edite o .env com variáveis (chaves JWT, API Keys, Evolution configs)

# Suba os containers
docker compose up -d --build
```

Verifique se os serviços `chatwoot`, `evolution`, `db`, `redis` e `proxy` estão rodando.

---

## Instalação em VPS

1. Configure o servidor (Ubuntu recomendado):

```bash
sudo apt update && sudo apt upgrade -y
```

2. Instale Docker + Docker Compose.

3. Clone o repositório, configure `.env` e suba com `docker compose up -d`.

4. Configure proxy reverso (Nginx/Traefik) e SSL com Let's Encrypt.

5. Aponte os domínios:

   * `app.seudominio.com` → Chatwoot
   * `api.seudominio.com` → Evolution API

---

## Configuração

Exemplo de variáveis no `.env`:

```
POSTGRES_USER=chatwoot
POSTGRES_PASSWORD=senha123
POSTGRES_DB=chatwoot_prod
REDIS_URL=redis://redis:6379

EVOLUTION_API_KEY=sua_chave_api
EVOLUTION_WEBHOOK_URL=https://api.seudominio.com/webhook

JWT_SECRET=uma_chave_jwt_secreta
RAILS_ENV=production
```

---

## Endpoints principais (exemplos)

### Chatwoot

* `POST /api/v1/accounts` — Criar workspace/conta
* `POST /api/v1/accounts/:id/users` — Adicionar usuário a um workspace
* `POST /api/v1/accounts/:id/channels/whatsapp` — Conectar WhatsApp via Evolution

### Evolution API

* `POST /sessions/add` — Conectar número/dispositivo WhatsApp
* `GET /sessions` — Listar sessões ativas
* `POST /message/send` — Enviar mensagem WhatsApp

---

## Exemplos de uso (cURL)

Criar sessão no Evolution API:

```bash
curl -X POST https://api.seudominio.com/sessions/add \
  -H "Authorization: Bearer $EVOLUTION_API_KEY" \
  -d '{"sessionName": "empresa1", "number": "5511999999999"}'
```

Enviar mensagem pelo WhatsApp:

```bash
curl -X POST https://api.seudominio.com/message/send \
  -H "Authorization: Bearer $EVOLUTION_API_KEY" \
  -d '{"sessionName": "empresa1", "number": "5511888888888", "text": "Olá, tudo bem?"}'
```

---

## Segurança

* Use HTTPS obrigatório em produção.
* Configure **API Keys** e rotacione periodicamente.
* Armazene credenciais no banco de forma segura.
* Restrinja acessos administrativos por IP.

---

## Deploy / Produção

* Configure backups automáticos do PostgreSQL.
* Monitore filas Redis e logs do Chatwoot/Evolution.
* Use balanceamento de carga se houver muitos clientes.
* Automatize deploy com CI/CD.

---

## Contribuição

1. Fork o repositório
2. Crie uma branch `feature/nome`
3. Faça commits claros
4. Abra PR com descrição detalhada

---

## Licença

Este projeto é distribuído sob licença MIT — veja `LICENSE`.

---

## Contato

📧 E-mail: `sergioharpazo@gmail.com`
🌐 Site: https://loja.antoniooliveira.shop/

---

> Este README pode ser adaptado para incluir exemplos reais de `docker-compose.yml` e fluxos de configuração. Se quiser, posso gerar os arquivos prontos para deploy imediato.
