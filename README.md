# ecommerce-agent

Produto de comércio agêntico construído sobre o repositório de referência
[`anthropics/commerce-agents`](https://github.com/anthropics/commerce-agents) e o artigo
[*The anatomy of effective commerce agents*](https://claude.com/blog/the-anatomy-of-effective-commerce-agents).

## Documentos

- [`docs/arquitetura-produto.md`](docs/arquitetura-produto.md): arquitetura de produto e oferta única
  (Trust & Transaction Kernel, Commerce Operator, Shopping Concierge, Gateway, Assurance), com o mapa
  do que vem do repositório de referência, o que é estendido e o que é novo, o empacotamento comercial
  e a sequência de construção.
- [`docs/viabilidade-hub-agentico-afiliados.md`](docs/viabilidade-hub-agentico-afiliados.md): análise de
  viabilidade do produto do lado da demanda, um hub agêntico de compras sobre os marketplaces brasileiros
  com Mandate Engine e receita por afiliação, distribuído em WhatsApp, Claude e ChatGPT. Cobre o que o
  repositório de referência entrega, acesso a catálogo e afiliação por marketplace, modos de concluir a
  compra, trilhos de pagamento no Brasil, concorrência, economia unitária, regulação e sequência.
- [`docs/proposta-produto-mandato.md`](docs/proposta-produto-mandato.md): proposta de produto
  comercialmente viável derivada da análise, o Mandato: agente de reposição da casa sob mandato do
  consumidor, pago pelo varejista parceiro por pedido entregue, com pagamento por Pix Automático e
  tokens de agente, WhatsApp como canal principal e o mesmo kernel do Commerce Control Plane.

## Tese em uma frase

Integre seu comércio uma vez e deixe três tipos de agente agirem sobre ele, o do seu time, o do seu
cliente e os do mercado, sob uma política, um ledger e uma suíte de evals.
