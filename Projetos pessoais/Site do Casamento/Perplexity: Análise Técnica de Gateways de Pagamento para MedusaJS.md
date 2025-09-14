# Perplexity: Análise Técnica de Gateways de Pagamento para MedusaJS

## 1. Integração Técnica

### Módulos e Plugins Disponíveis

**Stripe (Oficial)**

- **Plugin**: `medusa-payment-stripe`[^1]
- **Status**: Plugin oficial mantido pela equipe MedusaJS
- **Instalação**: `npm install medusa-payment-stripe`
- **Compatibilidade**: MedusaJS v1.20+ e v2.0[^2]

**Mercado Pago (Comunidade)**

- **Plugins disponíveis**:
  - `@minskylab/medusa-payment-mercadopago`[^3]
  - `@nicogorga/medusa-payment-mercadopago`[^4]
- **Status**: Plugins de comunidade
- **Instalação**: `npm install @minskylab/medusa-payment-mercadopago`

**PayPal (Oficial)**

- **Plugin**: `medusa-payment-paypal`[^5][^6]
- **Status**: Plugin oficial
- **Instalação**: `npm install medusa-payment-paypal`

### Processo de Configuração

**Variáveis de Ambiente Obrigatórias**:

**Stripe**:[^1]

```bash
STRIPE_API_KEY=sk_...
STRIPE_WEBHOOK_SECRET=whsec_...
```

**Mercado Pago**:[^3]

```bash
MERCADOPAGO_ACCESS_TOKEN=APP_USR...
MERCADOPAGO_SUCCESS_BACKURL=<your_success_url>
MERCADOPAGO_WEBHOOK_URL=<your_medusa_server_url>
```

**PayPal**:[^5]

```bash
PAYPAL_SANDBOX=true
PAYPAL_CLIENT_ID=<CLIENT_ID>
PAYPAL_CLIENT_SECRET=<CLIENT_SECRET>
PAYPAL_AUTH_WEBHOOK_ID=<WEBHOOK_ID>
```

**Registro no medusa-config.ts**:[^7]

```typescript
module.exports = defineConfig({
  modules: [
    {
      resolve: "@medusajs/medusa/payment",
      options: {
        providers: [
          {
            resolve: "medusa-payment-stripe",
            id: "stripe",
            options: {
              apiKey: process.env.STRIPE_API_KEY,
              webhookSecret: process.env.STRIPE_WEBHOOK_SECRET,
            },
          },
        ],
      },
    },
  ],
});
```

### Configuração de Webhooks

**URLs de Webhook Padrão**:[^8]

- Stripe: `/hooks/payment/stripe_stripe`
- Mercado Pago: `/hooks/payment/mercadopago_mercadopago`
- PayPal: `/hooks/payment/paypal_paypal`

Os webhooks são **essenciais** para notificações de status de pagamento, especialmente para:

- Pagamentos processados assincronamente
- Pagamentos gerenciados no painel do provedor
- Recuperação de transações interrompidas[^8]

## 2. Estrutura de Custos e Promoções

### Tabela 1: Comparativo de Taxas e Parcelamento

### Detalhamento das Taxas por Gateway

**Mercado Pago**:[^9][^10]

- **PIX**: 0% (primeiros 30 dias ou até R\$ 5.000) → 0,49% após período promocional[^10]
- **Cartão de Crédito**:
  - Na hora: 4,98%
  - 14 dias: 4,49%
  - 30 dias: 3,98%[^11]
- **Débito**: 1,99%[^10]

**Stripe Brasil**:[^12][^13]

- **Cartão doméstico**: 3,99% + R\$ 0,39 por transação
- **PIX**: R\$ 0,50 por transação PIX paga[^12]
- **Cartão internacional**: Taxa adicional aplicada

**PayPal Brasil**:[^5]

- **Taxa geral**: ~5% por transação
- **Configuração**: Via painel PayPal

### Promoções de Parcelamento Ativas

**Mercado Pago**:[^14][^15][^16]

- **Parcelamento sem juros**: Configurável até 12x
- **Configuração**: Via painel Mercado Pago → Seu Negócio → Custos → Parcelamento
- **Modalidade**: "Parcelado vendedor" - lojista assume os juros
- **Limitação**: Algumas integrações limitam a 10x sem juros[^14]

**Outros Gateways**:

- **Stripe**: Depende do processador de cartão local
- **PayPal**: Configurável via dashboard PayPal
- **PagSeguro**: Até 12x configurável (sem plugin oficial para MedusaJS)

## 3. Análise Comparativa

### Tabela 2: Síntese Comparativa

| Gateway          | Plugin MedusaJS                           | Facilidade de Integração | Taxa Pix       | Taxa Crédito à Vista  | Exemplo Promoção Parcelamento |
|:---------------- |:----------------------------------------- |:------------------------ |:-------------- |:--------------------- |:----------------------------- |
| **Stripe**       | medusa-payment-stripe (oficial)[^1]       | **Alta**                 | R\$ 0,50[^12]  | 3,99% + R\$ 0,39[^12] | N/A diretamente               |
| **Mercado Pago** | @minskylab/medusa-payment-mercadopago[^3] | **Média**                | 0% → 0,49%[^9] | 4,98% → 3,98%[^11]    | **Até 12x sem juros**[^16]    |
| **PayPal**       | medusa-payment-paypal (oficial)[^5]       | **Alta**                 | N/A            | ~5%[^5]               | Configurável via painel       |
| **PagSeguro**    | N/A (sem plugin oficial)                  | **Baixa**                | Até 1,89%[^11] | 3,19% - 4,99%[^11]    | Até 12x configurável          |

### Links para Documentação Oficial

- **Stripe**: https://docs.medusajs.com/resources/commerce-modules/payment/payment-provider/stripe[^17]
- **Mercado Pago**: https://medusajs.com/integrations/@minskylabmedusa-payment-mercadopago/[^3]
- **PayPal**: https://docs.medusajs.com/v1/plugins/payment/paypal[^5]

### Recomendações Técnicas

**Para parcelamento sem juros no Brasil**: **Mercado Pago** oferece a melhor solução com configuração nativa até 12x sem juros, apesar de usar plugin de comunidade.[^16]

**Para integração mais robusta**: **Stripe** possui plugin oficial com melhor suporte e documentação, mas requer configuração externa para parcelamento.[^1][^17]

**Para facilidade de implementação**: **PayPal** oferece plugin oficial com configuração direta.[^5][^6]

***

**Data da consulta**: 30 de agosto de 2025
**Observação**: Taxas e condições podem variar conforme negociação individual com cada gateway de pagamento.
<span style="display:none">[^18][^19][^20][^21][^22][^23][^24][^25][^26][^27][^28][^29][^30][^31][^32][^33][^34][^35][^36][^37][^38][^39][^40][^41][^42][^43][^44][^45][^46][^47][^48][^49][^50][^51][^52][^53][^54][^55][^56][^57][^58][^59][^60][^61][^62][^63]</span>

<div style="text-align: center">⁂</div>

[^1]: https://github.com/SGFGOV/medusa-payment-plugins

[^2]: https://www.npmjs.com/package/medusa-payment-stripe

[^3]: https://medusajs.com/integrations/@minskylabmedusa-payment-mercadopago/

[^4]: https://tonie.hashnode.dev/implementing-a-custom-payment-gateway-integration-in-medusajs

[^5]: https://docs.medusajs.com/v1/upgrade-guides/plugins/stripe/6-0-7

[^6]: https://github.com/NicolasGorga/medusa-payment-mercadopago

[^7]: https://medusajs.com/payment-module/

[^8]: https://funkyton.com/add-stripe-payment-module-to-medusajs-2-0/

[^9]: https://www.linkedin.com/posts/nicolas-gorga-bb9976184_medusa-v2-recently-re-introduced-the-ability-activity-7299612124824190977-BhXn

[^10]: https://docs.medusajs.com/resources/references/payment/provider

[^11]: https://docs.medusajs.com/resources/commerce-modules/payment/payment-provider/stripe

[^12]: https://github.com/NicolasGorga/medusa-payment-mercadopago-storefront

[^13]: https://medusajs.com/integrations/

[^14]: https://docs.medusajs.com/resources/nextjs-starter/guides/customize-stripe

[^15]: https://docs.medusajs.com/v1/plugins/payment/paypal

[^16]: https://docs.medusajs.com/resources/storefront-development/checkout/payment

[^17]: https://docs.medusajs.com/resources/commerce-modules/payment/module-options

[^18]: https://www.npmjs.com/package/medusa-payment-paypal

[^19]: https://docs.medusajs.com/resources/references/js-sdk/store/payment

[^20]: https://www.npmjs.com/package/@sgftech%2Fmedusajs-payment-mollie

[^21]: https://github.com/amaster507/medusa-payment-paypal

[^22]: https://docs.medusajs.com/v1/user-guide/regions/providers

[^23]: https://npmjs.com/package/medusa-payment-manual

[^24]: https://medusajs.com/integrations/medusa-payment-paypal/

[^25]: https://github.com/a11rew/medusa-payment-paystack

[^26]: https://docs.medusajs.com/v1/modules/carts-and-checkout/payment

[^27]: https://www.onesafe.io/blog/does-stripe-work-in-brazil

[^28]: https://empreendedores.mercadopago.com.br/quanto-custa-receber-pagamentos-via-pix-e-codigo-qr

[^29]: https://www.demarest.com.br/en/banco-central-divulga-estatisticas-de-pagamentos-de-varejo-e-de-cartoes-no-brasil-referentes-a-2023/

[^30]: https://support.stripe.com/questions/taxes-on-stripe-fees-for-brazil-based-businesses

[^31]: https://maquininha.cc/mercado-pago-taxa-pix/

[^32]: https://stripe.com/br/resources/more/payments-in-brazil

[^33]: https://stripe.com/en-br/resources/more/payment-gateway-fees

[^34]: https://www.reclameaqui.com.br/mercado-pago/taxa-no-pix_FqEM1PUoJiSj2W_z/

[^35]: https://paymentscmi.com/insights/payments-ecommerce-trends-brazil-2024/

[^36]: https://stripe.com/en-br/pricing

[^37]: https://www.youtube.com/watch?v=BIOWMOpFo0o

[^38]: https://www.commercegate.com/process-payments-in-brazil-with-ease-a-practical-guide/

[^39]: https://www.feecalculator.io/stripe-fees-brazil

[^40]: https://stripe.com/connect/pricing

[^41]: https://support.stripe.com/questions/payouts-in-brazil-card-receivables

[^42]: https://help.yampi.com.br/pt-BR/articles/6505343-como-configurar-o-mercado-pago-para-receber-os-juros

[^43]: https://atendimento.nuvemshop.com.br/pt_BR/12332-mercado-pago/como-configurar-o-parcelamento-no-mercado-pago

[^44]: https://faq.pagbank.com.br/duvida/regulamento-campanha-cartao-de-credito-pagbank-2025/2137

[^45]: https://www.nuvemshop.com.br/blog/gateway-de-pagamento-mais-barato/

[^46]: https://www.youtube.com/watch?v=uRwESQQX_pE

[^47]: https://www.elo.com.br/cartoes/

[^48]: https://community.shopify.com/t/gateway-de-pagamento-brasil/334798

[^49]: https://help.yampi.com.br/pt-BR/articles/7170108-como-configurar-os-juros-e-parcelamento-no-mercado-pago

[^50]: https://website.cfo.org.br/atencao-prazo-final-para-pagamento-com-desconto-de-10-anuidade-2025/

[^51]: https://stripe.com/br/resources/more/installment-payments-101-a-guide-for-businesses

[^52]: https://meubolsoemdia.com.br/Materias/mutirao-da-negociacao

[^53]: https://basedeconhecimento.tray.com.br/hc/pt-br/articles/6734720000795-Configurações-gerais-das-formas-de-pagamento

[^54]: https://docs.medusajs.com/resources/commerce-modules/payment/webhook-events

[^55]: https://github.com/marcosgomesneto/medusa-payment-mercadopago

[^56]: https://docs.stripe.com/webhooks

[^57]: https://github.com/medusajs/medusa/issues/6254

[^58]: https://medusajs.com/integrations/medusa-payment-paystack/

[^59]: https://www.npmjs.com/package/mercadopago?activeTab=dependents

[^60]: https://www.youtube.com/watch?v=dcSOpIzc1Og

[^61]: https://ppl-ai-code-interpreter-files.s3.amazonaws.com/web/direct-files/e3c2d58fbf29175aa2024a576ef9d665/8e7381d7-1d90-40a7-bad5-04fb993f54a2/33a288d2.csv

[^62]: https://ppl-ai-code-interpreter-files.s3.amazonaws.com/web/direct-files/e3c2d58fbf29175aa2024a576ef9d665/8e7381d7-1d90-40a7-bad5-04fb993f54a2/57ecb6f4.csv

[^63]: https://ppl-ai-code-interpreter-files.s3.amazonaws.com/web/direct-files/e3c2d58fbf29175aa2024a576ef9d665/8e7381d7-1d90-40a7-bad5-04fb993f54a2/bc8d8133.csv
