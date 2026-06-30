# 📊 Dashboard Financeiro — Setup GitHub Pages

## O que você vai ter

- Dashboard interativo no navegador
- Dados sincronizados automaticamente com a planilha
- Sem custo (GitHub Pages é gratuito)
- Atualização em tempo real via Google Apps Script

---

## Estrutura do repositório

```
dashboard-financeiro/
├── index.html      ← Dashboard principal
├── dados.json      ← Gerado automaticamente pelo Apps Script
└── README.md
```

---

## Passo 1 — Criar o repositório no GitHub

1. Acesse https://github.com/new
2. Nome do repositório: `dashboard-financeiro`
3. Visibilidade: **Público** (necessário para Pages gratuito)
4. Clique em **Create repository**

---

## Passo 2 — Subir os arquivos

**Via GitHub Desktop** (já que o git não está no PATH na máquina PUC):

1. Abra o GitHub Desktop
2. File → Add Local Repository → selecione a pasta com os arquivos
3. Adicione `index.html` e `dados.json`
4. Commit com mensagem "Primeiro commit — dashboard"
5. Push to origin

---

## Passo 3 — Ativar GitHub Pages

1. No repositório, clique em **Settings**
2. Menu lateral: **Pages**
3. Source: **Deploy from a branch**
4. Branch: `main` / `/ (root)`
5. Clique em **Save**

Aguarde ~1 minuto. Seu dashboard estará em:
`https://kesleysanzio.github.io/dashboard-financeiro/`

---

## Passo 4 — Configurar o Apps Script (atualização automática)

### 4.1 Criar token do GitHub

1. GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)
2. Generate new token → nome: "Dashboard Financeiro"
3. Permissões: marque apenas `repo` (acesso completo ao repositório)
4. Copie o token gerado (começa com `ghp_`)

### 4.2 Configurar o script na planilha

1. Abra sua planilha no Google Sheets
2. Menu: **Extensões → Apps Script**
3. Apague o código existente e cole o conteúdo de `apps_script.js`
4. Preencha as constantes no topo:

```javascript
const GITHUB_TOKEN  = 'ghp_SEU_TOKEN_AQUI';
const GITHUB_USER   = 'kesleysanzio';
const GITHUB_REPO   = 'dashboard-financeiro';
```

5. Salve (Ctrl+S)
6. Clique em **Executar** → selecione a função `setup` → autorize as permissões

### 4.3 Configurar gatilho automático

1. Menu lateral: ⏰ **Gatilhos** (ícone de relógio)
2. Clique em **+ Adicionar gatilho**
3. Configurações:
   - Função: `publicarDados`
   - Evento: **De planilha** → **Ao editar** (ou "Com base em tempo" → A cada hora)
4. Salve

**Pronto!** Sempre que você editar a planilha, o `dados.json` é atualizado no GitHub e o dashboard reflete os novos dados em instantes.

---

## Passo 5 — Apontar o dashboard para o JSON do GitHub

No `index.html`, na linha:
```javascript
const DATA_URL = null; // modo offline com dados embutidos
```

Troque para:
```javascript
const DATA_URL = 'https://raw.githubusercontent.com/kesleysanzio/dashboard-financeiro/main/dados.json';
```

Faça um novo commit e push. O dashboard agora carrega sempre do GitHub.

---

## Atualização manual

Se precisar forçar uma atualização sem editar a planilha:

1. Apps Script → Selecione `publicarDados` → Clique em ▶ Executar

---

## Observações de segurança

⚠️ **Não comite o `apps_script.js` com o token.** Ele fica apenas no Apps Script da planilha.

⚠️ O `dados.json` é **público** (visível a quem souber a URL). Não inclua senhas ou dados de clientes — os dados de clientes da aba "Dados de clientes" **não** são exportados pelo script.

---

## Suporte

Dashboard gerado por Claude (Anthropic) — Junho/2026.
