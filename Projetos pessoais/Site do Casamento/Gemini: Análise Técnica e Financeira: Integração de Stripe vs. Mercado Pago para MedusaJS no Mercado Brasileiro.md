# Gemini: Análise Técnica e Financeira: Integração de Stripe vs. Mercado Pago para MedusaJS no Mercado Brasileiro

**Data da Consulta:** Agosto de 2024

## **Introdução: Resumo Executivo**

Este relatório apresenta uma comparação técnica e financeira abrangente entre Stripe e Mercado Pago como soluções de gateway de pagamento para plataformas de e-commerce baseadas em MedusaJS que operam no Brasil. A análise está focada em três pilares críticos: Integração Técnica e Experiência do Desenvolvedor, Estrutura Financeira e Custo, e Adequação Estratégica ao Mercado, com uma análise aprofundada específica sobre a funcionalidade de "parcelado sem juros", um pilar fundamental do comércio eletrônico brasileiro. O objetivo é equipar líderes técnicos e de negócios com os dados necessários para tomar uma decisão informada e estratégica que equilibre estabilidade técnica, custo operacional e potencial de conversão de clientes.

## **Seção 1: Integração Técnica e Experiência do Desenvolvedor**

Esta seção avalia os aspectos práticos da integração de cada gateway no ecossistema MedusaJS, focando na qualidade dos plugins disponíveis, na clareza do processo de configuração e na robustez de funcionalidades essenciais como os webhooks.

### **1.1. Stripe: Integração Oficial, Mantida e Estável**

O caminho principal de integração para o Stripe é através do plugin oficial do MedusaJS, **@medusajs/payment-stripe**.1 Este plugin é mantido diretamente pela equipe principal do MedusaJS, o que assegura compatibilidade, atualizações regulares e adesão às melhores práticas. O seu status como plugin oficial é uma vantagem significativa, implicando um alto grau de confiabilidade e suporte.3  
A existência de um plugin oficial não é apenas uma conveniência; sinaliza uma parceria estratégica ou, no mínimo, um status de cidadão de primeira classe para o Stripe dentro do ecossistema Medusa. Isso implica que, à medida que o MedusaJS evolui (por exemplo, com novas versões ou mudanças arquitetônicas como a transição para módulos), o plugin do Stripe será atualizado em conjunto. Esta sincronia reduz drasticamente o risco de a integração quebrar após uma atualização da plataforma principal. Em contraste, o destino de um plugin comunitário está ligado à disponibilidade e ao interesse do seu mantenedor. Portanto, a escolha do plugin oficial do Stripe é uma decisão que diminui a dívida técnica a longo prazo e garante um ciclo de manutenção mais previsível, um fator crucial no cálculo do Custo Total de Propriedade (TCO) da integração de pagamento.  
**Processo de Configuração Passo a Passo**  
A configuração é bem documentada e padronizada dentro da arquitetura MedusaJS.

* **Instalação:** Um único comando instala o pacote: `npm i @medusajs/payment-stripe`.1  
* **Variáveis de Ambiente:** A configuração é gerenciada através de arquivos .env, requerendo a `STRIPE_API_EY`e, para produção, a ``STRIPE_BHOOK_RET``.2  
* **Registro no medusa-config.ts:** O plugin é registrado dentro do array modules, especificamente sob o módulo payment. A configuração é limpa e aponta diretamente para o pacote instalado, permitindo que opções como apiKey, webhookSecret e o modo capture sejam definidas.2

```ts
// medusa-config.ts  
module.exports \= defineConfig({  
 //...  
 modules:,  
 },  
 },  
 \],  
})
```

**Implementação de Webhooks**  
Webhooks são críticos para atualizar de forma assíncrona os status de pagamento (por exemplo, pagamento bem-sucedido, falha).

* **URL do Endpoint:** O Medusa fornece uma estrutura de URL de webhook padronizada: `{server_url}/hooks/payment/{provider_d}`. Para uma configuração padrão do Stripe, isso se traduz em `{server_l}/hooks/payment/stripe_ipe`.2  
* **Eventos Necessários:** A documentação oficial especifica os eventos de webhook exatos a serem escutados no painel do Stripe: payment_intent.amount_apturable_dated, payment_ent.succeeded, payment_nt.payment__d e payment\__.partially\___2 Essa especificidade remove a ambiguidade durante a configuração.  
* **Melhoria Arquitetural:** Versões recentes do plugin (por exemplo, v6.0.7) migraram o processamento de webhooks para usar o Event Bus Service do Medusa. Esta é uma prática recomendada, pois permite que o servidor responda imediatamente ao Stripe com um 200 OK e processe o evento em um trabalho em segundo plano, melhorando a confiabilidade e permitindo novas tentativas em caso de falha.7

**Avaliação da Experiência do Desenvolvedor**  
A experiência do desenvolvedor é classificada como **Alta**. O processo é simplificado, bem documentado e apoiado pela equipe oficial do MedusaJS. O uso de padrões arquitetônicos modernos (Event Bus) e instruções de configuração claras minimiza o atrito na integração e o risco de manutenção a longo prazo.

### **1.2. Mercado Pago: Navegando pelo Cenário de Plugins Comunitários**

Ao contrário do Stripe, não existe um plugin oficial do MedusaJS para o Mercado Pago. A integração depende de pacotes desenvolvidos pela comunidade, o que revela um cenário fragmentado.  
A existência de múltiplos plugins comunitários para o mesmo gateway é uma faca de dois gumes. Por um lado, demonstra um vibrante interesse da comunidade. Por outro, indica a falta de uma solução única e definitiva. Essa fragmentação força a equipe de desenvolvimento a gastar tempo valioso avaliando e comparando esses plugins, analisando sua saúde (último commit, issues abertas) e escolhendo a opção de "menor risco". Este processo de avaliação em si é um custo oculto. Além disso, se o plugin escolhido for abandonado, a equipe enfrenta uma escolha difícil: fazer um fork e mantê-lo por conta própria (um dreno significativo de recursos) ou migrar para outro plugin comunitário (incorrendo em custos de retrabalho).  
**Análise dos Plugins Disponíveis**

* **@minskylab/medusa-payment-mercadopago** 8: Parece ser uma escolha popular, com um bom número de estrelas e forks. Utiliza o Checkout Pro do Mercado Pago (baseado em redirecionamento) e fornece instruções claras para a configuração e implementação no lado do cliente usando um  
  preferenceId.  
* **@nicogorga/medusa-payment-mercadopago** 10: Uma opção mais recente e desenvolvida ativamente que utiliza a Checkout API. Sua documentação é muito detalhada, cobrindo pré-requisitos como o  
  ngrok para testes locais de webhooks. No entanto, afirma explicitamente que é um "WIP" (Trabalho em Progresso) e foi testado apenas para o Uruguai, o que apresenta um risco significativo para uma implementação focada no Brasil.  
* **@marcosgn/medusa-payment-mercadopago** 12: Este plugin é notável por mencionar explicitamente o suporte tanto para Cartão de Crédito quanto para  
  **Pix** através do Checkout Transparente, tornando-o altamente relevante. Contudo, sua documentação também observa uma atualização pendente para o Medusa 2.0, indicando potenciais problemas de compatibilidade.

**Configuração com um Plugin Representativo**  
Assumindo a escolha do @marcosgn/medusa-payment-mercadopago devido ao seu suporte explícito ao Pix:

* **Instalação:** `npm install @marcosgn/medusa-payment-mercadopago`.12  
* **Variáveis de Ambiente:** Requer `MERCADOPAGO_ACCESS_OKEN`, `MERCADOPAGO_BHOOK_`e, opcionalmente, `MERCADOPAGO_OOK\_T`.12  
* **Registro no medusa-config.js:** O plugin é adicionado ao array plugins no formato de configuração legado do Medusa (note a diferença da configuração baseada em módulos do Stripe).12

JavaScript

// medusa-config.js (formato legado)  
const plugins \=

**Implementação de Webhooks**  
A configuração é mais manual e potencialmente frágil. O desenvolvedor deve definir a URL completa do webhook nas variáveis de ambiente e configurá-la no painel do Mercado Pago. O desenvolvimento local exige um serviço de tunelamento como o ngrok para expor o servidor local aos eventos de webhook do Mercado Pago, adicionando uma camada extra de complexidade ao fluxo de trabalho de desenvolvimento.10  
**Avaliação da Experiência do Desenvolvedor**  
A experiência do desenvolvedor é classificada como **Baixa a Média**. A dependência de plugins comunitários introduz incerteza em relação à manutenção, suporte e compatibilidade com futuras versões do MedusaJS. A documentação, embora por vezes detalhada, pode ser inconsistente entre os diferentes plugins, e o processo de configuração (especialmente para webhooks) é menos simplificado do que o do plugin oficial do Stripe.

## **Seção 2: Análise Financeira: Taxas e Promoções de Parcelamento**

Esta seção disseca os custos diretos associados a cada gateway e analisa sua abordagem à funcionalidade crítica de "parcelado sem juros", que é um grande impulsionador de conversão no mercado brasileiro.

### **2.1. Estruturas de Taxas de Transação Padrão (Brasil)**

* **Stripe:**  
  * **Pix:** **1.19%** por transação bem-sucedida.13  
  * **Cartão de Crédito (Doméstico):** **3.99% \+ R`0.39** por transação bem-sucedida.13  
  * **Cartão de Crédito (Internacional):** Aplica-se uma taxa adicional de **2%**.14  
* **Mercado Pago:**  
  * **Pix:** **0.49%** ao receber via Código QR ou Checkout.16  
  * **Cartão de Crédito (À Vista):** A estrutura de taxas varia com base no período de liquidação. Para pagamentos via Código QR, a taxa pública é de **4.98%** para liquidação instantânea ou **3.98%** para liquidação em 30 dias.17 Estas são as taxas públicas mais concretas disponíveis e são assumidas como representativas das taxas de checkout online.

**Comparação de Custos Iniciais:** O Mercado Pago apresenta uma taxa significativamente menor para o Pix e uma estrutura de taxas competitiva, embora mais complexa, para cartões de crédito em comparação com o Stripe.

### **2.2. Análise Aprofundada: "Parcelado Sem Juros"**

* **Oferta Nativa do Mercado Pago:**  
  * **Funcionalidade:** O Mercado Pago oferece uma funcionalidade nativa e integrada para que os lojistas ofereçam parcelamento sem juros. O lojista configura o número máximo de parcelas que deseja oferecer (por exemplo, até 12x) e concorda em absorver a taxa de financiamento associada. O cliente vê uma opção clara de "X vezes sem juros" no checkout e paga em parcelas na fatura regular do seu cartão de crédito.18  
  * **Configuração:** Isso é gerenciado diretamente no painel da conta do lojista no Mercado Pago, em "Seu Negócio \> Taxas e Parcelas", onde ele pode ativar "Oferecer parcelamento do vendedor" e definir o número máximo de parcelas.18  
  * **Alinhamento com o Mercado:** Este modelo se alinha perfeitamente com as expectativas dos consumidores brasileiros, que estão acostumados com este método de pagamento para uma vasta gama de compras.19  
* **Abordagem do Stripe: Parcerias BNPL vs. Parcelamento Nativo:**  
  * **Importância Reconhecida:** A própria documentação do Stripe reconhece que o pagamento parcelado é uma "característica distinta do mercado de cartões de crédito brasileiro" e um comportamento de compra local chave.19  
  * **Falta de Funcionalidade Nativa:** Apesar deste reconhecimento, a documentação pública para o Brasil **não descreve** uma funcionalidade nativa de "parcelado sem juros" financiada pelo lojista, semelhante à do Mercado Pago. As páginas de preços listam uma única taxa para transações com cartão, sem diferenciar para parcelamentos.15  
  * **Solução Proposta (BNPL):** A estratégia global do Stripe para parcelamentos gira em torno da integração de provedores terceirizados de "Buy Now, Pay Later" (BNPL), como Affirm, Klarna e Afterpay.21 O Stripe possui até uma página de destino específica para o Affirm no Brasil.24

A distinção entre "parcelado" tradicional e BNPL é fundamental. Um observador menos experiente pode ver "parcelas" e assumir que a solução BNPL do Stripe é equivalente ao "parcelado" do Mercado Pago, o que seria um erro crítico.

1. **Mecanismo:** O "parcelado sem juros" é uma funcionalidade do *limite de crédito existente do cliente*, processada pela adquirente. O BNPL é um *empréstimo separado no ponto de venda*, oferecido por uma empresa financeira terceira (como a Affirm).  
2. **Experiência do Cliente:** Com o "parcelado", o cliente simplesmente escolhe o número de parcelas no checkout. Com o BNPL, o cliente é frequentemente redirecionado para o site do provedor BNPL ou para um pop-up para passar por uma verificação de crédito separada (embora muitas vezes rápida) e concordar com um novo conjunto de termos. Isso adiciona atrito ao processo de checkout.  
3. **Confiança e Hábito do Consumidor:** Os consumidores brasileiros têm décadas de familiaridade com o "parcelado" em seus cartões de crédito. É um sistema confiável e onipresente. O BNPL é um conceito mais novo e menos familiar. Forçar os clientes a um caminho de BNPL quando eles esperam uma opção simples de "parcelado" pode levar ao abandono do carrinho.

A dependência do Stripe de parceiros BNPL não atende adequadamente à necessidade específica do mercado por parcelamento nativo no cartão de crédito no Brasil. Isso representa uma lacuna significativa em sua oferta e uma grande vantagem competitiva para o Mercado Pago.

### **2.3. Tabela 1: Análise Comparativa das Ofertas de Parcelamento Sem Juros**

Esta tabela destaca as diferenças fundamentais na forma como cada gateway aborda a funcionalidade crucial de parcelamento sem juros para o mercado brasileiro.

| Característica                 | Mercado Pago                                                                                                                                                               | Stripe                                                                                                                                                                         |
|:------------------------------ |:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Nome da Funcionalidade**     | Parcelamento Sem Juros (Oferecer parcelas do vendedor)                                                                                                                     | Buy Now, Pay Later (BNPL) com Parceiros (ex: Affirm)                                                                                                                           |
| **Método de Implementação**    | **Nativo.** Integrado ao processamento de cartão de crédito.                                                                                                               | **Parceria de Terceiros.** Requer integração com um provedor de BNPL.                                                                                                          |
| **Configuração do Lojista**    | Direta no painel do Mercado Pago ("Taxas e Parcelas"), onde o lojista define o nº máximo de parcelas sem juros que deseja subsidiar.18                                     | Habilitação do método de pagamento BNPL (ex: Affirm) no painel da Stripe. As opções de parcelamento são gerenciadas pelo parceiro.24                                           |
| **Experiência do Cliente**     | O cliente seleciona o número de parcelas ("X vezes sem juros") diretamente no checkout, usando o limite do seu cartão de crédito existente. Experiência fluida e familiar. | O cliente seleciona a opção BNPL, podendo ser redirecionado para um fluxo de aprovação de crédito separado com o parceiro (Affirm), adicionando etapas e atrito ao checkout.21 |
| **Nº Máximo de Parcelas**      | Configurável pelo lojista, geralmente até 12x.18                                                                                                                           | Depende do parceiro BNPL e do perfil de crédito do cliente (ex: Affirm oferece até 36 meses, mas pode incluir juros).24                                                        |
| **Taxa de Processamento**      | O lojista arca com a taxa de financiamento, que é adicionada à taxa de transação padrão. As taxas específicas não são publicamente detalhadas.                             | O lojista paga a taxa de transação para o provedor BNPL (via Stripe), que é diferente da taxa de cartão de crédito padrão. O lojista recebe o valor total adiantado.22         |
| **Requisitos/Compatibilidade** | Compatível com os principais plugins de comunidade para MedusaJS.                                                                                                          | Requer que o plugin MedusaJS (@medusajs/payment-stripe) suporte a renderização de métodos de pagamento alternativos como o Affirm.                                             |
| **Limitações/Condições**       | O custo do financiamento é absorvido pelo lojista.                                                                                                                         | Não é o "parcelado sem juros" tradicional esperado pelos consumidores brasileiros. A aprovação do cliente depende de uma análise de crédito do parceiro BNPL.                  |

## **Seção 3: Síntese e Recomendações Estratégicas**

Esta seção final sintetiza os resultados técnicos e financeiros em uma comparação clara e acionável para orientar a decisão final.

### **3.1. Tabela 2: Resumo Comparativo Geral dos Gateways**

| Critério                     | Stripe                                                                                                                                                                         | Mercado Pago                                                                                                                     |
|:---------------------------- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |:-------------------------------------------------------------------------------------------------------------------------------- |
| **Plugin MedusaJS**          | **@medusajs/payment-stripe (Oficial)** 2                                                                                                                                       | **Plugins de Comunidade** (ex: @marcosgn/medusa-payment-mercadopago) 12                                                          |
| **Facilidade de Integração** | **Alta.** Plugin oficial, estável e bem documentado.                                                                                                                           | **Média.** Requer avaliação de plugins da comunidade; maior risco de manutenção.                                                 |
| **Taxa Pix**                 | 1.19% 15                                                                                                                                                                       | **0.49%** 17                                                                                                                     |
| **Taxa Crédito à Vista**     | 3.99% \+ R`0.39 15                                                                                                                                                             | \~4.98% (liberação na hora) / \~3.98% (liberação em 30 dias) 17                                                                  |
| **Promoção de Parcelamento** | **BNPL com parceiros (ex: Affirm).** Não é o "parcelado sem juros" nativo.24                                                                                                   | **"Parcelado Sem Juros" Nativo.** Lojista absorve o custo. Totalmente alinhado ao mercado brasileiro.18                          |
| **Link Documentação Plugin** | ([https://docs.medusajs.com/resources/commerce-modules/payment/payment-provider/stripe](https://docs.medusajs.com/resources/commerce-modules/payment/payment-provider/stripe)) | ([https://github.com/marcosgomesneto/medusa-payment-mercadopago](https://github.com/marcosgomesneto/medusa-payment-mercadopago)) |

### **3.2. Diferenciais Chave e Recomendação Final**

O trade-off central é claro: **Stripe** oferece uma experiência de desenvolvedor superior, mais segura e estável devido ao seu plugin oficial. **Mercado Pago** oferece uma adequação ao mercado brasileiro vastamente superior devido ao seu suporte nativo para "parcelado sem juros" e taxas de Pix mais competitivas.  
Uma simples comparação das taxas de transação é insuficiente para uma decisão completa. O Custo Total de Propriedade (TCO) deve ser considerado. O TCO do Stripe inclui taxas de transação mais altas, mas custos "ocultos" mais baixos relacionados ao tempo de desenvolvimento, manutenção e gerenciamento de riscos. O TCO do Mercado Pago, por outro lado, apresenta taxas de transação mais baixas, mas custos "ocultos" potencialmente mais altos associados ao gerenciamento de um plugin comunitário e ao risco de ele se tornar obsoleto.  
O impacto na conversão é talvez o fator mais decisivo. A falta de uma verdadeira opção de "parcelado sem juros" no Stripe não é uma lacuna de funcionalidade menor; é um potencial bloqueador de conversão no mercado brasileiro. Oferecer uma alternativa BNPL pode confundir os clientes e levar a taxas mais altas de abandono de carrinho.  
**Recomendação Estratégica Final:**

* **Para empresas que priorizam estabilidade técnica a longo prazo, previsibilidade e minimização da sobrecarga de engenharia:** **Stripe** é a escolha recomendada. Este caminho aceita um compromisso na adequação perfeita ao mercado em troca de uma integração robusta e oficialmente suportada. É a decisão técnica de menor risco.  
* **Para empresas que priorizam a penetração de mercado, otimização da conversão e eficiência de custos no Brasil:** **Mercado Pago** é a escolha inequivocamente superior. Seu suporte nativo para "parcelado sem juros" e taxas de Pix mais baixas atendem diretamente às principais expectativas do consumidor brasileiro. Este caminho exige que a empresa aceite e aloque recursos conscientemente para gerenciar o risco técnico associado ao uso de um plugin mantido pela comunidade. O potencial de aumento na receita e na participação de mercado provavelmente supera os riscos técnicos para a maioria das operações de e-commerce voltadas para o consumidor no Brasil.

#### **Referências citadas**

1. @medusajs/payment-stripe \- npm, acessado em agosto 30, 2025, [https://www.npmjs.com/package/%40medusajs%2Fpayment-stripe](https://www.npmjs.com/package/%40medusajs%2Fpayment-stripe)  
2. Stripe Module Provider \- Medusa Documentation, acessado em agosto 30, 2025, [https://docs.medusajs.com/resources/commerce-modules/payment/payment-provider/stripe](https://docs.medusajs.com/resources/commerce-modules/payment/payment-provider/stripe)  
3. Stripe \- Medusa.js, acessado em agosto 30, 2025, [https://medusajs.com/integrations/@medusajspayment-stripe/](https://medusajs.com/integrations/@medusajspayment-stripe/)  
4. Plugins \- Medusa Documentation, acessado em agosto 30, 2025, [https://docs.medusajs.com/v1/development/plugins/overview](https://docs.medusajs.com/v1/development/plugins/overview)  
5. medusa-payment-stripe \- NPM, acessado em agosto 30, 2025, [https://www.npmjs.com/package/medusa-payment-stripe](https://www.npmjs.com/package/medusa-payment-stripe)  
6. Use Stripe's Payment Element in the Next.js Starter Storefront \- Medusa Documentation, acessado em agosto 30, 2025, [https://docs.medusajs.com/resources/nextjs-starter/guides/customize-stripe](https://docs.medusajs.com/resources/nextjs-starter/guides/customize-stripe)  
7. Stripe Plugin: v6.0.7 \- Medusa docs, acessado em agosto 30, 2025, [https://docs.medusajs.com/v1/upgrade-guides/plugins/stripe/6-0-7](https://docs.medusajs.com/v1/upgrade-guides/plugins/stripe/6-0-7)  
8. MercadoPago \- Medusa.js, acessado em agosto 30, 2025, [https://medusajs.com/integrations/@minskylabmedusa-payment-mercadopago/](https://medusajs.com/integrations/@minskylabmedusa-payment-mercadopago/)  
9. minskylab/medusa-payment-mercadopago: A MedusaJS plugin for Mercado Pago, a payment platform in Latin America. It enables merchants to accept payments in their local currency. \- GitHub, acessado em agosto 30, 2025, [https://github.com/minskylab/medusa-payment-mercadopago](https://github.com/minskylab/medusa-payment-mercadopago)  
10. NicolasGorga/medusa-payment-mercadopago: Mercado ... \- GitHub, acessado em agosto 30, 2025, [https://github.com/NicolasGorga/medusa-payment-mercadopago](https://github.com/NicolasGorga/medusa-payment-mercadopago)  
11. @nicogorga/medusa-payment-mercadopago \- npm, acessado em agosto 30, 2025, [https://www.npmjs.com/package/@nicogorga/medusa-payment-mercadopago](https://www.npmjs.com/package/@nicogorga/medusa-payment-mercadopago)  
12. marcosgomesneto/medusa-payment-mercadopago: Mercadopago Transparent Checkout Provider for Meduas Commerce accept Credit Card and PIX \- GitHub, acessado em agosto 30, 2025, [https://github.com/marcosgomesneto/medusa-payment-mercadopago](https://github.com/marcosgomesneto/medusa-payment-mercadopago)  
13. Local payment methods pricing \- Stripe, acessado em agosto 30, 2025, [https://stripe.com/en-br/pricing/local-payment-methods](https://stripe.com/en-br/pricing/local-payment-methods)  
14. Pricing & Fees \- Stripe, acessado em agosto 30, 2025, [https://stripe.com/en-br/pricing](https://stripe.com/en-br/pricing)  
15. Preços e tarifas \- Stripe, acessado em agosto 30, 2025, [https://stripe.com/br/pricing](https://stripe.com/br/pricing)  
16. Todas as formas de aceitar Pix estão no Mercado Pago, acessado em agosto 30, 2025, [https://www.mercadopago.com.br/ferramentas-para-vender/aceitar-pix](https://www.mercadopago.com.br/ferramentas-para-vender/aceitar-pix)  
17. Quanto custa receber pagamentos com código QR? \- Mercado Pago, acessado em agosto 30, 2025, [https://www.mercadopago.com.br/ajuda/quanto-custa-receber-pagamentos-com-codigo-qr_3605](https://www.mercadopago.com.br/ajuda/quanto-custa-receber-pagamentos-com-codigo-qr_3605)  
18. Discounts, installments, and interest \- Payment configuration ..., acessado em agosto 30, 2025, [https://www.mercadopago.com.br/developers/en/docs/nuvemshop/payments-configuration/discounts-and-installment-payments](https://www.mercadopago.com.br/developers/en/docs/nuvemshop/payments-configuration/discounts-and-installment-payments)  
19. How to accept payments in Brazil \- Stripe, acessado em agosto 30, 2025, [https://stripe.com/en-jp/resources/more/payments-in-brazil](https://stripe.com/en-jp/resources/more/payments-in-brazil)  
20. How to accept payments in Brazil \- Stripe, acessado em agosto 30, 2025, [https://stripe.com/br/resources/more/payments-in-brazil](https://stripe.com/br/resources/more/payments-in-brazil)  
21. How do installment payments work? \- Stripe, acessado em agosto 30, 2025, [https://stripe.com/ae/resources/more/installment-payments-101-a-guide-for-businesses](https://stripe.com/ae/resources/more/installment-payments-101-a-guide-for-businesses)  
22. What is buy now, pay later? BNPL platforms for businesses \- Stripe, acessado em agosto 30, 2025, [https://stripe.com/guides/buy-now-pay-later](https://stripe.com/guides/buy-now-pay-later)  
23. Buy now, pay later: What businesses should know \- Stripe, acessado em agosto 30, 2025, [https://stripe.com/resources/more/how-businesses-can-accept-buy-now-pay-later](https://stripe.com/resources/more/how-businesses-can-accept-buy-now-pay-later)  
24. Affirm on Stripe: Let customers buy now and pay later, acessado em agosto 30, 2025, [https://stripe.com/en-br/payments/affirm](https://stripe.com/en-br/payments/affirm)