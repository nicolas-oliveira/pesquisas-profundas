# ibge-parser: um pacote em Python para leitura de microdados de censo do IBGE

Os dados do IBGE são de difícil acesso àqueles que não possuem expertisena área de tecnologia, principalmente na área estatística. Com anecessidade do uso deles, surgiu a ideia de tornar mais fácil o seuacesso. A execução do projeto aconteceu durante participação da [Senior Sistemas](https://www.senior.com.br/) no [Hacktoberfest](https://hacktoberfest.digitalocean.com/) e se tornou uma ferramenta para leitura de microdados de censo do IBGE.O projeto focou em desenvolver um pacote em Python, linguagem popular na comunidade *open source*. Assim, no dia 23 de outubro de 2020, o time de Pesquisa Aplicada, além de outras contribuições em projetos *open source,* elaborou o [ibge-parser](https://github.com/SeniorSA/ibge-parser), uma solução de código aberto para que a comunidade também possa contribuir.

# **Censo do IBGE**

O **censo** ou recenseamento **demográfico**, é um estudo estatístico referente a uma população que possibilita o  ecolhimento de várias informações, tais como o número de homens,  ulheres, crianças e idosos, onde e como vivem as pessoas. No Brasil,  sse estudo é realizado, normalmente, de dez em dez anos pelo Instituto  rasileiro de Geografia Estatística, [IBGE](https://www.ibge.gov.br/estatisticas/sociais/populacao/9662-censo-demografico-2000.html?=&t=microdados).

As informações do censo do IBGE são coletadas por entrevistas e estes dados são compilados e podem ser acessados pelo público a partir de microdados no próprio site do IBGE.

# O que são microdados?

O IBGE define **microdados** como o menor nível de desagregação dos dados de uma pesquisa, retratando, sob a forma de códigos numéricos, o conteúdo dos questionários, preservado o sigilo estatístico com vistas à não individualização das informações.

Os microdados não são de simples entendimento e acesso ao público em geral. Somente usuários especializados com conhecimento em programação tem acesso a leitura destes dados para fazer cruzamentos em diferentes agregações geográficas e elaborar múltiplas tabulações segundo sua perspectiva pessoal de interesse. Para programadores experientes, muitos são os pacotes e facilidades de leitura destes dados em softwares estatísticos como R, SAS ou STATA.

No entanto, para Python, linguagem popular no mundo *open source*, não havia um pacote para trabalhar o processamento destes dados. Sendo assim, pela necessidade de uso, pela proximidade com a linguagem Python e para democratização da informação, foi desenvolvido um pacote para processamento dos dados do censo, inicialmente do ano 2010, que transforma os dados de formato de largura fixa para o formato colunar.

# Como os dados são disponibilizados hoje?

Os microdados podem ser encontrados no site do [IBGE](https://www.ibge.gov.br/) na seção de Censo Demográfico seguindo o caminho:

*Home> Estatísticas> Sociais> População> Censo Demográfico> Microdados> Censo*

A documentação para interpretação destes dados fica disponível, bem como o censo realizado, para cada estado brasileiro. Ao escolher um estado, é feito o *download* automático de um arquivo compactado que contém diversos arquivos de largura fixa. Arquivos no formato de largura fixa são aqueles onde as colunas são definidas pela posição dos caracteres nos arquivos. Estes foram divididos nas amostras de pesquisa de modalidades sobre: domicílios, emigração, mortalidade e pessoas.

Cada arquivo pode ser aberto como um documento de texto comum e os registros estão organizados conforme a Figura 1.

<img title="" src="https://miro.medium.com/v2/resize:fit:808/1*bP1iN5uljYFRYYZF3SXWgw.png" alt="" width="698" data-align="center">

Figura 1 — Arquivo de largura fixa para o censo 2010 de amostra de pessoas para o estado de São Paulo.

Cada linha representa um questionário realizado pelo recenseamento. Para interpretação destes registros de comprimento fixo e extração das informações, a documentação disponibiliza as variáveis e suas posições no arquivo para cada variável colunar. A estrutura varia com as modalidades dos arquivos. O arquivo explicando o layout dos arquivos de largura fixa é encontrado em *Documentação> Layout> Layout_microdados_Amostra.xls* e é ilustrado na Figura 2.

| VAR   | NOME                                                                                                                                                                                              | POSIÇÃO INICIAL | POSIÇÃO FINAL | INT | DEC | TIPO |
| ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------- | ------------- | --- | --- | ---- |
| V0001 | UNIDADE DA FEDERAÇÃO:<br>11- Rondônia<br>12- Acre<br>13- Amazonas                                                                                                                                 | 1               | 2             | 2   |     | A    |
| V0002 | CÓDIGO DO MUNICÍPIO                                                                                                                                                                               | 3               | 7             | 5   |     | A    |
| V0011 | ÁREA DE PONDERAÇÃO                                                                                                                                                                                | 8               | 20            | 13  |     | A    |
| V0300 | CONTROLE                                                                                                                                                                                          | 21              | 28            | 8   |     | N    |
| V0010 | PESO AMOSTRAL                                                                                                                                                                                     | 29              | 44            | 3   | 13  | N    |
| V1001 | REGIÃO GEOGRÁFICA:<br>1- Região norte (uf=11 a 17)<br>2- Região nordeste (uf=21 a 29)<br>3- Região sudeste (uf=31 a 33 e 35)<br>4- Região sul (uf=41 a 43)<br>5- Região centro-oeste (uf=50 a 53) | 45              | 45            | 1   |     | C    |
| V1002 | CÓDIGO DA MESORREGIÃO:<br>A relação de códigos encontra-se no arquivo:                                                                                                                            | 46              | 47            | 2   |     | A    |
| V1003 | CÓDIGO DA MICRORREGIÃO:<br>A relação de códigos encontra-se no arquivo:                                                                                                                           | 48              | 50            | 3   |     | A    |
| V1004 | CÓDIGO DA REGIÃO METROPOLITANA:<br>A relação de códigos encontra-se no arquivo:                                                                                                                   | 51              | 52            | 2   |     | A    |

| Variável                   | Posição Inicial | Posição Final | Caracteres | Exemplo         |
| -------------------------- | --------------- | ------------- | ---------- | --------------- |
| **Código do Município**    | 1               | 7             | 7          | 3503901         |
| **Área de Ponderação**     | 8               | 21            | 14         | 35039010030001  |
| **Região Geográfica**      | 22              | 27            | 6          | 126600          |
| **Unidade da Federação**   | 28              | 42            | 15         | 095463703315139 |
| **Código da Microrregião** | 43              | 49            | 7          | 1847180         |
| **Peso Controle**          | 50              | 58            | 9          | 095474980       |
| **Código da Mesorregião**  | 59              | 65            | 7          | 1847180         |
| **Peso Amostral**          | 66              | 74            | 9          | 095474980       |

Figura 2 — Layout referente aos microdados da modalidade Pessoas contendo nome
 e posições de caracteres de cada variável colunar.

### Exemplo Prático de Decomposição

**Registro completo:** `35039013503901003001000126600095463703315139`

**Breakdown por posição:**

- **Município:** `3503901` (pos. 1-7)
- **Área Ponderação:** `35039010030001` (pos. 8-21)
- **Região:** `000126600` (pos. 22-27)
- **UF:** `095463703315139` (pos. 28-42)

Para interpretar as variáveis dos arquivos de largura fixa, temos que sempre contar o número de caracteres a partir da posição inicial daquele campo. Como é o caso da variável referente a unidade da federação, que começa na posição 1 e termina na posição 2, contendo 2 caracteres e segue pelo código do município, que começa na posição 3 e termina na 7, pois contém 5 caracteres. Todas as variáveis seguem o raciocínio análogo. A Figura 3 mostra essa separação de dígitos por variáveis trazendo as informações da documentação para o arquivo do censo de cada estado.

Figura 3 — Arquivo de largura fixa do censo do IBGE com separação de caracteres pelos nomes das variáveis correspondentes.

Separando as colunas pelas posições de caracteres encontradas no arquivo de documentação, os dados semiestruturados do arquivo de texto podem ser transformados em dados estruturados no formato tabular com as colunas contendo o ramal do nome e descrição das variáveis. A Figura 3 representa os dados já tratados e convertidos para um formato tabular, usando o ramal como cabeçalho.

| Index | V0001 | V0002 | V0011          | V0300 | V0010           | V1001 | V1002 | V1003 | V1004 | V1006 |
| ----- | ----- | ----- | -------------- | ----- | --------------- | ----- | ----- | ----- | ----- | ----- |
| 0     | 35    | 3901  | 35039010030001 | 12560 | 95463703315139  | 3     | 15    | 59    | 20    | 1     |
| 1     | 35    | 3901  | 35039010030001 | 48621 | 117278437503348 | 3     | 15    | 59    | 20    | 1     |
| 2     | 35    | 3901  | 35039010030001 | 48621 | 117278437503348 | 3     | 15    | 59    | 20    | 1     |
| 3     | 35    | 3901  | 35039010030001 | 48621 | 117278437503348 | 3     | 15    | 59    | 20    | 1     |
| 4     | 35    | 3901  | 35039010030001 | 48621 | 117278437503348 | 3     | 15    | 59    | 20    | 1     |
| 5     | 35    | 3901  | 35039010030001 | 48621 | 117278437503348 | 3     | 15    | 59    | 20    | 1     |
| 6     | 35    | 3901  | 35039010030001 | 48621 | 117278437503348 | 3     | 15    | 59    | 20    | 1     |
| 7     | 35    | 3901  | 35039010030001 | 48621 | 117278437503348 | 3     | 15    | 59    | 20    | 1     |
| 8     | 35    | 3901  | 35039010030001 | 50373 | 103074481487026 | 3     | 15    | 59    | 20    | 1     |
| 9     | 35    | 3901  | 35039010030001 | 50373 | 103074481487026 | 3     | 15    | 59    | 20    | 1     |
| 10    | 35    | 3901  | 35039010030001 | 50373 | 103074481487026 | 3     | 15    | 59    | 20    | 1     |

Figura 3 — Arquivo do censo do IBGE convertido para formato tabular.

# O pacote ibge-parser

Para facilitar a utilização do pacote, foram criados *enumeradores* para o usuário selecionar qual ou quais estados, modalidades e o ano 
que ele quer buscar o censo. Assim, o método para transformar os dados é
 invocado e apenas as informações desejadas são compiladas, conforme o 
código no Quadro 1.

```python
from ibgeparser.microdados import Microdados
from ibgeparser.enums import Anos, Estados, Modalidades

if __name__ == "__main__":
    # utiliza os enums para selecionar os dados desejados
    ano = Anos.DEZ
    estados = [Estados.SAO_PAULO]
    modalidades = [Modalidades.PESSOAS]

    # instancia a classe
    ibgeparser = Microdados()
    # obter os dados
    ibgeparser.obter_dados_ibge(ano, estados, modalidades)
```

Agora falando a nível de código do projeto em si, foi feito um trabalho simples. O código parte do download da documentação e dos arquivos de dados de cada modalidade e estado selecionado. Com os arquivos em mãos, inicia o trabalho de conversão dos microdados para uma estrutura tabular utilizando recursos do [Pandas](https://pandas.pydata.org/), onde é feita a identificação do ramal de cada coluna usando a documentação da modalidade e atribuindo os valores para cada coluna separadamente.

Para identificar a descrição de cada ramal, que é o código visto no *header* de cada coluna na Figura 3. Foi criado um método em que o usuário pode buscar pela descrição desejada, onde é feita a varredura pela documentação das modalidades selecionadas para obter mais detalhes da coluna. A utilização pode ser feita conforme o código no Quadro 2.

```python
from ibgeparser.microdados import Microdados
from ibgeparser.enums import Modalidades

if __name__ == "__main__":
    # usando os enums
    modalidades = [Modalidades.EMIGRACAO]

    # instanciando a classe
    ibgeparser = Microdados()
    # especificação de coluna
    ibgeparser.obter_especificacao_coluna('palavra-chave', modalidades)
```

Como o processo de *download* pode se tornar demorado dependendo dos estados selecionados, também foi adicionado um log simplificado para que o usuário acompanhe o andamento, assim não há impressão de processo travado ou coisa parecida. Para os desenvolvedores, também foi adicionado um log em modo *debug*, que traz informações detalhadas do andamento do processo. No Quadro 3, está um exemplo de como é o log simplificado da aplicação.

```bash
[I 201026 15:48:34 log:21] Obtendo as informações da documentação sobre Pessoas
[I 201026 15:48:34 log:21] Baixando informações do estado de Rondônia
[I 201026 15:49:58 log:21] Arquivo de RO de modalidade Pessoas extraído
[I 201026 15:49:58 log:21] Baixando informações do estado de Acre
[I 201026 15:50:41 log:21] Arquivo de AC de modalidade Pessoas extraído
[I 201026 15:50:41 log:21] Baixando informações do estado de Amazonas
[I 201026 15:52:49 log:21] Arquivo de AM de modalidade Pessoas extraído
[I 201026 15:52:49 log:21] Baixando informações do estado de Roraima
[I 201026 15:53:15 log:21] Arquivo de RR de modalidade Pessoas extraído
```

Como saída desse pacote, são gerados os arquivos csv de cada estado e modalidade selecionada. Esses arquivos serão salvos — partindo da raiz do projeto que importou o pacote — em uma pasta chamada “microdados-ibge” e a nomenclatura dos arquivos fica conforme a Figura 4.

![](https://miro.medium.com/v2/resize:fit:725/1*HW5C6T5mMgBAqh6SOTP7UQ.png)

Figura 4 — Exemplo de arquivos salvos pelo pacote.

Os arquivos originais do IBGE, que são adquiridos no site, são salvos em uma pasta temporária do sistema operacional utilizado e excluídos assim que o processo é finalizado, com ou sem erro de execução.

# Conclusão

A grande contribuição do pacote em Python, para leitura dos microdados do IBGE, foi tornar esses dados mais acessíveis. Aqueles que desejam analisar e cruzar os dados do recenseamento agora possuem, de forma fácil, um suporte para isso. Desta maneira, o pacote ibge-parser busca contribuir com a comunidade, disponibilizando uma ferramenta para a leitura dos dados, onde é possível escolher o estado e a modalidade para análise.

O pacote, então, pode ser útil para aqueles que precisam analisar a situação da população brasileira em diferentes modalidades disponíveis, como domicílios, emigração, mortalidade e pessoas. Esse tipo de consulta é comum para os profissionais de economia, jornalismo e estatística, que a princípio não possuem capacitação na área de tecnologia.

Aos profissionais de tecnologia, existe ainda espaço para melhorias e novas funcionalidades neste pacote, que podem melhorar ainda mais a experiência e a usabilidade. Sem contar que é possível que o IBGE disponibilize os dados no novo censo o qual ocorre normalmente a cada 10 anos. Se houverem mudanças na forma em que os dados são catalogados, haverá a necessidade de adicionar este novo modelo ao pacote.
