# Commerce Control Plane

**Arquitetura de produto e oferta única para comércio agêntico**

| | |
|---|---|
| Status | Proposta v0.1, 4 de setembro de 2026 |
| Base técnica | Artigo *The anatomy of effective commerce agents* (Anthropic) e o repositório de referência [`anthropics/commerce-agents`](https://github.com/anthropics/commerce-agents) (Apache 2.0, commit `fd4d592` de 31/08/2026) |
| Base de produto | Análise própria da referência e a proposta de "camada transacional e de governança para agentes" (Transaction Kernel, Operator, Concierge, Gateway, Mandate Engine), revisada |

## Resumo

O ativo do repositório de referência não é a conversa. É um kernel embrionário de transação controlada: **o modelo propõe, o código valida, uma pessoa ou uma política autoriza, o backend executa.** O produto é o que a Anthropic deixou explicitamente de fora: autenticação, conectores, ledger durável, motor de políticas, console de aprovação, compensação, evals, canais, LGPD, multi-tenant e billing.

A oferta única para o mercado de e-commerce:

> **Integre seu comércio uma vez. Deixe três tipos de agente agirem sobre ele, o do seu time, o do seu cliente e os do mercado, sob uma política, um ledger e uma suíte de evals.**

Nenhuma alternativa cobre as três superfícies com governança independente de plataforma. O agente nativo da plataforma de e-commerce cobre só a própria loja. As plataformas de atendimento cobrem só o canal. A Stripe e os protocolos (UCP, ACP, AP2) cobrem só o checkout.

Ordem de construção: **Commerce Operator primeiro, extraindo o kernel dele.** Depois o Shopping Concierge com checkout controlado. Gateway por parceria. Mandatos e reposição recorrente por último.

---

## 1. A oferta única

### 1.1 Três forças criam o mercado agora

1. **Agentes externos vão comprar.** O UCP (Google com Shopify, Etsy, Wayfair, Target e Walmart, anunciado na NRF em janeiro de 2026), o ACP (OpenAI e Stripe) e o AP2 (Google, mandatos de compra) definem como um agente que a empresa não controla descobre, compara e conclui compras. O lojista precisa ficar legível e transacionável para eles.
2. **As plataformas lançam agentes nativos.** Shopify Agentic Storefronts, VTEX AI Workspace, Stripe Agentic Commerce Suite. Eles cobrem a própria loja, tendem a operar numa plataforma só, e não governam ações sobre ERP, OMS, PIM, CRM e mídia.
3. **Empresas médias e grandes no Brasil operam stacks heterogêneas.** VTEX, Shopify ou Linx na loja; SAP ou TOTVS no back office; OMS, PIM, CRM e plataformas de mídia ao redor. Quem quer um agente que aja de verdade precisa de governança que cruze sistemas. Ninguém entrega isso como produto.

### 1.2 A proposição

**Integre uma vez. Três superfícies. Uma política. Um ledger.**

| Elemento | O que significa |
|---|---|
| Uma integração | Modelo canônico de comércio e conectores que implementam dois contratos: `StorefrontBackend` (loja) e `MerchantBackend` (operação). |
| Três superfícies | **Commerce Operator** para o time. **Shopping Concierge** para o cliente. **Agentic Commerce Gateway** para agentes externos (ChatGPT, Gemini, Copilot). |
| Uma política | Motor de guardrails, aprovação e níveis de autonomia, configurável por cliente, canal e tipo de ação. |
| Um ledger | Toda ação proposta, validada, aprovada, executada, verificada e compensada, com quem, quando e com base em quê. |
| Uma suíte de evals | Garantia contínua de comportamento, com casos por fluxo e fixtures hostis, rodando em CI e sobre amostra de produção. |

### 1.3 Por que é única

| Alternativa | Cobre | Não cobre | Nossa posição |
|---|---|---|---|
| Agente nativo da plataforma (Shopify, VTEX) | A própria loja: catálogo, busca, promoções, distribuição para agentes | Sistemas fora da plataforma; governança multi-sistema; agentes de back office sobre ERP e OMS; evals | Multi-plataforma, back office e governança. Usamos o agente nativo onde ele existir. |
| Plataformas de atendimento conversacional | Canal, atendimento, campanhas | Transação governada; operação do lojista; ledger | Ação transacional com proveniência, aprovação e trilha. |
| Stripe e protocolos (ACP, UCP, AP2) | Checkout, pagamento, feed para agentes externos | Agentes próprios; operação; política por canal; atribuição entre agentes | Consumimos os protocolos. Adicionamos política, atribuição e ledger. |
| Construção própria ou consultoria | Tudo, uma vez | Produto, evals, manutenção de conectores | Plataforma mais implementação, com evals como serviço contínuo. |

### 1.4 Para quem

**Cliente ideal.** Varejistas e marcas médias e grandes no Brasil, com loja em VTEX, Shopify Plus, Linx ou Adobe Commerce, back office em SAP, TOTVS ou similar, e um time de e-commerce que hoje aprova preço, promoção e reposição por planilha e e-mail. Também: telecom e serviços em que "trocar de plano" é a transação; distribuidores B2B com tabela por contrato.

**Não é o alvo.** PME com uma plataforma só (o agente nativo vai atendê-la). Marketplaces que querem ser a interface do consumidor. Quem procura só um chatbot.

---

## 2. Arquitetura em camadas

```
 SUPERFÍCIES     Site e app │ WhatsApp │ Portal do operador │ E-mail, Slack (digest) │ Agentes externos
                      │           │            │                    │                       │
 AGENTES         Shopping Concierge        Commerce Operator (+ delegado analítico)     Gateway
                 5 fluxos, UI como tool     5 fluxos, UI como tool                      UCP · ACP · AP2
                      │ propõe (carrinho, checkout)   │ propõe (stage_*)                │ sessão de checkout
 ┌────────────────────┴───────────────────────────────┴─────────────────────────────────┴────────────┐
 │  TRUST & TRANSACTION KERNEL                                                                       │
 │  identidade · proveniência · fencing · políticas · ledger · aprovação · execução                  │
 │  verificação · compensação · memória e LGPD · mandatos · auditoria e custo por tarefa            │
 └────────────────────┬───────────────────────────────┬──────────────────────────────────────────────┘
                      │ lê / executa após autorizar    │
 MODELO CANÔNICO   StorefrontBackend               MerchantBackend
 + CONECTORES      11 métodos + 3 opcionais        8 leituras · 5 stage_* · 3 ciclo de vida · 3 opcionais
                      │ chama a API existente          │
 SISTEMAS        Plataforma de e-commerce │ ERP, OMS │ PIM │ CRM e automação │ Mídia e analytics │ PSP, Pix

 TRANSVERSAL     Console (aprovação, ledger, políticas) · Assurance (evals, policy packs) · Observabilidade
```

### 2.1 As camadas

| Camada | Conteúdo | Origem |
|---|---|---|
| Superfícies | Widget no site e no app, WhatsApp, portal do operador, digest por e-mail ou Slack, job runner para o digest agendado, agentes externos | Web: fork de `examples/web-shared`. WhatsApp e digest por canal: novos. |
| Agentes | Um modelo por conversa. Skills carregadas sob demanda. Ferramentas de apresentação validadas no servidor. Delegado analítico read-only com orçamento. Runtime sobre a Messages API com cache em três segmentos. | Fork de `shopping-agent/`, `merchant-agent/` e `commerce-common/`. |
| Kernel | Identidade, proveniência, fencing, políticas, ledger, aprovação, execução, verificação, compensação, memória e LGPD, mandatos, auditoria | Metade herdada dos gates e do executor do repo, metade nova. Detalhe na seção 3. |
| Modelo canônico e conectores | As entidades de comércio e os dois contratos de backend, implementados por conector | Contratos do repo. Conectores novos. |
| Gateway | Exposição do comércio a agentes externos via UCP, ACP e AP2 | Novo, por parceria ou adaptador. Seção 6. |
| Transversal | Console, assurance (evals e policy packs), observabilidade e custo por tarefa | Formato de evals do plugin. Runner, console e painéis novos. |

### 2.2 Princípios de arquitetura

Vêm do artigo e viram regra de produto.

- **Um modelo por conversa.** Sem roteador de intenção, sem enxame de subagentes. Subagente só para análise pesada, autocontida e read-only.
- **A ferramenta chama o sistema que existe.** Busca, preço, estoque e elegibilidade são do cliente. Nada disso é reimplementado.
- **UI é chamada de ferramenta.** O modelo nomeia ids; o servidor valida o payload, junta os registros e emite o componente. Um id sem proveniência é descartado.
- **Prompt e ferramentas são os mesmos bytes em todo turno.** Dados por requisição vão na cerca, depois do breakpoint de cache.
- **Toda escrita passa por gate de proveniência e cap em código.** `checkout` não cobra. Escrita do lojista só se aplica com aprovação. Conteúdo de terceiros é dado cercado, não instrução.
- **Custo por tarefa concluída**, não por chamada. Modelo e esforço são escolhidos por sweep na suíte de evals.

---

## 3. Trust & Transaction Kernel

### 3.1 O ciclo de vida de uma ação

```
 PROPOR → VALIDAR → PRÉ-VISUALIZAR → AUTORIZAR → EXECUTAR → VERIFICAR → MONITORAR → (COMPENSAR)
                                     pessoa │ política                                    │
                                                                                           └─→ nova ação ligada à original
```

| Estado | O que acontece | No repositório hoje | O que construímos |
|---|---|---|---|
| Propor | O agente chama `stage_*` (operação) ou `add_to_cart` e `checkout` (cliente) | Ferramentas `stage_*`; `ChangeLedger.stage` em memória | O mesmo, sobre ledger durável |
| Validar | Proveniência (só ids devolvidos por ferramenta nesta sessão), caps, guardrails, campos protegidos | `gates.py` dos dois papéis; `check_guardrails` | Motor de políticas por cliente, canal e tipo de ação, versionado |
| Pré-visualizar | Antes e depois por item; margem quando o custo é conhecido; notas de guardrail | `StagedChange.margin_*`; `present_change_preview` | Previews por tipo de ação. Simulação preditiva só como módulo opcional, quando houver dados de demanda |
| Autorizar | Uma pessoa aprova no console, ou uma política autoriza dentro de limites (nível 3) | Marca `host_approve`; `require_host_approval` | Console com RBAC, maker-checker, delegação, SLA; aprovação por política com registro |
| Executar | Re-valida guardrails na config vigente, escreve no sistema real, idempotente, serializado | `apply_change` re-roda guardrails; lock por sessão; chave de idempotência prescrita em `docs/backends.md`, não implementada | Chaves de idempotência, fila por cliente, retry e timeout |
| Verificar | Relê o sistema e compara com o que foi aprovado | Não existe | Novo |
| Monitorar | Janela de impacto por mudança (vendas, margem, conversão); alerta; recomendação de manter ou reverter | Não existe | Novo, sobre as leituras do `MerchantBackend` |
| Compensar | Desfaz como nova mudança staged, ligada à original | Não existe | Novo |

Do lado do cliente, o mesmo ciclo: `add_to_cart` é Propor; os gates de carrinho são Validar; o cartão de checkout é Pré-visualizar; o pagamento no host é Autorizar e Executar; a confirmação volta como evento de app na próxima rodada. Autonomia de nível 3 no lado do cliente exige mandato (seção 3.4).

### 3.2 Componentes

| Componente | Função | Base no repo | Novo |
|---|---|---|---|
| Identidade e sessão | O host autentica e liga um principal à sessão. Nenhum argumento de ferramenta leva id de usuário. Credenciais ficam com o host, nunca com o modelo. | `demo_common/sessions.py` (demo) | SSO e OIDC por cliente; RBAC (operador, aprovador, admin); convidado como principal |
| Proveniência | Escrita só sobre ids que uma ferramenta devolveu nesta sessão; família com opções é retida e apontada para as variantes | `shopping_agent/gates.py`, `merchant_agent/gates.py` | Estado de proveniência persistido por sessão com versão |
| Fencing | Conteúdo de terceiros sanitizado, cercado e limitado antes de o modelo ler | `commerce_common/fencing.py` | Rótulos e limites por cliente |
| Motor de políticas | Os valores de `config.py` viram policy packs versionados: itens por mudança, delta de preço, profundidade de promoção, tamanho de reposição, orçamento de campanha, campos protegidos, quantidade e linhas de carrinho | Valores de demonstração nos dois `config.py`; efetivos no apply | Por cliente, canal e ação; versionamento; efeito no apply mantido |
| Ledger | Registro append-only por cliente: eventos do ciclo, ids lidos, política vigente, aprovador, chave de idempotência, resultado, compensação | `StagedChange` (atores, carimbos, notas, margem); eventos de `streaming.py` | Persistência, consulta, replay, retenção |
| Aprovação | Inbox de mudanças pendentes, cartão de preview, aprovar, editar, descartar, lote, delegação | Marca do host; `ChangePreviewCard` | Console completo com RBAC e trilha |
| Execução | Idempotência, serialização por sessão e cliente, retry, timeout; erro de domínio relatado ao modelo como próximo passo, não como falha | Lock por sessão; `domain_error`; `NotOffered`, `Unavailable`, `ChangeNotApplicable` | Chaves de idempotência e fila |
| Verificação e monitoramento | Pós-apply: releitura e comparação; janela de impacto; alertas | Não existe | Novo |
| Compensação | Reversão como nova ação staged | Não existe | Novo |
| Memória e LGPD | Fatos tipados (preferência, restrição, contexto), chave até 64 caracteres, valor até 200; filtro que recusa identificadores; extração assíncrona só do texto da troca; retenção; ver e apagar | `commerce_common/memory.py` | Consentimento, portal do titular, integração com exclusão de conta |
| Mandatos | Autorização persistente: titular, agente, escopo (lojas, categorias), limites por compra e por mês, substituições, validade, limiar de aprovação. Avaliado em Propor e Executar. Traduzido para AP2, Pix Automático e pedido de compra. | Não existe; a proveniência é por sessão e a memória recusa identificadores de propósito | Novo, fase 5 |
| Auditoria e custo | Uma linha por chamada de modelo (rodada, modelo, stop reason, uso, tempo, digest da sessão); leitura de cache; custo por tarefa concluída | `log_model_call` em `commerce_common/turn.py`; `cache_read_input_tokens` em `turn_complete` | Agregação por cliente, fluxo e tarefa |

### 3.3 Níveis de autonomia

Autonomia é uma configuração por tipo de ação, não um interruptor global.

| Nível | O agente pode | Como o kernel implementa |
|---|---|---|
| 0 · Informar | Responder, pesquisar, comparar | Só leituras; switches `enable_*` desligados removem as escritas |
| 1 · Preparar | Montar carrinho, plano, promoção ou pedido | Escritas de carrinho com gate e cap; `stage_*` sem apply |
| 2 · Propor | Criar mudança pendente e pedir aprovação específica | `require_host_approval` ligado; console |
| 3 · Executar sob mandato | Executar dentro de limites previamente autorizados | Aprovação por política, com registro; mandato no lado do cliente |
| 4 · Otimizar continuamente | Detectar, executar, monitorar, reajustar | Só com Verificar, Monitorar e Compensar maduros e domínio reversível |

Defaults recomendados por ação:

| Ação | Início | Meta em 12 meses |
|---|---|---|
| Busca, comparação, pesquisa de compra | 0 | 0 |
| Carrinho | 1 | 1 |
| Checkout | 1 (handoff) | 3 em reposição recorrente, com mandato |
| Digest e insights de operação | 0 | 0 |
| Conteúdo de listagem | 2 | 3 para correções de atributo dentro de regra |
| Reposição | 2 | 3 abaixo de um limite de quantidade e valor |
| Preço | 2 | 2 (3 só em faixas estreitas e reversíveis) |
| Promoção e campanha | 2 | 2 |

---

## 4. Os dois agentes

### 4.1 Commerce Operator

O primeiro produto. Responde "o que está afetando a receita hoje?", diagnostica, propõe ações com preview, encaminha para aprovação, executa o que foi autorizado e acompanha o impacto.

| Aspecto | Conteúdo |
|---|---|
| Fluxos (skills) | `performance-insights`, `catalog-listings`, `inventory-operations`, `pricing-promotions`, `marketing-campaigns` |
| Leituras | Snapshot do negócio, série de métrica, campanhas, busca e detalhe de listagem, alertas de estoque, exceções de pedido, contexto de preço; consulta SQL read-only pelo delegado analítico, com orçamento de linhas, caracteres e tempo |
| Escritas | `stage_listing_update`, `stage_price_update`, `stage_inventory_action`, `stage_promotion`, `stage_campaign`; `apply_change` e `discard_change` sobre ids da sessão |
| UI | `present_digest`, `present_metrics`, `present_change_preview`, mais extensões por vertical (calendário de ocupação, mix de planos, pacing de eventos, nos exemplos) |
| Superfícies | Portal web (fork do frame `portal/` de `web-shared`); digest agendado sem operador (`run_morning_digest.py`) entregue por e-mail, Slack ou WhatsApp; perguntas por Slack ou Teams |
| Modelo | Opus para o operador e Haiku para memória, como o repo; confirmado por sweep de custo por tarefa |
| O que muda do repo | Ledger durável; console de aprovação com RBAC; policy packs por cliente; skills e lexicons em português; monitoramento pós-apply; compensação; conectores reais |
| Piloto mínimo | Só as oito leituras: digest, insights e análise, sem caminho de escrita. O repo suporta esse corte sem mudar um byte do prompt. |

KPIs: tempo entre detecção e ação; percentual de recomendações aprovadas; receita ou margem incremental atribuível; redução de ruptura e de estoque parado; tempo para criar promoção ou campanha; ações revertidas; violações de política; custo por tarefa concluída.

### 4.2 Shopping Concierge

O agente que o cliente final usa no site, no app e no WhatsApp: pesquisa, compara, monta planos, enche o carrinho, responde sobre pedido e política, lembra o que o cliente disse.

| Aspecto | Conteúdo |
|---|---|
| Fluxos (skills) | `search-discovery`, `purchase-research`, `planning-goals`, `customer-care`, `memory-personalization` |
| Ferramentas | Busca, detalhe, carrinho (quatro), preferências, pedidos (dois), políticas, fulfillment; opcionais: handoff de checkout, contexto de conta, disclosure |
| UI | `present_products`, `present_comparison`, `present_plan`, `present_guide`, `present_order_status`, `checkout`, `present_disclosure`, `present_suggestions` |
| Canais | Widget web (fork do frame `storefront/`); WhatsApp por adaptador que traduz cada componente para lista, botões ou mensagem de catálogo, e para texto quando não cabe; app nativo pelo mesmo stream de eventos |
| Checkout controlado | `checkout` renderiza o carrinho. O cartão leva à rota própria ou ao checkout hospedado (`checkout_handoff`, uma URL por seller no marketplace). O pagamento acontece no host e volta como evento de app. Nada no agente cobra. |
| Localização | Lexicons dos gates de grounding em português (no repo são em inglês: "return", "refund", "tracking"); skills adaptadas ao comércio brasileiro (parcelamento, frete, Pix, nota fiscal, direito de arrependimento do CDC); disclosures redigidas no servidor para categorias reguladas |
| Modelo | Sonnet para a conversa e Haiku para memória, como o repo |
| O que muda do repo | Adaptador de WhatsApp; identidade real (convidado, login, vínculo de conta); memória com consentimento e portal do titular; conectores; localização |

KPIs: resolução sem humano; pedidos assistidos e conversão contra grupo de controle; ticket médio; recompra; taxa de afirmação sem fonte em amostra de produção; custo por conversa concluída.

---

## 5. Modelo canônico e conectores

### 5.1 Entidades

Produto (plain, família, variante), oferta e preço, disponibilidade, opções de fulfillment, carrinho, pedido, política, disclosure, preferências e contexto de conta (lado da loja); listagem, contexto de preço, alerta de estoque, exceção de pedido, snapshot e série de métrica, campanha, mudança staged (lado da operação). Fato de memória e sessão são do kernel.

O modelo de produto segue `docs/backends.md` do repo: uma **família** tem opções (tamanho, cor), uma **variante** tem valores de opção e aponta para a família, um produto **plain** não tem opções. Carrinho, preço e reposição exigem variante; pausa, promoção e edição de conteúdo aceitam a família. Uma família com mais de sessenta variantes é servida como várias famílias. Isso é a primeira coisa que quebra numa integração real, e por isso a auditoria de catálogo abre o diagnóstico comercial.

### 5.2 Os dois contratos

| Contrato | Métodos | Escritas | Regra |
|---|---|---|---|
| `StorefrontBackend` | 11 obrigatórios: busca, detalhe, carrinho (4), preferências, pedidos (2), políticas, fulfillment. 3 opcionais: `checkout_handoff`, `get_account_context`, `get_disclosure` | Só o carrinho. Nenhum método coloca pedido ou move dinheiro | Cada método age pelo cliente da sessão, com a credencial que o host guarda |
| `MerchantBackend` | 8 leituras, 5 `stage_*`, 3 de ciclo de vida (`get_pending_changes`, `apply_change`, `discard_change`). 3 opcionais: `execute_analysis_query`, `get_analysis_schema`, `get_merchant_context` | Só `apply_change` toca o sistema real, e só para mudança staged | Uma figura que a plataforma não tem volta como `None` com nota, nunca como zero |

Regras de conector herdadas do repo: fluxos com ordem fixa são impostos no backend (verificar, depois elegibilidade, depois submeter); exceções de domínio (`NotOffered`, `Unavailable`, `ChangeNotApplicable`) chegam ao modelo como próximo passo; a chave de idempotência deriva da sessão e do hash das linhas; eventos externos (pagamento concluído, código verificado) entram como evento de app na próxima rodada.

### 5.3 Conectores prioritários

| Ordem | Sistema | Contrato | Nota |
|---|---|---|---|
| 1 | VTEX | Ambos | Maior base enterprise no Brasil; começa pelas leituras |
| 2 | Shopify (Storefront e Admin GraphQL) | Ambos | Segunda base; leitura pronta cedo por documentação |
| 3 | Linx Commerce, Adobe Commerce | Ambos | Enterprise legado |
| 4 | SAP, TOTVS | Merchant (estoque, custo, pedidos) | O custo por item habilita a margem no preview |
| 5 | Bling, Tiny | Merchant | PME e omnichannel |
| 6 | PIM (Akeneo e similares) | Merchant (conteúdo de listagem) | Edição de conteúdo com campos protegidos |
| 7 | CRM e automação (Salesforce, RD Station) | Merchant (campanhas), Storefront (contexto de conta) | |
| 8 | Meta Ads, Google Ads | Merchant (campanhas, gasto, atribuição com ressalvas) | |
| 9 | BigQuery, Snowflake, Databricks | Merchant (`execute_analysis_query`) | Réplica read-only, SELECT único, caps |
| 10 | WhatsApp Cloud API | Canal | Adaptador de apresentação |
| 11 | PSP, Pix, Open Finance (JSR, Pix Automático) | Handoff de checkout; mandatos na fase 5 | Nunca custódia |

---

## 6. Gateway para agentes externos

O que os protocolos fazem: UCP descreve capacidades do lojista que um agente descobre e usa (produto, checkout, cupom, fidelidade, entrega), sobre REST, MCP ou A2A; ACP define feed de produto, sessão de checkout, atualização de carrinho e eventos de pedido, com o lojista como merchant of record; AP2 define mandatos de intenção e de carrinho e o comprovante de execução.

Decisão: **não construir o tradutor de protocolos no início.** Stripe e as plataformas já oferecem isso como recurso, e um adaptador próprio só se justifica quando o cliente exige multi-plataforma e independência de fornecedor ao mesmo tempo. Integramos por parceria ou adaptador de terceiro.

O que o kernel adiciona mesmo assim, e que ninguém entrega junto com o protocolo:

- **Política por canal.** Quais preços, promoções e estoques ficam visíveis para um agente externo; elegibilidade e regras de fraude por origem.
- **Atribuição.** Qual agente originou qual pedido, registrado no ledger, com o histórico de sessão que o protocolo expõe.
- **Vínculo de identidade e fidelidade.** O cliente autenticado no agente externo mapeado para a conta e o programa da loja.
- **Uma única fonte.** O mesmo modelo canônico alimenta o feed do protocolo, o Concierge e o Operator; uma mudança aplicada pelo Operator aparece nos três.

---

## 7. Assurance: evals, policy packs e observabilidade

O repo não traz runner de evals. Traz o formato de caso, os nomes dos scorers e as regras de autoria no plugin `commerce-builder`, e um cliente falso (`commerce_common/testing.py`) para testar gates sem modelo. Construímos o runner e a biblioteca.

| Peça | Conteúdo |
|---|---|
| Caso | Estado injetado (produtos vistos, carrinho, memória, fila staged), uma mensagem, expectativa: ferramenta chamada ou nunca chamada, componente, conteúdo do carrinho, tipo de mudança staged, memória, skill carregada, strings que devem ou não aparecer, número máximo de chamadas, rubrica de PASS e FAIL |
| Scorers | Graders em código sobre o stream de eventos; juiz por dimensão com saída estruturada, modelo fixo a temperatura zero, fingerprint de modelo e rubrica na gravação |
| Regras | Toda positiva tem uma negativa; memória em três casos (grava, recusa identificador, fato muda a escolha); avaliar argumentos finais e estado, não a rota |
| Fixtures hostis | Listagens, reviews e mensagens com instruções embutidas, sob seller fictício; cada caso assere a negativa em código (nunca chama, carrinho não contém, nada aplicado, memória não contém) e tem contraparte benigna |
| Ritmo | Regressão a cada merge em agente, skill, descrição de ferramenta ou fixture; conjunto do fluxo enquanto se mexe nele; suíte inteira ao trocar de modelo; amostra julgada de produção com as mesmas rubricas |
| Operação | Canário por versão do agente; switch por skill; congelamento antes de Black Friday e datas críticas; 50 a 100 casos por fluxo como ponto de partida |

**Policy packs por vertical.** Varejo (delta de preço, profundidade de promoção, reposição, orçamento), telecom (taxas reguladas como campos protegidos, disclosures do servidor), farmácia e saúde (categorias com restrição, referral obrigatório), viagens (inventário por data, janela de tarifa), ingressos (linhas de taxa fixas, valor de face), B2B (preço por contrato nunca exposto a outra conta, limites por centro de custo). Cada pack é um conjunto de valores de guardrail, campos protegidos, lexicons de grounding e casos de eval.

**Observabilidade.** Stream de eventos por turno (`text_delta`, `tool_call`, `tool_result`, `ui`, `cart_update`, `change_update`, `turn_complete`, `error`), uma linha por chamada de modelo, leitura de cache por turno, custo por tarefa concluída por fluxo e cliente.

---

## 8. Console

O plano de controle que o cliente opera.

- **Configuração do cliente.** Switches `enable_*` por sistema; valores de guardrail; nível de autonomia por ação; canais; identidade da marca (`brand_name`, `assistant_name`, `brand_voice`).
- **Inbox de aprovação.** Mudanças pendentes com preview, margem, notas de guardrail; aprovar, editar, descartar, lote, delegação; trilha.
- **Ledger.** Explorador por período, ação, ator, resultado; replay de uma sessão com os eventos e as ferramentas chamadas.
- **Evals.** Última rodada, falhas por prioridade, diff contra a baseline, cobertura por fluxo.
- **Custo e desempenho.** Custo por tarefa, latência por turno, taxa de cache, uso por modelo.
- **Memória e LGPD.** Fatos por titular, retenção, pedidos de exclusão, consentimento.
- **Conectores.** Saúde, latência, figuras indisponíveis (as `limitations` que o backend declara).

---

## 9. Do repositório ao produto

| Componente do repo | Ação | Uso no produto |
|---|---|---|
| `commerce-common` (fencing, memória, skills, prompt assembly, grounding, presentation, delegation, execution, streaming, turn) | Fork, assumir manutenção | Base do runtime e do kernel |
| `shopping-agent/core` (tipos, `StorefrontBackend`, prompt, ferramentas, gates, enrichment, executor, config) | Fork | Concierge |
| `merchant-agent/core` (tipos, `MerchantBackend`, prompt, ferramentas, gates, `changes.py`, `analysis.py`, executor, config) | Fork | Operator |
| Dez skills | Fork e localizar | Fluxos dos dois agentes |
| `runtime-messages-api` (dois papéis) | Fork | Runtime de produção |
| `runtime-agent-sdk`, `managed-agents` | Manter como opção | Consoles internos; Managed Agents se fizer sentido para hospedagem |
| `examples/web-shared` (cliente de API, hooks de sessão e turno, protocolo de eventos, transcript, frames de storefront e portal) | Fork | Widget web e portal |
| Componentes generativos dos exemplos | Referência | Cards do widget e do portal, redesenhados |
| `plugins/commerce-builder` (scaffold, add-flow, author-evals, review) | Fork como ferramenta interna de entrega | Acelera cada implementação |
| `docs/safety.md`, `docs/backends.md` | Base | Checklist de entrega e guia de conector |
| Lexicons de grounding, skills, prompts em inglês | Localizar | Português e vocabulário do comércio brasileiro |
| Valores de guardrail nos `config.py` | Substituir | Policy packs por cliente |
| `ChangeLedger` em memória, `SessionStore` em memória, marca de aprovação, `host_approve` | Estender | Ledger durável, sessão persistida, console |
| Chave de idempotência (prescrita) | Implementar | Execução |
| Verificação, monitoramento, compensação, mandatos, simulação de impacto | Novo | Kernel |
| Runner de evals, biblioteca de fixtures hostis, policy packs | Novo | Assurance |
| Conectores, adaptador de WhatsApp, autenticação, RBAC, multi-tenant, billing, portal do titular | Novo | Plataforma |

Decisão sobre runtime: **não reescrever para ser agnóstico de modelo antes do primeiro cliente.** Os gates, o executor e as ferramentas de apresentação já valem para qualquer modelo (é o que `safety.md` afirma), e o runtime já aceita Vertex, Bedrock e Foundry. Os ativos duráveis são ferramentas, políticas e evals, e o repo já os separa do modelo.

---

## 10. Empacotamento comercial

### 10.1 A entrada: Agentic Commerce Readiness

Diagnóstico de quatro a seis semanas, preço fixo. Cobre: auditoria de catálogo contra o modelo família, variante e plain; APIs e sistemas disponíveis; estoque e preço em tempo real; carrinho e checkout; identidade e fidelidade; políticas e fluxos de aprovação existentes; LGPD; prontidão para UCP e ACP; casos de uso e ROI.

Entregáveis: roadmap por módulo; policy pack inicial; os primeiros 50 casos de eval sobre o catálogo real; protótipo do Operator em modo leitura sobre um conector.

### 10.2 Os módulos

| Módulo | O que o cliente compra | Pré-requisito |
|---|---|---|
| Commerce Operator | Digest, anomalias, ações propostas com preview, aprovação, execução, monitoramento, auditoria | Conector merchant, pelo menos as leituras |
| Shopping Concierge | Site, app e WhatsApp: busca, comparação, planos, carrinho, pós-venda, checkout controlado, memória | Conector storefront; rota de checkout |
| Agentic Commerce Gateway | Presença em ChatGPT, Gemini e Copilot com política por canal, atribuição e vínculo de identidade | Conector storefront; parceiro de protocolo |
| Assurance (embutido na licença) | Evals contínuas, policy pack da vertical, monitoramento de produção, relatório de conformidade | Qualquer módulo |

### 10.3 Receita

- Fee de implementação e integração por conector e canal.
- Licença mensal por cliente, marca ou unidade, em faixas por número de conectores, canais e volume.
- Uso por tarefa concluída, mudança aplicada ou pedido assistido. Nunca por token: o cliente compra resultado, controle e integração.
- Success fee opcional só onde a causalidade é mensurável (margem recuperada por reposição ou preço; pedidos assistidos contra controle).
- Módulos e conectores adicionais.

Modelo de entrega: plataforma mais implementação. O plugin `commerce-builder`, forkado, é a ferramenta interna que reduz o custo de cada implementação.

---

## 11. Sequência

| Fase | Entrega | O que o kernel ganha | Duração estimada |
|---|---|---|---|
| 0 · Fundação | Fork e localização; runner de evals; conector VTEX em leitura; oferta de Readiness no mercado | Sessão persistida; observabilidade; formato de ledger | 6 semanas |
| 1 · Operator em leitura | Digest agendado, insights, delegado analítico sobre BigQuery ou réplica; primeiro cliente | Ledger durável; console mínimo; policy pack v1 | 2 a 3 meses |
| 2 · Operator com escrita | Conteúdo, reposição, preço, promoção e campanha em nível 2; conector SAP ou TOTVS para custo | Aprovação com RBAC; idempotência; verificação; compensação | 3 meses |
| 3 · Concierge | Widget web e WhatsApp com checkout controlado; memória com consentimento; piloto transacional sem pagamento (troca de plano em telecom, sobre a vertical que o repo já traz) | Identidade de cliente final; portal do titular; adaptador de canal | 3 meses |
| 4 · Gateway e nível 3 | Presença em agentes externos por parceiro, com política por canal e atribuição; auto-aprovação de ações de baixo risco | Aprovação por política; atribuição no ledger | 3 meses |
| 5 · Mandatos | Reposição recorrente sobre Pix Automático e Jornada Sem Redirecionamento; compras B2B integradas à política de procurement do cliente | Mandate Engine; tradução para AP2 e pedido de compra | 6 meses ou mais |

A regra: o kernel se prova num produto que o exercita. Nenhuma fase constrói um componente do kernel que a fase seguinte não use.

---

## 12. Riscos

| Risco | Mitigação |
|---|---|
| A plataforma de e-commerce empacota governança e comprime a camada do meio | Ir onde a plataforma não vai: multi-sistema, back office, B2B, verticais reguladas; dados de resultado como moat |
| Custo de manter conectores | Contrato pequeno (dois backends); conectores em leitura primeiro; escritas por prioridade; `None` com nota para o que falta |
| Erro na frente do consumidor | Operator primeiro; Concierge com nível 1 no checkout; evals de produção; disclosures no servidor |
| Dependência de um fornecedor de modelo | Ferramentas, políticas e evals separadas do modelo; sweep de modelo na suíte; runtimes para Vertex, Bedrock e Foundry |
| LGPD e memória | Filtro de escrita, retenção, portal do titular, exclusão integrada; memória desligável por cliente |
| Pagamento e regulação | Nunca custódia; handoff para checkout; mandatos só sobre trilhos existentes (Pix Automático, JSR, PO) e por último |
| Custo de evals | Casos por snapshot, não por conversa; juiz só na rubrica; regressão seletiva por merge |
| Referência sem manutenção | Fork assumido como código próprio desde o dia zero; suíte de testes do repo (83 arquivos) mantida |

---

## 13. Decisões em aberto

1. **Plataforma do primeiro conector:** VTEX ou Shopify. Depende do primeiro cliente.
2. **Hospedagem do runtime:** Messages API em infraestrutura própria (padrão) ou Managed Agents para reduzir operação. O ledger e o console são nossos nos dois casos.
3. **Parceiro de Gateway:** Stripe, a própria plataforma do cliente, ou adaptador UCP próprio quando houver dois clientes pedindo.
4. **Fronteira de "simulação":** preview determinístico no lançamento; modelo de demanda só com dados de pelo menos dois clientes.
5. **Nome do produto e das superfícies:** os nomes deste documento são de trabalho.
