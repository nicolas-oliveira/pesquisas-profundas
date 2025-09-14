| Gateway                                | Plugin_MedusaJS                        | Facilidade_Integracao | Taxa_PIX                          | Taxa_Credito_Vista                | Parcelamento_Sem_Juros                                                  | Link_Documentacao                                                                                                                     |
| -------------------------------------- | -------------------------------------- | --------------------- | --------------------------------- | --------------------------------- | ----------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| Stripe                                 | medusa-payment-stripe (oficial)        | Alta                  | R$ 0,50                           | 3,99% + R$ 0,39                   | N/A diretamente                                                         | [Stripe Module Provider - Medusa Documentation](https://docs.medusajs.com/resources/commerce-modules/payment/payment-provider/stripe) |
| Mercado Pago                           | "@minskylab/medusa-payment-mercadopago |                       |                                   |                                   |                                                                         |                                                                                                                                       |
| @nicogorga/medusa-payment-mercadopago" | Média                                  | 0% (30 dias) → 0,49%  | 4,98% (na hora) → 3,98% (30 dias) | Até 12x (configurável)            | https://medusajs.com/integrations/@minskylabmedusa-payment-mercadopago/ |                                                                                                                                       |
| PayPal                                 | medusa-payment-paypal (oficial)        | Alta                  | N/A                               | ~5%                               | Configurável                                                            | https://docs.medusajs.com/v1/plugins/payment/paypal                                                                                   |
| PagSeguro/Pagar.me                     | N/A (não encontrado)                   | Baixa                 | Até 1,89%                         | 4,99% (na hora) → 3,19% (30 dias) | Configurável                                                            | N/A                                                                                                                                   |

---

| Gateway      | Max_Parcelas_Sem_Juros | Taxa_Processamento | Requisitos_MedusaJS | Limitacoes_Especiais                |
| ------------ | ---------------------- | ------------------ | ------------------- | ----------------------------------- |
| Mercado Pago | 12x                    | 3,98% - 4,98%      | Plugin terceiros    | Taxa 0,49% após período promocional |
| Stripe       | N/A (via processador)  | 3,99% + R$ 0,39    | Plugin oficial      | Depende do processador local        |
| PayPal       | 12x                    | ~5%                | Plugin oficial      | Configuração via painel PayPal      |
| PagSeguro    | 12x                    | 3,19% - 4,99%      | Sem plugin oficial  | Integração manual necessária        |

---

| Gateway      | Variaveis_Ambiente                                                   | Webhook_URL                              | Plugin_Package                        |
| ------------ | -------------------------------------------------------------------- | ---------------------------------------- | ------------------------------------- |
| Stripe       | `STRIPE_API_KEY`, `STRIPE_WEBHOOK_SECRET`                            | `/hooks/payment/stripe_stripe`           | medusa-payment-stripe                 |
| Mercado Pago | `MERCADOPAGO_ACCESS_TOKEN`, `MERCADOPAGO_WEBHOOK_URL`                | `/hooks/payment/mercadopago_mercadopago` | @minskylab/medusa-payment-mercadopago |
| PayPal       | `PAYPAL_CLIENT_ID`, `PAYPAL_CLIENT_SECRET`, `PAYPAL_AUTH_WEBHOOK_ID` | `/hooks/payment/paypal_paypal`           | medusa-payment-paypal                 |
