# Gemini: Análise Estratégica de Gateways de Pagamento para E-commerce no Brasil: Um Relatório Técnico para Integração com MedusaJS

## Sumário Executivo

Este relatório apresenta uma análise técnica aprofundada das principais soluções de gateway de pagamento para e-commerce no Brasil, com foco no processamento de transações de cartão de crédito para o ano de 2025. O estudo foi conduzido sob o contexto específico de um site de lista de casamento, desenvolvido com o framework headless MedusaJS, que requer um carrinho de compras tradicional e a capacidade de compartilhar links de pagamento via WhatsApp de forma fluida.

A análise revela que o mercado brasileiro de pagamentos é marcado por uma intensa competição em taxas, mas apresenta uma notável disparidade na maturidade dos ecossistemas para desenvolvedores, especialmente no que tange a frameworks modernos como o MedusaJS. A decisão ótima transcende a simples escolha da menor taxa percentual, devendo considerar o Custo Total de Propriedade (TCO), que engloba os custos de desenvolvimento, integração, manutenção e o risco técnico associado.

As principais recomendações são segmentadas por estratégia de implementação:

1. **Opção de Menor Atrito (Recomendação Principal): Stripe.** Destaca-se pela sua integração nativa, robusta e oficialmente mantida com o MedusaJS.1 Esta abordagem garante o menor tempo de implementação (
   
   *time-to-market*) e o menor risco técnico, tornando-se a escolha ideal para equipes que priorizam velocidade e estabilidade, apesar de suas taxas nominais serem mais elevadas em comparação com os concorrentes locais.3

2. **Opção de Melhor Custo-Benefício (com Desenvolvimento Customizado): Mercado Pago.** Apresenta taxas mais competitivas e um conjunto de funcionalidades profundamente adaptado ao mercado brasileiro, como a integração nativa com o Meta Pay para pagamentos via WhatsApp.4 Contudo, a ausência de um plugin oficial para MedusaJS exige um investimento em desenvolvimento para criar uma integração customizada via API REST, utilizando um plugin da comunidade como base 5 ou construindo uma solução do zero. Esta via é recomendada para projetos com orçamento de desenvolvimento flexível e foco na otimização de custos operacionais a longo prazo.

A decisão estratégica final dependerá do balanço entre o orçamento e o cronograma do projeto. Recomenda-se a realização de uma Prova de Conceito (PoC) com a Stripe para validar a agilidade da integração, seguida de uma análise de esforço para o desenvolvimento customizado com o Mercado Pago, permitindo uma escolha final baseada em dados concretos.

## 1. Análise Comparativa Aprofundada dos Gateways de Pagamento no Brasil

### 1.1. Cenário Financeiro e Comercial: Taxas e Custos Adicionais

A estrutura de custos é um dos pilares fundamentais na escolha de um gateway de pagamento. A análise das taxas publicadas para 2025 revela um cenário competitivo, mas com nuances importantes relacionadas a prazos de recebimento e custos ocultos.

**Análise de Taxas de Transação (Crédito à Vista)**

As taxas para transações de crédito à vista, que representam o principal custo operacional, variam consideravelmente entre os provedores. A comparação a seguir considera um prazo de recebimento padrão de 30 dias (D+30) para uma base de comparação equitativa.

- **Stripe:** Posiciona-se como uma solução premium, com uma taxa de **3.99% + R$ 0,39** para cartões de crédito nacionais.3

- **PagBank (PagSeguro):** Oferece um modelo flexível. Para recebimento em 30 dias, a taxa é de **3.19% + R$ 0,40**. Caso o lojista opte por receber em 14 dias, a taxa sobe para **3.99% + R$ 0,40**.6

- **Mercado Pago:** Apresenta uma estrutura similar. Para vendas via WhatsApp, a taxa para recebimento em 30 dias é de **3.99%**, subindo para **4.99%** para recebimento instantâneo.4 As taxas para maquininhas, que podem servir como referência para negociação, são de
  
  **3.09%** para D+30.7

- **Rede (Stone):** Destaca-se por ter uma das taxas mais competitivas para links de pagamento, com **2.99%** para crédito à vista.8 No entanto, suas taxas para e-commerce são altamente personalizadas e negociadas com base no faturamento.9

- **Moip (Pagar.me):** Como parte do ecossistema Stone/PagBank, suas taxas históricas de **2.99% + R$ 0,39** para D+30 10 estão alinhadas com as ofertas atuais do grupo, indicando um forte posicionamento em custo.

- **Getnet:** Para sua solução de checkout online, as taxas partem de **3.50%** para crédito à vista.11

- **Iugu:** A taxa para crédito à vista é de **3.34%**, acrescida de uma tarifa de processamento de R$ 0,40 por transação.12

- **Cielo:** As taxas para e-commerce não são publicamente detalhadas, mas as taxas para maquininhas partem de **3.48%** 13, servindo como um ponto de partida para negociações.

**Estrutura de Custos Adicionais**

A tendência do mercado, impulsionada por players focados em tecnologia, é a eliminação de custos fixos para planos padrão. A maioria das soluções modernas, como Stripe, PagBank, Mercado Pago e Stone, opera em um modelo *pay-as-you-go*, sem cobrança de mensalidade, taxa de setup ou taxa de cancelamento.3

Esta abordagem remove barreiras de entrada para pequenas e médias empresas. No entanto, plataformas com um histórico mais corporativo, como Braspag e Cielo, podem ainda operar com modelos de contrato que incluem pacotes de transações e taxas de manutenção, especialmente para clientes de grande porte.15 As taxas de cancelamento de transações (estorno) geralmente não implicam em custos adicionais além da devolução da taxa de MDR proporcional ao valor estornado, como no caso do Pagar.me.17

**Potencial de Negociação e Modelos de Preços para Escala**

É fundamental compreender que as taxas listadas publicamente são, na maioria das vezes, "taxas de prateleira". Para negócios com um volume de transações significativo (geralmente acima de R$ 50.000 mensais), existe uma margem considerável para negociação. Provedores como Stone, Pagar.me e Cielo são conhecidos por sua flexibilidade em customizar planos de preços para clientes de maior porte.9 A estratégia recomendada para um novo negócio é iniciar com um provedor que ofereça a melhor combinação de facilidade de integração e taxas iniciais razoáveis. Após atingir um volume de vendas consistente, esses dados históricos se tornam uma poderosa ferramenta de negociação para obter taxas mais agressivas junto aos principais players nacionais.

### 1.2. Otimização da Experiência do Cliente e Performance Mobile

Em um mercado onde o tráfego mobile domina, a qualidade da experiência de checkout é um fator determinante para a conversão de vendas.

**Avaliação da Qualidade do Checkout Mobile**

A qualidade do checkout mobile deixou de ser um diferencial para se tornar um requisito básico. A verdadeira distinção reside na capacidade de oferecer uma experiência de pagamento que seja não apenas responsiva, mas também nativa e integrada ao fluxo da loja, sem redirecionamentos que quebram a jornada do usuário.

- **Stripe:** É a referência global em experiência de checkout. Sua solução, **Stripe Elements**, permite a construção de formulários de pagamento totalmente customizados e integrados ao front-end da loja, seguindo as melhores práticas de usabilidade mobile, como preenchimento automático e validação em tempo real.19

- **Mercado Pago e Pagar.me:** Investem pesadamente em suas soluções de **Checkout Transparente** e **Checkout Pro/Bricks**, que são altamente otimizadas para o público brasileiro. Eles oferecem funcionalidades como compra com um clique (*one-click buy*) e armazenamento seguro de cartões para facilitar compras futuras.18

- **Cielo e Braspag:** Oferecem o **Checkout Cielo**, uma página de pagamento externa, porém segura e otimizada.22 Também suportam carteiras digitais como Apple Pay e Google Pay, que simplificam o processo em dispositivos móveis.24

**Análise Prática da Funcionalidade de Links de Pagamento para WhatsApp**

Para o caso de uso de uma lista de casamento, a capacidade de gerar e compartilhar links de pagamento de forma simples e eficaz é crucial. Esta funcionalidade permite que convidados comprem presentes específicos sem a necessidade de navegar por todo o carrinho de compras.

- **Mercado Pago:** Oferece a experiência mais fluida através de sua integração direta com o **Meta Pay**.4 Isso permite que o processo de pagamento seja iniciado e, em grande parte, concluído dentro do próprio ecossistema do WhatsApp, minimizando a fricção e os redirecionamentos.4

- **PagBank, Cielo, Stone e outros:** Todos oferecem ferramentas robustas para a criação de links de pagamento que podem ser compartilhados em qualquer canal, incluindo o WhatsApp.8 O fluxo, no entanto, geralmente envolve o usuário clicando no link e sendo redirecionado para uma página de pagamento em um navegador móvel. Embora funcional, este passo adicional representa um potencial ponto de abandono em comparação com a experiência mais nativa do Mercado Pago.

- **PayPal:** Através do **PayPal.Me**, oferece uma solução de link pessoal que pode ser adaptada para uso comercial, sendo facilmente compartilhável.26

A capacidade de gerar esses links programaticamente via API é um requisito técnico essencial para automatizar o compartilhamento a partir do site da lista de casamento.

### 1.3. Arquitetura de Segurança e Prevenção a Fraudes

A segurança é inegociável em transações online. As plataformas de pagamento investem pesadamente em tecnologias para proteger tanto o lojista quanto o consumidor.

**Comparativo dos Mecanismos Antifraude (Uso de IA/Machine Learning)**

A sofisticação do sistema antifraude é um diferencial competitivo chave.

- **Stripe Radar:** É considerado o *benchmark* do setor. Utiliza modelos de *machine learning* treinados com dados de sua imensa rede global para atribuir pontuações de risco em tempo real. A plataforma permite a criação de regras customizadas e a aplicação de autenticação 3D Secure dinâmica para transações de alto risco, equilibrando segurança e conversão.27

- **Mercado Pago:** Desenvolveu um sistema proprietário robusto, baseado em redes neurais e IA, que analisa milhares de variáveis em tempo real. O processo inclui uma "segunda pontuação" com validação em bancos de dados externos e uma etapa de revisão manual por especialistas para casos ambíguos.28

- **PagBank:** Inova ao adicionar uma camada de segurança biométrica, utilizando tecnologia de reconhecimento facial para validar a identidade do comprador em certas transações, além das análises de fraude tradicionais.14

- **Cielo/Braspag e Vindi:** Adotam uma abordagem de parceria, integrando soluções de especialistas renomados em antifraude, como **ClearSale**, **Cybersource**, **ACI Worldwide** e **Konduto**. Isso garante um alto nível de proteção, mas a configuração pode ser mais complexa e, potencialmente, envolver custos adicionais dependendo do parceiro escolhido.29

**Verificação de Conformidade (PCI DSS) e Práticas de Tokenização**

Todas as plataformas analisadas afirmam possuir a certificação **PCI DSS Nível 1**, o mais alto padrão de segurança da indústria de cartões. Este é um pré-requisito obrigatório e não um diferencial.32

O mecanismo central para garantir a segurança e simplificar a conformidade do lojista é a **tokenização**. Em vez de armazenar os dados do cartão de crédito, o gateway os converte em um "token" não sensível. O ponto crítico para a equipe de desenvolvimento não é *se* a plataforma utiliza tokenização, mas *como* ela é implementada. Soluções como **Stripe Elements** e o **tokenizecard.js** do Pagar.me 36 são exemplos de implementações de checkout transparente que coletam os dados do cartão diretamente no navegador do cliente e os enviam de forma segura para o gateway, que retorna o token. Este método impede que dados sensíveis de cartão transitem ou sejam armazenados nos servidores da loja, reduzindo drasticamente o escopo de conformidade PCI do lojista para o questionário mais simples (SAQ A).

## 2. Análise Técnica de Integração com a Plataforma MedusaJS

A viabilidade técnica e o esforço de integração são fatores decisivos para o projeto, especialmente ao utilizar um framework *developer-first* como o MedusaJS.

### Tabela 1: Visão Geral Comparativa das Plataformas de Pagamento (Brasil, 2025)

Esta tabela consolida os critérios de avaliação em um formato comparativo, permitindo uma análise rápida dos pontos fortes e fracos de cada provedor em relação aos requisitos do projeto.

| Plataforma       | Taxas (Crédito à Vista)¹   | Taxa Fixa                   | Checkout Mobile                   | Links WhatsApp                | Anti-Fraude                          | Custos Adicionais²           |
| ---------------- | -------------------------- | --------------------------- | --------------------------------- | ----------------------------- | ------------------------------------ | ---------------------------- |
| **Stripe**       | 3.99%                      | R$ 0,39                     | Excelente (Nativo, Elements)      | Sim (Via API)                 | Radar (IA/ML Avançado)               | Nenhum                       |
| **PagBank**      | 3.19% - 3.99%              | R$ 0,40                     | Muito Bom (Checkout Transparente) | Sim (Nativo, com Envio Fácil) | Proprietário + Reconhecimento Facial | Nenhum                       |
| **Mercado Pago** | 3.99% - 4.99%              | R$ 0,00³                    | Excelente (Checkout Pro, Bricks)  | Sim (Nativo, com Meta Pay)    | Proprietário (IA/ML)                 | Nenhum                       |
| **Pagar.me**     | Sob Consulta (Base ~3-4%)  | Sob Consulta (Base ~R$0,40) | Excelente (Checkout Transparente) | Sim (Via API)                 | Integrado                            | Nenhum                       |
| **Rede (Stone)** | Sob Consulta (Base ~2.99%) | Sob Consulta                | Muito Bom (Checkout Próprio)      | Sim (Nativo)                  | Proprietário                         | Nenhum                       |
| **Cielo**        | Sob Consulta (Base ~3.5%)  | Sob Consulta                | Bom (Checkout Cielo)              | Sim (Nativo)                  | Parceiros (ClearSale, Cybersource)   | Sob Consulta                 |
| **Braspag**      | Sob Consulta               | Sob Consulta                | Bom (Checkout Próprio)            | Não nativo                    | Parceiros (ACI, Cybersource)         | Sob Consulta                 |
| **PayPal**       | ~4.79% + taxa fixa         | ~R$0.60                     | Bom (Checkout Próprio)            | Sim (PayPal.Me)               | Proprietário (IA)                    | Taxas de câmbio              |
| **InfinitePay**  | 4.20%                      | R$ 0,00                     | Focado em App (InfiniteTap)       | Sim (Nativo)                  | Proprietário                         | Nenhum                       |
| **Getnet**       | A partir de 3.50%          | R$ 0,20 (armazen.)          | Bom (Get Checkout)                | Sim (Nativo)                  | Proprietário                         | Taxa por link pago           |
| **Iugu**         | 3.34%                      | R$ 0,40                     | Bom (Checkout Próprio)            | Bom (Checkout Próprio)        | Proprietário                         | Mensalidade/Taxas adicionais |
| **Vindi**        | 3.87%                      | R$ 0,35                     | Bom (Checkout Próprio)            | Sim (Nativo)                  | Parceiros (Konduto, Clearsale)       | Mensalidade                  |

¹ Taxas para recebimento em ~30 dias. Prazos menores implicam taxas maiores.

² Refere-se a mensalidade, setup ou cancelamento para planos de e-commerce padrão.

³ A estrutura de taxa fixa do Mercado Pago pode variar; para vendas no Mercado Livre, há uma taxa fixa 37, mas para links de pagamento, a estrutura parece ser apenas percentual.

### Tabela 2: Análise de Compatibilidade e Integração com MedusaJS

Esta tabela é o núcleo da análise técnica, focando diretamente na viabilidade de implementação no ecossistema MedusaJS. A existência de um módulo nativo bem documentado pode superar a desvantagem de taxas ligeiramente mais altas, devido à economia em horas de desenvolvimento e à redução de riscos de manutenção.

| Plataforma       | Módulo Nativo MedusaJS | API REST                     | Webhooks           | Documentação Técnica                                                                         |
| ---------------- | ---------------------- | ---------------------------- | ------------------ | -------------------------------------------------------------------------------------------- |
| **Stripe**       | Sim (Oficial, V2)      | Sim (Excelente)              | Sim (Completo)     | [docs.stripe.com/api](https://docs.stripe.com/api)                                           |
| **PagBank**      | Não                    | Sim (Completa)               | Sim (Notificações) | [developer.pagbank.com.br](https://developer.pagbank.com.br/v1/reference/api-de-conciliacao) |
| **Mercado Pago** | Sim (Comunidade)       | Sim (Excelente)              | Sim (Completo)     | [mercadopago.com.br/developers](https://www.mercadopago.com.br/developers/pt/reference)      |
| **Pagar.me**     | Não                    | Sim (Excelente)              | Sim (Completo)     | [docs.pagar.me](https://docs.pagar.me/)                                                      |
| **Rede (Stone)** | Não                    | Sim (Disponível)             | Sim                | [docs.stone.com.br](https://docs.stone.com.br/)                                              |
| **Cielo**        | Não                    | Sim (Completa)               | Sim (Notificações) | [docs.cielo.com.br](https://docs.cielo.com.br/ecommerce-cielo/docs/sobre-api-ecommerce)      |
| **Braspag**      | Não                    | Sim (Completa)               | Sim (Notificações) | [braspag.github.io](https://braspag.github.io/)                                              |
| **PayPal**       | Sim (Oficial, V1)      | Sim (Completa)               | Sim (Completo)     | [developer.paypal.com/docs/api](https://developer.paypal.com/docs/api/payments/v1/)          |
| **InfinitePay**  | Não                    | Sim (Focada em App/Deeplink) | Parcial            | [infinitepay.io/desenvolvedores](https://www.infinitepay.io/desenvolvedores)                 |
| **Outros**       | Não                    | Geralmente Sim               | Geralmente Sim     | Variável                                                                                     |

### Notas Técnicas e Limitações Críticas

- **InfinityPay - Incompatibilidade Fundamental:** A análise da documentação e do modelo de negócio da InfinityPay revela um foco predominante no uso de seu aplicativo como um terminal de ponto de venda (PDV) via InfiniteTap ou para a geração de links de pagamento simples.38 Não foram encontradas evidências de uma API REST robusta projetada para suportar a integração com um fluxo de carrinho de compras complexo de uma plataforma de e-commerce externa. Esta limitação arquitetônica torna a InfinityPay inadequada para os requisitos do projeto e, portanto, deve ser descartada.

- **MedusaJS - O Dilema do Ecossistema de Plugins:** A pesquisa confirma que o MedusaJS, como plataforma *open-source* global, possui um ecossistema de integrações maduro e oficialmente suportado para players internacionais como Stripe e PayPal.1 Em contrapartida, para os principais players brasileiros (PagBank, Pagar.me, Cielo, Rede), não existem plugins oficiais mantidos pela equipe do Medusa.42 Esta lacuna cria um trade-off estratégico fundamental:
  
  - **Caminho 1 (Global Player):** Utilizar a Stripe resulta em uma integração *plug-and-play* e de baixo risco técnico 2, mas acarreta custos por transação mais elevados.
  
  - **Caminho 2 (Local Player):** Optar por um player nacional como o Mercado Pago oferece taxas mais baixas, mas exige que a equipe de desenvolvimento assuma a responsabilidade pela integração. Isso pode ser feito adaptando um plugin mantido pela comunidade (como o `@minskylab/medusa-payment-mercadopago` 5), com os riscos inerentes de falta de atualizações e suporte, ou desenvolvendo uma integração totalmente customizada do zero, o que implica em custos significativos de desenvolvimento e manutenção contínua.

- **Qualidade da Documentação e Suporte ao Desenvolvedor:** Existe uma clara estratificação na qualidade da experiência do desenvolvedor (*developer experience*). Provedores como **Stripe** 44,
  
  **Mercado Pago** 45 e
  
  **Pagar.me** 47 oferecem portais de desenvolvedor modernos, com documentação API RESTful clara, exemplos de código em várias linguagens, SDKs bem mantidos e ambientes de sandbox funcionais. Plataformas mais tradicionais, embora possuam APIs completas, podem apresentar uma documentação menos intuitiva e um processo de onboarding mais burocrático, exigindo um maior investimento de tempo da equipe de desenvolvimento.

## 3. Recomendações Estratégicas e Veredito Final

Com base na análise multifatorial, as recomendações são estruturadas para alinhar a escolha tecnológica com os objetivos estratégicos do negócio.

### Recomendação Principal (Custo-Benefício e Integração Simplificada): Stripe

Apesar de apresentar as taxas nominais mais altas entre os principais concorrentes, a **Stripe** é a recomendação principal devido ao seu Custo Total de Propriedade (TCO) potencialmente menor no contexto de uma implementação com MedusaJS. O fator decisivo é a existência de um plugin de pagamento oficial, versão 2, mantido pela equipe do Medusa.1 Isso elimina a complexidade, o custo e o risco associados ao desenvolvimento de uma integração customizada. Para uma equipe que valoriza a velocidade de lançamento e a estabilidade da plataforma, a economia em horas de desenvolvimento e manutenção supera a diferença nas taxas de transação. Adicionalmente, a qualidade superior de sua API 44, documentação 44 e sistema antifraude (Radar) 27 representa um padrão de excelência no mercado, garantindo uma base tecnológica sólida e escalável.

### Recomendação Alternativa (Escalabilidade e Foco no Mercado Local): Mercado Pago

Se a otimização das taxas de transação é a prioridade máxima e a equipe possui recursos de desenvolvimento disponíveis, o **Mercado Pago** emerge como a melhor alternativa. Suas taxas são mais competitivas e suas funcionalidades são desenhadas especificamente para o comportamento do consumidor brasileiro, com destaque para a integração fluida com o Meta Pay para pagamentos via WhatsApp.4 A implementação exigirá um esforço de engenharia, seja adaptando e assumindo a manutenção de um plugin da comunidade 5, seja construindo um novo. Embora o custo inicial seja maior, o resultado é uma solução altamente customizada, com custos operacionais menores em escala, o que pode ser vantajoso a longo prazo.

### Conclusão e Próximos Passos

A escolha do gateway de pagamento ideal para este projeto é uma decisão estratégica que equilibra agilidade, custo e risco técnico.

1. **Validação de Prioridades:** A primeira etapa é uma deliberação interna para definir a prioridade estratégica do projeto: minimizar o *time-to-market* e o risco técnico (favorecendo a Stripe) ou minimizar o custo por transação a longo prazo (favorecendo o Mercado Pago).

2. **Prova de Conceito (PoC):** Recomenda-se a implementação de uma PoC com a **Stripe**. O objetivo é validar a rapidez e a simplicidade da integração com o MedusaJS, testando o fluxo de pagamento de ponta a ponta em um ambiente de desenvolvimento.

3. **Análise de Esforço:** Simultaneamente, a equipe de desenvolvimento deve conduzir uma análise de esforço detalhada para a integração customizada com o **Mercado Pago**. Esta análise deve estimar as horas de desenvolvimento necessárias para construir e testar um módulo robusto, bem como o esforço contínuo de manutenção.

4. **Decisão Final:** Com os resultados da PoC da Stripe e a estimativa de custo/esforço para o Mercado Pago em mãos, a liderança do projeto poderá tomar uma decisão final informada, baseada em dados concretos e alinhada com os recursos e objetivos do negócio.

# Referências

- [1] - Stripe \- Medusa, acessado em agosto 24, 2025, [https://medusajs.com/integrations/@medusajspayment-stripe/](https://medusajs.com/integrations/@medusajspayment-stripe/)  

- [2] - Medusa \+ Nuxt.js \+ Stripe \- How to Create a Nuxt.js Ecommerce Storefront from Scratch Using Medusa Part 3, acessado em agosto 24, 2025, [https://dev.to/medusajs/medusa-nuxtjs-stripe-how-to-create-a-nuxtjs-ecommerce-storefront-from-scratch-using-medusa-part-3-3jg0](https://dev.to/medusajs/medusa-nuxtjs-stripe-how-to-create-a-nuxtjs-ecommerce-storefront-from-scratch-using-medusa-part-3-3jg0)  

- [3] - Stripe para Empresas: Conheça a plataforma para pagamentos ..., acessado em agosto 24, 2025, [https://wise.com/br/blog/stripe-pagamentos-online](https://wise.com/br/blog/stripe-pagamentos-online)  

- [4] - Como vender pelo WhatsApp? \- Mercado Pago, acessado em agosto 24, 2025, [https://www.mercadopago.com.br/ajuda/27896](https://www.mercadopago.com.br/ajuda/27896)  

- [5] - MercadoPago \- Medusa.js, acessado em agosto 24, 2025, [https://medusajs.com/integrations/@minskylabmedusa-payment-mercadopago/](https://medusajs.com/integrations/@minskylabmedusa-payment-mercadopago/)  

- [6] - Connecting PagBank as a Payment Provider | Help Center | Wix.com, acessado em agosto 24, 2025, [https://support.wix.com/en/article/connecting-pagseguro-as-a-payment-provider](https://support.wix.com/en/article/connecting-pagseguro-as-a-payment-provider)  

- [7] - Melhor maquininha de cartão em 2025: taxas e modelos \- Nuvemshop, acessado em agosto 24, 2025, [https://www.nuvemshop.com.br/blog/qual-a-melhor-maquininha-de-cartao/](https://www.nuvemshop.com.br/blog/qual-a-melhor-maquininha-de-cartao/)  

- [8] - Link de Pagamento Stone: veja as como funciona, vantagens e se vale a pena\! \- iDinheiro, acessado em agosto 24, 2025, [https://www.idinheiro.com.br/negocios/link-de-pagamento-stone/](https://www.idinheiro.com.br/negocios/link-de-pagamento-stone/)  

- [9] - Maquininha Stone é boa? Taxas, atendimento e vantagens \- Selectra, acessado em agosto 24, 2025, [https://selectra.net.br/financas/maquininha/stone](https://selectra.net.br/financas/maquininha/stone)  

- [10] - Como ativar a Moip? \- Central de Atendimento Nuvemshop, acessado em agosto 24, 2025, [https://atendimento.nuvemshop.com.br/pt\_BR/como-ativar-a-moip](https://atendimento.nuvemshop.com.br/pt_BR/como-ativar-a-moip)  

- [11] - Getnet \- Soluções de Pagamentos | 4002-4000, acessado em agosto 24, 2025, [https://www.getnet.com.br/](https://www.getnet.com.br/)  

- [12] - Infraestrutura de Tecnologia Financeira completa e flexível | iugu, acessado em agosto 24, 2025, [https://www.iugu.com/](https://www.iugu.com/)  

- [13] - Cielo | Maquininhas de crédito e débito para você vender mais\!, acessado em agosto 24, 2025, [https://www.cielo.com.br/](https://www.cielo.com.br/)  

- [14] - Conheça tudo sobre o Link de Pagamento PagBank \- Blog PagBank, acessado em agosto 24, 2025, [https://blog.pagseguro.uol.com.br/tudo-sobre-link-de-pagamento-pagbank/](https://blog.pagseguro.uol.com.br/tudo-sobre-link-de-pagamento-pagbank/)  

- [15] - Você realmente conhece a Braspag? \- Cielo E-commerce | Plataforma de pagamentos online, acessado em agosto 24, 2025, [https://www.braspag.com.br/voce-realmente-conhece-a-braspag/](https://www.braspag.com.br/voce-realmente-conhece-a-braspag/)  

- [16] - Cielo E-commerce | Plataforma de pagamentos online, acessado em agosto 24, 2025, [https://www.braspag.com.br/](https://www.braspag.com.br/)  

- [17] - Dashboard | Como estornar uma cobrança? \- Central de Ajuda Stone \- Pagar.me, acessado em agosto 24, 2025, [https://pagarme.helpjuice.com/p2-manual-da-dashboard/dashboard-como-cancelar-uma-cobran%C3%A7a](https://pagarme.helpjuice.com/p2-manual-da-dashboard/dashboard-como-cancelar-uma-cobran%C3%A7a)  

- [18] - Pagar.me: Solução Completa para Pagamentos Online, acessado em agosto 24, 2025, [https://www.pagar.me/](https://www.pagar.me/)  

- [19] - Mobile checkout best practices for ecommerce businesses \- Stripe, acessado em agosto 24, 2025, [https://stripe.com/resources/more/mobile-checkout-best-practices-for-ecommerce-businesses](https://stripe.com/resources/more/mobile-checkout-best-practices-for-ecommerce-businesses)  

- [20] - Mercado Pago: cuenta digital \- Apps on Google Play, acessado em agosto 24, 2025, [https://play.google.com/store/apps/details?id=com.mercadopago.wallet](https://play.google.com/store/apps/details?id=com.mercadopago.wallet)  

- [21] - Pagamentos online e gestão financeira \- Pagar.me, acessado em agosto 24, 2025, [https://www.pagar.me/negocios-que-atendemos](https://www.pagar.me/negocios-que-atendemos)  

- [22] - About Checkout Cielo, acessado em agosto 24, 2025, [https://docs.cielo.com.br/link-en/docs/about-checkout-cielo](https://docs.cielo.com.br/link-en/docs/about-checkout-cielo)  

- [23] - Ecommerce Website Design | Cielo, acessado em agosto 24, 2025, [https://octet.design/project/ecommerce-web-design-cielo/](https://octet.design/project/ecommerce-web-design-cielo/)  

- [24] - Braspag Pagador \- Microsoft AppSource, acessado em agosto 24, 2025, [https://appsource.microsoft.com/en-us/product/web-apps/braspag.braspag\_pagador?tab=overview](https://appsource.microsoft.com/en-us/product/web-apps/braspag.braspag_pagador?tab=overview)  

- [25] - About Link de Pagamento \- Cielo E-commerce, acessado em agosto 24, 2025, [https://docs.cielo.com.br/link-en/docs/about-link-de-pagamento](https://docs.cielo.com.br/link-en/docs/about-link-de-pagamento)  

- [26] - PayPal.Me, acessado em agosto 24, 2025, [https://www.paypal.com/paypalme/](https://www.paypal.com/paypalme/)  

- [27] - Stripe Radar | Payment and Credit Card Fraud Detection, acessado em agosto 24, 2025, [https://stripe.com/radar](https://stripe.com/radar)  

- [28] - Antifraude Mercado Pago: entenda como funciona, acessado em agosto 24, 2025, [https://conteudo.mercadopago.com.br/antifraude-mercado-pago-como-funciona-o-sistema-que-cuida-bem-do-seu-dinheiro](https://conteudo.mercadopago.com.br/antifraude-mercado-pago-como-funciona-o-sistema-que-cuida-bem-do-seu-dinheiro)  

- [29] - Payments with fraud analysis \- Cielo E-commerce, acessado em agosto 24, 2025, [https://docs.cielo.com.br/gateway-en/docs/how-to-analyze-the-fraud-risk-in-a-online-payment](https://docs.cielo.com.br/gateway-en/docs/how-to-analyze-the-fraud-risk-in-a-online-payment)  

- [30] - Braspag se asocia con ACI Worldwide para combatir el fraude en el ..., acessado em agosto 24, 2025, [https://tiinside.com.br/es/10/07/2017/Braspag-se-asocia-con-ACI-Worldwide-para-frenar-el-fraude-en-el-comercio-electr%C3%B3nico/](https://tiinside.com.br/es/10/07/2017/Braspag-se-asocia-con-ACI-Worldwide-para-frenar-el-fraude-en-el-comercio-electr%C3%B3nico/)  

- [31] - Como funciona a integração da Vindi com um antifraude?, acessado em agosto 24, 2025, [https://atendimento.vindi.com.br/hc/pt-br/articles/213234028-Como-funciona-a-integra%C3%A7%C3%A3o-da-Vindi-com-um-antifraude](https://atendimento.vindi.com.br/hc/pt-br/articles/213234028-Como-funciona-a-integra%C3%A7%C3%A3o-da-Vindi-com-um-antifraude)  

- [32] - Selo PCI \- PagBank Developers, acessado em agosto 24, 2025, [https://developer.pagbank.com.br/v1/docs/comecando-selo-pci](https://developer.pagbank.com.br/v1/docs/comecando-selo-pci)  

- [33] - PCI DSS \- Seguridad \- Mercado Pago Developers, acessado em agosto 24, 2025, [https://www.mercadopago.com.br/developers/es/docs/security/pci](https://www.mercadopago.com.br/developers/es/docs/security/pci)  

- [34] - What is PCI DSS Compliance? | PayPal US, acessado em agosto 24, 2025, [https://www.paypal.com/us/brc/article/pci-dss-compliance-basics](https://www.paypal.com/us/brc/article/pci-dss-compliance-basics)  

- [35] - What Is PCI DSS Tokenization? It's Guidelines & Best Practices \- SISA, acessado em agosto 24, 2025, [https://www.sisainfosec.com/blogs/what-is-pci-dss-tokenization-its-guidelines-explained/](https://www.sisainfosec.com/blogs/what-is-pci-dss-tokenization-its-guidelines-explained/)  

- [36] - Bem-vindo ao Pagar.me\!, acessado em agosto 24, 2025, [https://docs.pagar.me/docs/overview-principal](https://docs.pagar.me/docs/overview-principal)  

- [37] - Quanto custa vender um produto? \- Mercado Livre, acessado em agosto 24, 2025, [https://www.mercadolivre.com.br/ajuda/quanto-custa-vender-um-produto\_1338](https://www.mercadolivre.com.br/ajuda/quanto-custa-vender-um-produto_1338)  

- [38] - InfinitePay Tap, Conta, Cartão \- Apps on Google Play, acessado em agosto 24, 2025, [https://play.google.com/store/apps/details?id=io.cloudwalk.infinitepaydash](https://play.google.com/store/apps/details?id=io.cloudwalk.infinitepaydash)  

- [39] - Integração InfinitePay: Vendas Ágeis Com Checkout Gratuito, acessado em agosto 24, 2025, [https://www.infinitepay.io/blog/integracao-infinitepay](https://www.infinitepay.io/blog/integracao-infinitepay)  

- [40] - InfinitePay, acessado em agosto 24, 2025, [https://www.infinitepay.io/](https://www.infinitepay.io/)  

- [41] - PayPal \- Medusa docs, acessado em agosto 24, 2025, [https://docs.medusajs.com/v1/plugins/payment/paypal](https://docs.medusajs.com/v1/plugins/payment/paypal)  

- [42] - Integrate your commerce stack with your favourite tools \- Medusa.js, acessado em agosto 24, 2025, [https://medusajs.com/integrations/](https://medusajs.com/integrations/)  

- [43] - Plugins \- Medusa Documentation, acessado em agosto 24, 2025, [https://docs.medusajs.com/v1/development/plugins/overview](https://docs.medusajs.com/v1/development/plugins/overview)  

- [44] - Stripe API Reference, acessado em agosto 24, 2025, [https://docs.stripe.com/api](https://docs.stripe.com/api)  

- [45] - Start \- Mercado Pago Developers, acessado em agosto 24, 2025, [https://www.mercadopago.com.br/developers/en/reference](https://www.mercadopago.com.br/developers/en/reference)  

- [46] - Introdução \- Mercado Pago Developers, acessado em agosto 24, 2025, [https://www.mercadopago.com.br/developers/pt/reference](https://www.mercadopago.com.br/developers/pt/reference)  

- [47] - Pagar.me, acessado em agosto 24, 2025, [https://docs.pagar.me/](https://docs.pagar.me/)
