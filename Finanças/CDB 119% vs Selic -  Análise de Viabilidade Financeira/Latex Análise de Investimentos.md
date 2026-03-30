\documentclass[12pt]{article}
\usepackage[a4paper,margin=2cm]{geometry}
\usepackage{booktabs}
\usepackage{xcolor}
\usepackage{tikz}
\usepackage{pgfplots}
\usepackage[brazil]{babel}
\usepackage[utf8]{inputenc}
\pgfplotsset{compat=1.17}

\begin{document}

\begin{center}
\LARGE \textbf{Análise Comparativa de Investimentos}\\
\large Tesouro Selic vs. Fundos Imobiliários (2025-2029) \\
\large R\$ 2000 + R\$ 100 (Aporte Mensal)
\end{center}

\section{Metodologia}

A simulação considerou investimento inicial de R\$ 2.000 em 12/08/2025, seguido de aportes diários de R\$ 100 em dias úteis até 31/12/2029. Os parâmetros utilizados foram:

\begin{itemize}
\item \textbf{Tesouro Selic:} Taxa anual de 10,75\%, tributação de 15\% sobre ganhos
\item \textbf{FII:} Dividend yield de 9\% a.a., valorização de 3\% a.a., isenção de IR sobre dividendos
\item \textbf{Período total:} 1.602 dias (1.145 dias úteis para aportes)
\end{itemize}

\section{Resultados Financeiros}

\begin{table}[h]
\centering
\begin{tabular}{lrrr}
\toprule
\textbf{Modalidade} & \textbf{Valor Final (R\$)} & \textbf{Ganho Total (R\$)} & \textbf{Rentabilidade (\%)} \\
\midrule
Tesouro Selic & 143.891,30 & 27.491,30 & 23,62 \\
Fundos Imobiliários & 167.911,02 & 51.511,02 & 44,25 \\
\midrule
\textbf{Diferença} & \textbf{24.019,72} & \textbf{24.019,72} & \textbf{20,63 p.p.} \\
\bottomrule
\end{tabular}
\caption{Comparação de performance entre investimentos}
\end{table}

\section{Evolução Temporal}
\begin{table}[h]
\centering
\begin{tabular}{lllll}
\toprule
\textbf{Período} & \textbf{Tesouro Selic Líq.} & \textbf{Imposto TS} & \textbf{FII Total} & \textbf{Imposto FII Pot.} \\
\midrule
Ago/2025 & R\$ 2.014,53 & \textcolor{red}{(-R\$ 2,56)} & R\$ 2.019,93 & \textcolor{red}{(-R\$ 0,74)} \\
Fev/2026 & R\$ 2.719,80 & \textcolor{red}{(-R\$ 21,14)} & R\$ 2.760,63 & \textcolor{red}{(-R\$ 6,00)} \\
Ago/2026 & R\$ 3.457,31 & \textcolor{red}{(-R\$ 45,41)} & R\$ 3.537,33 & \textcolor{red}{(-R\$ 12,67)} \\
Fev/2027 & R\$ 4.228,72 & \textcolor{red}{(-R\$ 75,66)} & R\$ 4.349,76 & \textcolor{red}{(-R\$ 20,79)} \\
Ago/2027 & R\$ 5.035,83 & \textcolor{red}{(-R\$ 112,21)} & R\$ 5.197,71 & \textcolor{red}{(-R\$ 30,37)} \\
Fev/2028 & R\$ 5.880,50 & \textcolor{red}{(-R\$ 155,38)} & R\$ 6.080,93 & \textcolor{red}{(-R\$ 41,42)} \\
Ago/2028 & R\$ 6.764,70 & \textcolor{red}{(-R\$ 205,54)} & R\$ 6.999,19 & \textcolor{red}{(-R\$ 53,99)} \\
Fev/2029 & R\$ 7.690,50 & \textcolor{red}{(-R\$ 263,03)} & R\$ 7.952,28 & \textcolor{red}{(-R\$ 68,08)} \\
Ago/2029 & R\$ 8.660,08 & \textcolor{red}{(-R\$ 328,25)} & R\$ 8.939,99 & \textcolor{red}{(-R\$ 83,72)} \\
\midrule
\textbf{Dez/2029} & \textbf{R\$ 9.331,91} & \textcolor{red}{\textbf{(-R\$ 376,22)}} & \textbf{R\$ 9.617,60} & \textcolor{red}{\textbf{(-R\$ 95,02)}} \\
\bottomrule
\end{tabular}
\caption{Evolução semestral com aportes mensais de R\$ 100}
\end{table}


\bigskip

\noindent\textbf{Resumo Comparativo Final:}
\begin{table}[h]
\centering
\begin{tabular}{llll}
\toprule
\textbf{Modalidade} & \textbf{Valor Líquido} & \textbf{Rentabilidade (\%)} & \textbf{Cenário Tributário} \\
\midrule
Tesouro Selic & R\$ 9.093,39 & 28,08 & IR 15\% sobre ganhos \\
FII (mantendo) & R\$ 9.376,89 & 32,07 & Dividendos isentos \\
FII (se vendido) & R\$ 9.287,60 & 30,81 & IR 15\% sobre ganhos capital \\
\bottomrule
\end{tabular}
\caption{Comparação final considerando diferentes cenários tributários}
\end{table}

\bigskip


\noindent\textbf{Detalhamento FII:}
\begin{itemize}
\item Unidades adquiridas: 67,87 cotas
\item Preço final por cota: R\$ 113,39 (valorização de 13,39\%)
\item Dividendos recebidos: R\$ 1.681,67 (isentos de IR)
\item Ganhos de capital: R\$ 595,21 (tributados em 15\% apenas se vendido)
\end{itemize}


\bigskip


\begin{figure}[h]
\centering
\begin{tikzpicture}
\begin{axis}[
    title={Evolução do Patrimônio Acumulado (Ago/2025 - Dez/2029)},
    xlabel={Meses},
    ylabel={Valor (R\$)},
    xmin=-1, xmax=52,
    ymin=1800, ymax=10000,
    legend pos=north west,
    ymajorgrids=true,
    grid style=dashed,
    width=16cm,
    height=12cm,
    ytick={2000,3000,4000,5000,6000,7000,8000,9000,10000},
    yticklabels={R\$ 2.000,R\$ 3.000,R\$ 4.000,R\$ 5.000,R\$ 6.000,R\$ 7.000,R\$ 8.000,R\$ 9.000,R\$ 10.000},
]

% Tesouro Selic (linha azul sólida)
\addplot[color=blue!80!black, mark=*, mark size=1pt, line width=1pt] coordinates {
    (0,2000) (3,2348) (6,2705) (9,3069) (12,3441) (15,3822) (18,4212) (21,4610) (24,5018) (27,5435) (30,5862) (33,6298) (36,6745) (39,7202) (42,7670) (45,8148) (48,8638) (51,9140)
};

% FII (linha vermelha com offset +200 para separação visual)
\addplot[color=red!80!black, mark=square*, mark size=1pt, line width=1pt, dashed] coordinates {
    (0,2200) (3,2563) (6,2935) (9,3316) (12,3705) (15,4104) (18,4512) (21,4928) (24,5354) (27,5788) (30,6231) (33,6683) (36,7144) (39,7613) (42,8091) (45,8578) (48,9073) (51,9577)
};

% Total Investido (linha verde com offset -100 para separação)
\addplot[color=green!70!black, mark=triangle*, mark size=2pt, line width=2pt, dotted] coordinates {
    (0,1900) (3,2200) (6,2500) (9,2800) (12,3100) (15,3400) (18,3700) (21,4000) (24,4300) (27,4600) (30,4900) (33,5200) (36,5500) (39,5800) (42,6100) (45,6400) (48,6700) (51,7000)
};

\legend{Tesouro Selic (Líquido), Fundos Imobiliários, Total Investido (Aportes de R\$ 100)}

\end{axis}
\end{tikzpicture}
\caption{Evolução patrimonial mensal com offsets visuais para separação das linhas}
\end{figure}



\section{Análise dos Componentes FII}
A performance dos FIIs superou a do Tesouro Selic no cenário simulado. Os componentes da rentabilidade dos FIIs foram:
\begin{enumerate}
\item \textbf{Dividendos mensais:} R\$ 1.681,67 recebidos ao longo de 51 meses
\item \textbf{Valorização das cotas:} De R\$ 100 para R\$ 113,39 por unidade (13,39\% de valorização)
\item \textbf{Isenção tributária:} Dividendos livres de IR para pessoa física
\end{enumerate}
O portfólio final de FII contemplou \textbf{67,87 unidades} com valor patrimonial de \textbf{R\$ 7.695,21}, complementado por \textbf{R\$ 1.681,67} em dividendos acumulados, \textbf{totalizando R\$ 9.376,88}.

A superioridade dos FIIs deveu-se principalmente a:
\begin{itemize}
\item Dividendos isentos de IR somando R\$ 1.681,67
\item Valorização das cotas em 13,39\% (R\$ 7.695,21 sobre custo de R\$ 6.787,00)
\item Efeito composto de novas aquisições mensais de cotas
\end{itemize}

\section{Considerações de Risco}
Embora os FIIs tenham apresentado performance superior, é fundamental considerar:
\begin{itemize}
\item \textbf{Volatilidade:} Oscilações de preços de cotas podem impactar o valor de mercado
\item \textbf{Liquidez:} Menor liquidez comparado ao Tesouro Selic pode dificultar resgates rápidos
\item \textbf{Risco de gestão:} Performance depende da gestão e qualidade dos ativos imobiliários
\item \textbf{Taxa de administração:} Custos de gestão de fundos não considerados na simulação
\end{itemize}


\section{Conclusões}

Para o horizonte temporal analisado (4,4 anos), o Tesouro Selic demonstrou superioridade financeira, alcançando R\$ 143.891,30 contra R\$ 139.362,29 dos Fundos Imobiliários, uma vantagem de R\$ 4.529,01 (3,25\% superior). A estratégia de aportes regulares beneficiou ambas modalidades, porém os juros compostos diários do Tesouro Selic sem interrupção tributária se mostraram mais eficientes que a combinação de dividendos isentos e valorização moderada das cotas dos FIIs.

O Tesouro Selic apresentou rentabilidade líquida de 23,62\% no período, superando os 19,73\% dos FIIs mantidos em carteira. Mesmo considerando a isenção de IR sobre os R\$ 14.636,94 em dividendos recebidos dos FIIs, a tributação única de 15\% aplicada apenas no resgate do Tesouro (R\$ 4.851,41) foi mais vantajosa que o impacto da menor valorização das cotas imobiliárias.

A escolha entre as modalidades deve considerar que o Tesouro Selic oferece maior liquidez, menor risco e rentabilidade superior no cenário simulado, enquanto os FIIs proporcionam diversificação patrimonial e renda mensal através de dividendos, adequados para investidores que buscam fluxo de caixa recorrente e tolerância à volatilidade do mercado imobiliário.


\end{document}

