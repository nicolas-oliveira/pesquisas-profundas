# Grok 1 - Apostila Didática: Cálculo da Massa de Café Moído com Colheres de Medição

## Objetivo

Esta apostila explica como calcular a massa de café moído (em mg) usando colheres de medição, integrando princípios de física básica e considerando fatores como compactação, umidade e granulometria. Também relaciona a massa de café com a quantidade ideal de água, conforme padrões da indústria.

## Princípios Físicos Envolvidos

A massa de café em uma colher depende da **densidade aparente** do café moído, definida como:

$$
\rho = \frac{m}{V}
$$

onde:

- $\rho$: densidade (g/cm³ ou g/ml),
- $m$: massa (g),
- $V$: volume (cm³ ou ml).

A densidade aparente inclui espaços de ar entre as partículas. Fatores que a afetam incluem:

- **Compactação**: Maior compactação reduz espaços de ar, aumentando a densidade.
- **Umidade**: Café úmido pode ter maior densidade.
- **Granulometria**: Moagem fina aumenta a densidade devido a menor espaço entre partículas.

A densidade típica do café moído torrado varia de 0,3 a 0,4 g/cm³, com média de 0,35 g/cm³, conforme estudos científicos.

## Cálculo da Massa de Café

A massa é calculada por:

$$
m = \rho \times V
$$

Para massa em miligramas:

$$
m_{\text{mg}} = \rho \times V \times 1000
$$

onde:

- $\rho$: densidade (g/cm³),
- $V$: volume da colher (ml = cm³),
- $m_{\text{mg}}$: massa (mg).

Usamos:

- Densidade média: $\rho = 0,35 \text{ g/cm}^3$,
- Densidade mínima: $\rho_{\text{min}} = 0,3 \text{ g/cm}^3$,
- Densidade máxima: $\rho_{\text{max}} = 0,4 \text{ g/cm}^3$.

## Volumes das Colheres

As colheres têm os seguintes volumes:

- Vermelho: 15 ml,
- Amarelo: 7,5 ml,
- Azul: 5 ml,
- Verde: 2,5 ml,
- Laranja: 1,25 ml.

## Relação com a Quantidade de Água

A *Specialty Coffee Association* (SCA) recomenda a proporção de 1:15 a 1:18 (massa de café para água). Usamos 1:18:

$$
V_{\text{água}} = m_{\text{café}} \times 18
$$

onde:

- $V_{\text{água}}$: volume de água (ml),
- $m_{\text{café}}$: massa do café (g).

## Cálculo da Massa de Café com Colheres de Medição

```vega-lite
{
  "$schema": "https://vega.github.io/schema/vega-lite/v5.json",
  "title": {
    "text": "Massa de Café e Água Recomendada por Colher",
    "fontSize": 16,
    "anchor": "start"
  },
  "data": {
    "values": [
      {"Colher": "Vermelho", "Tipo": "Massa Estimada (mg)", "Valor": 5250},
      {"Colher": "Amarelo", "Tipo": "Massa Estimada (mg)", "Valor": 2625},
      {"Colher": "Azul", "Tipo": "Massa Estimada (mg)", "Valor": 1750},
      {"Colher": "Verde", "Tipo": "Massa Estimada (mg)", "Valor": 875},
      {"Colher": "Laranja", "Tipo": "Massa Estimada (mg)", "Valor": 437.5}
    ]
  },
  "mark": {
    "type": "bar",
    "tooltip": true
  },
  "encoding": {
    "x": {
      "field": "Colher",
      "type": "nominal",
      "title": "Cor da Colher",
      "axis": {
        "labelAngle": 0
      }
    },
    "y": {
      "field": "Valor",
      "type": "quantitative",
      "title": "Valor"
    },
    "color": {
      "field": "Tipo",
      "type": "nominal",
      "title": "Medição",
      "scale": {
        "domain": ["Massa Estimada (mg)"],
        "range": ["#377e7f"]
      }
    },
    "xOffset": {
      "field": "Tipo",
      "type": "nominal"
    }
  },
  "width": 500,
  "height": 300
}
```

```vega-lite
{
  "$schema": "https://vega.github.io/schema/vega-lite/v5.json",
  "title": {
    "text": "Massa de Café e Água Recomendada por Colher",
    "fontSize": 16,
    "anchor": "start"
  },
  "data": {
    "values": [
      {"Colher": "Vermelho", "Tipo": "Água Recomendada (ml)", "Valor": 94.5},
      {"Colher": "Amarelo", "Tipo": "Água Recomendada (ml)", "Valor": 47.25},
      {"Colher": "Azul", "Tipo": "Água Recomendada (ml)", "Valor": 31.5},
      {"Colher": "Verde", "Tipo": "Água Recomendada (ml)", "Valor": 15.75},
      {"Colher": "Laranja", "Tipo": "Água Recomendada (ml)", "Valor": 7.875}
    ]
  },
  "mark": {
    "type": "bar",
    "tooltip": true
  },
  "encoding": {
    "x": {
      "field": "Colher",
      "type": "nominal",
      "title": "Cor da Colher",
      "axis": {
        "labelAngle": 0
      }
    },
    "y": {
      "field": "Valor",
      "type": "quantitative",
      "title": "Valor"
    },
    "color": {
      "field": "Tipo",
      "type": "nominal",
      "title": "Medição",
      "scale": {
        "domain": ["Água Recomendada (ml)"],
        "range": ["#369ee5"]
      }
    },
        "xOffset": {
      "field": "Tipo",
      "type": "nominal"
    }
  },
  "width": 500,
  "height": 300
}
```

#### Pontos-Chave:

- A densidade do café moído torrado varia de 0,3 a 0,4 g/cm³, com média de 0,35 g/cm³.
- Massa (mg) = volume (ml) × densidade (g/ml) × 1000.
- Proporção ideal de água: 1:18 (1 g de café para 18 ml de água).
- Variações na compactação podem alterar a massa em até ±14%.

## Princípios Físicos

Densidade (ρ) é a massa por unidade de volume (ρ=m/V). Para café moído, usamos densidade aparente, que inclui espaços de ar. Fatores como compactação, umidade e granulometria afetam a densidade.

## Cálculos

Usando densidade média de 0,35 g/cm³, calculamos a massa para colheres de 15 ml (vermelho), 7,5 ml (amarelo), 5 ml (azul), 2,5 ml (verde) e 1,25 ml (laranja). A água recomendada segue a proporção 1:18.

## Resultados

A tabela a seguir apresenta os cálculos para cada colher, com margens de erro devido à variação de densidade:

| Cor      | Volume (ml) | Massa Mínima (mg) | Massa Estimada (mg) | Massa Máxima (mg) | Água Recomendada (ml) |
| -------- | ----------- | ----------------- | ------------------- | ----------------- | --------------------- |
| Vermelho | 15          | 4500              | 5250                | 6000              | 94,5                  |
| Amarelo  | 7,5         | 2250              | 2625                | 3000              | 47,25                 |
| Azul     | 5           | 1500              | 1750                | 2000              | 31,5                  |
| Verde    | 2,5         | 750               | 875                 | 1000              | 15,75                 |
| Laranja  | 1,25        | 375               | 437,5               | 500               | 7,875                 |

*Tabela: Massa de café e água recomendada por colher.*

## Margens de Erro

A variação na densidade (0,3 a 0,4 g/cm³) implica uma margem de erro de aproximadamente ±14% na massa estimada. Compactação excessiva ou moagem diferente pode alterar os resultados.

## Fontes

- *SciELO Brazil*: "Physical characterization of Arabica ground coffee with different roasting degrees" (https://www.scielo.br/j/aabc/a/F44XMrhpdQtMbjCySbg3pBM/?lang=en).
- *ResearchGate*: "Preservation of roasted and ground coffee during storage. Part 2: Bulk density and intergranular porosity" (https://www.researchgate.net/publication/304145181).
- *Specialty Coffee Association*: Padrões para proporção café/água (https://sca.coffee/research/coffee-standards).
- Halliday, D., Resnick, R.: *Fundamentals of Physics*.

## Conclusão

Esta apostila fornece uma abordagem clara para calcular a massa de café moído usando colheres de medição, com base em princípios físicos e dados científicos. Ajustes podem ser feitos conforme o gosto pessoal.
