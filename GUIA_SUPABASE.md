# Ligar a nuvem (Supabase) — para todos verem a mesma versão

Faça **uma vez**. Depois disso, toda semana você só sobe as planilhas e clica em Recalcular: a equipe inteira passa a ver a versão nova, sem você mexer em mais nada.

> Nada aqui é de TI. São 4 passos, com copiar e colar.

## Passo 1 — Criar o projeto (grátis)
1. Acesse **supabase.com** e clique em **Start your project** (entre com Google, GitHub ou e-mail).
2. Clique em **New project**. Dê um nome (ex.: `minerva-triagem`), crie uma senha de banco (guarde), escolha a região **South America (São Paulo)** e clique em **Create new project**.
3. Espere cerca de 2 minutos até o projeto ficar pronto.

## Passo 2 — Criar a tabela (copiar e colar)
1. No menu à esquerda, clique em **SQL Editor** → **New query**.
2. Cole exatamente o bloco abaixo e clique em **Run**:

```sql
create table if not exists triagem_dados (
  id text primary key default 'atual',
  p1 jsonb,
  p2 jsonb,
  atualizado_em timestamptz default now()
);
alter table triagem_dados enable row level security;
create policy "ler"      on triagem_dados for select using (true);
create policy "inserir"  on triagem_dados for insert with check (true);
create policy "atualizar" on triagem_dados for update using (true);
```

## Passo 3 — Copiar os dois valores
1. No menu à esquerda, clique na engrenagem **Project Settings** → **API**.
2. Copie o **Project URL** (algo como `https://xxxx.supabase.co`).
3. Copie a chave **anon public** (uma sequência longa).

## Passo 4 — Ativar (escolha o mais fácil)
- **Mais fácil:** cole esses dois valores no chat com o Claude, que ele finaliza o `status_triagem.html` pronto e você só sobe no GitHub.
- **Ou faça você:** abra o `status_triagem.html` num editor de texto, procure por `COLE AQUI OS DOIS VALORES`, preencha `SB_URL` e `SB_KEY`, salve e suba no GitHub.

## Pronto. Como fica a rotina semanal
Suba a **P1** e a **P2** no bloco "Atualizar dados" → clique em **Recalcular**. Aparecerá *"Publicado para a equipe ✓"*. Quem abrir o link verá automaticamente a última versão, com a data no topo.

---

**Nota de segurança:** as permissões acima liberam leitura e escrita pela chave pública — adequado para uma ferramenta interna com acesso protegido (Vercel Deployment Protection) e dados agregados (sem nomes de candidatos). Se um dia quiser restringir mais, dá para adicionar autenticação depois, sem refazer nada.
