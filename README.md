# Hotelzinho

SaaS para hotel e creche de cães. Plano **Grátis** (5 pets, 10 reservas) e **Plus** (R$ 49/mês, ilimitado, WhatsApp, painel de vencimentos, CSV, contrato sem marca).

**Estado:** o `index.html` já tem login (entrar, criar conta, esqueci a senha, sair) e grava no Supabase quando o `config.js` está preenchido. Com o `config.js` vazio ele roda em modo local (dados no navegador, Plus simulado). Tudo isso **ainda não foi testado** contra um projeto real.

## As 13 peças e onde estão
| # | Peça | Onde |
|---|---|---|
| 1 | App | `index.html` |
| 2 | No ar 24h | GitHub Pages ou Vercel (abaixo) |
| 3 | Banco de dados | `supabase/schema.sql` |
| 4 | Login | Supabase Auth (Authentication → Providers) |
| 5 | Cada usuário só vê os seus dados | RLS no `schema.sql` |
| 6 | API | API automática do Supabase + funções em `supabase/functions` |
| 7 | Gateway de pagamento | `criar-assinatura` (Asaas) |
| 8 | Webhook | `webhook-asaas` |
| 9 | Cobrança mensal | assinatura `MONTHLY` do Asaas |
| 10 | E-mail transacional | Resend (webhook) + SMTP do Supabase Auth para "esqueci a senha" |
| 11 | GitHub | este repositório |
| 12 | Variáveis de ambiente | `.env.example` (chaves fora do código) |
| 13 | Monitoramento e backup | `.github/workflows/backup.yml` + UptimeRobot e Sentry |

## Passo a passo
1. Crie um projeto no Supabase e rode `supabase/schema.sql` no SQL Editor.
2. Crie conta no Asaas (comece pelo sandbox), no Resend (verifique seu domínio) e configure o SMTP do Supabase Auth com o Resend.
3. `supabase secrets set ...` com as chaves do `.env.example` (as 3 do Asaas/Resend/webhook), depois:
   - `supabase functions deploy criar-assinatura`
   - `supabase functions deploy webhook-asaas --no-verify-jwt` (o Asaas não envia o token do Supabase; a função se protege com `ASAAS_WEBHOOK_TOKEN`).
4. No Asaas, cadastre o webhook apontando para a função `webhook-asaas`, com o mesmo `ASAAS_WEBHOOK_TOKEN`.
5. Em Settings → Secrets do GitHub, crie `SUPABASE_DB_URL` para o backup. **Repositório privado.**
6. Preencha `config.js` com a URL e a chave **anon** do Supabase. No Supabase, em Authentication → URL Configuration, coloque a URL do seu site em Site URL e Redirect URLs (senão os links de confirmação e de senha não voltam pro site).
7. Publique o front-end (GitHub Pages: Settings → Pages → `main` / root) e troque `SEU-DOMINIO.com.br` nos arquivos.
8. Cadastre o domínio no Google Search Console e envie o `sitemap.xml`.

## Testar antes de lançar
1. Crie uma conta, confirme o e-mail e entre. 2. Cadastre 5 pets e veja o 6º ser bloqueado. 3. Crie uma segunda conta e confirme que ela não vê os dados da primeira. 4. No sandbox do Asaas, assine o Plus e veja o plano mudar após o pagamento. 5. Teste "esqueci a senha".

## Segurança
- Nunca coloque `SERVICE_ROLE_KEY`, `ASAAS_API_KEY` ou `RESEND_API_KEY` no front-end.
- Os limites do plano Grátis são aplicados no banco, não só na tela.
- Modelos de contrato são genéricos; peça revisão de um advogado.
