# Entendendo Conda e Pip

Conda e pip são frequentemente considerados quase idênticos.

Embora algumas das funcionalidades dessas duas ferramentas se sobreponham, elas foram projetadas e devem ser usadas para propósitos diferentes. [Pip](https://pip.pypa.io/en/stable/) é a ferramenta recomendada pela Python Packaging Authority para instalar pacotes do [Python Package Index](https://pypi.org/), PyPI. O Pip instala software Python empacotado como wheels ou distribuições de código-fonte. Esta última pode exigir que o sistema tenha compiladores compatíveis, e possivelmente bibliotecas, instalados antes que a invocação do pip seja bem-sucedida.

[Conda](https://conda.io/docs/) é um gerenciador de pacotes e ambientes multiplataforma que instala e gerencia pacotes Conda do [repositório Anaconda](https://repo.anaconda.com/), bem como da [Nuvem Anaconda](https://anaconda.org/). Os pacotes Conda são binários. Nunca é necessário ter compiladores disponíveis para instalá-los. Além disso, os pacotes do Conda não se limitam a softwares Python. Eles também podem conter bibliotecas C ou C++, pacotes R ou qualquer outro software.

Isso destaca uma diferença fundamental entre o Conda e o Pip. O Pip instala pacotes Python, enquanto o Conda instala pacotes que podem conter software escrito em qualquer linguagem. Por exemplo, antes de usar o Pip, um interpretador Python deve ser instalado por meio de um gerenciador de pacotes do sistema ou baixando e executando um instalador. O Conda, por outro lado, pode instalar pacotes Python, bem como o interpretador Python diretamente.

Outra diferença fundamental entre as duas ferramentas é que o Conda tem a capacidade de criar ambientes isolados que podem conter diferentes versões do Python e/ou os pacotes instalados neles. Isso pode ser extremamente útil ao trabalhar com ferramentas de ciência de dados, pois diferentes ferramentas podem conter requisitos conflitantes, o que pode impedir que todas sejam instaladas em um único ambiente. O Pip não possui suporte integrado para ambientes, mas depende de outras ferramentas como [virtualenv](https://virtualenv.pypa.io/en/latest/) ou [venv](https://docs.python.org/3/library/venv.html) para criar ambientes isolados. Ferramentas como [pipenv](https://pipenv.readthedocs.io/en/latest/), [poetry](https://poetry.eustace.io/) e [hatch](https://github.com/ofek/hatch) encapsulam o pip e o virtualenv para fornecer um método unificado para trabalhar com esses ambientes.

O Pip e o Conda também diferem na forma como as relações de dependência dentro de um ambiente são atendidas. Ao instalar pacotes, o pip instala dependências em um loop serial recursivo. Não há esforço para garantir que as dependências de todos os pacotes sejam atendidas simultaneamente. Isso pode levar a ambientes com falhas sutis, caso os pacotes instalados anteriormente na ordem tenham versões de dependências incompatíveis em relação aos pacotes instalados posteriormente. Em contraste, o Conda usa um solucionador de satisfatibilidade (SAT) para verificar se todos os requisitos de todos os pacotes instalados em um ambiente foram atendidos. Essa verificação pode levar mais tempo, mas ajuda a evitar a criação de ambientes com falhas. Desde que os metadados do pacote sobre dependências estejam corretos, o Conda produzirá previsivelmente ambientes funcionais.

Dadas as semelhanças entre o Conda e o Pip, não é surpreendente que alguns tentem combinar essas ferramentas para criar ambientes de ciência de dados. Um dos principais motivos para combinar o Pip com o Conda é quando um ou mais pacotes estão disponíveis para instalação apenas via Pip. Mais de 1.500 pacotes estão disponíveis no repositório Anaconda, incluindo os frameworks mais populares de ciência de dados, aprendizado de máquina e IA. Estes, juntamente com milhares de pacotes adicionais disponíveis na nuvem Anaconda a partir do canal, incluindo [conda-forge](https://conda-forge.org/) e [bioconda](https://bioconda.github.io/), podem ser instalados usando o Conda. Apesar dessa grande coleção de pacotes, ela ainda é pequena em comparação com os mais de 150.000 pacotes disponíveis no PyPI. Ocasionalmente, é necessário um pacote que não está disponível como um pacote Conda, mas está disponível no PyPI e pode ser instalado com o Pip. Nesses casos, faz sentido tentar usar o Conda e o Pip.

## Comparação entre conda e pip

|                              | conda                           | pip                            |
| ---------------------------- | ------------------------------- | ------------------------------ |
| gerencia                     | binários                        | wheel ou source                |
| pode exigir compiladores     | não                             | sim                            |
| tipos de pacotes             | qualquer                        | somente Python                 |
| cria ambiente                | sim, integrado                  | não, requer virtualenv ou venv |
| verificações de dependências | sim                             | não                            |
| fontes de pacotes            | repositório e nuvem do Anaconda | PyPI                           |
