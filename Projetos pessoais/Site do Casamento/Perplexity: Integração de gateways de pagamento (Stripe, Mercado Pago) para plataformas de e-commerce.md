# Perplexity: Integração de gateways de pagamento (Stripe, Mercado Pago) para plataformas de e-commerce

**Resposta direta:** Em MedusaJS, Stripe tem provedor oficial com configuração por variáveis de ambiente e webhooks obrigatórios; Mercado Pago depende de plugins de comunidade que suportam cartão e Pix, com webhooks e configuração via .env/medusa-config.ts, e em ambos o “parcelamento sem juros” é ofertado pelo lojista (sem subsídio público do gateway) até limites definidos no checkout, tipicamente até 12x no Brasil. Para taxas públicas, Stripe divulga Pix a 1,19% e estrutura de preços por acordo para cartão; Mercado Pago divulga Pix a 0,99% e crédito à vista a partir de 4,99% (recebimento imediato), sendo as parcelas sem juros uma configuração do lojista com custo embutido das tarifas de parcelamento.[^1][^2][^3][^4][^5][^6][^7][^8][^9][^10]

## Integração técnica

- Stripe (oficial): usar o Stripe Module Provider no Payment Module, registrar no medusa-config.ts com ID e configurar webhooks em {server}/hooks/payment/{provider_id}, ouvindo eventos como payment_intent.succeeded, amount_capturable_updated e payment_failed. Variáveis comuns: STRIPE_API_KEY e STRIPE_WEBHOOK_SECRET, adicionadas no backend e referenciadas nas options do plugin (medusa-payment-stripe), conforme guias de integração práticos para Medusa 2.0. Desde a v6.0.7, o processamento de webhooks usa o Event Bus (recomenda-se Redis Event Bus) e requer resposta imediata 200 com processamento assíncrono, o que impacta a configuração de produção.[^2][^11][^12][^1]
- Mercado Pago (comunidade): há plugins publicados (ex.: @minskylab/medusa-payment-mercadopago, @marcosgn/medusa-payment-mercadopago) com instalação via npm/yarn e registro no medusa-config.js; exigir variáveis como MERCADOPAGO_ACCESS_TOKEN, MERCADOPAGO_WEBHOOK_URL e, opcionalmente, MERCADOPAGO_WEBHOOK_SECRET, além de URLs de sucesso/retorno. Os plugins suportam cartão e Pix com webhooks para atualização de status, e a recomendação do Medusa é sempre habilitar webhooks para provedores de pagamento a fim de sincronizar mudanças de pagamento.[^3][^4][^5]

## Webhooks

- Stripe: configurar endpoint por provedor em {server_url}/hooks/payment/{provider_id} e assinar eventos de Payment Intent; obrigatório em produção para refletir captura, sucesso e falhas no backend do Medusa. A arquitetura recente desloca o processamento para o Event Bus, melhorando confiabilidade e retentativas, o que exige módulo de Event Bus no backend.[^1][^2]
- Mercado Pago: configurar a URL de webhook no painel e no plugin para receber notificações de pagamentos e ordens; os plugins citados expõem rotas de webhook dedicadas e validam o segredo quando configurado.[^5][^3]

## Custos e promoções (Parcelamento – Tabela 1)

- Stripe: Pix com taxa pública de 1,19% por transação e demais métodos locais em página de preços; cartões no Brasil seguem pricing “blended” ou “IC+” por acordo, sem promoção pública de subsídio de juros em parcelados no cartão. O ecossistema suporta parcelamento via cartão e também BNPL com parcelas sem juros oferecidas pelos provedores BNPL (ex.: Afterpay/Klarna), mas isso não equivale a subsídio de juros no cartão tradicional do gateway.[^13][^6][^7][^14][^10]
- Mercado Pago: Pix divulgado a 0,99% para recebimento na hora e crédito à vista de 4,99% para recebimento imediato (varia por prazo de recebimento), com possibilidade de configurar “parcelamento sem juros” no checkout definindo o número máximo de parcelas, sendo o custo absorvido pelo lojista.[^8][^9]

Tabela 1 — Promoções e condições de parcelamento sem juros (foco prático)

| Gateway      | Máx. parcelas sem juros                                                                    | Taxa aplicável (Pix / Crédito à vista)                                              | Requisitos/compatibilidade com MedusaJS                                                                                 | Limitações/condições                                                                                                                  |
|:------------ |:------------------------------------------------------------------------------------------ |:----------------------------------------------------------------------------------- |:----------------------------------------------------------------------------------------------------------------------- |:------------------------------------------------------------------------------------------------------------------------------------- |
| Stripe       | Até 12x no cartão, definido pelo lojista no Brasil (sem subsídio público do gateway) [^10] | Pix: 1,19% por transação; cartão: pricing por acordo (blended ou IC+) [^6][^7]      | Provedor oficial no Payment Module; plugin medusa-payment-stripe; webhooks obrigatórios; Event Bus em produção [^1][^2] | “Sem juros” é custo do lojista; BNPL pode oferecer parcelas sem juros via parceiros, mas não é cartão parcelado subsidiado [^14][^10] |
| Mercado Pago | Até 12x no checkout, configurado pelo lojista como “sem juros” [^9]                        | Pix: 0,99% (recebimento na hora); crédito à vista: 4,99% (recebimento na hora) [^8] | Plugins de comunidade (ex.: @minskylab, @marcosgn) com suporte a cartão e Pix; webhooks exigidos [^3][^5]               | “Sem juros” é definido pelo lojista e embute custo das tarifas de parcelamento; dependência de plugins não-oficiais [^9][^5]          |

## Comparativos

Tabela 2 — Síntese técnica para parcelamento sem juros

| Gateway      | Plugin MedusaJS                                                                      | Facilidade de integração                                  | Taxa Pix                          | Taxa crédito à vista              | Exemplo de promoção de parcelamento                                                                            | Documentação                                                           | Atualizado em   |
|:------------ |:------------------------------------------------------------------------------------ |:--------------------------------------------------------- |:--------------------------------- |:--------------------------------- |:-------------------------------------------------------------------------------------------------------------- |:---------------------------------------------------------------------- |:--------------- |
| Stripe       | medusa-payment-stripe (provedor oficial) [^1][^2][^15]                               | Alta (docs oficiais, módulos v2, Event Bus) [^1][^2]      | 1,19% por Pix [^6]                | Por acordo (blended ou IC+) [^7]  | Sem subsídio do gateway; lojista oferece até 12x sem juros; BNPL pode ser “sem juros” via parceiros [^14][^10] | Docs do provedor Stripe no Medusa [^1]                                 | 31/08/2025 [^1] |
| Mercado Pago | @minskylab/medusa-payment-mercadopago; @marcosgn/medusa-payment-mercadopago [^3][^5] | Média (plugins de comunidade, setup de webhooks) [^3][^5] | 0,99% (recebimento imediato) [^8] | 4,99% (recebimento imediato) [^8] | Lojista configura “até 12x sem juros” no checkout; custo absorvido pelo lojista [^9]                           | Página de integração Mercado Pago no Medusa; README do plugin [^3][^5] | 31/08/2025 [^3] |

### Observações de configuração

- Stripe: definir STRIPE_API_KEY e STRIPE_WEBHOOK_SECRET no backend; registrar o provedor no medusa-config e criar o endpoint de webhook no Stripe apontando para /hooks/payment/{provider_id}, assinando eventos de Payment Intent. Em produção, habilitar Event Bus (Redis) para processar webhooks com retentativas e evitar race conditions.[^11][^2][^1]
- Mercado Pago: definir MERCADOPAGO_ACCESS_TOKEN e URL de webhook nas variáveis de ambiente; registrar o plugin no medusa-config.js; garantir a rota de webhook e validação de segredo quando aplicável.[^3][^5]

### Conclusão prática

- Para “parcelamento sem juros”, a responsabilidade financeira é do lojista em ambos os gateways; não foram identificadas promoções públicas vigentes de “sem juros” subsidiadas diretamente pelo gateway nos cartões, embora Stripe suporte BNPL com parcelas sem juros via parceiros e Mercado Pago permita ativar “sem juros” no checkout com custo absorvido pelo lojista. Pix apresenta custo mais baixo em ambos, com Stripe a 1,19% e Mercado Pago a 0,99% segundo páginas públicas, o que pode ser relevante para estratégias de incentivo de pagamento à vista.[^6][^14][^9][^10][^8]
  <span style="display:none">[^16][^17][^18][^19][^20][^21][^22][^23][^24][^25][^26][^27][^28][^29][^30][^31]</span>

<div style="text-align: center">⁂</div>

[^1]: https://docs.medusajs.com/resources/commerce-modules/payment/payment-provider/stripe

[^2]: https://docs.medusajs.com/v1/upgrade-guides/plugins/stripe/6-0-7

[^3]: https://medusajs.com/integrations/@minskylabmedusa-payment-mercadopago/

[^4]: https://docs.medusajs.com/resources/commerce-modules/payment/webhook-events

[^5]: https://github.com/marcosgomesneto/medusa-payment-mercadopago

[^6]: https://stripe.com/en-br/pricing/local-payment-methods

[^7]: https://stripe.com/br/legal/pricing-policy

[^8]: https://www.nuvemshop.com.br/blog/como-cobrar-online-com-o-mercadopago/

[^9]: https://empresas.mercadopago.com.br/como-oferecer-parcelamentos-sem-juros-no-mercado-pago

[^10]: https://stripe.com/br/resources/more/payments-in-brazil

[^11]: https://blog.nomodo.io/how-to-setup-stripe-plugin-in-medusajs/

[^12]: https://funkyton.com/add-stripe-payment-module-to-medusajs-2-0/

[^13]: https://stripe.com/en-br/pricing

[^14]: https://stripe.com/resources/more/installment-payments-101-a-guide-for-businesses

[^15]: https://medusajs.com/integrations/@medusajspayment-stripe/

[^16]: https://docs.medusajs.com/v1/development/plugins/overview

[^17]: https://www.youtube.com/watch?v=dcSOpIzc1Og

[^18]: https://github.com/NicolasGorga/medusa-payment-mercadopago

[^19]: https://stripe.com/en-br/resources/more/pix-replacing-cards-cash-brazil

[^20]: https://stripe.com/br/payment-method/pix

[^21]: https://finovate.com/stripe-taps-ebanx-to-facilitate-pix-payments-in-brazil/

[^22]: https://www.youtube.com/watch?v=nXGMZm-pJSA

[^23]: https://www.prnewswire.com/news-releases/stripe-users-can-now-accept-pix-in-brazil-via-ebanx-302526007.html

[^24]: https://stripe.com/payments

[^25]: https://www.youtube.com/watch?v=BIOWMOpFo0o

[^26]: https://www.ecommercebrasil.com.br/artigos/magazine-luiza-e-mercado-livre-reajustam-tarifas-no-marketplace-para-2025-em-ate-20

[^27]: https://www.youtube.com/watch?v=ceApe6N0jFY

[^28]: https://eventos.ecommercebrasil.com.br/forum/suporte-ao-participante

[^29]: https://www.youtube.com/watch?v=uRwESQQX_pE

[^30]: https://www.youtube.com/watch?v=ZbGcdLblXQU

[^31]: https://www.zintego.com/blog/top-brazil-payment-gateways-for-international-business-best-options-for-2025/
