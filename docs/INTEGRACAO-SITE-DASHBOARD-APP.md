# Integração do ecossistema CIADI: Site + Dashboard + CIADI+

## Objetivo

Os três clientes devem trabalhar sobre o mesmo projeto Supabase, com autenticação individual e autorização imposta no servidor. Não se devem copiar dados clínicos entre bases distintas nem confiar em filtros feitos apenas no navegador.

## Componentes verificados no código

- **Site institucional:** `INDEX.HTML.CIADI.html`; configura o projeto Supabase `egpkkttbcaukqnnxyhjv` e usa a chave publicável.
- **Dashboard Windows:** repositório `ivanioalberto83-cmyk/CIADI-`, ficheiro `CIADI_DASHBOARD_FINANCEIRO.html`; usa o mesmo URL de Supabase e a mesma chave publicável.
- **CIADI+ Android:** repositório `ivanioalberto83-cmyk/ciadi-plus-android`; `SupabaseConfig.kt` aponta para o mesmo projeto e o repositório usa Auth, PostgREST e um gestor Realtime.
- **Entrada do site:** `index.html` encaminha para o site institucional completo em `INDEX.HTML.CIADI.html`.
- **Triagem pública:** o formulário usa a RPC existente `ciadi_registrar_triagem_publica`, que valida os campos e regista em `ciadi_triagens`; não deve inserir diretamente na tabela interna de triagens.

Esta verificação confirma configuração comum, não confirma que todos os fluxos funcionem ponta a ponta em produção.

## Fluxos de integração a validar

| Fluxo | Origem | Destino esperado | Validação necessária |
|---|---|---|---|
| Login | Site / Android / Dashboard | Supabase Auth | Utilizador ativo e perfil/role válidos |
| Pedido de triagem pública | Formulário do site | RPC `ciadi_registrar_triagem_publica` → `ciadi_triagens` | Código de referência devolvido e pedido visível à gestão via RLS |
| Agendamento clínico | Portal ou Dashboard | `agendamentos` | Mesmo registo visível aos perfis autorizados |
| Assistido/paciente | Dashboard | Site/Android | Associação correta e política RLS aplicada |
| Formulários clínicos e A.T. | Dashboard / Android | Tabelas clínicas CIADI | Guardar, reabrir, permissões e histórico |
| SOS | CIADI+ Android | RPC de criação SOS e fluxo de despacho | Criar protocolo, receber no Dashboard, reconhecer e auditar |
| Chat/notificações | CIADI+ Android | Tabelas CIADI Chat/Notificações + Realtime | Enviar, receber, deduplicar e respeitar autorização |
| Conteúdo institucional | Dashboard | `conteudos_site` e conteúdos relacionados | Publicação apenas de conteúdo autorizado no site |

## Regras de segurança obrigatórias

1. Apenas chave **publishable/anon** pode estar no browser ou na aplicação Android. Nunca incorporar `service_role`, segredos de Edge Functions, palavras-passe ou tokens de utilizadores no repositório.
2. Ativar e verificar RLS em todas as tabelas com dados pessoais, clínicos, financeiros ou de SOS. O acesso deve ser autorizado no banco, não apenas escondido na interface.
3. Não tornar públicos documentos clínicos, fichas, mensagens, contactos de emergência, dados de pacientes ou registos financeiros.
4. O fluxo SOS deve validar a identidade, a autorização e o destinatário no servidor; manter registo de entrega, falha, reconhecimento e auditoria.
5. Mudanças de esquema só através de migrações versionadas, com backup verificável, compatibilidade retroativa e plano de rollback.
6. Não usar pacientes reais em testes. Usar contas e registos fictícios de homologação.
7. Não declarar conformidade APD, AGT ou prontidão de produção apenas porque os três clientes usam o mesmo Supabase.

## Plano de homologação ponta a ponta

- [ ] Confirmar que os três clientes usam o projeto esperado e que nenhum segredo privilegiado está no código distribuído.
- [ ] Entrar com uma conta de teste de cada perfil e confirmar logout/expiração de sessão.
- [ ] Submeter um pedido de triagem fictício pelo site e confirmar o código de referência e a leitura no Dashboard por um perfil de gestão.
- [ ] Criar um agendamento fictício e verificar a leitura apenas pelos perfis autorizados.
- [ ] Criar e consultar um formulário clínico fictício; verificar permissões, estado, histórico e anexos privados.
- [ ] Enviar SOS de teste no Android; verificar receção no Dashboard, protocolo, reconhecimento e trilho de auditoria.
- [ ] Enviar mensagem de teste e confirmar Realtime no destinatário autorizado, sem duplicados.
- [ ] Publicar conteúdo institucional de teste e verificar a renderização no site.
- [ ] Testar RLS com contas sem vínculo e confirmar que não conseguem ler dados de outros pacientes.
- [ ] Confirmar backups e ensaiar recuperação antes de qualquer mudança de produção.

## Estado

**Configuração comum encontrada:** sim.

**Fluxos completos comprovados em ambiente real:** ainda não.

**Publicação do site, instalação atualizada do Dashboard e APK atualizado validados neste documento:** não. Cada artefacto deve ser construído e testado separadamente antes de ser anunciado como pronto.
