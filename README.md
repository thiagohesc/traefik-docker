# 🚀 Traefik v3 com Docker

Este repositório configura o **Traefik v3.4.3** como proxy reverso dinâmico com Docker, pronto para produção com HTTPS automático via Let's Encrypt e suporte ao painel administrativo.

---

## ✅ Recursos habilitados

- 🔒 HTTPS automático com Let's Encrypt
- 🔁 Redirecionamento de HTTP para HTTPS
- 📊 Dashboard local (painel administrativo)
- 🐳 Integração com Docker via socket (somente leitura)
- 💾 Certificados persistentes em volume Docker
- 📦 Logs rotativos para evitar crescimento excessivo

---

## 📁 Estrutura de arquivos

```
.
├── docker-compose.yml
├── .env                  # Contém o e-mail usado para certificados
└── volumes/
```

---

## ⚙️ Como usar

### 1. Clone o repositório

```bash
git clone https://github.com/seuusuario/seurepositorio.git
cd seurepositorio
```

### 2. Crie o arquivo `.env` com seu e-mail

```env
TRAEFIK_EMAIL=seu-email@dominio.com
```

### 3. Suba o Traefik

```bash
docker compose up -d
```

---

## 🌐 Acessos

| Serviço   | Endereço                            | Exposição          |
|-----------|-------------------------------------|---------------------|
| HTTP      | http://seu-servidor:80              | Público             |
| HTTPS     | https://seu-servidor:443            | Público             |
| Dashboard | http://127.0.0.1:8080               | **Somente local**   |

> **Importante:** o dashboard está disponível apenas via `localhost`. Em produção, evite expor o painel sem autenticação.

---

## 📦 Exemplo de serviço com Traefik

```yaml
services:
  whoami:
    image: traefik/whoami
    container_name: whoami
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.whoami.rule=Host(`app.seudominio.com`)"
      - "traefik.http.routers.whoami.entrypoints=websecure"
      - "traefik.http.routers.whoami.tls.certresolver=le"
    networks:
      - traefik-net
```

---

## 🔐 Boas práticas

- Remova `--api.insecure=true` em produção.
- Use middleware de autenticação se quiser expor o dashboard.
- Faça backup do volume `traefik_certs` para não perder os certificados.

---

## 🧼 Para remover tudo

```bash
docker compose down -v
```

---

## 📚 Referências

- [📘 Traefik Docs](https://doc.traefik.io/traefik/)
- [🔧 Traefik Examples](https://github.com/traefik/traefik)

---