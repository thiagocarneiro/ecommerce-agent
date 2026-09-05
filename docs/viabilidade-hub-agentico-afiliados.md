# Hub agêntico de compras com modelo de afiliados

**Análise de viabilidade**

| | |
|---|---|
| Status | Análise v0.1, 5 de setembro de 2026 |
| Pergunta | É possível construir um hub que reúne ofertas dos marketplaces, entende a necessidade do usuário e as variáveis que importam para ele (preço, frete grátis, prazo, avaliações, limites de um mandato), busca nas lojas, conclui a compra quase sem intervenção, é remunerado por afiliação como o Buscapé e o Zoom, e roda dentro do ChatGPT, do Claude, de um app ou de uma extensão? |
| Base | Artigo *The anatomy of effective commerce agents* (Anthropic, 2/9/2026), repositório `anthropics/commerce-agents` (commit `fd4d592`, lido arquivo a arquivo), o documento [`arquitetura-produto.md`](arquitetura-produto.md) deste repositório, e pesquisa na web em 5/9/2026 sobre protocolos, marketplaces brasileiros, pagamentos, regulação e concorrentes. As fontes estão numeradas no fim. |

## Veredito

**Sim, com uma condição estrutural.** O produto é construível hoje até o ponto em que o usuário dá o toque final na loja. A partir daí, "compra automática" e "remuneração por afiliado" se contradizem: o afiliado é pago por um clique humano rastreado por cookie, e a compra automática elimina exatamente esse clique. O produto viável em 2026 é um **hub que decide por você e deixa você confirmar com um toque**, remunerado por afiliação, com um Mandate Engine que amadurece em fases até a compra sem toque para um subconjunto de lojas, categorias e trilhos de pagamento, com outra fonte de receita.

Em uma tabela, o que o usuário pediu e o que dá para fazer:

| Pedido | Viável hoje? | Condição |
|---|---|---|
| Hub com os produtos dos grandes marketplaces | Parcial | Shopee, Amazon, AliExpress e centenas de lojas via Awin têm API ou feed de afiliado. O Mercado Livre, 32% do e-commerce, bloqueou a API pública de busca para aplicações comuns desde o fim de 2025 e não tem API de afiliado documentada. Cobrir o líder exige acordo comercial ou integração menos formal. |
| Usuário descreve a necessidade e as variáveis | Sim | O repositório de referência já faz isso: skills `search-discovery` e `purchase-research`, filtros de preço e nota, opções de frete com prazo e taxa. Falta localizar para português e adicionar o ranking multiobjetivo no backend. |
| Mandate Engine (limites, lojas, categorias, substituições, aprovação por valor, validade) | Sim para propor e preparar; parcial para executar | Como política em código e ledger, é novo mas simples. Como autorização de pagamento, depende de trilhos que estão chegando ao Brasil em 2026 (Mastercard Agent Pay, Visa Intelligent Commerce, pagamentos automáticos do Open Finance). O Pix Automático não serve para compras avulsas em lojas variadas. |
| Compra quase automática | "Quase" sim; "automática" só em nicho | Um toque na loja preserva afiliação, CDC e termos de uso. Sem toque, só em lojas que aceitem checkout por agente (UCP, ACP, tokens de agente), sob acordo direto, sem receita de afiliado. |
| Modelo de afiliados como Buscapé e Zoom | Sim, com economia fina | Comissões de 1% a 5% em eletrônicos não pagam a conversa; 10% a 20% em moda, beleza e esporte pagam. A Méliuz opera com take rate de cerca de 6% sobre R$ 5,6 bi de GMV. O mix de categoria decide o negócio. |
| Dentro do ChatGPT, Claude, Gemini, app, extensão | ChatGPT e Claude sim; Gemini não como hub; extensão evitar | ChatGPT aceita apps com checkout externo e proíbe anúncios; o Claude tem diretório de conectores com apps de consumo verificados e não opera checkout próprio; o UCP do Google é para lojistas, não para hubs terceiros; extensões de navegador são o canal mais exposto legalmente (Amazon proíbe links de afiliado em extensões e processou o agente do navegador Comet). |

O maior risco não é técnico. É que os donos das superfícies (OpenAI, Google, Amazon, Perplexity) e os marketplaces (Mercado Livre, Shopee, Amazon) estão construindo o hub para si. O espaço defensável para um player brasileiro é o que eles não fazem bem: cobertura dos marketplaces daqui, Pix, parcelamento, frete e prazo por CEP, WhatsApp, e mandatos com governança auditável.

---

## 1. O que está sendo proposto, decomposto

O hub tem seis capacidades. Cada uma tem uma viabilidade diferente, e o restante do documento segue essa ordem.

| # | Capacidade | Exemplo | Seção |
|---|---|---|---|
| 1 | Entender a necessidade e as variáveis | "Quero um ar-condicionado 12.000 BTU inverter, até R$ 2.500, frete grátis, chegar em 5 dias, nota acima de 4,5" | 3 |
| 2 | Buscar nos marketplaces e lojas | Consultar Mercado Livre, Amazon, Shopee, Magalu, Casas Bahia, Kabum em paralelo | 4 |
| 3 | Ranquear e explicar | Preço total com frete, prazo, nota do produto e do seller, parcelamento; dizer por que a primeira é a primeira | 3 |
| 4 | Concluir a compra dentro de um mandato | Limite por compra, mensal, lojas permitidas, substituições, aprovação acima de um valor | 5 e 6 |
| 5 | Ser remunerado por afiliação | Comissão do marketplace na venda atribuída | 4 e 8 |
| 6 | Distribuir dentro dos assistentes e canais | ChatGPT, Claude, Gemini, WhatsApp, app, extensão | 7 |

---

## 2. Relação com o artigo e com o Commerce Control Plane

O artigo da Anthropic e o repositório tratam de **um lojista** que embute um agente de compras na própria loja e um agente de operação no próprio back office. O artigo não menciona marketplaces multi-vendedor, comparação de preços, afiliação, agentes externos, protocolos (ACP, UCP, AP2) nem compra delegada [1]. O documento [`arquitetura-produto.md`](arquitetura-produto.md) deste repositório é o produto B2B do lado da oferta, e diz explicitamente que "marketplaces que querem ser a interface do consumidor" não são o alvo.

O hub é o produto **do lado da demanda**. Os dois não competem; compartilham o kernel:

| Componente | Commerce Control Plane (oferta) | Hub (demanda) |
|---|---|---|
| Trust & Transaction Kernel | Governa ações do agente do lojista sobre preço, estoque, promoção | Governa ações do agente do consumidor sobre carrinho e compra |
| Mandate Engine | Fase 5: reposição recorrente do cliente da loja | Núcleo do produto desde a fase 1 |
| Conectores | `MerchantBackend` sobre VTEX, Shopify, SAP | `StorefrontBackend` federado sobre APIs de afiliado e feeds |
| Gateway para agentes externos | O lojista exposto a ChatGPT, Gemini, Copilot | O hub é um desses agentes externos; pode ser o primeiro cliente do Gateway |
| Evals, ledger, console, LGPD | Iguais | Iguais |

Consequência prática: cerca de metade do que o hub precisa é o mesmo código do kernel descrito no outro documento. Construir os dois com um kernel só é a única forma de o hub ter governança de verdade sem duplicar o esforço.

---

## 3. O que o repositório de referência entrega para o hub

Leitura do `anthropics/commerce-agents` no commit `fd4d592` (31/08/2026), o mesmo que o artigo cita.

### 3.1 Já existe e serve direto

| Peça do repo | Onde | Uso no hub |
|---|---|---|
| Contrato `StorefrontBackend` (11 métodos, 3 opcionais) | `shopping-agent/core/shopping_agent/backend.py` | O hub implementa um backend **federado**: `search_products` consulta N marketplaces em paralelo e devolve uma lista única; `get_product_details` resolve o id no marketplace de origem; `get_fulfillment_options` traduz frete e prazo. |
| `checkout_handoff` (uma URL por seller) | `backend.py`, `types.py` (`CheckoutHandoff.seller`), `enrichment.py` (`enrich_checkout`) | É o gancho exato do modelo de afiliados: o backend gera o **deep link de afiliado** por loja e o executor coloca a URL no cartão depois da chamada do modelo. O modelo nunca vê nem inventa a URL. |
| Postura "referral surface" | `plugins/commerce-builder/commands/scaffold-commerce-agent.md`, pergunta 7 | O scaffold já prevê "uma superfície que lista ofertas de outros vendedores e encaminha o cliente para comprar, sem carrinho, pagamento ou pedido próprios". É o Buscapé em forma de agente. |
| Gates de proveniência e caps | `gates.py` | Só ids devolvidos por busca ou detalhe nesta sessão entram no carrinho ou nos cartões. Protege contra id inventado e contra listagem que tenta injetar instrução. |
| Fencing de conteúdo de terceiros | `commerce_common/fencing.py` | Títulos, descrições e reviews de marketplace chegam ao modelo como dado cercado e sanitizado. Num hub, todo o conteúdo é de terceiros; isso é obrigatório. |
| Modelo de produto | `types.py` | `Product` já tem `price`, `rating`, `review_count`, `in_stock`, `labels`, `attributes`; `SearchFilters` tem `min_price`, `max_price`, `min_rating`, `sort`; `FulfillmentOption` tem `method`, `eta`, `fee`. As variáveis do usuário têm campo. |
| Skills `search-discovery` e `purchase-research` | `shopping-agent/skills/` | Já descrevem a conversa de "necessidade com restrições" e de "me ensine a escolher". Precisam de localização e do vocabulário brasileiro (parcelamento, Pix, frete, marketplace, seller). |
| Memória tipada com filtro | `commerce_common/memory.py` | Preferências e restrições duráveis ("tamanho 42", "não compro de seller sem nota fiscal"). Recusa identificadores por padrão. |
| Servidor MCP por papel | `shopping-agent/managed-agents/storefront-mcp-server/`, `commerce_common/mcp_server.py` | O mesmo executor, com proveniência por conexão, publica as ferramentas via MCP. É o transporte que o Claude (conectores) e o ChatGPT (Apps SDK, construído sobre MCP) consomem. |
| Web search opcional | `enable_web_search` em `commerce_common/config.py` | Para critérios de categoria, não para achar oferta. |
| Formato de evals e fixtures hostis | `plugins/commerce-builder/skills/commerce-evals/` | Testar "nunca leva ao carrinho um id que não veio de busca" e "review com instrução embutida não muda a escolha". |

### 3.2 Não existe e o hub precisa construir

| Lacuna | Evidência no repo | O que construir |
|---|---|---|
| Entidade **oferta** (mesmo produto, vários vendedores) | `docs/backends.md`: "One product sold by several sellers at different prices. A record has one price: return the offer you would sell, or list each offer as its own record." | Normalização: produto canônico (GTIN ou EAN, título normalizado) com N ofertas (loja, preço, frete, prazo, parcelamento, nota do seller, URL de afiliado). Cada oferta vira um `Product` com `attributes` de seller, mais uma `PresentationExtension` `present_offers`. |
| Conectores de marketplace | README: "MCP connectors: none ship" | Um conector por marketplace ou rede de afiliados, com credencial, limite de taxa e cache próprios. É o grosso do trabalho e o custo recorrente. Seção 4. |
| Carrinho multi-loja | O carrinho do repo é de uma loja; `checkout_handoff` devolve uma URL por seller | Desligar o carrinho (`enable_cart=False`, postura referral) e encaminhar direto, ou carrinho por loja com um handoff cada. |
| Compra e pagamento | `docs/safety.md`: "Nothing places an order or charges. `StorefrontBackend` has no such method." | Toda a automação da compra é fora do repo. Seções 5 e 6. |
| Mandato | Ausente; a proveniência é por sessão | O Mandate Engine da proposta (titular, agente, lojas, categorias, limites, substituições, prazo, limiar de aprovação, validade) é novo. O outro documento o coloca na fase 5; aqui ele entra na fase 1. |
| Ranking multiobjetivo | `SearchFilters.sort` tem quatro modos | Função de score no backend com as variáveis do usuário e uma explicação por oferta. O modelo escolhe entre as melhores; não calcula. |
| Identidade nos marketplaces | `docs/backends.md`, passo 1: a credencial fica com o host | Comprar em nome do usuário exigiria a credencial dele em cada marketplace. Os termos de uso proíbem. Seção 5. |
| Monitoramento de preço e alertas | Ausente | Um mandato "compre quando cair abaixo de X" exige job agendado que reexecuta a busca e dispara aprovação ou compra. |

### 3.3 Como as variáveis do mandato mapeiam para o kernel

| Variável do mandato | Onde é avaliada | Base no repo |
|---|---|---|
| Preço-alvo, limite por compra | Validar (gate em código) e Autorizar (política) | `max_quantity_per_item` e `max_cart_lines` mostram o padrão: cap em código, não no prompt |
| Frete grátis, prazo máximo | Ranking no backend e gate que recusa oferta fora do prazo | `FulfillmentOption.eta` e `fee` |
| Avaliação mínima | Filtro de busca e gate | `SearchFilters.min_rating`, `Product.rating` |
| Lojas permitidas, categorias | Roteamento do backend federado | Seller como dimensão de busca (`SearchFilters.attributes`) |
| Substituições até 10% mais caro | Motor de políticas | Novo |
| Exigir aprovação acima de R$ 300 | Aprovação por pessoa ou por política | `require_host_approval` no lado merchant mostra o mecanismo; no lado shopping não existe |
| Limite mensal, validade | Ledger e mandato | Novo |

---

## 4. Acesso ao catálogo e afiliação, marketplace a marketplace

Verificado na web em 5/9/2026. Comissões e janelas mudam; os valores abaixo são os publicados nas fontes citadas e servem para ordem de grandeza.

| Marketplace | Catálogo e preço | Afiliação | Atribuição | Restrição a agentes |
|---|---|---|---|---|
| **Mercado Livre** (32% do e-commerce [20]) | O endpoint público de busca `/sites/MLB/search` passou a devolver 403 para aplicações comuns desde o fim de 2025, sem documentação, com dezenas de reclamações de desenvolvedores até meados de 2026 [9]. Sem API de afiliado documentada. | Programa de Afiliados: 0% a 16% por categoria (esporte até 16%, beleza 12%, eletrônicos 5%, TVs 4%); paga via Mercado Pago, mínimo R$ 30, até 60 dias [8] | Link rastreado gerado no portal; cookie no navegador | Não encontrada regra específica sobre agentes. O app do ML ganhou um assistente próprio com mais de 100 funções em 2026 [21]. **Inferência:** o líder tende a fechar, não abrir, o catálogo para hubs. |
| **Amazon Brasil** (18% [20]) | Product Advertising API 5.0, exige conta de Associado com 3 vendas em 180 dias | 1% a 15% por categoria [10] | Sessão de 24 horas após o clique; carrinho até 89 dias [10] | A política de 14/4/2026 tem uma seção "Agentes": um agente só pode acessar o Conteúdo do Programa se se identificar em todo request com `Agent/[nome]` no user-agent, sem imitar humano nem contornar CAPTCHA. Proíbe Links Especiais em "plug-in para navegador, extensão ou qualquer aplicativo instalável" salvo apps móveis aprovados. Proíbe usar o conteúdo para treinar modelos. [10] |
| **Shopee** (22%, +8 p.p. em 2025 [20]) | Affiliate Open API em GraphQL (`open-api.affiliate.shopee.com.br`): ofertas, taxa de comissão real, links curtos com até 5 sub-IDs, relatório de conversão, feed do catálogo; aprovação manual em 5 a 15 dias [11] | Moda e beleza 12% a 20%; demais categorias menores [11] | Link com sub-ID; relatório de conversão por API (o mais próximo de atribuição server-to-server entre os grandes) | Não encontrada |
| **Magalu** (12% [20]) | Sem API de afiliado encontrada | Parceiro Magalu: até 12% por categoria; mínimo de saque R$ 50 [12] | Link do portal; cookie | Não encontrada. Magalu passou a vender dentro do Mercado Livre em 2025 [20]. |
| **Casas Bahia, Americanas, Kabum, Carrefour e outras** | Rede **Awin**: Publisher API e feed de produtos no formato Google Shopping [13] | Casas Bahia até 9% [13]; demais variam | Link Awin; cookie; sub-IDs | Regras de rede contra clique artificial |
| **AliExpress** | Portals API de afiliado (busca de produto, geração de link) | Variável por categoria | Link; cookie | Não verificada nesta rodada |
| **Google Shopping** | Merchant Center é do lojista; UCP expõe catálogo e checkout a agentes do Google [4] | Não é canal de afiliado | Não se aplica | O Google é o hub |

O que isso significa:

- **A cobertura começa por onde há API**: Shopee, Awin (dezenas de grandes varejistas de uma vez), Amazon (com aprovação), AliExpress. Isso cobre cerca de metade do mercado por participação e a maior parte das categorias de comissão alta.
- **O Mercado Livre é o buraco.** Sem busca por API, resta o link de afiliado do portal (sem dado estruturado de preço e frete) ou um acordo comercial. Um hub que não mostra o Mercado Livre perde credibilidade com o consumidor brasileiro. Este é o primeiro item de risco comercial do produto.
- **Toda afiliação é paga por clique humano com cookie.** Nenhum dos programas paga por uma compra que um servidor concluiu sem que o usuário tenha aberto a página da loja. Só a Shopee e as redes oferecem sub-IDs e relatórios que permitem reconciliar a conversa com a venda.
- **A Amazon já regula agentes por contrato.** A cláusula de identificação `Agent/[nome]` é, na prática, um caminho: um hub que se identifica e não finge ser humano opera dentro da regra; um que roda como extensão ou automação escondida, não. A Amazon também bloqueou legalmente e tecnicamente agentes externos em 2025 e 2026 [16][17], enquanto o CEO diz esperar parcerias [17].

---

## 5. Concluir a compra: quatro modos e o que cada um custa

| Modo | Como funciona | Viável hoje no Brasil? | Afiliação | Risco legal e de plataforma |
|---|---|---|---|---|
| **A. Handoff com um toque** | O agente resolve a oferta e abre o link de afiliado da loja (produto ou carrinho pré-montado, conforme o marketplace). O usuário confirma e paga na loja. | **Sim.** É o que o repo faz com `checkout_handoff`. | **Preservada** (clique, cookie, sub-ID) | Baixo. O lojista é o fornecedor; o hub é intermediador de indicação. O direito de arrependimento do CDC (art. 49, 7 dias) fica com a loja [22]. |
| **B. Agente no navegador do usuário** | Um agente (Claude for Chrome, extensão, Comet) preenche o checkout na sessão logada do usuário. | Tecnicamente sim; contratualmente não nos grandes. | Preservada se o clique de afiliado ocorreu no navegador; frágil | Alto. A Amazon obteve liminar contra o Comet em março de 2026; o Nono Circuito a derrubou em 4/8/2026 dizendo que, no CFAA, quem "acessa" é o usuário, mas preservou quebra de termos de uso e responsabilidade contratual [16]. A Amazon proíbe links de afiliado em extensões [10]. Fora dos EUA não há precedente. |
| **C. Checkout por protocolo** | O hub, como agente, descobre capacidades do lojista (UCP, ACP) e conclui o checkout com o lojista como merchant of record. | Parcial. UCP está ativo no AI Mode, Gemini e YouTube Shopping para lojistas elegíveis [4][5]; ACP existe como padrão aberto, mas a OpenAI **retirou o checkout dentro do ChatGPT em março de 2026** e voltou a mandar o usuário para a loja [2][3]. Adoção por VTEX, Nuvemshop, Tray é possível por ser open source, sem anúncios de Magalu ou Mercado Livre [5]. | **Perdida.** UCP e ACP não têm conceito de afiliado. A receita vem de acordo com o lojista (a OpenAI cobrava 4% [2]). | Médio. Precisa de contrato por lojista ou por plataforma. O CDC vale igual; a responsabilidade do intermediador cresce com o controle que ele exerce sobre a transação [23]. |
| **D. Hub como comprador ou revendedor** | O hub compra com pagamento próprio e revende, ou paga em nome do usuário com credencial em custódia. | Não recomendado. | Perdida | Alto: custódia de valores e de cartão, obrigações de instituição de pagamento, nota fiscal, responsabilidade solidária integral. O outro documento diz "nunca custódia" e isso vale aqui. |

A leitura em uma frase: **o modo A é o produto de 2026; o modo C é o produto de 2027 para as lojas que aderirem; o modo B é uma armadilha; o modo D é outro negócio.**

O "quase automático" que o usuário descreveu é o modo A bem feito: o agente já escolheu, já aplicou o mandato, já montou o carrinho quando a loja permite deep link de carrinho, e o toque do usuário é a aprovação. Isso preserva a afiliação, mantém o CDC claro e não depende de nenhum protocolo.

---

## 6. Mandate Engine e pagamento no Brasil

O mandato da proposta (titular, agente autorizado, lojas, categorias, limite por compra, limite mensal, substituições, prazo de entrega, aprovação acima de um valor, validade) tem duas metades. A primeira é **política em código**: avaliada em Propor e Executar, registrada no ledger, testada por evals. Isso é novo, mas simples, e é o mesmo padrão dos caps do repo. A segunda é **autorização de pagamento**, e aí o Brasil de 2026 oferece o seguinte:

| Trilho | O que é | Status em set/2026 | Serve ao mandato? |
|---|---|---|---|
| **Mastercard Agent Pay** | Tokens temporários de agente, "Know Your Agent", limites definidos pelo consumidor | Ativo no Brasil; Itaú e Santander processaram as primeiras transações reais [14] | **Sim**, é o desenho exato: o consumidor define limites e o agente paga com token, sem ver o cartão. Depende do emissor do usuário e do adquirente da loja aceitarem. |
| **Visa Intelligent Commerce** | Equivalente da Visa; Trusted Agent Protocol | Piloto no Brasil previsto para o fim de 2026, com OpenAI, Perplexity, Microsoft e Anthropic como parceiros [14] | Sim, a partir do piloto |
| **AP2 (Google)** | Formato de mandatos assinados: IntentMandate, CartMandate, PaymentMandate | v0.2 em abril de 2026; contribuído à FIDO Alliance em maio junto com o Verifiable Intent da Mastercard; compõe com os trilhos de cartão [6][7] | Como **formato** do mandato, sim: é a serialização natural do que a proposta descreve. Como trilho, ainda não. |
| **Open Finance: Jornada Sem Redirecionamento e pagamentos automáticos** | Pix autorizado sem sair do app iniciador; débitos até um limite sem autenticar a cada pagamento | JSR obrigatória desde 6/2/2026 (Res. BCB 541/2025); público geral desde 22/4/2026 [15] | Sim para Pix, mas exige um **Iniciador de Transação de Pagamento** autorizado pelo Banco Central (capital mínimo R$ 1 milhão, participante do Open Finance) [19]. O hub não deve ser ITP; deve usar um parceiro ITP. |
| **Pix Automático** | Recorrência autorizada para um recebedor PJ | Obrigatório desde 2025 | **Não.** O FAQ do Banco Central diz que "não se aplica a pagamentos não periódicos a empresas", e o recebedor da cobrança tem de ser o mesmo da recorrência [18]. Serve a reposição recorrente numa mesma loja, não a compras avulsas em lojas variadas. |
| **Cartão do usuário em custódia do hub** | O hub guarda o cartão e paga em sites terceiros | Não fazer | Viola termos dos marketplaces, cria obrigações de instituição de pagamento e responsabilidade integral. |

Conclusões para o Mandate Engine:

1. **Níveis 1 e 2 (preparar e propor) não precisam de trilho nenhum.** O mandato filtra, ranqueia, monta e pede o toque. Isso é o produto da fase 1.
2. **Nível 3 (executar dentro do mandato) chega em 2027** para quem tiver Agent Pay ou Visa Intelligent Commerce disponível no emissor e lojas que aceitem checkout por agente. É um subconjunto: começar por reposição recorrente em categorias de baixo risco, exatamente o exemplo de "supermercado e limpeza" da proposta.
3. **Nível 3 não é remunerado por afiliação.** Precisa de acordo com o lojista, assinatura do usuário, ou split com o trilho de pagamento. Isso deve estar no plano desde o início, não como surpresa.
4. **O mandato deve ser serializável em AP2** desde a fase 1, mesmo sem usar o trilho, para não reescrever quando os emissores e lojistas aceitarem.
5. **Aprovação acima de um valor** é um toque no WhatsApp ou no app, com o cartão de preview do repo. É o `require_host_approval` do lado merchant transplantado para o lado shopping.

---

## 7. Distribuição dentro dos assistentes e canais

| Superfície | Como um hub terceiro entra | Regras que importam | Avaliação |
|---|---|---|---|
| **ChatGPT** | App via Apps SDK (construído sobre MCP), listado no diretório após revisão | As diretrizes exigem "checkout externo, direcionando o usuário a concluir a compra no seu próprio domínio", proíbem anúncios ("o app não pode existir primariamente como veículo de publicidade"), limitam comércio a bens físicos e proíbem coletar dado de cartão. Nada explícito sobre links de afiliado ou comparadores [3]. | **Viável, com risco de política.** "Seu próprio domínio" foi escrito para lojistas; um comparador redireciona para domínios de terceiros. Precisa de leitura jurídica e talvez de conversa com a OpenAI. Além disso, o próprio ChatGPT mostra produtos e links de lojistas desde 2025 e, após retirar o checkout nativo em março de 2026, se posicionou como descoberta com compra na loja [2][3]: o hub compete com a função nativa. |
| **Claude** | Conector MCP publicado no diretório de conectores, com verificação da Anthropic (posse da API, autenticação, tools que fazem o que dizem, anotações de escrita) [24] | O diretório já tem apps de consumo (Instacart, Uber Eats, Booking.com, StubHub) [25]. A Anthropic se posiciona como "camada de inteligência, não vitrine nem checkout" [26]. | **Melhor alinhamento.** A Anthropic não opera checkout próprio nem resultados de compras nativos; um hub verificado preenche um espaço vazio. O servidor MCP do repo é o ponto de partida. |
| **Gemini e Google AI Mode** | O UCP é para o **lojista** expor catálogo e checkout ao Google; o Google faz a comparação e o Universal Cart [4][5] | Sem caminho documentado para um hub terceiro | **Não como hub.** O Google é o hub. O caminho é o inverso: os lojistas clientes do Commerce Control Plane entram no UCP. |
| **Copilot** | Merchant Program do Microsoft Copilot é para lojistas | Não verificado nesta rodada | Baixa prioridade |
| **WhatsApp** | Canal próprio do hub via Cloud API; o outro documento já prevê o adaptador | A Meta lançou Business AI no Brasil em fevereiro de 2026 (cobrado por token desde agosto) e um marketplace nativo com Pix para PMEs [27] | **Canal principal no Brasil.** É onde o consumidor está e onde o toque de aprovação do mandato é natural. |
| **App e site próprios** | Fork de `examples/web-shared` | Nenhuma restrição de terceiro | Necessário como fallback e como destino do link nos assistentes |
| **Extensão de navegador** | Injeta comparação nas páginas dos marketplaces | A Amazon proíbe Links Especiais em extensões e plug-ins [10]; o caso Comet mostra a disposição de litigar [16] | **Evitar no lançamento.** É a superfície com maior exposição contratual e menor controle. |

Dado de demanda que sustenta a aposta nos assistentes: o tráfego de referência de IA para varejo nos EUA cresceu 62% ao ano em julho de 2026 e 1.219% desde outubro de 2024, e desde março de 2026 converte 42% melhor que o tráfego não-IA (Adobe) [28]. Marketplaces lideram o tráfego de referência de IA em todos os setores (Similarweb) [29]. No Brasil, 76% dizem pretender usar agentes de IA para comprar e 67% se dizem confiantes em deixar o agente comprar (Visa) [30]; 40% aceitam que a IA compre por eles, mas 95% temem riscos de segurança (CNDL/SPC) [31].

---

## 8. Concorrência e posição

| Player | O que faz | Estado em 2026 | Lição para o hub |
|---|---|---|---|
| **OpenAI** | Resultados de compras no ChatGPT; ACP com Stripe; Instant Checkout | Lançou em set/2025, expandiu a 1 milhão de lojas Shopify em fev/2026, **retirou o checkout nativo em mar/2026**; hoje recomenda e manda para a loja [2][3] | Checkout dentro do chat não estava pronto nem para quem controla a superfície. Handoff é o padrão de 2026. |
| **Google** | UCP, Universal Cart, checkout agêntico no AI Mode, Gemini e YouTube; AP2 | Ativo nos EUA para lojistas elegíveis [4][5] | O Google será o hub mais forte onde chegar. No Brasil, depende da adesão das plataformas locais. |
| **Amazon** | Rufus substituído por Alexa for Shopping (mai/2026); Buy for Me compra em outros sites; bloqueia agentes externos [17] | Jardim murado; conversas sobre parcerias | O maior varejista do mundo não quer ser fonte de um hub. Entrar pelo Associates, com identificação de agente, é o único caminho hoje. |
| **Perplexity** | Comet, Instant Buy com PayPal, sem taxa ao lojista, 45 milhões de usuários mensais [32] | Litígio com a Amazon | Monetiza por assinatura, não por transação. Um hub sem assinatura não tem essa opção. |
| **Mercado Livre** | Assistente próprio no app; bloqueou a API de busca | Líder com 32% e GMV de cerca de R$ 185 bi [20] | O hub precisa do ML mais do que o ML precisa do hub. Acordo comercial ou cobertura parcial. |
| **Shopee** | Afiliados com API completa; cresceu 8 p.p. em 2025 [20] | 22% | O parceiro natural de lançamento. |
| **Zoom e Buscapé** | Comparadores; CPC e comissão; Mosaico incorporada pelo Banco PAN em 2021; Buscapé hoje oferece cashback [33][34] | Em declínio ("o quase fim de um ícone", NeoFeed [34]) | O modelo de comparador puro perdeu para o marketplace e para o Google. A versão agêntica precisa entregar decisão, não lista. |
| **Méliuz** | Cashback por afiliação; GMV R$ 5,6 bi, receita Shopping Brasil R$ 349 mi em 2025, EBITDA ajustado R$ 92,9 mi [35][36] | Lucrativa, crescendo 15% em GMV | O afiliado brasileiro que dá certo devolve parte da comissão ao usuário. Take rate bruto de cerca de 6%. |
| **Meta WhatsApp** | Business AI, marketplace nativo com Pix [27] | Cobrado por token | O canal também vira concorrente para PMEs. |

**Onde o hub cabe.** Nenhum desses cobre, ao mesmo tempo, os marketplaces brasileiros, Pix e parcelamento, frete e prazo por CEP, WhatsApp e um mandato auditável. O Google e a OpenAI chegam ao Brasil por último e por plataforma; a Amazon fecha; o Mercado Livre fecha; a Shopee abre. O hub é viável como **o agente de compras do brasileiro** enquanto essas superfícies não cobrem o mercado local, e como **fornecedor de conector e mandato** para elas quando cobrirem.

---

## 9. Economia unitária

Ordem de grandeza, com as premissas explícitas. Os números de mercado são de 2025: faturamento de R$ 235,5 bi, 438,9 milhões de pedidos, ticket médio R$ 536,6 [37].

**Custo de uma conversa.** Modelo Sonnet 5 a US$ 2 por milhão de tokens de entrada e US$ 10 por milhão de saída; leitura de cache a 10% do preço de entrada [38]. Conversa de 6 rodadas com prefixo estático de 8 mil tokens em cache, 8 mil tokens novos por rodada (resultados cercados de busca e detalhe) e 500 tokens de saída por rodada com esforço baixo.

| Item | Cálculo | Custo |
|---|---|---|
| Entrada nova por rodada | 8 mil × US$ 2,5/M (escrita de cache) | US$ 0,020 |
| Leituras de cache (prefixo e histórico) | 8 mil a 40 mil × US$ 0,20/M | US$ 0,002 a 0,008 |
| Saída | 500 × US$ 10/M | US$ 0,005 |
| Por rodada | | ≈ US$ 0,03 |
| Por conversa (6 rodadas) | | ≈ US$ 0,18 |
| Extração de memória (Haiku 4.5) e APIs de marketplace | | ≈ US$ 0,01 |
| **Total por conversa** | a R$ 5,50 por dólar | **≈ R$ 1,00** (faixa R$ 0,50 a R$ 2,00 conforme o número de rodadas e o tamanho dos resultados) |

**Receita de uma conversa.** Comissão × ticket × taxa de conversão atribuída por conversa.

| Cenário | Comissão | Ticket | Conversão por conversa | Receita esperada por conversa |
|---|---|---|---|---|
| Eletrônicos no ML ou Amazon | 4% | R$ 1.200 | 3% | R$ 1,44 |
| Eletrônicos, conversão fraca | 4% | R$ 1.200 | 1,5% | R$ 0,72 |
| Moda e beleza na Shopee | 15% | R$ 180 | 6% | R$ 1,62 |
| Casa e esporte no ML | 12% | R$ 400 | 5% | R$ 2,40 |
| Mix Méliuz (take rate bruto ~6% [35]) | 6% | R$ 536 | 5% | R$ 1,61 |

Leitura:

- **A margem por conversa é fina e positiva só com controle de custo.** Entre R$ 0,70 e R$ 2,40 de receita contra R$ 0,50 a R$ 2,00 de custo de modelo, antes de aquisição de usuário, equipe e infraestrutura. O produto não sobrevive a conversas longas com modelo caro; sobrevive com cache agressivo, ranking determinístico no backend, Haiku para rodadas simples e Sonnet só onde a decisão exige.
- **Eletrônicos, a categoria que o consumidor mais compara, é a que menos paga.** O hub que se posicionar como "comparador de eletrônicos" repete a economia do Buscapé. O mix precisa de moda, beleza, esporte, casa e recorrência.
- **Uma segunda linha de receita é necessária.** As candidatas, em ordem de compatibilidade com o produto: cashback compartilhado (modelo Méliuz, que também resolve retenção), CPC de lojistas por posição (modelo Zoom, que compromete a neutralidade e deve ser rotulado), assinatura do agente com mandato (modelo Perplexity Pro, viável só quando o nível 3 existir), e receita B2B pelo Gateway e pelos conectores (o outro documento).
- **Aquisição.** Dentro dos assistentes, o custo de aquisição é baixo e o controle é nulo. No WhatsApp e no app, o inverso. O plano precisa dos dois.

---

## 10. Regulação e risco jurídico

| Tema | O que vale | Consequência para o hub |
|---|---|---|
| CDC, art. 49 | Direito de arrependimento em 7 dias para compra fora do estabelecimento [22] | No modo A, a obrigação é da loja. Nos modos C e D, o hub entra na cadeia e a tendência do STJ é ampliar a responsabilidade solidária de intermediadores, ainda que sem torná-los garantidores universais [23]. |
| LGPD | Memória do agente é dado pessoal; a ANPD colocou IA entre os quatro eixos de fiscalização de 2026-2027 [39] | Consentimento, retenção, portal do titular e filtro de escrita do repo. O mandato é um dado sensível de hábitos de consumo e limites financeiros. |
| PL 2338/2023 (marco da IA) | Aprovado no Senado em dez/2024; na Câmara em 2026 com votação prevista para maio, modelo de risco à europeia, multas de até R$ 50 milhões [40] | Um agente de compras tende a ser de risco baixo ou moderado. Acompanhar a versão final. |
| Iniciação de pagamento | ITP é instituição de pagamento autorizada pelo BCB, capital mínimo R$ 1 milhão [19] | O hub usa parceiro ITP; não pede autorização própria antes de ter volume. |
| Termos de uso dos marketplaces | Amazon regula agentes por contrato [10]; Nono Circuito limitou o CFAA mas preservou termos de uso [16] | Operar identificado, dentro dos programas de afiliado, sem credencial do usuário. |
| Custódia | Nunca | Igual ao outro documento. |

---

## 11. Riscos e mitigações

| Risco | Probabilidade | Mitigação |
|---|---|---|
| As superfícies de IA fazem o hub nativamente e o app terceiro perde relevância | Alta, em 12 a 24 meses para o Brasil | Cobrir o que elas não cobrem (marketplaces locais, Pix, prazo por CEP, WhatsApp); tornar o hub o conector que elas usam; construir base própria no WhatsApp |
| Mercado Livre fecha o catálogo ou proíbe agentes | Alta | Lançar sem depender do ML para busca estruturada; buscar acordo; usar o link de afiliado como mínimo |
| Afiliação não paga compra concluída sem clique | Certa | Manter o toque humano no modo A; sub-IDs e relatórios (Shopee, Awin); acordos diretos para o nível 3 |
| Economia fina | Alta | Mix de categoria; custo por conversa como KPI desde o dia 1; segunda linha de receita |
| Mudança de política do ChatGPT ou do diretório do Claude | Média | Não depender de uma superfície; WhatsApp e app como base |
| Erro na frente do consumidor (oferta errada, seller ruim, compra fora do mandato) | Média | Gates em código, evals com fixtures hostis, aprovação acima do limiar, ledger e compensação (cancelamento dentro do prazo do CDC) |
| Custo de manter conectores | Certa | Contrato pequeno; priorizar APIs oficiais de afiliado e feeds; conectores em leitura |
| Dependência de fornecedor de modelo | Baixa | Ferramentas, políticas e evals separadas do modelo, como no repo |

---

## 12. Arquitetura proposta do hub

```
 SUPERFÍCIES    WhatsApp │ Web e app │ Conector Claude (MCP) │ App ChatGPT (MCP) │ Alertas
                    │          │              │                    │                │
 AGENTE         Shopping Concierge do hub (fork de shopping-agent, skills em português)
                    │ propõe: shortlist, oferta escolhida, carrinho por loja
 ┌──────────────────┴──────────────────────────────────────────────────────────────────┐
 │  TRUST & TRANSACTION KERNEL (compartilhado com o Commerce Control Plane)           │
 │  proveniência · fencing · políticas · MANDATE ENGINE · ledger · aprovação por toque │
 │  monitor de preço · compensação · memória e LGPD · auditoria e custo por conversa   │
 └──────────────────┬──────────────────────────────────────────────────────────────────┘
                    │ StorefrontBackend FEDERADO
 NORMALIZAÇÃO   produto canônico (GTIN) → N ofertas (loja, preço, frete, prazo, parcelas, nota, URL)
                ranking multiobjetivo pelas variáveis do mandato
                    │
 CONECTORES     Shopee Affiliate API │ Awin (Casas Bahia, Kabum, Americanas…) │ Amazon PA-API
                AliExpress Portals │ Mercado Livre (link de afiliado; acordo) │ feeds próprios
                    │
 HANDOFF        deep link de afiliado por loja (modo A) → 2027: UCP/ACP + Agent Pay/Visa IC (modo C)
```

Decisões de arquitetura que diferem do lojista único:

- **Carrinho por loja, não carrinho do hub.** `checkout_handoff` devolve uma entrada por seller; o cartão mostra um botão por loja.
- **Ranking no backend, escolha no modelo.** O backend calcula o score pelas variáveis do mandato e devolve as cinco melhores com a explicação; o modelo apresenta e responde perguntas. Isso controla custo e torna o ranking testável sem modelo.
- **Mandato avaliado em dois pontos.** Em Propor (filtra ofertas fora do mandato antes de o modelo as ver) e em Executar (recusa handoff fora do mandato mesmo que o modelo tente).
- **Identificação de agente em todo request** a marketplace, no formato que a Amazon exige, desde o dia 1.
- **Um servidor MCP, três consumidores.** O mesmo servidor atende o conector do Claude, o app do ChatGPT e o app próprio, com proveniência por conexão como no repo.

---

## 13. Sequência

| Fase | Entrega | O que prova | Critério para seguir | Duração |
|---|---|---|---|---|
| 0 · Comparador conversacional | Fork e localização; backend federado sobre Shopee, Awin e Amazon; ranking pelas variáveis; handoff de afiliado; WhatsApp e web; 50 evals com fixtures hostis; conector MCP do Claude submetido | Que a conversa gera clique e venda atribuída; custo por conversa | Comissão por conversa maior que o custo de modelo em pelo menos duas categorias | 8 a 10 semanas |
| 1 · Mandato de níveis 1 e 2 | Mandate Engine como política e ledger; memória com consentimento; monitor de preço com alerta; aprovação por toque no WhatsApp; app no ChatGPT | Que o usuário cria mandato e volta sem ser chamado; recorrência | Taxa de retorno em 30 dias e mandatos ativos por usuário | 3 meses |
| 2 · Cobertura e receita | Acordo ou integração com Mercado Livre; cashback compartilhado; AliExpress e Magalu; console de ledger e custo | Que a cobertura fecha a credibilidade e a segunda receita paga a aquisição | Margem de contribuição positiva por usuário ativo | 3 meses |
| 3 · Nível 3 em nicho | Compra sem toque em lojas com checkout por agente (UCP ou ACP via plataformas) e pagamento por Agent Pay ou Visa Intelligent Commerce ou Pix via parceiro ITP; mandato serializado em AP2; começar por reposição recorrente | Que a compra sem toque funciona dentro do mandato com zero incidentes de política | Incidentes de política e estornos abaixo de um limiar; acordo de receita com os lojistas do nicho | 6 meses ou mais, condicionado aos trilhos |

Regra igual à do outro documento: nenhuma fase constrói componente que a seguinte não use. E uma regra própria: **nenhuma fase depende de o Mercado Livre abrir a API ou de a OpenAI reabrir o checkout.**

---

## 14. Decisões em aberto

1. **Mercado Livre:** tentar acordo comercial antes do lançamento ou lançar sem busca estruturada no ML e negociar com tração.
2. **Segunda receita:** cashback (retenção, mas devolve margem) ou CPC (margem, mas compromete a neutralidade). A recomendação é cashback, pelo precedente da Méliuz e pela coerência com "o agente do usuário".
3. **Primeira superfície de terceiro:** conector do Claude (menos atrito de política) ou app do ChatGPT (mais audiência). A recomendação é submeter os dois, com o WhatsApp como base.
4. **Nome e posicionamento:** "comparador" repete o Buscapé; "agente que compra" promete o nível 3 antes da hora. Algo entre "decide por você, você confirma".
5. **Relação com o Commerce Control Plane:** um produto com dois lados ou dois produtos com um kernel. A recomendação é um kernel, dois produtos, uma equipe de kernel.

---

## Fontes

1. Anthropic, *The anatomy of effective commerce agents*, 2/9/2026: https://claude.com/blog/the-anatomy-of-effective-commerce-agents e https://github.com/anthropics/commerce-agents
2. Elogic, *ChatGPT Commerce & Agentic Shopping Statistics 2026* (taxa de 4%, relançamento de fev/2026, recuo de mar/2026 reportado por The Information): https://elogic.co/blog/chatgpt-commerce-statistics/ ; Stripe, *Stripe powers Instant Checkout in ChatGPT*: https://stripe.com/newsroom/news/stripe-openai-instant-checkout ; Hypotenuse, *ChatGPT Instant Checkout: What Happened to It in 2026* (4/8/2026): https://www.hypotenuse.ai/blog/chatgpts-instant-checkout-the-next-phase-of-agentic-commerce ; Retail Media News, *OpenAI recua no checkout nativo do ChatGPT*: https://www.retailmedianews.com.br/artigos/openai-recua-no-checkout-nativo-do-chatgpt-e-o-futuro-do-e-commerce-com-ia-passa-a-ser-a-integracao-do-carrinho-omnichannel/
3. OpenAI, *App submission guidelines*, Apps SDK: https://developers.openai.com/apps-sdk/app-submission-guidelines
4. Google Merchant Center Help, *About the Universal Commerce Protocol (UCP)*: https://support.google.com/merchants/answer/16837055 ; Google Developers Blog, *Under the Hood: UCP*: https://developers.googleblog.com/under-the-hood-universal-commerce-protocol-ucp/ ; Google, *Universal Cart, agentic shopping*: https://blog.google/products-and-platforms/products/shopping/google-shopping-cart/
5. E-Commerce Brasil, *UCP do Google: por que o "HTTP do comércio" pode ser o anúncio mais importante da NRF 2026*: https://www.ecommercebrasil.com.br/artigos/ucp-do-google-por-que-o-http-do-comercio-pode-ser-o-anuncio-mais-importante-da-nrf-2026 ; Conversion, *O Google lança o UCP*: https://www.conversion.com.br/blog/universal-commerce-protocol/
6. Eco, *AP2 Protocol Explained*: https://eco.com/support/en/articles/15192002-ap2-protocol-explained-google-s-agentic-commerce-standard-2026
7. UCP Blog, *Agentic payments: Visa, Mastercard, Adyen, FIDO (June 2026)*: https://universalcommerceprotocol.blog/en/agentic-payments-june-2026/
8. Rally de Vendas, *Comissão Afiliado Mercado Livre: quanto por cento em 2026*: https://blog.rallydevendas.com.br/comissao-afiliado-mercado-livre-quanto-por-cento ; Hunter HUB, *Programa de Afiliados Mercado Livre*: https://hunterhub.com.br/blog/programa-de-afiliados-mercado-livre-como-funciona-comissao-afiliados-ml/
9. Reclame Aqui, reclamações de desenvolvedores sobre 403 em `/sites/MLB/search` (2025-2026), por exemplo: https://www.reclameaqui.com.br/mercado-livre/bloqueio-do-endpoint-de-busca-publica-forbidden-na-api-do-mercado-livre_yi5DSFwTKmoXaNgP/
10. Amazon.com.br, *Políticas do Programa de Associados* (vigência 14/4/2026): https://associados.amazon.com.br/help/operating/policies/ ; *Anexo de comissões*: https://associados.amazon.com.br/help/operating/advertisingfees
11. Shopee, *Affiliate Open API*: https://affiliate.shopee.com.br/open_api ; Hub do Afiliado, *Como ser afiliado Shopee e liberar a API oficial (2026)*: https://hubdoafiliado.com/blog/como-ser-afiliado-shopee-e-liberar-a-api-oficial-guia-definitivo-2026
12. Inteligência Setorial, *Afiliados Magalu: comissões de até 12%*: https://inteligenciasetorial.com.br/afiliados-magalu/
13. Awin, *Casas Bahia BR Programa do Afiliado*: https://ui.awin.com/merchant-profile/17629 ; Awin, *Product Data Feed*: https://success.awin.com/s/article/What-is-a-Product-Data-Feed?language=pt_BR
14. Upnetix, *Mastercard ativa pagamentos por agentes de IA no Brasil: Itaú e Santander já processaram as primeiras transações reais*: https://upnetix.com.br/mastercard-agent-pay-brasil-pagamentos-agentes-ia/ ; Finsiders, *Pagamentos agênticos com IA: Brasil avança com Mastercard, Visa e fintechs*: https://finsidersbrasil.com.br/tendencias-de-pagamento/pagamentos-agenticos-avancam-no-brasil-e-atraem-bancos-bandeiras-e-fintechs/ ; Bloomberg Línea, *Mastercard terá serviço no Brasil no começo de 2026*: https://www.bloomberglinea.com.br/negocios/pagamento-com-agentes-de-ia-mastercard-tera-servico-no-brasil-no-comeco-de-2026/
15. Sensedia, *Jornada Sem Redirecionamento no Open Finance*: https://www.sensedia.com.br/post/fique-por-dentro-da-obrigatoriedade-da-jsr-no-open-finance-em-2026 ; Pluggy, *Open Finance 2026*: https://www.pluggy.ai/blog/open-finance-2026-novidades
16. Cooley, *Ninth Circuit Rules on AI Agent 'Access' to Third-Party Websites Under CFAA* (6/8/2026): https://www.cooley.com/news/insight/2026/2026-08-06-ninth-circuit-rules-on-ai-agent-access-to-third-party-websites-under-cfaa ; CNBC, *Amazon wins court order to block Perplexity's AI shopping agent* (10/3/2026): https://www.cnbc.com/2026/03/10/amazon-wins-court-order-to-block-perplexitys-ai-shopping-agent.html
17. CNBC, *Amazon ditches Rufus chatbot, launches Alexa shopping agent* (13/5/2026): https://www.cnbc.com/2026/05/13/amazon-ditches-rufus-ai-chatbot-in-favor-of-alexa-shopping-agent.html ; CNBC, *Amazon faces a dilemma: fight AI shopping agents, or join them* (24/12/2025): https://www.cnbc.com/2025/12/24/amazon-faces-a-dilemma-fight-ai-shopping-agents-or-join-them.html
18. Banco Central do Brasil, *FAQ Pix Automático* (participantes): https://www.bcb.gov.br/content/estabilidadefinanceira/pix/pix-automatico-FAQ-participantes.pdf
19. Silva Lopes Advogados, *ITP: o que é e como funciona*: https://silvalopes.adv.br/iniciador-de-transacao-de-pagamento-instituicao-de-pagamento-do-bacen/ ; Celcoin, *Capital mínimo para iniciador de pagamentos*: https://celcoin.com.br/articles/capital-minimo-iniciador-pagamentos-brasil/
20. BXTData, *E-commerce Brasil 2026: Mercado Livre e Shopee disputam liderança*: https://www.bxtdata.com/pt/insights/8955/ ; NeoFeed, *Magalu entra no Mercado Livre*: https://neofeed.com.br/negocios/magalu-entra-no-mercado-livre-o-nome-jogo-e-escala-para-as-duas-empresas/
21. Harvard Deusto, *Tendencias de e-commerce para 2026: del buscador al agente de IA*: https://www.harvard-deusto.com/tendencias-de-e-commerce-para-2026-del-buscador-al-agente-de-ia
22. Lei 8.078/1990 (CDC), art. 49: https://www.planalto.gov.br/ccivil_03/leis/l8078compilado.htm
23. Conjur, *Marketplaces e responsabilidade solidária: limites à equiparação no CDC* (20/2/2026): https://www.conjur.com.br/2026-fev-20/marketplaces-e-responsabilidade-solidaria-limites-normativos-a-equiparacao-automatica-no-cdc/
24. Anthropic, *Connector verification*: https://claude.com/docs/connectors/verification ; Sunpeak, *Claude Connector Directory Submission (agosto de 2026)*: https://sunpeak.ai/blogs/claude-connector-directory-submission/
25. PYMNTS, *Anthropic Turns Claude Into a Front Door for Daily Apps*: https://www.pymnts.com/news/artificial-intelligence/2026/anthropic-turns-claude-into-a-front-door-for-daily-apps/
26. Robert Hu, *Claude for Commerce: How Anthropic's Shopping and Merchant Agents Work*: https://theroberthu.com/blog/anthropic-claude-commerce-intelligence-layer
27. Forbes Brasil, *WhatsApp Business lança IA agêntica para PMEs* (fev/2026): https://forbes.com.br/forbes-tech/2026/02/whatsapp-business-lanca-ia-agentica-para-pmes/ ; SocialHub, *WhatsApp Business Marketplace In-App 2026*: https://www.socialhub.pro/blog/whatsapp-business-2026-meta-marketplace-nativo-compras-in-app-pme-brasil/
28. Digital Commerce 360, *Adobe: AI-referral traffic spending, converting more than counterparts* (19/8/2026): https://www.digitalcommerce360.com/2026/08/19/adobe-ai-referral-traffic-data-july-2026/
29. Similarweb, *AI Referral Traffic by Industry: 2026 Data*: https://aisearch.similarweb.com/blog/ai-referral-traffic-by-industry/
30. Meio & Mensagem, *Estudo: 76% dos brasileiros pretendem usar IA nas compras* (Visa): https://www.meioemensagem.com.br/marketing/estudo-76-dos-brasileiros-pretendem-usar-ia-nas-compras
31. CNDL/SPC, *40% dos brasileiros já aceitam que a IA compre por eles, mas 95% temem riscos de segurança*: https://cndl.org.br/varejosa/40-dos-brasileiros-ja-aceitam-que-a-ia-compre-por-eles-mas-95-temem-riscos-de-seguranca-mostra-pesquisa/
32. DesignRush, *Perplexity Pushes Into AI Commerce With New Instant Buy and PayPal Flow*: https://news.designrush.com/perplexity-enters-ai-commerce-race-instant-buy-paypal-integration ; Stellagent, *Perplexity Shopping*: https://stellagent.ai/insights/perplexity-shopping-buy-with-pro
33. Wikipédia, *Zoom (comparador de preços)*: https://pt.wikipedia.org/wiki/Zoom_(comparador_de_pre%C3%A7os) ; Buscapé: https://www.buscape.com.br/
34. NeoFeed, *Buscapé: o quase fim de um ícone da internet brasileira*: https://neofeed.com.br/blog/home/buscape-o-quase-fim-de-um-icone-da-internet-brasileira/
35. Visno Invest, *Méliuz registra EBITDA ajustado de R$ 92,9 mi em 2025*: https://visnoinvest.com.br/news/12167/meliuz-cash3-registra-ebitda-ajustado-de-r-929-mi-em-2025
36. Méliuz RI, *Earnings Release 1T26* e apresentação de resultados de 15/5/2026: https://ri.meliuz.com.br/
37. Mundo do Marketing, *E-commerce brasileiro cresce 15,3% e fatura R$ 235,5 bilhões em 2025*: https://mundodomarketing.com.br/e-commerce-brasileiro-cresce-15-3-e-fatura-r-235-5-bilhoes-em-2025 ; Central do Varejo, *E-commerce deve faturar R$ 259,8 bi em 2026 (ABIACOM)*: https://centraldovarejo.com.br/e-commerce-deve-faturar-r-2598-bilhoes-em-2026-projeta-abiacom/
38. Tabela de preços da API da Anthropic conforme a referência interna consultada em 5/9/2026 (Sonnet 5: US$ 2 e US$ 10 por milhão de tokens; Haiku 4.5: US$ 1 e US$ 5; leitura de cache a 10% da entrada, escrita a 125%). Conferir em https://claude.com/pricing antes de orçar.
39. OpenClaw, *ANPD e IA: fiscalização de dados pessoais em 2026*: https://openclaw.ia.br/blog/anpd-fiscalizacao-ia-dados-pessoais-2026/
40. Senado Federal, *PL 2338/2023*: https://www25.senado.leg.br/web/atividade/materias/-/materia/157233 ; Entercast, *Marco Legal da IA: votação na Câmara em maio de 2026*: https://www.entercastconsulting.com.br/blog/marco-legal-ia-brasil-votacao-camara-maio-2026
