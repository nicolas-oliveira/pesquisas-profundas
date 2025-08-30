# Relatório de Cálculo de Cafeína em Colheres de Café

Este relatório apresenta os cálculos para determinar a massa de café e cafeína por colher, o número de colheres necessárias para obter 100 mg de cafeína e a quantidade ideal de água conforme a proporção Golden Ratio (1:16). Os cálculos são baseados em física básica, utilizando densidade do café moído e concentração de cafeína, com margens de erro de ±15% para densidade e cafeína, conforme especificado.

## 1. Densidade do Café Moído

A densidade do café moído foi adotada como \(0,38 \pm 0,06 \, $\text{g/ml}$), com base em estudos do *Journal of Food Engineering* (vol 78(4)), que reporta valores entre 0,25–0,45 g/ml para café filtrado. A variação de ±15% reflete fatores como compactação e granulometria.

**Fórmula**:

$$
\rho = \frac{m}{V}
$$

Onde:

- \($\rho$\): densidade (g/ml)
- \(m\): massa (g)
- \(V\): volume (ml)

O valor de 0,38 g/ml está dentro do intervalo reportado e foi validado por estudos que indicam densidade de café cru em torno de 0,43 g/cm³ e café torrado escuro em 0,28 g/cm³.

## 2. Conversão de Volume para Massa de Café

A massa de café por colher é calculada pela fórmula:

$$
m_{\text{café}} = V_{\text{colher}} \times \rho_{\text{café}} \times 1000
$$

Onde:

- \($m_{\text{café}}$): massa de café (mg)
- \($V_{\text{colher}}$\): volume da colher (ml)
- \($\rho_{\text{café}}$): densidade do café (0,38 g/ml)
- Fator 1000: converte gramas para miligramas

A margem de erro de ±15% é aplicada à massa calculada.

## 3. Cálculo de Cafeína por Colher

A concentração de cafeína é de 12 mg/g, conforme USDA FoodData Central (ID: 171890), valor consistente com estimativas de 10–15 mg/g para café moído. A fórmula é:

$$
m_{\text{cafeína}} = m_{\text{café}} \times 0,012
$$


Onde:

- \($m_{\text{cafeína}}$\): massa de cafeína (mg)
- \($m_{\text{café}}$\): massa de café (mg)
- 0,012: concentração de cafeína (12 mg/g)

A margem de erro de ±15% é aplicada ao resultado.

## 4. Número de Colheres para 100 mg de Cafeína

O número de colheres necessário para atingir 100 mg de cafeína é calculado por:

$$
n = \frac{100}{m_{\text{cafeína}}}
$$


Onde:

- \(n\): número de colheres
- \($m_{\text{cafeína}}$\): cafeína por colher (mg)

O resultado é arredondado para uma casa decimal para praticidade.

## 5. Quantidade Ideal de Água (Golden Ratio)

A proporção Golden Ratio (1:16) determina o volume de água:

$$
V_{\text{água}} = \frac{m_{\text{café}}}{1000} \times 16
$$


Onde:

- \($V_{\text{água}}$\): volume de água (ml)
- \($m_{\text{café}}$\): massa de café (mg)
- Fator 1000: converte mg para g
- 16: proporção Golden Ratio

## Cálculos Explícitos para Duas Cores

### Colher Vermelha (15 ml)

- **Massa de Café**:
  
  $$
  m_{\text{café}} = 15 \times 0,38 \times 1000 = 5700 \, \text{mg}
  $$
- Margem de erro:
  
  $$
  5700 \times 0,15 = 855 \, \text{mg} \quad \rightarrow \quad 5700 \pm 855 \, \text{mg}
  $$

- **Massa de Cafeína**:
  
  $$
  m_{\text{cafeína}} = 5700 \times 0,012 = 68,4 \, \text{mg}
  $$
  
  - Margem de erro:
    
    $$
    68,4 \times 0,15 = 10,26 \, \text{mg} \quad \rightarrow \quad 68,4 \pm 10,3 \, \text{mg}
    $$

- **Colheres para 100 mg de Cafeína**:
  
  $$
  n = \frac{100}{68,4} \approx 1,46 \quad \rightarrow \quad 1,5 \, \text{colheres}
  $$

- **Volume de Água**:
  
  $$
  V_{\text{água}} = \frac{5700}{1000} \times 16 = 5,7 \times 16 = 91,2 \, \text{ml}
  $$

### Colher Laranja (1,25 ml)

- **Massa de Café**:
  
  $$
  m_{\text{café}} = 1,25 \times 0,38 \times 1000 = 475 \, \text{mg}
  $$
  
  - Margem de erro:
    
    $$
    475 \times 0,15 = 71,25 \, \text{mg} \quad \rightarrow \quad 475 \pm 71 \, \text{mg}
    $$

- **Massa de Cafeína**:
  
  $$
  m_{\text{cafeína}} = 475 \times 0,012 = 5,7 \, \text{mg}
  $$
  
  - Margem de erro:
    
    $$
    5,7 \times 0,15 = 0,855 \, \text{mg} \quad \rightarrow \quad 5,7 \pm 0,9 \, \text{mg}
    $$

- **Colheres para 100 mg de Cafeína**:
  
  $$
  n = \frac{100}{5,7} \approx 17,54 \quad \rightarrow \quad 17,5 \, \text{colheres}
  $$

- **Volume de Água**:
  
  $$
  V_{\text{água}} = \frac{475}{1000} \times 16 = 0,475 \times 16 = 7,6 \, \text{ml}
  $$

## Tabela Final

| Cor      | Volume (ml) | Massa café (mg) | Cafeína (mg) | Colheres para 100 mg cafeína | Água (ml) |
| -------- | ----------- | --------------- | ------------ | ---------------------------- | --------- |
| Vermelho | 15          | 5700 ± 855      | 68,4 ± 10,3  | 1,5                          | 91,2      |
| Amarelo  | 7,5         | 2850 ± 428      | 34,2 ± 5,1   | 2,9                          | 45,6      |
| Azul     | 5           | 1900 ± 285      | 22,8 ± 3,4   | 4,4                          | 30,4      |
| Verde    | 2,5         | 950 ± 143       | 11,4 ± 1,7   | 8,8                          | 15,2      |
| Laranja  | 1,25        | 475 ± 71        | 5,7 ± 0,9    | 17,5                         | 7,6       |

## Resposta Direta

Para 100 mg de cafeína: use 1,5 colheres vermelhas ou 17,5 laranjas.

## Fontes

- **Densidade**: [Journal of Food Engineering, vol 78(4)](https://www.sciencedirect.com/journal/journal-of-food-engineering)
- **Cafeína**: [USDA FoodData Central ID: 171890](https://fdc.nal.usda.gov/)

## Notas

- Os cálculos foram validados com base nas fontes fornecidas e estão consistentes com os valores reportados.
- As margens de erro de ±15% foram aplicadas conforme instruído, refletindo variações em densidade e concentração de cafeína.
- A proporção Golden Ratio (1:16) foi utilizada para determinar o volume de água, garantindo consistência com práticas de preparo de café.