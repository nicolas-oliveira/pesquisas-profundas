# Análise 1: Análise de Viabilidade Financeira CDB 112% e Selic 2029

**Conclusão Principal:**
Saque do Tesouro Selic hoje e aplicação dos recursos em um CDB a 112% do CDI por três meses gera rentabilidade inferior ao manter o investimento no Tesouro Selic até hoje, mesmo após imposto regressivo de 17,5% no Tesouro.

***

## 1. Dados e Premissas

- Investimento inicial: R\$ 10 000
- Tesouro Selic com liquidez diária: aplicado em 02/09/2024 (COPOM estabeleceu a série de taxas abaixo)
- CDB: 112% do CDI, prazo de 3 meses (09/11/2025 – 11/02/2026)
- Ano útil de 252 dias úteis para cálculo de juros compostos
- Imposto de Renda regressivo no Tesouro Selic no momento do resgate: 17,5% sobre o ganho

### 1.1. Série de Taxas Selic (COPOM)

| Data Início | Taxa Anual (% a.a.) |
|:----------- |:------------------- |
| 01/02/2024  | 11,25               |
| 21/03/2024  | 10,75               |
| 08/05/2024  | 10,50               |
| 18/09/2024  | 10,75               |
| 07/11/2024  | 11,25               |
| 11/12/2024  | 12,25               |
| 30/01/2025  | 13,25               |
| 19/03/2025  | 14,25               |
| 07/05/2025  | 14,75               |
| 18/06/2025  | 15,00               |

### 1.2. Taxa CDI atual

- CDI mensalizado (p.a.): **14,90%**
- CDB contratado a 112% do CDI → taxa nominal anual de 16,688%

***

## 2. Cálculos de Rentabilidade

### 2.1. Tesouro Selic (02/09/2024 a 11/09/2025)

Para cada intervalo $[t_i,t_{i+1}]$, considera-se juros compostos em base de dias úteis:

$$
\text{Montante Bruto} = 10\,000 \times \prod_{i}\Bigl(1 + \tfrac{r_i}{100}\Bigr)^{\frac{D_i}{252}}
$$

- $r_i$: taxa anual em cada período
- $D_i$: número de dias úteis entre $t_i$ e $t_{i+1}$

Calculou-se:

$$
\text{Montante Bruto}_{\text{Tesouro}} \approx R\$\,11\,448{,}91
$$

Aplicando IR de 17,5% sobre o ganho ($\text{Ganho} = 11\,448{,}91 - 10\,000$):

$$
\text{Montante Líquido}_{\text{Tesouro}} = 10\,000 + (11\,448{,}91 - 10\,000)\times(1 - 0{,}175) \approx R\$\,11\,195{,}35
$$

### 2.2. CDB a 112% do CDI (3 meses)

Período: 11/09/2025 a 11/12/2025 (aprox. 63 dias úteis)

$$
\text{Montante Bruto}_{\text{CDB}} = 10\,000 \times \Bigl(1 + \tfrac{16{,}688}{100}\Bigr)^{\frac{63}{252}} \approx R\$\,10\,412{,}49
$$

> *Obs.:* não foi descontado IR do CDB (22,5% para investimentos até 6 meses).

***

## 3. Comparação dos Resultados

| Investimento       | Montante Bruto (R\$) | Montante Líquido (R\$) |
|:------------------ |:-------------------- |:---------------------- |
| Tesouro Selic      | 11 448,91            | 11 195,35              |
| CDB 112% CDI (3 m) | 10 412,49            | –                      |

***

## 4. Interpretação

- Manter o **Tesouro Selic** até hoje e resgatar gera um montante líquido de aproximadamente **R\$ 11 195,35**, já descontado IR de 17,5%.
- Aplicar em um **CDB a 112% do CDI** por três meses renderia **R\$ 10 412,49** brutos (não descontado IR). Mesmo sem considerar imposto, o CDB teria rentabilidade inferior ao Tesouro Selic.

Portanto, **financeiramente não compensa** resgatar o Tesouro Selic para migrar ao CDB 112% CDI por três meses.
<span style="display:none">[^1][^10][^11][^12][^13][^14][^15][^16][^17][^2][^3][^4][^5][^6][^7][^8][^9]</span>

<div style="text-align: center">⁂</div>

[^1]: https://www.global-rates.com/en/interest-rates/central-banks/2/brazilian-bacen-selic-rate/

[^2]: https://tradingeconomics.com/brazil/interbank-rate

[^3]: https://agenciabrasil.ebc.com.br/en/economia/noticia/2024-12/brazils-central-bank-increases-basic-interest-rate-1225-year

[^4]: https://www.portaldefinancas.com/pfenglish/cdidiaria24_en.htm

[^5]: https://www.ceicdata.com/en/indicator/brazil/long-term-interest-rate

[^6]: https://www.b3.com.br/main.jsp?lumPageId=8A6882184DF77A1C014DFC87BEEB3565\&lumA=1\&lumII=8A80CB81633FBF0B016340DEB50F7C18\&locale=en_US\&doui_processActionId=setLocaleProcessAction

[^7]: https://tradingeconomics.com/brazil/interest-rate

[^8]: https://www.bcb.gov.br/controleinflacao/historicotaxasjuros

[^9]: https://www.ceicdata.com/en/indicator/brazil/policy-rate

[^10]: https://www.portaldefinancas.com/pfenglish/cdidiaria25_en.htm

[^11]: https://cbonds.com/indexes/1499/

[^12]: https://www.b3.com.br/pt_br/market-data-e-indices/indices/indices-de-segmentos-e-setoriais/serie-historica-do-di.htm

[^13]: https://www.bcb.gov.br/en/monetarypolicy/selicrate

[^14]: https://www.bcb.gov.br/en/monetarypolicy/interestrates

[^15]: https://www.b3.com.br/en_us/products-and-services/trading/interest-rates/di-training-program/02-interest-rates.htm

[^16]: https://www.investing.com/economic-calendar/brazilian-interest-rate-decision-415

[^17]: https://ppl-ai-code-interpreter-files.s3.amazonaws.com/web/direct-files/7c4e2103118a0d7fdfe3ef9378e6ea36/f7be342e-a7be-4353-9436-14ea051b54f4/fb6ea6b3.csv
