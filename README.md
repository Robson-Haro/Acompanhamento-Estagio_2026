# Minerva Foods · Acompanhamento de Processo Seletivo — Estágio 2026

Site estático (sem build) com quatro abas: **Etapas do Processo Seletivo**, **Cronograma**, **Dados do Programa** e **Status da Triagem — Candidaturas Qualificadas**.

## Arquivos

| Arquivo | Função |
|---|---|
| `index.html` | Sistema principal com as abas. A aba "Status da Triagem" carrega o painel abaixo via `iframe`, com altura auto-ajustável. |
| `status_triagem.html` | Painel de candidaturas qualificadas (100% no navegador: lê P1 `.xlsx` e P2 `.csv`, recalcula tudo localmente). |
| `vercel.json` | Configuração de hospedagem estática e cabeçalhos de segurança. |

## Deploy no GitHub + Vercel

1. Crie um repositório no GitHub e envie os três arquivos (`index.html`, `status_triagem.html`, `vercel.json`) na raiz.
   ```bash
   git init
   git add index.html status_triagem.html vercel.json README.md
   git commit -m "Sistema de acompanhamento + Status da Triagem"
   git branch -M main
   git remote add origin https://github.com/SEU_USUARIO/SEU_REPO.git
   git push -u origin main
   ```
2. Em [vercel.com](https://vercel.com) → **Add New → Project** → importe o repositório.
3. Framework Preset: **Other** (é site estático, sem build). Deixe os campos de build vazios e publique.
4. A cada `git push`, a Vercel republica automaticamente.

## Atualização semanal dos dados

O painel de triagem já vem com a base atual embutida. Para atualizar, há dois caminhos:

- **Local (rápido):** abra a aba Status da Triagem e use o painel "Atualizar dados" para subir a P1 e a P2 da semana. O recálculo é no navegador — vale apenas para quem fez o upload.
- **Publicado para todos:** gere a nova versão do `status_triagem.html` com a base da semana e faça `git push`. A Vercel republica e todos passam a ver os números novos.

## Proteção de dados (LGPD)

- Os **nomes dos candidatos foram removidos** da base publicada; a tela exibe apenas agregados (cidade, curso, instituição, contagens).
- Ainda assim, trata-se de uma ferramenta **interna**. Recomenda-se restringir o acesso à URL (proteção por senha/SSO da Vercel ou hospedagem em ambiente autenticado), evitando exposição pública.

## Sobre o Supabase

O `index.html` já traz o cliente do Supabase preparado para sincronizar as partes colaborativas (anotações e cronograma) entre usuários. Hoje ele opera em **modo local** (cada navegador guarda o seu estado) porque as chaves não estão preenchidas. Para tornar essas partes compartilhadas pela equipe, preencha no `index.html`:

```js
const SB_URL = "https://SEU_PROJETO.supabase.co";
const SB_KEY = "SUA_ANON_PUBLIC_KEY";
```

e crie uma tabela `app_state` (colunas: `id` text (PK), `data` jsonb, `updated_at` timestamptz). O painel **Status da Triagem não depende do Supabase** — é somente leitura/cálculo.
