# Mandato: reposição da casa por mandato

**Proposta de produto comercialmente viável**

| | |
|---|---|
| Status | Proposta v0.1, 5 de setembro de 2026. Nome de trabalho: **Mandato**. |
| Origem | Conclusões de [`viabilidade-hub-agentico-afiliados.md`](viabilidade-hub-agentico-afiliados.md) (o hub B2C de afiliados não fecha a conta) e kernel de [`arquitetura-produto.md`](arquitetura-produto.md) (Trust & Transaction Kernel, Gateway, Assurance). Verificações adicionais na web em 5/9/2026, numeradas no fim. |
| Tese | **Um agente que cuida da reposição da casa sob um mandato do consumidor, compra nos varejistas parceiros pela melhor combinação de preço, prazo e substituição, e é pago pelo varejista por pedido entregue, não por clique.** |

## Resumo

O hub original tinha três defeitos que nenhuma execução resolve: o afiliado paga o clique que a compra automática elimina; a busca vira commodity dentro dos assistentes; e uma conversa de modelo custa mais do que a comissão que gera. Este desenho troca as três premissas.

| Premissa do hub | Premissa do Mandato |
|---|---|
| Comprar qualquer coisa em qualquer lugar | Repor o que a casa consome todo mês: limpeza, higiene, despensa, pet, bebê, farmácia sem receita |
| Receita de afiliado por clique | Receita do varejista parceiro por pedido entregue, atribuído por integração, sem cookie |
| Uma conversa por compra | Uma conversa de onboarding, depois ciclos automáticos em que o modelo só entra na exceção |
| Cobertura dos grandes marketplaces | Três a cinco varejistas parceiros por cidade, com Amazon e Mercado Livre como fallback via afiliado para o que os parceiros não têm |
| Aquisição paga no WhatsApp | O varejista traz o cliente: o agente é o canal de recorrência dele |
| Compra sem toque como promessa futura | Compra sem toque desde a fase 2, porque é o único caso em que os trilhos de pagamento já existem no Brasil |

O produto tem um cliente que paga hoje (o varejista, que já paga de 12% a 26% a marketplaces de delivery [1]), um usuário com dor real e frequente (a casa que refaz a mesma compra toda semana), um mecanismo de atribuição que não depende de clique, e um caminho de pagamento sem licença própria. É um negócio de dezenas de milhões de reais por ano em escala nacional, não de bilhões. O upside vem depois: o mesmo kernel vira a infraestrutura que expõe esses varejistas aos agentes do Google e da OpenAI.

---

## 1. Por que reposição

Recorrência é onde o mandato faz sentido, onde os trilhos existem e onde o varejista paga.

| Fato | Fonte | Consequência |
|---|---|---|
| 15% dos brasileiros já compram supermercado online; alimentos e bebidas cresceram 31% no e-commerce em 2025 | [2] | Hábito formado numa base grande e em crescimento |
| Varejo farmacêutico digital chegou a R$ 27,5 bi em 2025, mais de 11% do setor; vendas por site e app cresceram 54,8% em doze meses | [3] | A categoria de maior margem entre as recorrentes já é digital |
| A Amazon opera o Programe e Poupe no Brasil: entregas recorrentes com 10% de desconto, frete grátis a partir da segunda, só no cartão, categorias de limpeza, higiene, pet, bebê e despensa | [4] | O maior varejista do mundo confirma a demanda e mostra o limite: uma loja só, cadência fixa, sem comparação, sem substituição inteligente, sem WhatsApp |
| Pix Automático cresceu 182% em transações entre o quarto trimestre de 2025 e o primeiro de 2026; reduz churn involuntário e custa 65% menos que cartão numa assinatura | [5] | O trilho de recorrência mais barato do país está pronto e o varejista tem motivo para adotá-lo |
| iFood cobra 12% no plano básico e 23% no plano com entrega, mais 3,2% a 3,5% de pagamento, chegando a 26,5% | [1] | O varejista já aceita pagar muito por demanda de terceiros; um canal de recorrência a 5% a 8% é barato |
| Mastercard Agent Pay processou transações reais no Brasil com Itaú e Santander; Visa pilota no fim de 2026 | [6] | Multi-loja com pagamento por agente vira possível em 2027 sem o hub ser instituição de pagamento |

E o que a reposição evita: a compra de eletrônicos que paga 4% e exige comparação universal; a dependência do Mercado Livre; o erro caro na frente do consumidor (uma cesta de supermercado errada custa uma substituição, não um estorno de R$ 2.500).

---

## 2. Para quem

**Usuário.** A casa que refaz a mesma compra: famílias com crianças, donos de pet, pessoas que já compram supermercado ou farmácia pelo app e odeiam a tarefa. Vive no WhatsApp. Aceita que um agente decida o trivial e quer ser avisada do que sai do padrão. As pesquisas de 2026 dizem que 67% dos brasileiros se sentem confiantes em deixar um agente comprar e 95% temem a segurança [7]: o produto precisa mostrar o mandato e o ledger, não escondê-los.

**Cliente que paga.** Varejistas com e-commerce e entrega próprios, em categorias recorrentes: redes de supermercado regionais e nacionais, redes de farmácia, redes de pet, atacarejo com delivery. Eles têm três dores: pagam caro por demanda de marketplace de delivery, perdem recorrência para a Amazon e para o Mercado Livre, e não sabem como ficar legíveis para os agentes do Google e da OpenAI que estão chegando.

**Não é o alvo.** Quem compra eletrônicos de vez em quando. Varejistas sem operação de entrega. Medicamentos com receita.

---

## 3. A experiência

1. **Entrada.** O usuário chega por um link do varejista parceiro ("ative a reposição automática e ganhe R$ 20 no primeiro pedido") ou pelo WhatsApp. Manda a foto da última nota, cola a lista ou diz o que compra sempre. O agente monta a **lista da casa**: itens, marca fixa ou livre, quantidade, cadência estimada.
2. **Mandato.** Cinco perguntas, com respostas como chips: lojas permitidas (padrão: quem trouxe, mais as parceiras), limite por pedido, limite mensal, regra de substituição (mesma categoria até 10% mais caro; marca fixa nos itens que importam), prazo máximo de entrega, valor acima do qual pede aprovação, validade. O mandato fica salvo, legível, editável, e é registrado no ledger.
3. **Ciclo.** Na cadência prevista, ou quando o usuário diz "acabou o sabão", o agente monta o pedido, cota nos parceiros (preço total com frete, prazo, substituições necessárias) e escolhe. No **nível 2**, manda o resumo pelo WhatsApp com um toque para confirmar; o pagamento é o que o varejista já aceita. No **nível 3**, executa dentro do mandato e manda o comprovante.
4. **Exceção.** Item indisponível, preço acima do teto, prazo estourado: aplica a regra de substituição ou pergunta uma vez, com as opções como chips. É o único momento em que o modelo conversa de verdade; o resto é job.
5. **Pós-compra.** Rastreio pela integração do parceiro. Problema na entrega aciona o parceiro com o pedido em mãos. "O que você fez e por quê" é uma pergunta que o ledger responde.
6. **Fora da recorrência.** "Preciso de uma cafeteira" cai no **modo pesquisa**: o hub leve de afiliados (Shopee, Awin, Amazon), sem promessa de compra automática. Existe para não mandar o usuário embora, não para ser o negócio.

---

## 4. O que é diferente, e de quem

| Alternativa | O que faz | O que não faz | Mandato |
|---|---|---|---|
| Assistente genérico (ChatGPT, Claude com busca) | Responde e lista | Não tem estado, não cota com frete por CEP, não age, não guarda mandato | O assistente vira canal: o conector do Mandato é a ferramenta que ele chama |
| Amazon Programe e Poupe [4] | Recorrência com desconto numa loja | Uma loja, cadência fixa, cartão obrigatório, sem substituição inteligente, sem comparação | Multi-loja, cadência aprendida, Pix, substituição por regra |
| iFood e Rappi [1] | Demanda pontual com entrega, a 12% a 26% | Pedido a pedido, sem mandato, sem otimização entre lojas, caro para o varejista | Recorrência governada a 5% a 8% |
| App do próprio varejista | Compra na loja | Não compara, não otimiza, não vive no WhatsApp | O agente é o canal de recorrência que o app não tem |
| Méliuz e cashbacks | Devolve parte da comissão após o clique | Não age, não repõe | Modelos compatíveis; cashback pode ser benefício do Mandato |
| Google e OpenAI | Chegarão com dado vivo e checkout por protocolo | Não fazem recorrência governada; chegam ao Brasil por plataforma e por último | O kernel do Mandato é o que expõe o varejista a eles (fase 3) |

A diferença que dura não é a busca. É a combinação de **mandato, execução recorrente e pagamento dentro de limites**, sobre varejistas que integraram uma vez.

---

## 5. Quem paga e quanto

### 5.1 Linhas de receita

| Linha | Quem paga | Mecanismo | Quando |
|---|---|---|---|
| **CPA por pedido entregue** | Varejista | 5% a 8% do valor do pedido, negociado por categoria; atribuição por id de pedido na integração, sem cookie | Fase 0 |
| **Canal agêntico (SaaS)** | Varejista | Mensalidade por loja ou região: integração, credencial delegada por cliente, política por canal, ledger, evals, painel. É o módulo Gateway e Assurance do outro documento | Fase 2 |
| **Marcas** | Indústria de bens de consumo | Posição sugerida quando o usuário não fixou marca, rotulada como tal, dentro das regras do mandato. Compromete neutralidade se mal feito; entra só com disclosure | Fase 3 |
| **Mandato+** | Consumidor | Assinatura opcional: otimização multi-loja, proteção de preço, prioridade. Testar a R$ 9,90 a R$ 14,90 antes de decidir se existe | Teste na fase 1 |
| **Afiliado no modo pesquisa** | Marketplaces | Comissão por clique como o hub original | Marginal |

### 5.2 Economia unitária ilustrativa

Premissas explícitas; os números de mercado são de 2025 e 2026 [1][2][3][5], os de custo vêm da análise anterior.

| Item | Premissa | Valor por casa ativa por mês |
|---|---|---|
| Cesta recorrente | R$ 450 em dois ou três pedidos, entre supermercado, farmácia e pet | |
| Receita CPA | 6% da cesta | **R$ 27** |
| Custo de modelo | Onboarding amortizado mais exceções; ciclos são jobs determinísticos | R$ 1,50 |
| Infraestrutura, APIs, parceiro de pagamento | | R$ 1,50 |
| Suporte e operação em escala | | R$ 3,00 |
| **Contribuição** | | **≈ R$ 21** |
| Aquisição via parceiro | Co-marketing e cupom do varejista | R$ 20 a R$ 40 |
| Aquisição via mídia | Referência | R$ 80 a R$ 120 |
| Payback | | 1 a 2 meses via parceiro; 4 a 6 via mídia |

| Escala | Casas ativas | Receita CPA anual | Mais SaaS (30 varejistas a R$ 8 mil por mês) |
|---|---|---|---|
| Piloto | 2.000 | R$ 650 mil | |
| Cidade | 20.000 | R$ 6,5 milhões | R$ 2,9 milhões |
| Nacional | 100.000 | R$ 32 milhões | R$ 2,9 milhões ou mais |

Leitura honesta: **é um negócio de dezenas de milhões por ano em escala nacional.** O que o faz valer mais é a fase 3, quando o kernel que executa mandatos vira a infraestrutura que expõe o varejo recorrente brasileiro aos agentes externos, e a receita passa a ser por transação em qualquer agente, não só no nosso.

---

## 6. Arquitetura

```
 SUPERFÍCIES    WhatsApp (principal) │ App e web │ Conector Claude (MCP) │ App ChatGPT (MCP)
                     │                    │              │                     │
 AGENTE         Agente de reposição: fork de shopping-agent, skills em português
                (reposição, lista-da-casa, excecao, pesquisa-avulsa, cuidado-pos-compra)
                     │ propõe: pedido cotado, substituições, aprovação
 ┌───────────────────┴──────────────────────────────────────────────────────────────────┐
 │  TRUST & TRANSACTION KERNEL (compartilhado com o Commerce Control Plane)             │
 │  MANDATE ENGINE · lista da casa · políticas · proveniência · fencing · ledger        │
 │  aprovação por toque · scheduler de ciclos · monitor · compensação · memória e LGPD  │
 └───────────────────┬──────────────────────────────────────────────────────────────────┘
                     │ StorefrontBackend FEDERADO sobre parceiros
 COTAÇÃO        produto canônico (EAN) → ofertas por parceiro (preço, frete por CEP, prazo, estoque)
                score determinístico pelo mandato; explicação por oferta
                     │
 CONECTORES     Parceiro A (API própria) │ Parceiro B (UCP ou ACP pela plataforma) │ Parceiro C
                (checkout hospedado com carrinho pré-montado) │ Fallback afiliado (Shopee, Awin, Amazon)
                     │
 PAGAMENTO      Pix Automático (loja única) │ Agent Pay ou Visa IC (multi-loja) │ JSR via ITP parceiro
                │ Um toque: Pix copia e cola ou cartão salvo no varejista (sempre disponível)
                     │
 VAREJISTAS     Supermercado │ Farmácia │ Pet │ Atacarejo
```

### 6.1 O que vem do repositório de referência e do kernel

| Peça | Origem | Uso |
|---|---|---|
| `StorefrontBackend`, gates de proveniência, fencing, memória, servidor MCP, evals | `anthropics/commerce-agents` | Base do agente e do conector; a análise anterior detalha o mapeamento |
| `checkout_handoff` por seller | Repo | Nível 2: um botão por parceiro com carrinho pré-montado |
| Ledger, motor de políticas, aprovação, compensação, console | Commerce Control Plane, fases 1 e 2 | Iguais |
| Mandate Engine | Novo aqui, fase 5 lá | Núcleo do produto; o outro documento o herda pronto |
| Gateway (exposição a agentes externos) | Commerce Control Plane, fase 4 | Fase 3 do Mandato: os parceiros ficam legíveis para Google e OpenAI pelo mesmo kernel |

### 6.2 Decisões de desenho

- **A lista da casa é a entidade central**, não o carrinho. Item, marca fixa ou livre, quantidade, cadência aprendida, última compra, tolerância de substituição. O carrinho é derivado dela a cada ciclo.
- **Cotação determinística no backend.** Score por preço total com frete, penalidade de prazo, penalidade de substituição. O modelo apresenta e explica; não calcula. Testável sem modelo.
- **Mandato avaliado em dois pontos.** Em Propor, filtra ofertas fora das regras antes de o modelo as ver. Em Executar, recusa o pedido fora do mandato mesmo que o modelo tente. O teto mensal e a validade vivem no ledger.
- **Credencial delegada, nunca senha.** O usuário vincula a conta do varejista ao Mandato pelo fluxo do próprio varejista (OAuth ou telefone). O varejista emite uma credencial de agente escopada àquele cliente e àquele mandato. Isso é o que o módulo Gateway vende ao varejista, e é o que os termos de uso permitem, ao contrário de usar a senha do usuário.
- **Identificação de agente em todo request**, no padrão que a Amazon já exige em contrato [8].
- **Nível 2 sempre disponível.** Um toque com Pix copia e cola ou cartão salvo no varejista não depende de trilho novo. O nível 3 é opt-in por mandato e por parceiro.
- **Nunca custódia.** O varejista é o vendedor e recebe o pagamento. O Mandato não guarda cartão nem saldo.
- **Modelo por custo.** Haiku para ciclos e exceções simples, Sonnet para onboarding e exceções ambíguas, escolhido por sweep na suíte de evals. Custo por casa por mês é KPI desde a fase 0.

---

## 7. Pagamento por tipo de mandato

| Tipo de mandato | Trilho | Como funciona | Status |
|---|---|---|---|
| **Loja única recorrente** ("supermercado X toda semana, até R$ 400") | **Pix Automático** | O usuário autoriza a recorrência do varejista com valor máximo igual ao teto do mandato; a cada ciclo o Mandato envia o pedido pela integração e o varejista cobra pelo Pix Automático. Valor variável é permitido; o recebedor é sempre o mesmo CNPJ, como o Banco Central exige [9] | Pronto, se o PSP do varejista oferece Pix Automático como recebedor; a adoção cresce rápido [5] |
| **Multi-loja variável** ("qualquer parceira, até R$ 500 por pedido") | **Mastercard Agent Pay**, depois **Visa Intelligent Commerce** | O usuário define limites no app do emissor; o Mandato paga com token de agente; o adquirente do varejista aceita o token | Agent Pay ativo com Itaú e Santander [6]; exige emissor do usuário e adquirente do parceiro habilitados. 2027 para escala |
| **Multi-loja por Pix** | **Open Finance: pagamentos automáticos via JSR**, com parceiro ITP | Consentimento até um limite, sem autenticar a cada pagamento; um recebedor por consentimento a confirmar | JSR obrigatória desde fevereiro de 2026 [10]; a verificar se o consentimento cobre mais de um recebedor. O Mandato não pede licença de ITP |
| **Qualquer um, fallback** | **Um toque** | Resumo no WhatsApp, botão confirmar, pagamento no meio que o varejista já aceita | Pronto |

---

## 8. Jurídico

| Tema | Posição | A validar |
|---|---|---|
| Natureza do serviço | O Mandato atua como mandatário do consumidor com poderes delimitados, no sentido do contrato de mandato do Código Civil; o ledger é a prova de que agiu dentro dos poderes | Redação do termo de mandato e do consentimento; parecer jurídico |
| CDC | O varejista é o fornecedor e responde pelo produto, entrega e arrependimento; o Mandato é intermediador de indicação e execução. A tendência do STJ amplia a responsabilidade de intermediadores conforme o controle que exercem [11] | SLA com parceiros; reserva para incidentes; seguro |
| LGPD | A lista da casa é dado de consumo; itens de farmácia beiram dado de saúde. Consentimento específico, retenção curta, portal do titular, exclusão integrada. A ANPD prioriza IA em 2026 e 2027 [12] | Medicamentos com receita ficam fora por desenho |
| Pagamentos | Nunca custódia; Pix Automático e tokens de agente são do varejista e do emissor; ITP é parceiro | Contrato com ITP quando a fase 2 chegar |
| Termos de uso dos varejistas | Credencial delegada emitida pelo parceiro, agente identificado; nada de senha do usuário | Cláusula padrão de canal agêntico no contrato com o parceiro |

---

## 9. Go-to-market

1. **Uma cidade, três parceiros.** Um supermercado com entrega própria, uma rede de farmácia, uma rede de pet. Escolher pelo varejista-âncora: quem tem e-commerce em plataforma com API ou checkout hospedado, entrega própria e um time de CRM que quer recorrência.
2. **A venda ao varejista é a do outro documento.** Entrada pelo diagnóstico de Agentic Commerce Readiness; o Mandato é o primeiro agente externo que ele atende, com governança. Argumento em uma linha: "canal de recorrência a 6% em vez de 23%, cliente com mandato tem churn menor, e você fica pronto para os agentes do Google e da OpenAI com o mesmo kernel".
3. **O varejista traz o usuário.** Banner no app, cupom no primeiro pedido, mensagem no WhatsApp da loja. O padrão do mandato favorece quem trouxe ("loja preferida"); o usuário pode ampliar. Essa tensão é real e fica explícita no contrato: comparação só entre parceiros, com disclosure.
4. **Assistentes como topo de funil barato.** Conector no diretório do Claude (gestão do mandato e modo pesquisa) e app no ChatGPT (modo pesquisa com checkout externo). Medir custo de aquisição; não depender.
5. **Segunda cidade só depois do gate 2.**

---

## 10. Fases e critérios de morte

| Fase | Entrega | Gate para seguir | Duração |
|---|---|---|---|
| **0 · Um parceiro, um toque** | Fork e localização; skill de reposição; lista da casa; mandato v1 em código e ledger; WhatsApp; um parceiro por API ou checkout hospedado; nível 2; 200 casas; 50 evals com fixtures hostis | 40% das casas criam mandato; 2 ou mais pedidos por casa por mês; custo por casa abaixo de R$ 5; incidentes abaixo de 2% | 8 semanas |
| **1 · Três parceiros, cotação** | Cotação multi-loja com frete por CEP; substituições por regra; scheduler e monitor; CPA contratado com os três; conector do Claude; teste de Mandato+; 2.000 casas | Retenção de 60 dias acima de 55%; receita CPA por casa acima de R$ 15 por mês; incidentes abaixo de 1%; pelo menos um parceiro renova o CPA por escrito | 3 meses |
| **2 · Sem toque** | Nível 3 para loja única via Pix Automático; Agent Pay com um emissor parceiro para multi-loja; SaaS de canal agêntico vendido a pelo menos cinco varejistas; segunda cidade; 10.000 casas | Mais de 30% dos pedidos sem toque; estornos e reclamações abaixo de 0,5%; margem de contribuição positiva por casa incluindo aquisição | 3 a 4 meses |
| **3 · Infraestrutura** | Parceiros expostos a agentes externos (UCP, ACP) pelo mesmo kernel; marcas com disclosure; mandato serializado em AP2; 100.000 casas; venda do kernel B2B | Receita de transações vindas de agentes que não são o nosso | 6 a 12 meses |

Regra de morte: **se dois gates de uma mesma fase falharem, o produto B2C para e o kernel segue para o B2B.** A fase 0 custa pouco justamente porque o repositório de referência entrega metade do que ela precisa.

---

## 11. Riscos honestos

| Risco | Probabilidade | Mitigação |
|---|---|---|
| Varejistas não integram, ou integram devagar | Alta | Começar por quem tem API ou checkout hospedado; o nível 2 funciona com carrinho pré-montado sem escrita no sistema do parceiro |
| Varejista não aceita comparação com concorrentes | Alta | "Loja preferida" como padrão; comparação só entre parceiros; CPA menor para quem trouxe o cliente |
| Consumidor não passa ao nível 3 | Média | O nível 2 é produto completo e rentável; nível 3 é opt-in por item e por loja |
| Amazon, Mercado Livre e Shopee fazem reposição com IA na própria loja | Alta | Eles são loja única; a aposta é multi-loja, WhatsApp e mandato; se abrirem integração, viram parceiros |
| Margem de supermercado não sustenta CPA de 6% | Média | CPA por categoria (farmácia e pet pagam mais); SaaS; marcas |
| Erro de execução (cesta errada, cobrança fora do limite) | Média | Limites em código, aprovação acima do valor, ledger, compensação pelo parceiro, categorias de baixo custo de erro |
| Trilhos de pagamento atrasam | Média | Nível 2 e Pix Automático não dependem de Agent Pay nem de Visa |
| Tamanho do mercado | Certa | Negócio de dezenas de milhões; a fase 3 é o que muda a escala |

---

## 12. O que este produto não é

- Não é comparador universal. O modo pesquisa é acessório e paga só o próprio custo.
- Não é extensão de navegador nem agente que usa a senha do usuário.
- Não é instituição de pagamento nem guarda cartão.
- Não depende do Mercado Livre abrir API nem da OpenAI reabrir o checkout.
- Não compra medicamento com receita nem categoria com custo de erro alto.
- Não promete "compra automática de qualquer coisa"; promete "a casa abastecida dentro do que você autorizou".

---

## 13. Decisões em aberto

1. **Vertical de entrada.** Farmácia sem receita tem margem, 11% de penetração digital e crescimento de 55%, mas beira dado de saúde. Supermercado tem frequência e cesta maior, com margem fina. Pet tem engajamento alto e assinaturas já existem. Recomendação: supermercado e farmácia juntos numa cidade, pet como terceiro parceiro.
2. **Varejista-âncora e cidade.** Depende de quem tem plataforma aberta e time de CRM disposto.
3. **Emissor parceiro para Agent Pay.** Itaú ou Santander já processam; conversar antes da fase 2.
4. **Mandato+.** Só existe se o teste da fase 1 mostrar disposição a pagar.
5. **Nome.** "Mandato" é jurídico e explica o produto; testar se o consumidor entende.

---

## Fontes

Complementares às da análise de viabilidade, que cobre protocolos, marketplaces, trilhos de pagamento, regulação e concorrentes.

1. Brendi, *Taxa do iFood para restaurantes com delivery: guia para 2026*: https://brendi.com.br/blog/taxa-ifood-restaurantes-delivery-2026/ ; *iFood: quanto cobra em taxas em 2026*: https://brendi.com.br/blog/ifood-quanto-cobra/
2. E-Commerce Brasil, *15% dos brasileiros já realizam compras de supermercado online*: https://www.ecommercebrasil.com.br/artigos/varejo-digital-15-dos-brasileiros-ja-realizam-compras-de-supermercado-online ; DScomm, *O crescimento das vendas de alimentos no e-commerce em 2025*: https://dscomm.com.br/tudo-sobre-loja-virtual-news/venda-online-alimentos/
3. ABRE, *Varejo farmacêutico digital atinge R$ 27,5 bilhões em 2025*: https://www.abre.org.br/tecnologia/varejo-farmaceutico-digital-atinge-r-275-bilhoes-em-2025/ ; Abrafarma, *E-commerce de medicamentos se consolida no Brasil*: https://www.abrafarma.com.br/noticias/e-commerce-de-medicamentos-se-consolida-no-brasil-com-movimentacao-recorde
4. Amazon.com.br, *Programe e Poupe*: https://www.amazon.com.br/b?ie=UTF8&node=20870612011 ; *Termos e Condições do Programe e Poupe*: https://www.amazon.com.br/gp/help/customer/display.html?nodeId=GWFU7NECYFLW6M6U
5. PagBrasil, *Pix Automático: análise de dados do 1º trimestre de 2026*: https://www.pagbrasil.com/pt-br/blog/pagamento-recorrente/pix-automatico-2026/ ; Central do Varejo, *Volume de transações via Pix Automático cresce 182%*: https://centraldovarejo.com.br/volume-de-transacoes-via-pix-automatico-cresce-182/
6. Upnetix, *Mastercard ativa pagamentos por agentes de IA no Brasil*: https://upnetix.com.br/mastercard-agent-pay-brasil-pagamentos-agentes-ia/ ; Finsiders, *Pagamentos agênticos avançam no Brasil*: https://finsidersbrasil.com.br/tendencias-de-pagamento/pagamentos-agenticos-avancam-no-brasil-e-atraem-bancos-bandeiras-e-fintechs/
7. Meio & Mensagem, *76% dos brasileiros pretendem usar IA nas compras* (Visa): https://www.meioemensagem.com.br/marketing/estudo-76-dos-brasileiros-pretendem-usar-ia-nas-compras ; CNDL/SPC, *40% aceitam que a IA compre por eles, 95% temem riscos*: https://cndl.org.br/varejosa/40-dos-brasileiros-ja-aceitam-que-a-ia-compre-por-eles-mas-95-temem-riscos-de-seguranca-mostra-pesquisa/
8. Amazon.com.br, *Políticas do Programa de Associados*, seção "Agentes" (14/4/2026): https://associados.amazon.com.br/help/operating/policies/
9. Banco Central do Brasil, *FAQ Pix Automático*: https://www.bcb.gov.br/content/estabilidadefinanceira/pix/pix-automatico-FAQ-participantes.pdf
10. Sensedia, *Obrigatoriedade da JSR no Open Finance em 2026*: https://www.sensedia.com.br/post/fique-por-dentro-da-obrigatoriedade-da-jsr-no-open-finance-em-2026
11. Conjur, *Marketplaces e responsabilidade solidária: limites à equiparação no CDC* (20/2/2026): https://www.conjur.com.br/2026-fev-20/marketplaces-e-responsabilidade-solidaria-limites-normativos-a-equiparacao-automatica-no-cdc/
12. OpenClaw, *ANPD e IA: fiscalização de dados pessoais em 2026*: https://openclaw.ia.br/blog/anpd-fiscalizacao-ia-dados-pessoais-2026/
