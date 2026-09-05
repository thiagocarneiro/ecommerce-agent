# Mandato: pitch

**A casa abastecida dentro do que você autorizou. Pago pelo varejista, por pedido entregue.**

| | |
|---|---|
| Status | Pitch v0.1, 5 de setembro de 2026. Nome de trabalho: Mandato. |
| Base | [`proposta-produto-mandato.md`](proposta-produto-mandato.md) (desenho do produto), [`viabilidade-hub-agentico-afiliados.md`](viabilidade-hub-agentico-afiliados.md) (por que o hub de afiliados não fecha) e [`arquitetura-produto.md`](arquitetura-produto.md) (kernel). Fontes numeradas no fim; os números de mercado são de 2025 e 2026. |
| Público | Serve para os dois lados: o varejista que assina o piloto e o investidor que financia até o gate 2. A seção 14 tem o pedido para cada um. |

---

## 1. Uma frase

O Mandato é um agente que cuida da reposição da casa, o que uma família compra todo mês em supermercado, farmácia e pet, sob um mandato que o consumidor define uma vez: lojas, limites, substituições, prazo, aprovação. Ele cota nos varejistas parceiros, compra pela melhor combinação e é pago pelo varejista por pedido entregue.

---

## 2. A dor

### Do consumidor

A mesma compra, todo mês, feita à mão. Abrir três apps, comparar preço e frete, lembrar o que acabou, refazer o carrinho, aceitar substituição ruim, pagar. Quem já compra supermercado online (15% dos brasileiros [1]) e farmácia online (11% do setor [2]) resolveu a ida à loja, não a tarefa. A Amazon prova a demanda com o Programe e Poupe [3], mas oferece uma loja só, cadência fixa, só cartão, sem comparação e sem WhatsApp.

### Do varejista

Três dores, todas com preço:

- **Paga caro por demanda que não é dele.** iFood cobra 12% no plano básico e 23% no plano com entrega, mais 3,2% a 3,5% de pagamento, até 26,5% [4]. O varejista aluga o cliente a cada pedido.
- **Perde a recorrência para quem tem app melhor.** Amazon, Mercado Livre e Shopee vendem supermercado, farmácia e pet, e crescem nessas categorias mais que o varejista físico [1][5].
- **Não sabe como ficar legível para os agentes que estão chegando.** Google (UCP), OpenAI (ACP) e as bandeiras (Agent Pay, Visa Intelligent Commerce) estão definindo como um agente descobre, compara e compra. O varejista médio brasileiro não tem time para isso.

### Do mercado

A busca virou commodity dentro dos assistentes, e o checkout dentro do chat falhou: a OpenAI lançou e retirou o Instant Checkout em seis meses [6]. O valor migrou para quem **verifica e executa** com governança. Ninguém no Brasil vende isso ao varejo recorrente.

---

## 3. Por que agora

| Sinal | Número | Fonte |
|---|---|---|
| Pix Automático decolou | Transações cresceram 182% entre o quarto trimestre de 2025 e o primeiro de 2026; custa 65% menos que cartão numa recorrência | [7] |
| Pagamento por agente chegou ao Brasil | Mastercard Agent Pay processou transações reais com Itaú e Santander; Visa pilota no fim de 2026 com OpenAI, Perplexity, Microsoft e Anthropic | [8] |
| O consumidor quer | 76% dos brasileiros pretendem usar agentes de IA para comprar; 67% se dizem confiantes em deixar o agente comprar; 95% temem a segurança | [9][10] |
| O tráfego de IA converte | Tráfego de referência de IA para varejo cresceu 62% ao ano em julho de 2026 e converte 42% melhor que o tráfego comum | [11] |
| O WhatsApp virou canal de comércio com IA | Meta lançou Business AI no Brasil em fevereiro de 2026 e um marketplace nativo com Pix | [12] |
| O código-base existe | A Anthropic publicou em setembro de 2026 uma referência aberta de agentes de comércio com gates de proveniência, memória e handoff de checkout, sob Apache 2.0 | [13] |

A janela é de 12 a 24 meses: o tempo que Google e OpenAI levam para cobrir o varejo recorrente brasileiro por plataforma. Quem tiver os varejistas integrados e os mandatos ativos antes disso vira o conector que eles usam.

---

## 4. A solução

**O que o consumidor vê.** No WhatsApp: "manda a foto da última nota ou me diz o que você compra sempre". Cinco perguntas viram o mandato. Depois, um resumo por semana com um toque para confirmar. Quando confiar, nem o toque: o comprovante chega e o ledger explica o que foi feito e por quê.

**O que o varejista vê.** Um canal de recorrência que ele não tem, a 5% a 8% por pedido entregue, com o cliente vinculado à conta dele, credencial de agente escopada, política por canal e um painel do que o agente fez. E, no mesmo contrato, a preparação para os agentes externos.

**Como funciona, em cinco passos.**

1. **Lista da casa.** O agente extrai itens, marcas, quantidades e cadência da nota, da lista ou da conversa.
2. **Mandato.** Lojas permitidas, limite por pedido e por mês, regra de substituição (mesma categoria até 10% mais caro; marca fixa onde importa), prazo, valor que exige aprovação, validade. Registrado no ledger; serializável no formato de mandatos que Google e Mastercard levaram à FIDO [14].
3. **Cotação.** A cada ciclo, o backend consulta os parceiros e calcula preço total com frete por CEP, prazo e substituições. Ranking determinístico; o modelo só apresenta e explica.
4. **Execução.** Nível 2: resumo e um toque. Nível 3: dentro do mandato, o agente envia o pedido pela integração e o varejista cobra pelo trilho combinado.
5. **Pós-compra.** Rastreio, exceção, compensação pelo parceiro, e a resposta a "o que você fez com meu dinheiro" em uma tela.

**O que fica de fora.** Eletrônicos e compra avulsa (existe um modo pesquisa acessório, por afiliado). Medicamento com receita. Extensão de navegador. Senha do usuário. Custódia de dinheiro.

---

## 5. Para quem

### ICP varejista

| Critério | Qualifica | Desqualifica |
|---|---|---|
| Categoria | Supermercado, farmácia, pet, atacarejo com delivery | Moda, eletrônicos, bens duráveis |
| Operação | E-commerce próprio e entrega própria ou parceira, em pelo menos uma capital | Só loja física; só marketplace |
| Plataforma | API disponível, ou plataforma que expõe checkout hospedado ou UCP (VTEX, Shopify, Linx, Nuvemshop e similares) | Sistema fechado sem integração possível |
| Time | CRM ou fidelidade com meta de recorrência | Ninguém dono do cliente digital |
| Economia | Já paga marketplace de delivery ou mídia de performance | Não investe em aquisição |
| Tamanho | Faturamento digital acima de R$ 50 milhões por ano, ou rede regional dominante | Loja única |

Os primeiros três parceiros: um supermercado, uma farmácia e um pet, na mesma cidade.

### ICP consumidor

Casas com compra recorrente alta e tempo escasso: famílias com crianças, donos de pet, casais que já compram supermercado e farmácia por app. Classe A e B urbana, WhatsApp como interface principal, cesta recorrente de R$ 300 a R$ 700 por mês. Chegam pelo varejista, não por mídia.

### Anti-ICP

Quem compra por lazer, compara eletrônico por hobby ou quer o menor preço absoluto do país em cada item. O Mandato otimiza a cesta dentro dos parceiros; não é o Buscapé.

---

## 6. Mercado: TAM, SAM e SOM

Medido em gasto das casas (GMV), porque a receita do Mandato é uma fração dele. Premissas explícitas.

| | Definição | Valor | Como foi calculado |
|---|---|---|---|
| **TAM** | Gasto anual das casas brasileiras nas três categorias, todos os canais | **≈ R$ 1,47 trilhão** | Supermercados R$ 1,145 tri (Abras, 2025) [15] + varejo farmacêutico R$ 246 bi (2025) [2] + pet R$ 77 bi (2025) [16] |
| **SAM** | A parte já digital, em categorias que cabem num mandato | **≈ R$ 50 a 60 bilhões por ano** | Farmácia digital R$ 27,5 bi [2], descontando receita controlada: ≈ R$ 20 bi. Supermercado online: sem número oficial de GMV; estimativa de 2% a 3% do setor, ≈ R$ 25 a 35 bi, coerente com 15% dos brasileiros comprando online [1][15]. Pet online R$ 4,2 bi, 5,4% do setor [16]. |
| **SAM em casas** | Casas que já compram recorrente online | **≈ 10 a 13 milhões** | 15% de 203 milhões de pessoas ≈ 30 milhões de pessoas [1]; 72,4 milhões de domicílios ocupados no país [17]; razão de 2,3 a 2,8 pessoas por domicílio comprador |
| **Pool de receita no SAM** | O que os varejistas pagariam se toda a recorrência digital passasse por agentes a 6% | **≈ R$ 3 a 3,6 bilhões por ano** | 6% do SAM. É o teto teórico, não a meta. |
| **SOM em 3 a 5 anos** | Casas ativas com mandato, na fatia que o Mandato captura | **100 a 300 mil casas; R$ 0,5 a 1,6 bilhão de GMV; R$ 32 a 97 milhões de receita CPA por ano** | Casa ativa gasta R$ 450 por mês pelo Mandato; CPA de 6%; 1% a 2,5% do SAM em casas |
| **Receita adicional no SOM** | SaaS de canal agêntico e marcas | **R$ 5 a 15 milhões por ano** | 30 a 80 varejistas a R$ 8 a 15 mil por mês; marcas na fase 3 |

Leitura: o SOM é um negócio de dezenas de milhões por ano. O que multiplica é a fase de infraestrutura, quando o mesmo kernel processa mandatos vindos de qualquer agente, não só do nosso, e a receita passa a ser por transação no SAM inteiro.

---

## 7. Modelo de negócio

### 7.0 Em uma frase, e o fluxo do dinheiro

**Cobramos do varejista uma comissão sobre cada pedido recorrente que o Mandato gerou e ele entregou. É isso. O resto é complemento.**

```
 Consumidor ──── paga R$ 450 ao varejista (Pix Automático, Agent Pay ou um toque) ───► Varejista
 Varejista  ──── entrega o pedido ──────────────────────────────────────────────────► Consumidor
 Varejista  ──── fatura mensal do Mandato: 6% dos pedidos entregues = R$ 27 ────────► Mandato
```

O dinheiro do consumidor nunca passa pelo Mandato. A comissão incide sobre o valor dos produtos do pedido entregue, sem frete, e é apurada pelo id do pedido na integração. Pedido cancelado ou devolvido não gera comissão.

| Faixa | Sobre o quê | Por que o varejista paga |
|---|---|---|
| 3% a 4% | Pedidos de uma casa que o próprio varejista trouxe, na "loja preferida" | Ele já tinha o cliente; paga pela automação e pela frequência que o app dele não entrega |
| 6% a 8% | Pedidos de uma casa que veio de outro parceiro ou do WhatsApp, ou pedido que o Mandato deslocou para ele por preço, prazo ou estoque | Demanda nova, comparável ao que paga a um marketplace de delivery, pela metade |
| Piso | Pedido mínimo e comissão mínima por pedido, a negociar | Cesta pequena não paga a operação |

Sequência: fases 0 e 1 vivem só de comissão. O SaaS entra na fase 2, quando existirem painel e exposição a agentes externos. Marcas entram na fase 3. Assinatura do consumidor só se o teste da fase 1 provar que alguém paga.

### 7.1 Linhas de receita

| Linha | Quem paga | Preço | Quando entra |
|---|---|---|---|
| **CPA por pedido entregue** | Varejista | 5% a 8% do pedido, por categoria; atribuição pelo id do pedido na integração, sem cookie | Dia 1 |
| **Canal agêntico (SaaS)** | Varejista | R$ 8 a 15 mil por mês por rede ou região: credencial delegada, política por canal, ledger, evals, painel, e a exposição a agentes externos | Fase 2 |
| **Marcas** | Indústria de consumo | Posição sugerida quando o consumidor não fixou marca, rotulada; dentro do mandato | Fase 3 |
| **Mandato+** | Consumidor | R$ 9,90 a 14,90 por mês, opcional: otimização multi-loja, proteção de preço | Só se o teste da fase 1 mostrar disposição a pagar |
| **Afiliado** | Marketplaces | Modo pesquisa para o que os parceiros não têm | Marginal |

### 7.2 Por que o varejista paga

| O que ele paga hoje | Quanto | O que o Mandato cobra | Diferença |
|---|---|---|---|
| Marketplace de delivery por pedido pontual | 12% a 26,5% [4] | 5% a 8% por pedido recorrente | Menos da metade, por um cliente que volta sozinho |
| Mídia de performance por cliente novo | R$ 80 a 120 por aquisição (referência) | Co-marketing do próprio varejista | O cliente já é dele; o Mandato o faz voltar |
| Consultoria e integração para agentes externos | Projetos avulsos | SaaS mensal | Mesmo kernel, contínuo |

### 7.3 Economia unitária por casa

| Item | Por casa ativa por mês |
|---|---|
| Cesta pelo Mandato | R$ 450 |
| Receita CPA (6%) | **R$ 27** |
| Modelo (onboarding amortizado e exceções; ciclos são jobs) | R$ 1,50 |
| Infraestrutura, APIs, parceiro de pagamento | R$ 1,50 |
| Suporte e operação em escala | R$ 3,00 |
| **Contribuição** | **≈ R$ 21** |
| Aquisição via parceiro | R$ 20 a 40, payback de 1 a 2 meses |

O custo de modelo é baixo porque a conversa acontece uma vez. Depois, o agente roda como job determinístico e só chama o modelo na exceção. O hub de afiliados, ao contrário, pagava uma conversa inteira por compra e recebia comissão de 4% em eletrônicos [18].

---

## 8. Diferenciação e concorrência

### 8.1 De quem tiramos dinheiro

Não combatemos o iFood na entrega nem a 99 na corrida. Combatemos em três frentes, e cada uma tira dinheiro de alguém diferente.

| Frente | De quem tiramos | O que disputamos | Como ganhamos |
|---|---|---|---|
| **Orçamento do varejista** | iFood, Rappi, 99Food, Keeta (taxa por pedido); Google e Meta (mídia de performance) | A fatia do orçamento de canal que paga por demanda e retenção | Metade do preço por um pedido que se repete sozinho. O varejista entrega; não fazemos logística |
| **Cesta recorrente da casa** | Amazon (Programe e Poupe), Mercado Livre Supermercado, Shopee, assinaturas dos apps de pet e farmácia | Quem é o lugar onde a casa repõe | Multi-loja, WhatsApp e mandato; eles são loja única |
| **Camada do agente** | Google (UCP, AI Mode), OpenAI, Meta (WhatsApp Business AI), o assistente do Mercado Livre, bancos com Agent Pay | Quem fica entre o consumidor e o varejista quando a compra vira delegada | Chegar antes, com varejistas integrados e mandatos ativos, e virar o conector deles |

Não combatemos: o varejista, que é o cliente; as bandeiras e os iniciadores de pagamento, que são os trilhos; as plataformas de e-commerce, que são o canal de integração. O Mercado Livre é concorrente na cesta e parceiro possível se abrir integração. O iFood é concorrente no orçamento e poderia ser parceiro de entrega para varejista sem logística; o plano não conta com isso.

### 8.2 Alternativas do consumidor e do varejista

| Quem | O que faz | Por que não resolve a dor | Por que não nos copia amanhã |
|---|---|---|---|
| Amazon Programe e Poupe [3] | Recorrência com desconto | Uma loja, cadência fixa, só cartão, sem comparação nem substituição inteligente | A Amazon não vai cotar no concorrente |
| iFood e Rappi [4] | Demanda pontual com entrega | Pedido a pedido, caro, sem mandato | O modelo deles é take rate alto sobre pedido avulso |
| App do varejista | Compra na loja | Não compara, não otimiza, não vive no WhatsApp | Cada varejista faria só a própria loja |
| Google e OpenAI | Dado vivo e checkout por protocolo | Chegam ao Brasil por plataforma e por último; não fazem recorrência governada | Quando chegarem, o Mandato é o conector que expõe os parceiros a eles |
| Méliuz e cashbacks | Devolvem comissão após o clique | Não agem, não repõem | Compatível: cashback pode ser benefício do Mandato |
| Assistentes genéricos | Listam | Sem estado, sem frete por CEP, sem ação | O conector do Mandato é a ferramenta que eles chamam |

**O que dura.** Três coisas que levam tempo para copiar: varejistas integrados com credencial delegada e política por canal; mandatos ativos com histórico e ledger; e uma suíte de evals que prova que o agente nunca comprou fora do que foi autorizado. A busca não é uma delas.

---

## 9. Go-to-market

| Etapa | Movimento | Canal | Argumento |
|---|---|---|---|
| **Land** | Uma capital, três parceiros: supermercado, farmácia, pet | Venda direta ao diretor de e-commerce ou CRM; entrada pelo diagnóstico de prontidão agêntica | "Canal de recorrência a 6% em vez de 23%. Seu cliente, sua conta, seu pagamento. Você fica pronto para os agentes do Google e da OpenAI com o mesmo contrato." |
| **Ativar** | O varejista traz as casas | Banner no app, cupom no primeiro pedido, mensagem no WhatsApp da loja | Aquisição a R$ 20 a 40 por casa, em vez de R$ 80 a 120 |
| **Expand** | SaaS de canal agêntico; Agent Pay com um emissor parceiro; segunda cidade | Renovação com base no ledger: pedidos, recorrência, incidentes | "Aqui está o que o agente vendeu, a que custo, com zero compras fora de política" |
| **Scale** | Kernel como infraestrutura: os parceiros expostos a agentes externos; marcas | Parcerias com plataformas de e-commerce e bandeiras | Receita por transação em qualquer agente |
| **Topo de funil barato** | Conector no diretório do Claude e app no ChatGPT | Diretórios dos assistentes | Custo de aquisição perto de zero; não é dependência |

Ciclo de venda esperado ao varejista: 60 a 90 dias do primeiro contato ao piloto, porque o piloto não exige escrita no sistema dele (o nível 2 funciona com carrinho pré-montado no checkout hospedado).

---

## 10. Produto e tecnologia

- **Kernel compartilhado.** Trust & Transaction Kernel do Commerce Control Plane: proveniência, fencing, políticas, ledger, aprovação, compensação, memória com LGPD. O Mandate Engine nasce aqui e o produto B2B o herda.
- **Base aberta.** Fork do `anthropics/commerce-agents` (Apache 2.0): contrato de backend, gates, handoff de checkout por seller, servidor MCP e formato de evals já existem [13]. Metade do trabalho da fase 0 está feito.
- **Cotação determinística.** Score no backend por preço total, prazo e substituição; testável sem modelo; custo previsível.
- **Credencial delegada.** O varejista emite credencial de agente escopada ao cliente e ao mandato; nada de senha do usuário; agente identificado em todo request, no padrão que a Amazon já exige em contrato [19].
- **Pagamento sem licença própria.** Pix Automático para mandato de loja única (valor variável, teto do pagador, mesmo recebedor, como o Banco Central define [20]); Agent Pay e Visa Intelligent Commerce para multi-loja; parceiro ITP para Pix via Open Finance; um toque como fallback universal. Nunca custódia.
- **Assurance.** Suíte de evals com fixtures hostis por fluxo; canário por versão; congelamento em datas críticas. É o que se mostra ao varejista e ao regulador.

---

## 11. Plano de 18 meses e KPIs

| Fase | Meses | Entrega | Gate |
|---|---|---|---|
| 0 · Um parceiro, um toque | 1 a 2 | Fork, skill de reposição, lista da casa, mandato v1, WhatsApp, um parceiro, 200 casas, 50 evals | 40% criam mandato; 2 ou mais pedidos por casa por mês; custo por casa abaixo de R$ 5; incidentes abaixo de 2% |
| 1 · Três parceiros, cotação | 3 a 5 | Cotação multi-loja com frete por CEP, substituições, scheduler, CPA contratado, conector do Claude, teste do Mandato+, 2.000 casas | Retenção de 60 dias acima de 55%; CPA por casa acima de R$ 15 por mês; incidentes abaixo de 1%; um parceiro renova por escrito |
| 2 · Sem toque | 6 a 9 | Nível 3 via Pix Automático; Agent Pay com um emissor; SaaS em cinco varejistas; segunda cidade; 10.000 casas | Mais de 30% dos pedidos sem toque; estornos abaixo de 0,5%; contribuição positiva por casa com aquisição |
| 3 · Infraestrutura | 10 a 18 | Parceiros expostos a agentes externos pelo mesmo kernel; marcas; mandato em AP2; 100.000 casas; kernel B2B | Receita de transações vindas de agentes que não são o nosso |

KPIs que aparecem toda semana: casas ativas com mandato, pedidos por casa, receita CPA por casa, custo de modelo por casa, taxa de pedidos sem toque, incidentes de política, retenção de 60 dias, NPS do parceiro.

Regra de morte: dois gates falhando na mesma fase encerram o B2C e o kernel segue para o B2B. O custo de descobrir isso é a fase 0.

---

## 12. Time e recursos

Ordem de grandeza para chegar ao gate 2 em 9 meses, com 15 meses de caixa. Estimativa, não orçamento.

| Frente | Pessoas | Nota |
|---|---|---|
| Agente, kernel e evals | 2 | Fork da referência; Mandate Engine; suíte de evals |
| Integrações e pagamentos | 2 | Conectores dos parceiros; Pix Automático; Agent Pay; parceiro ITP |
| Produto e canal (WhatsApp, app) | 1 | Onboarding, mandato, aprovação por toque |
| Parcerias e vendas ao varejo | 1 | Três parceiros na cidade 1; SaaS na fase 2 |
| Operação e suporte | 1 | Exceções, incidentes, compensação |
| Jurídico e compliance | Parcial | Termo de mandato, LGPD, contratos com parceiros |

Uso de recursos em 15 meses: equipe ≈ R$ 3,5 a 4,5 milhões; modelo e infraestrutura ≈ R$ 0,3 a 0,5 milhão; co-marketing com parceiros ≈ R$ 0,5 milhão; reserva ≈ R$ 0,5 milhão. **Total ≈ R$ 5 a 6 milhões.**

---

## 13. Riscos e respostas

| Risco | Resposta |
|---|---|
| Varejista não integra ou demora | Nível 2 roda com carrinho pré-montado no checkout hospedado, sem escrita no sistema dele |
| Varejista não aceita comparação com concorrente | "Loja preferida" como padrão; comparação só entre parceiros; CPA menor para quem trouxe a casa |
| Consumidor não passa ao nível 3 | O nível 2 é produto completo e rentável; nível 3 é opt-in por loja e por item |
| Amazon, Mercado Livre e Shopee lançam reposição com IA | São loja única; a aposta é multi-loja, WhatsApp e mandato; se abrirem integração, viram parceiros |
| Margem de supermercado não sustenta 6% | CPA por categoria; farmácia e pet pagam mais; SaaS e marcas |
| Erro de execução | Limites em código, aprovação acima do valor, ledger, compensação pelo parceiro, categorias de baixo custo de erro |
| Trilhos de pagamento atrasam | Um toque e Pix Automático não dependem de Agent Pay nem de Visa |
| Tamanho do mercado | Dezenas de milhões no SOM; a fase 3 muda a escala |

---

## 14. O pedido

**Ao varejista.** Um piloto de 90 dias na sua cidade, com o Mandato integrando pelo seu checkout hospedado ou API, sem custo de integração, sem exclusividade, com CPA só sobre pedido entregue e um painel do que o agente fez. Você entra com 500 clientes do seu CRM e um cupom de primeiro pedido. Ao fim, decidimos juntos o CPA e o SaaS.

**Ao investidor.** R$ 5 a 6 milhões para levar o Mandato do fork ao gate 2 em 9 meses: três varejistas contratados, 10 mil casas com mandato, 30% dos pedidos sem toque e contribuição positiva por casa. O que se compra é a posição de conector do varejo recorrente brasileiro antes de Google e OpenAI cobrirem o mercado, com um kernel que já tem um segundo produto esperando por ele.

---

## Fontes

1. E-Commerce Brasil, *15% dos brasileiros já realizam compras de supermercado online*: https://www.ecommercebrasil.com.br/artigos/varejo-digital-15-dos-brasileiros-ja-realizam-compras-de-supermercado-online ; DScomm, *O crescimento das vendas de alimentos no e-commerce em 2025*: https://dscomm.com.br/tudo-sobre-loja-virtual-news/venda-online-alimentos/
2. ABRE, *Varejo farmacêutico digital atinge R$ 27,5 bilhões em 2025*: https://www.abre.org.br/tecnologia/varejo-farmaceutico-digital-atinge-r-275-bilhoes-em-2025/ ; Abrafarma, *E-commerce de medicamentos se consolida no Brasil*: https://www.abrafarma.com.br/noticias/e-commerce-de-medicamentos-se-consolida-no-brasil-com-movimentacao-recorde
3. Amazon.com.br, *Programe e Poupe*: https://www.amazon.com.br/b?ie=UTF8&node=20870612011
4. Brendi, *Taxa do iFood para restaurantes com delivery: guia para 2026*: https://brendi.com.br/blog/taxa-ifood-restaurantes-delivery-2026/
5. BXTData, *E-commerce Brasil 2026: Mercado Livre e Shopee disputam liderança*: https://www.bxtdata.com/pt/insights/8955/
6. Hypotenuse, *ChatGPT Instant Checkout: What Happened to It in 2026*: https://www.hypotenuse.ai/blog/chatgpts-instant-checkout-the-next-phase-of-agentic-commerce ; Elogic, *ChatGPT Commerce Statistics 2026*: https://elogic.co/blog/chatgpt-commerce-statistics/
7. PagBrasil, *Pix Automático: análise de dados do 1º trimestre de 2026*: https://www.pagbrasil.com/pt-br/blog/pagamento-recorrente/pix-automatico-2026/
8. Upnetix, *Mastercard ativa pagamentos por agentes de IA no Brasil*: https://upnetix.com.br/mastercard-agent-pay-brasil-pagamentos-agentes-ia/ ; Finsiders, *Pagamentos agênticos avançam no Brasil*: https://finsidersbrasil.com.br/tendencias-de-pagamento/pagamentos-agenticos-avancam-no-brasil-e-atraem-bancos-bandeiras-e-fintechs/
9. Meio & Mensagem, *76% dos brasileiros pretendem usar IA nas compras* (Visa): https://www.meioemensagem.com.br/marketing/estudo-76-dos-brasileiros-pretendem-usar-ia-nas-compras
10. CNDL/SPC, *40% aceitam que a IA compre por eles, 95% temem riscos*: https://cndl.org.br/varejosa/40-dos-brasileiros-ja-aceitam-que-a-ia-compre-por-eles-mas-95-temem-riscos-de-seguranca-mostra-pesquisa/
11. Digital Commerce 360, *Adobe: AI-referral traffic data, July 2026*: https://www.digitalcommerce360.com/2026/08/19/adobe-ai-referral-traffic-data-july-2026/
12. Forbes Brasil, *WhatsApp Business lança IA agêntica para PMEs*: https://forbes.com.br/forbes-tech/2026/02/whatsapp-business-lanca-ia-agentica-para-pmes/ ; SocialHub, *WhatsApp Business Marketplace In-App 2026*: https://www.socialhub.pro/blog/whatsapp-business-2026-meta-marketplace-nativo-compras-in-app-pme-brasil/
13. Anthropic, *The anatomy of effective commerce agents* e `anthropics/commerce-agents`: https://claude.com/blog/the-anatomy-of-effective-commerce-agents ; https://github.com/anthropics/commerce-agents
14. UCP Blog, *Agentic payments: Visa, Mastercard, Adyen, FIDO (June 2026)*: https://universalcommerceprotocol.blog/en/agentic-payments-june-2026/
15. Central do Varejo, *Ranking Abras 2025: setor supermercadista fatura R$ 1,145 trilhão em 2025*: https://centraldovarejo.com.br/maiores-redes-de-mercado-do-brasil-ranking-abras-2025-lista-lideres-do-varejo-alimentar/
16. O Presente Pet, *Mercado pet deve faturar R$ 77,2 bi em 2025* (Abinpet): https://opresentepet.com.br/mercado-pet-deve-faturar-r-772-bi-em-2025-abaixo-do-esperado/ ; Abinpet e IPB, participação do e-commerce de 5,4%: https://www.gov.br/agricultura/pt-br/assuntos/camaras-setoriais-tematicas/documentos/camaras-setoriais/animais-e-estimacao/2025/44a-ro-30-10-2025/release-projecao-2025-setor-pet.pdf
17. IBGE, *Censo 2022: país tem 90 milhões de domicílios, 72,4 milhões ocupados*: https://agenciadenoticias.ibge.gov.br/agencia-noticias/2012-agencia-de-noticias/noticias/37238-pais-tem-90-milhoes-de-domicilios-34-a-mais-que-em-2010
18. Análise de viabilidade deste repositório, seções 4 e 9: [`viabilidade-hub-agentico-afiliados.md`](viabilidade-hub-agentico-afiliados.md)
19. Amazon.com.br, *Políticas do Programa de Associados*, seção "Agentes" (14/4/2026): https://associados.amazon.com.br/help/operating/policies/
20. Banco Central do Brasil, *FAQ Pix Automático*: https://www.bcb.gov.br/content/estabilidadefinanceira/pix/pix-automatico-FAQ-participantes.pdf
