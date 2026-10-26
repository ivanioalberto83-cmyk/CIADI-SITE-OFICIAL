# CIADI — Site Oficial

Site institucional do Centro Integrado de Apoio e Desenvolvimento Individual (CIADI), em português de Angola.

## Ficheiros principais

- `INDEX.HTML.CIADI.html` — site institucional completo, com módulos do portal e configuração do Supabase.
- `index.html` — ponto de entrada que encaminha para o site institucional completo.
- `docs/INTEGRACAO-SITE-DASHBOARD-APP.md` — arquitetura partilhada, fluxos de integração e plano de homologação.

## Integração

O site, o Dashboard Windows e o CIADI+ Android estão configurados para o mesmo projeto Supabase. Isto estabelece uma base comum, mas não prova que cada fluxo esteja validado ponta a ponta. Consultar o documento de integração e executar os testes com contas e dados fictícios.

## Publicação

O repositório está na branch `main`. Para publicar através do GitHub Pages, abrir **Settings → Pages** e configurar **Deploy from a branch → main → /(root)**. Depois, confirmar o URL de publicação e testar a página no navegador.

A publicação neste repositório não altera automaticamente o alojamento DNS nem o site de produção em `www.ciadi.ao`. Verificar domínio, DNS e HTTPS separadamente.

## Antes de produção

- Validar textos, serviços, contactos e identidade visual com a direção do CIADI.
- Confirmar que o logótipo e as imagens são oficiais e que existe autorização para publicar fotografias identificáveis de crianças.
- Confirmar RLS e permissões no Supabase para todos os dados pessoais, clínicos, financeiros e SOS.
- Não incluir chaves privilegiadas, palavras-passe ou tokens no browser, APK ou repositório.
- Testar em dispositivos móveis, teclado, leitor de ecrã e navegadores suportados.
- Não anunciar conformidade APD/AGT nem integração completa antes de auditoria e testes documentados.

## Contacto institucional

- Email: geral@ciadi.ao
- Morada: Condomínio Jardim de Rosas, Talatona, Luanda, Angola
