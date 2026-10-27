# Hotelzinho

Sistema web para hotel e creche de cães com plano Grátis (5 pets, 10 reservas) e plano Plus (ilimitado, WhatsApp, painel de vencimentos, relatório e contrato sem marca). O Plus é simulado nesta versão.

**Estado atual:** protótipo front-end. Os dados ficam no `localStorage` do navegador (sem login, banco ou pagamento).

## Publicar no GitHub Pages
1. Crie um repositório e envie estes arquivos na raiz.
2. Settings → Pages → Deploy from a branch → `main` / `(root)`.
3. Aponte seu domínio próprio (Settings → Pages → Custom domain).

## Antes de publicar (SEO)
- Troque `SEU-DOMINIO.com.br` em `index.html`, `robots.txt` e `sitemap.xml`.
- Cadastre o site no Google Search Console e envie o `sitemap.xml`.
- Crie páginas de conteúdo (ex.: "contrato de hospedagem para pets", "como montar uma creche canina").

## Roadmap (as 13 peças do vídeo)
| Peça | Sugestão |
|---|---|
| App + deploy 24h | Vercel ou Netlify |
| Banco de dados | Supabase (Postgres) |
| Login | Supabase Auth |
| Cada usuário vê só os seus dados | Row Level Security |
| API | Supabase / funções serverless |
| Pagamento + webhook | Asaas ou Mercado Pago |
| Cobrança mensal | Assinaturas recorrentes (Pix/boleto/cartão) |
| E-mail transacional | Resend |
| GitHub | Este repositório |
| Variáveis de ambiente | Nunca versionar chaves (`.env` já está no `.gitignore`) |
| Monitoramento | Sentry + UptimeRobot |
| Backup | Backup diário do banco |

## Aviso
Os modelos de contrato são genéricos e não substituem orientação jurídica.
