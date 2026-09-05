# Mandato: pitch

**A casa abastecida dentro do que você autorizou. Você confirma com um toque. O varejista paga por pedido entregue.**

| | |
|---|---|
| Status | Pitch v0.2, 5 de setembro de 2026, revisado após o teste no gbrain ([`gbrain-analise-mandato.md`](gbrain-analise-mandato.md)). A seção 15 lista o que mudou da v0.1. Nome de trabalho: Mandato. |
| Base | [`proposta-produto-mandato.md`](proposta-produto-mandato.md) (desenho do produto), [`viabilidade-hub-agentico-afiliados.md`](viabilidade-hub-agentico-afiliados.md) (por que o hub de afiliados não fecha) e [`arquitetura-produto.md`](arquitetura-produto.md) (kernel). Fontes numeradas no fim; os números de mercado são de 2025 e 2026. |
| Público | Serve para os dois lados: o varejista que assina o piloto e o investidor que financia por tranches. A seção 14 tem o pedido para cada um. |

---

## 1. Uma frase

O Mandato é um agente que cuida da reposição da casa, o que uma família compra todo mês em supermercado, farmácia e pet, sob um mandato que o consumidor define uma vez: lojas, limites, substituições, prazo, aprovação. Ele cota nos varejistas parceiros, prepara o pedido e o consumidor confirma com um toque no WhatsApp. O varejista paga por pedido entregue. Com o tempo, dentro do mandato e onde o trilho permitir, nem o toque.

---

## 2. A dor

### Do consumidor

A mesma compra, todo mês, feita à mão. Abrir três apps, comparar preço e frete, lembrar o que acabou, refazer o carrinho, aceitar substituição ruim, pagar. Quem já compra supermercado online (15% dos brasileiros [1]) e farmácia online (11% do setor [2]) resolveu a ida à loja, não a tarefa. A Amazon prova a demanda com o Programe e Poupe [3], mas oferece uma loja só, cadência fixa, só cartão, sem comparação e sem WhatsApp.

### Do varejista

Três dores, todas com preço:

- **Paga caro por demanda que não é dele.** O plano do iFood sem entrega custa 12% mais 3,2% a 3,5% de pagamento, cerca de 15%; com entrega, até 26,5% [4]. O varejista aluga o cliente a cada pedido.
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

**O que o consumidor vê.** No WhatsApp: "manda a foto da última nota ou me diz o que você compra sempre". Cinco perguntas viram o mandato. Depois, um resumo por semana com um toque para confirmar. O ledger explica o que foi feito e por quê. Quando o trilho permitir e o consumidor quiser, o toque some para a loja preferida.

**O que o varejista vê.** Um canal de recorrência que ele não tem, a 3% a 8% por pedido entregue, com o cliente vinculado à conta dele, credencial de agente escopada, política por canal e um painel do que o agente fez. E, no mesmo contrato, a preparação para os agentes externos.

**O alinhamento que faz o modelo fechar.** O varejista que traz a casa é a **loja preferida** do mandato. O agente otimiza dentro das lojas que o consumidor autorizou, com a preferida em primeiro; a cotação entre parceiros existe para cobrir falta, ruptura e prazo, é rotulada e entra no contrato. Não vendemos ao varejista um canal que desvia o cliente dele, e não vendemos ao consumidor o menor preço do país. Vendemos a casa abastecida sem esforço, com quem ele já compra.

**Como funciona, em cinco passos.**

1. **Lista da casa.** O agente extrai itens, marcas, quantidades e cadência da nota, da lista ou da conversa.
2. **Mandato.** Lojas permitidas com a preferida em primeiro, limite por pedido e por mês, regra de substituição (mesma categoria até 10% mais caro; marca fixa onde importa), prazo, valor que exige aprovação, validade. Registrado no ledger; serializável no formato de mandatos que Google e Mastercard levaram à FIDO [14].
3. **Cotação.** A cada ciclo, o backend consulta os parceiros e calcula preço total com frete por CEP, prazo e substituições. Ranking determinístico; o modelo só apresenta e explica.
4. **Execução.** Um toque é o produto: resumo no WhatsApp, confirmar, pagamento no meio que o varejista já aceita. Sem toque só em dois casos, ambos opt-in: mandato de loja única pago por Pix Automático, e pares de emissor e adquirente habilitados no Agent Pay, enumerados antes de prometer.
5. **Pós-compra.** Rastreio, exceção, compensação pelo parceiro, e a resposta a "o que você fez com meu dinheiro" em uma tela.

**O que fica de fora.** Eletrônicos e compra avulsa: existe um modo pesquisa acessório, por link de afiliado, sem promessa de receita nem de compra automática. Medicamento com receita. Extensão de navegador. Senha do usuário. Custódia de dinheiro. Amazon e Mercado Livre como "fallback": são concorrentes na cesta; o que o parceiro não tem vira um link, não um catálogo.

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
| Postura | Aceita cotação rotulada entre parceiros para cobrir falta e prazo, com a própria loja como preferida | Exige exclusividade total |

Os primeiros dois parceiros: um supermercado e uma farmácia na mesma cidade. O terceiro, pet, entra depois do gate 0.

### ICP consumidor

Casas com compra recorrente alta e tempo escasso: famílias com crianças, donos de pet, casais que já compram supermercado e farmácia por app. Classe A e B urbana, WhatsApp como interface principal, cesta recorrente de R$ 300 a R$ 700 por mês. Chegam pelo varejista, não por mídia.

### Anti-ICP

Quem compra por lazer, compara eletrônico por hobby ou quer o menor preço absoluto do país em cada item. O Mandato otimiza a cesta dentro dos parceiros; não é o Buscapé.

---

## 6. Mercado: TAM, SAM e SOM

Medido em gasto das casas (GMV), porque a receita do Mandato é uma fração dele. Premissas explícitas. É um dimensionamento de cima para baixo; o piloto produz o de baixo para cima.

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
 Consumidor ──── paga R$ 450 ao varejista (Pix, cartão salvo, Pix Automático ou Agent Pay) ───► Varejista
 Varejista  ──── entrega o pedido ─────────────────────────────────────────────────────────────► Consumidor
 Varejista  ──── fatura mensal do Mandato: 6% dos pedidos entregues = R$ 27 ───────────────────► Mandato
```

O dinheiro do consumidor nunca passa pelo Mandato. A comissão incide sobre o valor dos produtos do pedido entregue, sem frete, e é apurada pelo id do pedido na integração. Pedido cancelado ou devolvido não gera comissão.

| Faixa | Sobre o quê | Por que o varejista paga |
|---|---|---|
| 3% a 4% | Pedidos de uma casa que o próprio varejista trouxe, na loja preferida | Ele já tinha o cliente; paga pela automação e pela frequência que o app dele não entrega |
| 6% a 8% | Pedidos de uma casa que veio de outro parceiro ou do WhatsApp, ou pedido que o Mandato deslocou para ele por falta, prazo ou ruptura | Demanda nova, comparável ao que paga a um marketplace de delivery, pela metade |
| Piso | Pedido mínimo e comissão mínima por pedido, a negociar | Cesta pequena não paga a operação |

**A faixa é hipótese, não preço.** Ela será testada em três faixas nas dez primeiras conversas comerciais, contra três contrafactuais que o varejista tem na cabeça: os cerca de 15% do plano do iFood sem entrega [4], o custo de aquisição da própria mídia dele e a margem líquida da categoria. **Por que contrato direto e não afiliado comum:** a rede de afiliados paga clique com cookie e não sabe o que é recorrência; o Mandato entrega atribuição por id de pedido, credencial delegada por cliente, regra de substituição e quem responde pela exceção. Isso não existe em rede de afiliado, e é o que justifica o contrato.

Sequência: fases 0 e 1 vivem só de comissão. O SaaS entra na fase 2, quando existirem painel e exposição a agentes externos. Marcas entram na fase 3. Assinatura do consumidor só se o teste da fase 1 provar que alguém paga.

### 7.1 Linhas de receita

| Linha | Quem paga | Preço | Quando entra |
|---|---|---|---|
| **CPA por pedido entregue** | Varejista | 3% a 8% do pedido, por categoria e origem; atribuição pelo id do pedido na integração, sem cookie | Dia 1 |
| **Canal agêntico (SaaS)** | Varejista | R$ 8 a 15 mil por mês por rede ou região: credencial delegada, política por canal, ledger, evals, painel, e a exposição a agentes externos | Fase 2 |
| **Marcas** | Indústria de consumo | Posição sugerida quando o consumidor não fixou marca, rotulada; dentro do mandato | Fase 3 |
| **Mandato+** | Consumidor | R$ 9,90 a 14,90 por mês, opcional: otimização multi-loja, proteção de preço | Só se o teste da fase 1 mostrar disposição a pagar |
| **Afiliado** | Marketplaces | Modo pesquisa para o que os parceiros não têm; link, não catálogo | Marginal, sem promessa |

### 7.2 Por que o varejista paga

| O que ele paga hoje | Quanto | O que o Mandato cobra | Diferença |
|---|---|---|---|
| Marketplace de delivery, plano sem entrega | Cerca de 15% (12% mais 3,2% a 3,5% de pagamento) [4] | 3% a 8% por pedido recorrente | Metade ou menos, por um cliente que volta sozinho |
| Marketplace de delivery, plano com entrega | Até 26,5% [4] | Não comparável: o Mandato não entrega | Comparação só para quem tem entrega própria |
| Mídia de performance por cliente novo | R$ 80 a 120 por aquisição (referência) | Co-marketing do próprio varejista | O cliente já é dele; o Mandato o faz voltar |
| Consultoria e integração para agentes externos | Projetos avulsos | SaaS mensal | Mesmo kernel, contínuo |

### 7.3 Economia unitária por casa, com sensibilidade ao preço

| Item | Por casa ativa por mês |
|---|---|
| Cesta pelo Mandato | R$ 450 |
| Modelo (onboarding amortizado e exceções; ciclos são jobs) | R$ 1,50 |
| Infraestrutura, APIs, parceiro de pagamento, WhatsApp por token | R$ 1,50 |
| Suporte e operação em escala | R$ 3,00 |
| Aquisição via parceiro | R$ 20 a 40 |

| Comissão média realizada | Receita por casa | Contribuição por casa | Payback da aquisição |
|---|---|---|---|
| 3% | R$ 13,50 | R$ 7,50 | 3 a 5 meses |
| 4% | R$ 18,00 | R$ 12,00 | 2 a 3 meses |
| **6% (hipótese central)** | **R$ 27,00** | **R$ 21,00** | **1 a 2 meses** |
| 8% | R$ 36,00 | R$ 30,00 | 1 mês |

O custo de modelo é baixo porque a conversa acontece uma vez. Depois, o agente roda como job determinístico e só chama o modelo na exceção. Abaixo de 4% de comissão média realizada o negócio não paga a operação de exceção, e esse é um dos critérios de morte da fase 0.

---

## 8. Diferenciação e concorrência

### 8.1 De quem tiramos dinheiro

Não combatemos o iFood na entrega nem a 99 na corrida. Combatemos em três frentes, e cada uma tira dinheiro de alguém diferente.

| Frente | De quem tiramos | O que disputamos | Como ganhamos |
|---|---|---|---|
| **Orçamento do varejista** | iFood, Rappi, 99Food, Keeta (taxa por pedido); Google e Meta (mídia de performance) | A fatia do orçamento de canal que paga por demanda e retenção | Metade do preço por um pedido que se repete sozinho. O varejista entrega; não fazemos logística |
| **Cesta recorrente da casa** | Amazon (Programe e Poupe), Mercado Livre Supermercado, Shopee, assinaturas dos apps de pet e farmácia | Quem é o lugar onde a casa repõe | Multi-loja dentro do autorizado, WhatsApp, Pix e mandato; eles são loja única e, no caso da Amazon, só cartão |
| **Camada do agente** | Google (UCP, AI Mode), OpenAI, Meta (WhatsApp Business AI), o assistente do Mercado Livre, bancos com Agent Pay | Quem fica entre o consumidor e o varejista quando a compra vira delegada | Chegar antes, com varejistas integrados e mandatos ativos, e virar o conector deles |

Não combatemos: o varejista, que é o cliente; as bandeiras e os iniciadores de pagamento, que são os trilhos; as plataformas de e-commerce, que são o canal de integração. O Mercado Livre e a Amazon são concorrentes na cesta, não fallback. O iFood é concorrente no orçamento e poderia ser parceiro de entrega para varejista sem logística; o plano não conta com isso.

### 8.2 Alternativas do consumidor e do varejista

| Quem | O que faz | Por que não resolve a dor | Por que não nos copia amanhã |
|---|---|---|---|
| Amazon Programe e Poupe [3] | Recorrência com desconto | Uma loja, cadência fixa, só cartão, sem comparação nem substituição inteligente | A Amazon não vai cotar no concorrente nem aceitar Pix Automático de outro recebedor |
| iFood e Rappi [4] | Demanda pontual com entrega | Pedido a pedido, caro, sem mandato | O modelo deles é take rate alto sobre pedido avulso |
| App do varejista | Compra na loja | Não compara, não otimiza, não vive no WhatsApp | Cada varejista faria só a própria loja |
| Google e OpenAI | Dado vivo e checkout por protocolo | Chegam ao Brasil por plataforma e por último; não fazem recorrência governada; o Google não tem Pix nativo | Quando chegarem, o Mandato é o conector que expõe os parceiros a eles |
| Méliuz e cashbacks | Devolvem comissão após o clique | Não agem, não repõem | Compatível: cashback pode ser benefício do Mandato |
| Assistentes genéricos | Listam | Sem estado, sem frete por CEP, sem ação | O conector do Mandato é a ferramenta que eles chamam |

**A cunha, dita de um jeito só.** Nem a Amazon (só cartão) nem o Google (sem Pix) cobrem a reposição paga por Pix recorrente no Brasil. O Mandato cobra do varejista por ciclo de reposição completado, pago por Pix. Foi a ideia com maior nota entre as 72 que o gbrain gerou sobre o brain do produto [21].

**O que dura.** Três coisas que levam tempo para copiar: varejistas integrados com credencial delegada e política por canal; mandatos ativos com histórico e ledger; e uma suíte de evals que prova que o agente nunca comprou fora do que foi autorizado. A busca não é uma delas, e o mandato como formato também não: o AP2 está na FIDO e o kernel de referência é Apache 2.0. O que é nosso é a execução, o dado do mandato vivo e o contrato com o varejista.

---

## 9. Go-to-market

| Etapa | Movimento | Canal | Argumento |
|---|---|---|---|
| **Land** | Uma capital, dois parceiros: supermercado e farmácia; pet depois do gate 0 | Venda direta ao diretor de e-commerce ou CRM; entrada pelo diagnóstico de prontidão agêntica | "Canal de recorrência a 3% a 8% em vez de 15%. Seu cliente, sua conta, seu pagamento, sua loja como preferida. Você fica pronto para os agentes do Google e da OpenAI com o mesmo contrato." |
| **Ativar** | O varejista traz as casas | Banner no app, cupom no primeiro pedido, mensagem no WhatsApp da loja | Aquisição a R$ 20 a 40 por casa, em vez de R$ 80 a 120 |
| **Expand** | SaaS de canal agêntico; Agent Pay com um emissor parceiro; segunda cidade | Renovação com base no ledger: pedidos, recorrência, incidentes | "Aqui está o que o agente vendeu, a que custo, com zero compras fora de política" |
| **Scale** | Kernel como infraestrutura: os parceiros expostos a agentes externos; marcas | Parcerias com plataformas de e-commerce e bandeiras | Receita por transação em qualquer agente |
| **Topo de funil barato** | Conector no diretório do Claude e app no ChatGPT | Diretórios dos assistentes | Custo de aquisição perto de zero; não é dependência |

Duas perguntas entram na primeira reunião com cada varejista, e as respostas vão para o brain: "você aceita cotação rotulada entre parceiros, com sua loja como preferida?" e "o que você faz quando a plataforma oferecer isso de graça?".

O canal WhatsApp tem plano próprio, porque é a proteção mais fraca: consulta escrita a um BSP sobre a política da Meta para agente de compra de terceiro antes do piloto, custo por token medido nas primeiras mil conversas, e app e web mantidos como canal real desde a fase 1.

Ciclo de venda esperado ao varejista: 60 a 90 dias do primeiro contato ao piloto, porque o piloto não exige escrita no sistema dele.

---

## 10. Produto e tecnologia

- **Kernel compartilhado.** Trust & Transaction Kernel do Commerce Control Plane: proveniência, fencing, políticas, ledger, aprovação, compensação, memória com LGPD. O Mandate Engine nasce aqui e o produto B2B o herda.
- **Base aberta, com a metade certa.** Fork do `anthropics/commerce-agents` (Apache 2.0): contrato de backend, gates, handoff de checkout por seller, servidor MCP e formato de evals existem e não transacionam [13]. A metade que carrega o risco é nova: Mandate Engine, conectores de parceiro, pagamento e exceção. Qualquer concorrente herda a mesma base aberta; o que é proprietário é o ledger, os mandatos vivos, as integrações e os contratos.
- **Cotação determinística.** Score no backend por preço total, prazo e substituição; testável sem modelo; custo previsível.
- **Credencial delegada.** O varejista emite credencial de agente escopada ao cliente e ao mandato; nada de senha do usuário; agente identificado em todo request, no padrão que a Amazon já exige em contrato [19].
- **Pagamento sem licença própria.** Um toque com Pix ou cartão salvo no varejista como padrão. Pix Automático para mandato de loja única (valor variável, teto do pagador, mesmo recebedor, como o Banco Central define [20]). Agent Pay e Visa Intelligent Commerce para multi-loja, só para pares de emissor e adquirente habilitados. Parceiro ITP para Pix via Open Finance. Nunca custódia.
- **Assurance.** Suíte de evals com fixtures hostis por fluxo; canário por versão; congelamento em datas críticas. É o que se mostra ao varejista e ao regulador.

---

## 11. Plano de 18 meses e gates

| Fase | Meses | Entrega | Gate |
|---|---|---|---|
| 0 · Wizard of Oz | 1 a 2 | Concierge humano com o agente por trás, no WhatsApp; 30 a 80 casas trazidas por dois parceiros; lista da casa e mandato v1 em código e ledger; um toque com o pagamento que o varejista já aceita; nenhuma integração de escrita, nenhum trilho novo; cadastros de Awin e Shopee pedidos no dia 1 | Confirmação da proposta em 24 horas acima de 40%; segunda cesta em pelo menos 50% das casas; cobertura do parceiro acima de 70% dos itens; comissão realizada ou acordada acima de 4%; compensações abaixo de 2 por 100 pedidos; ao menos um parceiro renova por escrito com cotação rotulada no contrato. Qualquer um abaixo mata. |
| 1 · Três parceiros, cotação | 3 a 5 | Kernel real e conectores em leitura; cotação multi-loja com frete por CEP; substituições por regra; scheduler; CPA contratado com os três; conector do Claude; teste do Mandato+; 2.000 casas | Retenção de 60 dias acima de 55%; receita CPA por casa acima de R$ 15 por mês; incidentes abaixo de 1%; custo por casa abaixo de R$ 5 |
| 2 · Um toque em escala, sem toque em nicho | 6 a 9 | Sem toque para mandato de loja única via Pix Automático; SaaS em cinco varejistas; segunda cidade; 10.000 casas; lista de emissores e adquirentes habilitados no Agent Pay | Mais de 20% dos pedidos de loja única sem toque; estornos abaixo de 0,5%; contribuição positiva por casa com aquisição |
| 3 · Infraestrutura | 10 a 18 | Agent Pay multi-loja com um emissor parceiro; parceiros expostos a agentes externos pelo mesmo kernel; marcas; mandato em AP2; 100.000 casas; kernel B2B | Receita de transações vindas de agentes que não são o nosso |

KPIs que aparecem toda semana: casas ativas com mandato, pedidos por casa, taxa de confirmação, comissão realizada por casa, custo de modelo e de WhatsApp por casa, taxa de pedidos sem toque, incidentes de política, retenção de 60 dias, NPS do parceiro.

Regra de morte: dois gates falhando na mesma fase encerram o B2C e o kernel segue para o B2B. A fase 0 custa entre R$ 300 e 500 mil e responde às três perguntas que hoje não têm evidência: se a casa confirma, se o varejista paga e se ele traz o cliente.

---

## 12. Hipóteses centrais e o que já sabemos

O teste no gbrain mostrou que o pitch v0.1 era feito de fatos de terceiros e desenho próprio, sem uma medição nossa. Esta tabela separa as duas coisas e diz quem assina cada hipótese.

| Hipótese | Evidência hoje | Confiança | Como fecha |
|---|---|---|---|
| H1 · A casa confirma uma proposta pronta com um toque e repete | Só intenção declarada (Visa: 76% e 67%) e o Programe e Poupe como prova indireta | Média | Fase 0: confirmação e segunda cesta |
| H2 · O varejista paga por pedido entregue e traz o cliente | Nenhuma: nenhum varejista, LOI ou conversa registrada | Média-baixa | Dez conversas comerciais e dois LOIs antes do fim da fase 0 |
| H3 · 3% a 8% é aceitável | Só âncoras de terceiros (iFood, Méliuz, OpenAI); nenhuma disposição a pagar medida | Baixa | Teste de preço em três faixas nas mesmas conversas |
| H4 · O varejista aceita cotação rotulada entre parceiros com a própria loja como preferida | Nenhuma; a tensão é reconhecida, não testada | Baixa | Cláusula no contrato do piloto; resposta na primeira reunião |
| H5 · A cesta variável cabe nos trilhos sem licença própria | Pix Automático cabe em loja única; multi-loja depende de emissor e adquirente; JSR exige parceiro ITP | Média | Parecer jurídico curto e vinte transações reais com PSP ou ITP parceiro na fase 2 |
| H6 · O custo por casa fica abaixo de R$ 5 por mês | Estimativa de modelo; WhatsApp por token sem medição | Média | Mil conversas medidas na fase 0 |
| H7 · A Meta não fecha a Cloud API para agente de compra de terceiro | Nenhuma política pública encontrada | Baixa | Consulta escrita a um BSP antes do piloto |

---

## 13. Time e recursos

Ordem de grandeza para chegar ao gate 2 em 9 meses, com 15 meses de caixa, liberado por tranches. Estimativa, não orçamento.

| Frente | Pessoas | Nota |
|---|---|---|
| Agente, kernel e evals | 2 | Fork da referência; Mandate Engine; suíte de evals |
| Integrações e pagamentos | 2 | Conectores dos parceiros; Pix Automático; Agent Pay; parceiro ITP |
| Produto e canal (WhatsApp, app) | 1 | Onboarding, mandato, aprovação por toque |
| Parcerias e vendas ao varejo | 1 | Dois parceiros na cidade 1; SaaS na fase 2 |
| Operação e suporte | 1 | Concierge na fase 0; exceções, incidentes, compensação depois |
| Jurídico e compliance | Parcial | Termo de mandato, LGPD, contratos com parceiros, parecer sobre trilhos |

Uso de recursos em 15 meses: equipe ≈ R$ 3,5 a 4,5 milhões; modelo e infraestrutura ≈ R$ 0,3 a 0,5 milhão; co-marketing com parceiros ≈ R$ 0,5 milhão; reserva ≈ R$ 0,5 milhão. **Total ≈ R$ 5 a 6 milhões, com a primeira tranche de R$ 300 a 500 mil condicionada ao gate 0.**

---

## 14. Riscos e respostas

| Risco | Resposta |
|---|---|
| Pagador e produto desalinhados: o varejista não quer comparação | Loja preferida como padrão; cotação rotulada só para falta, ruptura e prazo; comissão menor para quem trouxe a casa; cláusula no contrato do piloto |
| Varejista não integra ou demora | Fase 0 sem integração de escrita; nível 2 com carrinho pré-montado no checkout hospedado |
| Preço rejeitado | Teste em três faixas; comissão média abaixo de 4% é critério de morte |
| Consumidor não passa ao nível 3 | Um toque é produto completo e rentável; sem toque é opt-in por loja e por item |
| Amazon, Mercado Livre e Shopee lançam reposição com IA | São loja única e, no caso da Amazon, só cartão; a aposta é multi-loja dentro do autorizado, WhatsApp, Pix e mandato; se abrirem integração, viram parceiros |
| Meta fecha ou encarece o canal | Consulta a BSP antes do piloto; custo por token medido; app e web como canal real |
| Margem de supermercado não sustenta a comissão | Comissão por categoria; farmácia e pet pagam mais; SaaS e marcas |
| Erro de execução | Limites em código, aprovação acima do valor, ledger, compensação pelo parceiro, categorias de baixo custo de erro |
| Trilhos de pagamento atrasam | Um toque e Pix Automático não dependem de Agent Pay nem de Visa |
| Tamanho do mercado | Dezenas de milhões no SOM; a fase 3 muda a escala |

### 14.1 E se Google, OpenAI, Anthropic ou Meta entrarem?

Não dá para impedir a entrada. O desenho faz com que a entrada aumente o volume em vez de matar o negócio, porque eles competem pela interface do consumidor e o Mandato fica do outro lado da interface. O teste no gbrain reordenou as proteções: as sólidas são o catálogo local e a posição do lado do varejista; as fracas são o canal WhatsApp e o mandato como primitiva [21].

**O que cada um vai fazer, pelo que já fizeram.**

| Quem | Evidência | Leitura |
|---|---|---|
| OpenAI | Lançou e retirou o checkout dentro do chat em seis meses; ficou com descoberta e links; integra por Shopify, Stripe e grandes marcas [6]. Cobrava cerca de 4% do lojista enquanto o checkout existiu | Precifica o mesmo bolso, mas não assina contrato com varejista regional nem cuida da exceção |
| Anthropic | "Camada de inteligência, não vitrine nem checkout"; publicou o código de agentes de comércio como referência aberta [13]. Parceira do piloto Visa no Brasil [8] | Canal e fornecedor hoje; mais perto do trilho do que a leitura sugere; não contar com neutralidade permanente |
| Google | UCP, Universal Cart, AI Mode, Gemini, Google Pay, mandatos AP2 na FIDO, Merchant Center | A ameaça real; chega ao Brasil por plataforma e por grandes varejistas, sem WhatsApp, sem Pix nativo e sem operação de exceção |
| Meta | WhatsApp Business AI cobrado por token e marketplace nativo com Pix [12] | O maior risco de canal no Brasil: a proteção e a dependência são a mesma coisa |

**Cinco proteções, na ordem que o teste indicou.**

1. **Catálogo local e relação de reposição com o varejista.** Substituição por regra, janela de entrega por CEP, Pix Automático por varejista, Agent Pay com Itaú e Santander, nota fiscal, CDC, suporte em português. O Google integra a Shopify; não integra a farmácia regional. Magalu e Mercado Livre não anunciaram UCP; estão internalizando agente.
2. **Ficar do lado do varejista.** O que a plataforma precisa do lojista é dado vivo, checkout por agente, credencial delegada, política por canal, atribuição e alguém que responda pela exceção. Isso é o kernel. Se o varejo recorrente brasileiro fica legível para qualquer agente através dele, a entrada do Google aumenta o volume do Mandato.
3. **Chegar antes e no padrão deles.** Varejistas integrados e mandatos ativos antes do UCP chegar ao Brasil; implementação local de UCP, ACP e AP2 com Pix dentro. O implementador local do padrão vira parceiro, não alvo.
4. **Ser dono do mandato vivo, não do formato.** O formato é do AP2 e da FIDO; a referência de código é Apache 2.0. O que é nosso é o mandato com histórico, ledger, responsabilidade e o contrato de quem executa. Serializar em AP2 desde o dia 1 para ser executável a partir do Gemini ou do ChatGPT.
5. **Não depender de nenhuma superfície.** É a proteção mais fraca, porque o WhatsApp é a Meta. Por isso tem plano próprio: BSP, custo por token, app e web reais, conectores no Claude e no ChatGPT como topo de funil.

**Cenários.**

| Cenário | Probabilidade | Resposta |
|---|---|---|
| Plataformas cobrem grandes lojistas por UCP e ACP; varejo recorrente local segue fragmentado | Alta, 12 a 24 meses | O Mandato é o integrador; o agente próprio segue como nicho rentável |
| Google lança reposição agêntica no Brasil com Pix | Média, 24 meses ou mais | Pivô para o lado do varejista: provedor de agente de negócio UCP; receita por transação em qualquer agente. Mesmo kernel |
| Anthropic ou OpenAI restringem apps de comércio ou cobram taxa | Média | Perde-se um canal de topo de funil; WhatsApp e app seguem |
| Meta restringe agente de terceiro na Cloud API ou faz recorrência multi-lojista | Média | App e web como canal; o Mandato como camada que os lojistas do WhatsApp usam para atender o agente da Meta |
| Um deles, ou Mercado Livre, iFood ou um banco, compra a empresa | Possível a partir da fase 2 | Saída legítima; o ativo é o varejo integrado mais os mandatos ativos |

**O que não protege.** Competir em qualidade de modelo, competir em polimento de interface, exclusividade com varejista, e reter dado contra o consumidor.

---

## 15. O pedido

**Ao varejista.** Um piloto de 60 dias na sua cidade, sem integração de escrita e sem custo: o Mandato monta os pedidos das casas que você indicar, cota com a sua loja como preferida e manda o resumo no WhatsApp para o cliente confirmar com um toque e pagar do jeito que já paga a você. Você entra com 40 clientes do seu CRM e um cupom de primeiro pedido, e aceita a cláusula de cotação rotulada. Ao fim, decidimos juntos a comissão, testada em três faixas, e o SaaS.

**Ao investidor.** R$ 5 a 6 milhões em tranches para levar o Mandato do fork ao gate 2 em 9 meses. A primeira tranche, de R$ 300 a 500 mil, financia oito semanas de Wizard of Oz com dois parceiros e 30 a 80 casas, e responde às três perguntas que hoje não têm evidência: se a casa confirma, se o varejista paga e se ele traz o cliente. Se qualquer critério de morte da fase 0 falhar, o resto não é liberado e o kernel segue para o B2B. O que se compra é a posição de conector do varejo recorrente brasileiro antes de Google e OpenAI cobrirem o mercado, com um kernel que já tem um segundo produto esperando por ele.

---

## 16. O que mudou da v0.1 para a v0.2

Todas as mudanças vêm do teste no gbrain [21].

| Mudança | Motivo |
|---|---|
| "Sem toque desde a fase 2" virou "um toque é o produto"; sem toque só para loja única via Pix Automático e pares habilitados no Agent Pay, enumerados antes de prometer | Pix Automático exige recebedor único e recorrência periódica; Agent Pay exige emissor e adquirente habilitados; Visa só pilota no fim de 2026 |
| A comparação com o iFood passou de 26,5% para os cerca de 15% do plano sem entrega | A comparação com o plano com entrega não é like-for-like: o Mandato não entrega |
| A comissão de 3% a 8% passou de preço a hipótese, com três contrafactuais e teste em três faixas; sensibilidade de 3% a 8% na economia unitária; comissão média abaixo de 4% é critério de morte | Não havia nenhuma disposição a pagar medida, só âncoras de terceiros |
| A tensão entre o varejista pagador e a comparação entre parceiros virou desenho explícito: loja preferida, cotação rotulada só para falta, ruptura e prazo, cláusula no contrato, pergunta na primeira reunião | Era a hipótese mais frágil apontada pelo teste |
| Amazon e Mercado Livre deixaram de ser "fallback" e viraram concorrentes; o modo pesquisa é link, não catálogo, sem promessa de receita | Contradição interna entre os documentos |
| A fase 0 virou um Wizard of Oz de oito semanas com 30 a 80 casas e dois parceiros, sem integração de escrita, com os seis critérios de morte do teste | As três lacunas que importam fecham por menos do que custa uma sprint de conector |
| As proteções contra plataformas foram reordenadas: catálogo local e lado do varejista no topo, WhatsApp e mandato como primitiva na base; a Meta ganhou plano próprio; a Anthropic deixou de ser lida como neutra | O teste mostrou que a proteção do canal e a dependência do canal são a mesma coisa, e que Anthropic e OpenAI estão no piloto Visa do Brasil |
| O repositório aberto deixou de ser "metade do trabalho da fase 0" e passou a ser "a metade que não transaciona" | A metade nova é a que carrega o risco |
| Entrou a seção 12 com as sete hipóteses centrais, a evidência atual, a confiança e como cada uma fecha | O brain não tinha nenhum take: ninguém assinava a tese nem dizia com que convicção |
| O pedido ao investidor passou a ser por tranches, com a primeira condicionada ao gate 0 | Coerência com a regra de morte |

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
21. Teste do Mandato no gbrain de Garry Tan, com as respostas brutas: [`gbrain-analise-mandato.md`](gbrain-analise-mandato.md) e [`gbrain/`](gbrain/)
