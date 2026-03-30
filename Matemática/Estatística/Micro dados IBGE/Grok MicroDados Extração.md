# Micro dados IBGE: Extração dos dados

- **Baixe os dados**: Use `urllib.request` para baixar arquivos ZIP de 2023 do IBGE FTP.
- **Extraia os arquivos**: Use zipfile para extrair os arquivos de texto de largura fixa.
- **Leia o dicionário**: Consulte o dicionário de dados para posições de idade (`V2009`), renda (e.g., `V403312`), situação de trabalho (`VD4001`).
- **Processe os dados**: Leia o arquivo linha por linha, filtre homens de 27 anos, calcule taxa de desemprego e salário médio.

## Como Começar a Pesquisa

Para começar a análise dos microdados da PNADC em Python sem bibliotecas especializadas, siga estes passos:

#### 1. Baixar os Arquivos

Os microdados da PNADC estão disponíveis no [FTP do IBGE](https://ftp.ibge.gov.br/Trabalho_e_Rendimento/Pesquisa_Nacional_por_Amostra_de_Domicilios_continua/Anual/Microdados/Trimestre/). Para 2023, baixe os arquivos ZIP de cada trimestre (Trimestre_1 a Trimestre_4). Exemplo para o primeiro trimestre:

```python
import urllib.request

url = "https://ftp.ibge.gov.br/Trabalho_e_Rendimento/Pesquisa_Nacional_por_Amostra_de_Domicilios_continua/Anual/Microdados/Trimestre/Trimestre_4/Dados/PNADC_2023_trimestre4_20240816.zip"
local_file = "PNADC_2023_trimestre1.zip"
urllib.request.urlretrieve(url, local_file)
```

Repita para os outros trimestres, ajustando a URL.

## 2. Extrair os Arquivos

Use o módulo zipfile para extrair os arquivos de dados, que são em formato de texto com largura fixa:

```python
import zipfile

with zipfile.ZipFile("PNADC_2023_trimestre1.zip", "r") as zip_ref:
    zip_ref.extractall("extracted_data")
```

O arquivo extraído será algo como `PNADC_2023_trimestre1.txt`.

## 3. Consultar o Dicionário de Dados

O dicionário de dados, como dicionário [PNADC 2023](https://ftp.ibge.gov.br/Trabalho_e_Rendimento/Pesquisa_Nacional_por_Amostra_de_Domicilios_continua/Anual/Microdados/Trimestre/Trimestre_1/Documentacao/dicionario_PNADC_microdados_trimestre1_20240527.xls), detalha as posições e significados das variáveis. As variáveis relevantes são:

- **Sexo**: Provavelmente `V0080` (0 = masculino, 1 = feminino), posição aproximada 80-80.
- **Idade**: `V2009`, posição 103-104 (2 caracteres).
- **Renda**: `V403312` (renda em dinheiro do trabalho principal), posição 189-198 (10 caracteres).
- **Situação de Trabalho**: `VD4001` (1 = Ocupado, 2 = Desocupado, 3 = Inativo), posição 512-512.
