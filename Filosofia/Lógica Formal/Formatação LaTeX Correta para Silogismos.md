# Formatação LaTeX Correta para Silogismos

Para silogismos em LaTeX, o formato correto precisa apresentar as premissas e conclusão como uma estrutura lógica, não apenas como uma lista. Aqui estão as melhores maneiras de formatar:

## Opção 1: Formato Alinhado com Linha Divisória

$$
\begin{array}{l}
P_1: \text{Existem apenas dois pares de brincos de rubi.} \\
P_2: \text{Se tanto Genoveva quanto Griselda tivessem brincos de rubi,} \\
\phantom{P_2: }\text{Guilhermina teria sabido que os seus são de esmeralda.} \\
\hline
\therefore \text{Conclusão}
\end{array}
$$

## Opção 2: Usando o Ambiente `array` Simples

$$
\begin{array}{l}
P: \text{Existem apenas dois pares de brincos de rubi.\ \$0.3em} \\

\text{Se tanto Genoveva quanto Griselda tivessem brincos de rubi,} \\
\text{Guilhermina teria sabido que os seus são de esmeralda.}
\end{array}
$$



## Opção 3: Formato Mais Tradicional para Lógica Formal

$$
\begin{array}{@{}l@{}}
P \quad Existem apenas dois pares de brincos de rubi. \$\$0.5em]

\quad Se tanto Genoveva quanto Griselda tivessem brincos de rubi, \\
\quad Guilhermina teria sabido que os seus são de esmeralda.
\end{array}
$$

## Opção 4: Com Numeração das Premissas

$$
\begin{aligned}
P_1&: \text{Existem apenas dois pares de brincos de rubi.} \\
P_2&: \text{Se tanto Genoveva quanto Griselda tivessem brincos de rubi,} \\
&\phantom{:}\text{ Guilhermina teria sabido que os seus são de esmeralda.}
\end{aligned}
$$

**Recomendação**: Use a **Opção 1** para silogismos formais, pois ela mantém a estrutura lógica clara com premissas identificadas e uma linha divisória antes da conclusão. O `\phantom{}` serve para alinhar adequadamente texto que continua na linha seguinte.[^1][^2][^3]

As principais correções em relação ao código original:

- Remover `\right\` incorreto
- Usar ambiente `array` ou `aligned` para estrutura
- Aplicar `\text{}` para texto em português
- Usar `P_1`, `P_2` para identificar premissas
- Adicionar linha divisória (`\hline`) antes da conclusão quando apropriado
  <span style="display:none">[^10][^11][^12][^13][^14][^15][^16][^17][^18][^19][^20][^4][^5][^6][^7][^8][^9]</span>

<div align="center">⁂</div>

[^1]: https://filosofianaescola.com/logica/silogismo/

[^2]: https://www.ime.unicamp.br/~mfirer/tex2000.pdf

[^3]: https://nupet.daelt.ct.utfpr.edu.br/tcc/engenharia/doc/tcc2/Apostila_TCC_LaTeX_Victor.pdf

[^4]: https://github.com/Grupo-de-Estudos-em-Logica-da-UFRN/Para-Todxs-Natal

[^5]: https://petcomputacaoufrgs.github.io/intro-ao-latex/fontes.html

[^6]: https://www.ufsm.br/app/uploads/sites/679/2019/09/Apostila_LaTeX_v_1.pdf

[^7]: https://devschannel.com/latex/formatacao

[^8]: https://groups.google.com/g/latex-br/c/s_JOQ2ilaLA

[^9]: https://wp.ufpel.edu.br/fernandosimoes/files/2012/06/latex-introducao.pdf

[^10]: https://pt.wikibooks.org/wiki/Latex/Estilos_de_texto

[^11]: https://pt.overleaf.com/learn/latex/Theorems_and_proofs

[^12]: https://www.reddit.com/r/LaTeX/comments/qkmp7k/typesetting_logic_proofs_in_latex/

[^13]: https://aprendolatex.wordpress.com/2010/02/08/texto-normal-em-ambiente-matematico/

[^14]: http://each.uspnet.usp.br/sarajane/wp-content/uploads/2016/10/manual-latex-1.pdf

[^15]: https://pt.wikipedia.org/wiki/LaTeX

[^16]: https://www.youtube.com/watch?v=K8m35kRA154

[^17]: https://pt.wikipedia.org/wiki/Lista_de_s%C3%ADmbolos_l%C3%B3gicos

[^18]: https://ctan.math.washington.edu/tex-archive/info/latex-via-exemplos/latex-via-exemplos.pdf

[^19]: https://petmecanica.ufes.br/sites/petengenhariamecanica.ufes.br/files/field/anexo/apostila_latex.pdf

[^20]: https://igce.rc.unesp.br/Home/Pos-Graduacao44/programasdepos/matematicaemredenacional/tutorial.pdf
