# O Mandato testado no gbrain

**Análise final do teste da ideia no gbrain, de Garry Tan**

| | |
|---|---|
| Status | Relatório do teste, 5 de setembro de 2026 |
| O que é o gbrain | Camada de memória e síntese para agentes de IA, construída por Garry Tan (Y Combinator) para rodar os próprios agentes. Ingere páginas em markdown, extrai grafo de entidades e linha do tempo, e responde perguntas com síntese citada, detecção de contradições e análise de lacunas (o que o brain não sabe). Repositório: https://github.com/garrytan/gbrain |
| Como foi rodado | gbrain 0.48.2.0 instalado por `git clone` e `bun link`; brain local em PGLite; sem chave de embedding (busca por palavra-chave); síntese pelo provedor `claude-cli`, que usa o `claude` desta sessão como subprocesso, com Claude Opus 5 na camada de síntese; modo de busca `tokenmax` a partir da quarta pergunta. Brain com 25 páginas: proposta do Mandato, pitch, análise de viabilidade do hub, arquitetura do Commerce Control Plane, 14 páginas de empresas e 7 de conceitos, todas com fatos citados, 29 links e 15 entradas de linha do tempo. |
| O que foi perguntado | Sete perguntas de sócio de YC em office hours (hipótese mais frágil; por que o varejista paga; contradições; lacunas de investidor; defesa contra plataformas; experimento de 60 dias; matar ou seguir) e um brainstorm por bisociação. As respostas brutas estão em [`gbrain/`](gbrain/). |

## Veredito do gbrain, em uma frase

> "O brain sustenta um Mandato como camada de execução, exceção e governança sobre trilhos de terceiros no Brasil, e não sustenta um Mandato como comparador ou hub de afiliados; a única variável que decide a coisa é se o varejista paga por pedido entregue e traz o cliente, e sobre isso o brain hoje não tem nenhuma evidência, só desenho." ([q8](gbrain/q8.md))

Concordo com o veredito. O que segue é o que ele derrubou, o que confirmou, as lacunas por prioridade, o experimento que ele desenhou e o que muda no plano.

---

## 1. O que o gbrain derrubou ou enfraqueceu

| # | Achado | Evidência que o gbrain usou | Onde |
|---|---|---|---|
| 1 | **A hipótese mais frágil não é a demanda; é o alinhamento entre quem paga e o que o produto entrega.** O varejista paga por pedido e mede share of wallet; o produto promete comparar entre parceiros e substituir marca, exatamente as duas coisas que o pagador tem incentivo de desligar. | Programe e Poupe da Amazon resolveu a tensão eliminando a comparação; Mercado Livre constrói o próprio agente; plataformas podem adotar UCP | [q1](gbrain/q1.md) |
| 2 | **A comissão de 3% a 8% é âncora, não preço.** A comparação com os 26,5% do iFood não é like-for-like: o plano com entrega inclui logística; a comparação honesta é com os 12% do plano básico, e aí a vantagem cai pela metade. E se Amazon já paga 1% a 15% e Casas Bahia até 9% na Awin, o varejista pode perguntar por que assinar contrato direto em vez de deixar o Mandato operar como afiliado comum. | Páginas de iFood, Amazon, Awin, Mercado Livre, OpenAI (4%) | [q2](gbrain/q2.md) |
| 3 | **"Compra sem toque desde a fase 2" é otimista.** Pix Automático exige recebedor único e recorrência periódica; Agent Pay exige a interseção de emissor do usuário e adquirente do varejista, que ninguém enumerou; Visa só pilota no fim de 2026; JSR exige parceiro ITP. O produto honesto é "um toque", com sem-toque para um subconjunto. | FAQ do Banco Central, páginas de Mastercard, Visa, Agent Pay | [q4](gbrain/q4.md), [q7](gbrain/q7.md) |
| 4 | **Há três contradições internas nos documentos.** O afiliado é declarado inviável e mantido como modo pesquisa; Amazon e Mercado Livre são fallback e concorrentes ao mesmo tempo; a viabilidade diz "um toque" e a proposta diz "sem toque na fase 2". | Cotejo entre as páginas do projeto, do pitch e da viabilidade | [q3](gbrain/q3.md), [q8](gbrain/q8.md) |
| 5 | **A proteção mais fraca contra as plataformas é o WhatsApp.** A proteção e a dependência são a mesma coisa: o Mandato é cliente da Cloud API da Meta, e a Meta já vende IA agêntica por token e marketplace nativo com Pix na mesma superfície. A segunda mais fraca é o mandato como primitiva, que o Google levou à FIDO e cuja referência de código é Apache 2.0. As mais sólidas são o catálogo local de reposição e a posição do lado do varejista. | Páginas de Meta, Google, AP2, Anthropic, Visa | [q5](gbrain/q5.md) |
| 6 | **Não há evidência própria de nada.** Nenhum varejista, LOI, teste com usuário, custo por tarefa medido, taxa de confirmação ou retenção. Tudo é fato de terceiro e desenho. | Análise de lacunas em todas as respostas | [q4](gbrain/q4.md) |

Duas observações do gbrain que eu não tinha feito com essa clareza: a Anthropic e a OpenAI são parceiras do piloto Visa de pagamento agêntico no Brasil, o que as coloca mais perto da transação do que a leitura "canal e fornecedor" sugere ([q5](gbrain/q5.md)); e "o varejista traz o cliente" pede ao parceiro para remar contra o próprio incentivo, porque ele está sendo convidado a aceitar comparação com concorrentes ([q7](gbrain/q7.md)).

## 2. O que o gbrain confirmou

- **A lição do Buscapé está incorporada.** O comparador puro perdeu para o marketplace e para o Google; a versão agêntica precisa entregar decisão e ação, não lista. O desenho do Mandato faz isso ([q8](gbrain/q8.md)).
- **Os incumbentes param onde a dor brasileira começa.** Google chega por plataforma e por último, sem WhatsApp, sem Pix nativo e sem operação de exceção; a Anthropic se posiciona como camada de inteligência; ACP e UCP não têm conceito de afiliado e deixam o lojista como merchant of record ([q8](gbrain/q8.md)).
- **Os trilhos destravaram em 2026.** Pix Automático cresceu 182% num trimestre, JSR obrigatória desde fevereiro, Agent Pay ativo com Itaú e Santander ([q8](gbrain/q8.md)).
- **O ativo defensável é o kernel de confiança**, não o modelo de linguagem, e a padronização vira porta de saída na fase 3 ([q8](gbrain/q8.md)).
- **O brainstorm apontou a mesma cunha que o desenho já tem**: rev share por ciclo de reposição completado via Pix recorrente, atacando a lacuna dupla de Amazon (só cartão) e Google (sem Pix). Foi a ideia com maior nota entre 72 geradas ([brainstorm](gbrain/brainstorm.md)).

## 3. Lacunas por prioridade, com o experimento barato que fecha cada uma

Consolidado da resposta [q4](gbrain/q4.md).

| Prioridade | O que o brain não sabe | Experimento barato |
|---|---|---|
| P0 | Se o usuário delega compra recorrente. Só há intenção declarada (Visa: 76% e 67%), nenhuma taxa de confirmação, repetição ou retenção observada | Concierge manual (Wizard of Oz) com 30 a 50 casas no WhatsApp por 6 a 8 semanas, sem kernel e sem integração; medir confirmação em 24h, pedidos por casa por mês e retenção na semana 8 |
| P1 | Nenhum varejista parceiro, LOI ou tempo real de integração; o próprio pitch marca "varejista não integra" como risco alto | 10 conversas comerciais com supermercados e farmácias regionais buscando 2 LOIs pagos; um piloto de nível 2 cronometrado de ponta a ponta |
| P2 | Disposição a pagar 3% a 8%; iFood e Méliuz não provam recorrência intermediada | Teste de preço em três faixas nas mesmas 10 conversas, registrando o ponto de recusa por segmento |
| P3 | Se a cesta variável cabe no Pix Automático; se alugar ITP funciona | Parecer jurídico curto sobre o FAQ do Banco Central; 20 transações reais com um PSP ou ITP já autorizado |
| P4 | Custo por conversa no WhatsApp cobrado por token; política da Meta sobre agente de compra de terceiro | 1.000 conversas no piloto do P0 com custo medido; consulta escrita a um BSP |
| P5 | Cobertura real do fallback de afiliado, com Mercado Livre sem API, Amazon com regra de agentes, Shopee com aprovação manual | Submeter os quatro cadastros hoje; medir cobertura de uma cesta-teste de 50 SKUs contra Awin e Shopee |
| P6 | CAC, LTV e margem de contribuição; "o varejista traz o cliente" sem dado de ativação | Um parceiro dispara para a própria base; medir ativação, custo por casa ativada e margem nas primeiras 200 cestas |
| P7 | Qualidade do agente sem linha de base | Rodar 200 casos do harness de evals já desenhado e publicar pass rate e custo por tarefa |
| P8 | O que é proprietário e o que é Apache 2.0 herdado | Mapa de uma página: código aberto, integração com parceiro, ledger e mandato próprios |
| P9 | O que o varejista faz quando o mesmo serviço vier de graça pela plataforma | Perguntar literalmente nas 10 entrevistas do P1 |

## 4. O experimento de 60 dias que o gbrain desenhou

Da resposta [q7](gbrain/q7.md). Difere do plano de fases do pitch em três pontos: dois parceiros em vez de três a cinco, cerca de 80 casas em vez de 200, e nenhuma linha de código de pagamento.

**Construir.** O loop mínimo do kernel até a autorização humana, com ledger, proveniência, fencing e custo por tarefa. Mandate Engine em modo de política, com aprovação em 100% dos pedidos. Dois conectores de parceiro em leitura com handoff de checkout. Cotação por afiliado via Awin e Shopee só para o que o parceiro não tem, com o cadastro da Shopee pedido no dia 1. Evals de produção. WhatsApp com app e web como fallback.

**Não construir.** Compra sem toque. Licença de ITP. Busca no Mercado Livre. Extensão de navegador. Exposição a UCP, ACP ou AP2. Aquisição paga. Segunda cidade. Tela de comparação em lista.

**Métricas.** Confirmação da proposta em 24 horas; segunda cesta por casa; cobertura do parceiro sem cair no afiliado; comissão realizada; atribuição confirmada server-to-server; compensações por 100 pedidos; custo por tarefa incluindo o token do WhatsApp; substituições aceitas contra rejeitadas.

**Critérios de morte no dia 60.** Qualquer um mata: confirmação abaixo de 40%; segunda cesta abaixo de 50% das casas; cobertura do parceiro abaixo de 70% dos itens; comissão realizada abaixo de 4%; mais de 2 compensações por 100 pedidos; zero varejista disposto a renovar com comparação e disclosure no contrato. Os limiares são propostos, não medidos.

## 5. Onde o gbrain errou ou não viu

O teste vale mais quando se diz o que a ferramenta não conseguiu.

- **Recuperação truncada.** Sem chave de embedding, a busca é por palavra-chave e os documentos longos chegam cortados ao modelo. Em [q3](gbrain/q3.md) a proposta do Mandato não foi recuperada e a viabilidade veio cortada no veredito; em [q5](gbrain/q5.md) a tabela da seção 13.1 do pitch veio truncada e as cinco proteções foram reconstruídas. Subir para o modo `tokenmax` melhorou, mas não resolveu.
- **Lacunas que não são lacunas.** Algumas "coisas que o brain não sabe" estão nos documentos e não foram recuperadas: a economia unitária por casa está na seção 7.3 do pitch e na seção 5.2 da proposta; TAM, SAM e SOM estão na seção 6 do pitch; o pedido ao investidor está na seção 14; os gates numéricos estão na seção 10 da proposta; o Pix Automático tem página própria e foi dado como ausente em [q1](gbrain/q1.md). Isso enfraquece parte da lista P0 a P9, mas não a P0, a P1, a P2 nem a P6, que são lacunas reais.
- **Grafo raso.** As páginas de empresa apontam para o Mandato, mas os links internos dos documentos longos não viraram arestas, então a travessia de grafo só contribuiu com âncora explícita.
- **Uma chamada falhou.** Na primeira tentativa da pergunta 2, o modelo tentou executar um comando em vez de responder e a saída veio vazia; foi refeita com a instrução explícita de não usar ferramentas.
- **Sem "takes".** O gbrain distingue fato de opinião com dono e confiança, e o brain não tinha nenhum take. Todas as respostas registram isso como lacuna: ninguém no brain assina a tese central nem diz com que convicção.

## 6. O que muda no plano depois do teste

1. **A promessa de pagamento muda de "sem toque desde a fase 2" para "um toque é o produto".** Sem toque vira fase 3, restrito a mandato de loja única via Pix Automático e a pares emissor e adquirente habilitados no Agent Pay, enumerados antes de prometer.
2. **A contradição do afiliado se resolve por escrito.** O modo pesquisa é cotação e fallback de link, sem promessa de receita; Amazon e Mercado Livre saem do texto como "fallback" e entram como "concorrentes com link de afiliado como mínimo".
3. **O preço deixa de ser afirmação.** A faixa de 3% a 8% vira hipótese a testar contra três contrafactuais: os 12% do plano básico do iFood, o CAC próprio do varejista e a margem líquida por categoria.
4. **A fase 0 vira um Wizard of Oz antes de qualquer integração.** Seis a oito semanas, 30 a 80 casas, dois parceiros, concierge humano com o agente por trás. As três primeiras lacunas se fecham por menos do que custa uma sprint de conector.
5. **A tensão com o varejista entra no contrato do piloto**, não fica como nota de risco: loja preferida como padrão, comparação só entre parceiros, comissão menor para quem trouxe a casa, e a pergunta "o que você faz quando a plataforma oferecer isso de graça" feita na primeira reunião.
6. **A Meta ganha um plano próprio**: consulta escrita a um BSP sobre política de agente de terceiro, custo por token medido, e app e web mantidos como canal real, não fallback teórico.
7. **O brain ganha takes.** Cada hipótese central recebe dono, data e confiança, para que o próximo teste distinga o que foi medido do que foi desenhado.

## Como reproduzir

```bash
git clone https://github.com/garrytan/gbrain.git ~/gbrain && cd ~/gbrain && bun install && bun link
gbrain init --pglite --non-interactive --no-embedding
gbrain config set models.tier.deep claude-cli:claude-opus-5        # síntese via o claude local
gbrain config set models.tier.subagent claude-cli:claude-sonnet-5
gbrain config set models.tier.utility claude-cli:claude-haiku-4-5-20251001
gbrain config set search.mode tokenmax
gbrain import ~/brain --no-embed && gbrain extract links --source db && gbrain extract timeline --source db
gbrain think "Avalie o Mandato como um sócio do Y Combinator avaliaria..." --anchor projects/mandato
gbrain brainstorm "Que cunha de entrada ... ninguém tentou ainda?" --no-save --yes --limit 6
```

O diretório `~/brain` usado no teste tem as páginas de projeto, análise, empresas e conceitos descritas no cabeçalho; com uma chave de embedding (Voyage ou OpenAI), a recuperação deixa de truncar e as lacunas falsas da seção 5 desaparecem.
