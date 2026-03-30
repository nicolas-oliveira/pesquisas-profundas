<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" style="height:64px;margin-right:32px"/>

# Projeções até 2029 e CDB 120% CDI por 24 meses

**Resumo das Projeções:**

- Manter o Tesouro Selic até 02/09/2029 gera, em termos líquidos, aproximadamente **R\$ 18 786,96**.
- Aplicar hoje R\$ 10 000 em CDB a 120% do CDI por 24 meses resulta, após IR de 15%, em aproximadamente **R\$ 13 450,94**.
- Financeiramente, **compensa manter o Tesouro Selic até 2029** em vez de migrar ao CDB.

***

## 1. Tesouro Selic até 02/09/2029

1. Supondo taxas SELIC periódicas até 18/06/2025 e, a partir daí, **15% a.a.** constante até 2029.
2. Cálculo do montante bruto:

$$
M_{\text{bruto}} = 10\,000 \times \prod_{i}\bigl(1 + \tfrac{r_i}{100}\bigr)^{\frac{D_i}{252}}
$$

onde $r_i$ são as taxas a.a. e $D_i$ os dias úteis em cada subperíodo.
3. Resultado bruto estimado:

$$
M_{\text{bruto}} \approx R\$\,20\,337,60
$$
4. Imposto de Renda (15% para prazo acima de 24 meses):

$$
M_{\text{líquido}} = 10\,000 + (20\,337,60 - 10\,000)\times(1 - 0{,}15) \approx R\$\,18\,786,96
$$

***

## 2. CDB 120% do CDI por 24 meses

1. CDI atual: **14,90% a.a.** → CDB a 120% CDI: **17,88% a.a.**
2. Período de 11/09/2025 a 11/09/2027 (aprox. 504 dias úteis)
3. Montante bruto:

$$
M_{\text{bruto}} = 10\,000 \times \bigl(1 + \tfrac{17{,}88}{100}\bigr)^{\frac{504}{252}} \approx R\$\,14\,059,93
$$
4. IR (15%):

$$
M_{\text{líquido}} = 10\,000 + (14\,059,93 - 10\,000)\times(1 - 0{,}15) \approx R\$\,13\,450,94
$$

***

## 3. Conclusão

A projeção mostra que **manter o Tesouro Selic até 2029** proporciona um valor líquido final (≈ R\$ 18 786,96) significativamente superior ao que se obteria ao migrar para o **CDB 120% CDI por 24 meses** (≈ R\$ 13 450,94).

Portanto, do ponto de vista financeiro, **não compensa** resgatar o Tesouro Selic para investir no CDB de 24 meses.

***

### Passo a Passo em LaTeX

```latex
\begin{align*}
\text{Montante bruto (Selic)} &= P \times \prod_{i=1}^{n}\left(1+\frac{r_i}{100}\right)^{\frac{D_i}{252}}\\
P &= 10\,000,\quad r_i = \text{taxas SELIC periódicas},\quad D_i = \text{dias úteis em cada período}\\
\text{IR Selic} &= 15\%\\
M_{\text{líquido,Selic}} &= P + \bigl(M_{\text{bruto}} - P\bigr)\times(1 - 0{,}15)\\
\text{Montante bruto (CDB)} &= P \times \left(1 + \frac{120\%\times\mathrm{CDI}}{100}\right)^{\frac{D}{252}}\\
\mathrm{CDI} &= 14{,}90\%,\quad \text{taxa CDB} = 17{,}88\%\\
D &= \text{número de dias úteis em 24 meses}\\
\text{IR CDB} &= 15\%\\
M_{\text{líquido,CDB}} &= P + \bigl(M_{\text{bruto,CDB}} - P\bigr)\times(1 - 0{,}15)
\end{align*}
```

<span style="display:none">[^1]</span>

<div style="text-align: center">⁂</div>

[^1]: https://ppl-ai-code-interpreter-files.s3.amazonaws.com/web/direct-files/ab004e967c82fb1b5d28ac4968f29230/58d138e4-327e-495c-82f3-fb30499353f4/85900599.csv

