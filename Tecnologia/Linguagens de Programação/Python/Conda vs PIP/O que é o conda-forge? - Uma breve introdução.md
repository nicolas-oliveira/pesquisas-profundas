# O que é o conda-forge? - Uma breve introdução

O conda-forge é um esforço comunitário que fornece pacotes conda para uma ampla gama de softwares. Sentiu falta de um pacote que gostaria de instalar com o conda? - É provável que já o tenhamos empacotado para você! Você pode procurar por pacotes online. Procure por pacotes fornecidos pela nossa organização conda-forge.

Não consegue encontrar um pacote ou apenas versões desatualizadas de um pacote? - Todos são bem-vindos para contribuir com nossa pilha de pacotes! Consulte a seção "Envolvendo-se" para obter uma visão geral sobre como começar a contribuir.

# Por que o conda-forge?

A equipe de empacotamento da Anaconda, Inc. fornece uma infinidade de pacotes em seu canal padrão.

Mas e se um pacote que você está procurando não estiver no canal padrão? Antigamente, os usuários só tinham a opção de criar uma conta no Anaconda Cloud e criar seu próprio canal.

Isso trazia uma série de desvantagens:

- Localizar pacotes era difícil devido à dispersão deles em vários canais. - A combinação de pacotes entre canais nem sempre era possível devido a incompatibilidades binárias.
- Os pacotes estavam disponíveis apenas para arquiteturas nas quais o desenvolvedor tinha interesse ou acesso.
- Os canais eram frequentemente abandonados, e a atualização exigia a localização de novos canais.

O conda-forge é um esforço da comunidade que aborda os seguintes problemas:

- Todos os pacotes são compartilhados em um único canal chamado conda-forge.
- Garantimos que todos os pacotes estejam atualizados.
- Padrões comuns garantem que todos os pacotes tenham versões compatíveis.
- Por padrão, criamos pacotes para macOS, Linux AMD64 e Windows AMD64. Outras arquiteturas também estão disponíveis mediante solicitação (por exemplo, Apple Silicon, PowerPC, Linux ARM).
- Muitos pacotes são atualizados por vários mantenedores, com a opção fácil de se tornar um mantenedor.
- Uma equipe de desenvolvedores principal ativa também está tentando manter pacotes abandonados.

Você pode consultar o Glossário.

# Como posso instalar pacotes do conda-forge?

Usar o conda-forge é fácil!

- Certifique-se de que o `conda >=4.9`:
  
  ```bash
  conda --version
  conda update conda
  ```

- Adicione o conda-forge como o canal de maior prioridade:
  
  ```bash
  conda config --add channels conda-forge
  ```

- Ative a prioridade estrita do canal (a estrita será ativada por padrão no conda 5.0):
  
  ```bash
  conda config --set channel_priority strict
  ```
  
  A partir de agora, usar o conda `install <nome-do-pacote>` também encontrará pacotes em nossos canais do conda-forge.

> **Importante**: Além da prioridade do canal, recomendamos sempre instalar seus pacotes dentro de um novo ambiente em vez do ambiente base (anteriormente conhecido como `root`), e também recomendamos o uso do miniforge em vez da distribuição Anaconda. O uso de ambientes facilita a depuração de problemas com pacotes e garante a estabilidade do seu ambiente base. Evitar a distribuição Anaconda reduz as chances de instalações insolúveis/conflitantes, além de reduzir o download.

> **Observação**: Esteja ciente de que a ordem dos seus canais de pacotes do Conda é importante, especialmente ao combinar o Conda-Forge com outros canais, como o Bioconda.

> **Observação**: O Miniforge é um esforço da comunidade para fornecer instaladores semelhantes ao Miniconda, com o recurso adicional de que o Conda-Forge é o canal padrão. O Miniforge é a maneira mais fácil de começar a usar o Conda-Forge! Consulte "Usando múltiplos canais" para obter mais informações e dicas.

# Dicas e truques - Usando múltiplos canais

É bastante comum instalar um pacote do conda-forge e, ao tentar usá-lo, receber um erro como (exemplo do OS X):

```
ImportError: dlopen(.../site-packages/rpy2/rinterface/_rinterface.so, 2): Biblioteca não carregada: @rpath/libicuuc.54.dylib
Referenciado em: .../site-packages/rpy2/rinterface/_rinterface.so
Motivo: imagem não encontrada
```

Isso acontece porque a versão correta do `icu`, ou de qualquer outro pacote no erro, não está presente ou o pacote está completamente ausente.

Você pode confirmar isso executando o comando conda list e procurando pelo pacote em questão.

## Por que isso acontece?

O conda-forge e os padrões não são 100% compatíveis. No exemplo acima, sabe-se que os padrões usam o icu 54.*, enquanto o conda-forge depende do icu 56.*. Essa incompatibilidade pode levar a erros quando o ambiente de instalação mistura pacotes de vários canais.

> **Observação**: Todos os recursos de pinagem de software do conda-forge podem ser encontrados em: https://github.com/conda-forge/conda-forge-pinning-feedstock/blob/master/recipe/conda_build_config.yaml

## Como corrigir?

Versões mais recentes do conda (>=4.6) introduziram um recurso de prioridade de canal estrita. Digite `conda config --describe channel_priority` para mais informações.

A solução é adicionar o canal conda-forge sobre os padrões no seu arquivo `.condarc` ao usar pacotes conda-forge e ativar a prioridade estrita do canal com:

```
$ conda config --set channel_priority strict
```

Isso garantirá que todas as dependências venham do canal conda-forge, a menos que existam apenas nos padrões.

Veja como um arquivo .condarc ficaria:

```bash
cat .condarc
```

Resulta em:

```bash
channel_priority: strict
channels:
- conda-forge
- defaults
```

Além da prioridade do canal, recomendamos sempre instalar seus pacotes dentro de um novo ambiente em vez do ambiente base do anaconda/miniconda. Usar envs facilita a depuração de problemas com pacotes e garante a estabilidade do seu ambiente raiz.

# Usando Bibliotecas de Interface Externa de Passagem de Mensagens (MPI)

Em alguns sistemas de computação de alto desempenho (HPC), espera-se que os usuários utilizem os binários MPI disponíveis no sistema, em vez daqueles criados pelo conda-forge. Esses binários são normalmente especializados para o sistema e interagem adequadamente com agendadores de tarefas, etc. No entanto, essa prática cria problemas para os usuários do conda-forge. Ao instalar um pacote do conda-forge que depende de MPI, o conda instalará os binários MPI criados pelo conda-forge e o pacote será vinculado a esses binários. Essa configuração geralmente não funciona ou funciona de maneiras inesperadas em sistemas HPC.

Para resolver esses problemas, o conda-forge criou compilações fictícias especiais das bibliotecas `mpich` e `openmpi`, que são simplesmente pacotes shell sem conteúdo. Esses pacotes permitem que o solucionador do conda produza ambientes corretos, evitando a instalação de binários MPI do conda-forge. Você pode instalar o pacote fictício com o seguinte comando:

```bash
conda install "mpich=x.y.z=external_*"
conda install "openmpi=x.y.z=external_*"
```

Contanto que você tenha as cópias locais da biblioteca `mpich`/`openmpi` em seus caminhos de vinculação e a versão local corresponda à versão do Conda dentro do intervalo ABI correto, este procedimento deverá funcionar. Em tempo de execução, o pacote conda-forge que depende do MPI deverá encontrar a cópia local de `mpich`/`openmpi` e vincular a ela.

Outro ponto a favor do uso de seus próprios binários MPI especializados para o sistema é que, se você se preocupa com o desempenho máximo, deve construir/instalar seu backend MPI você mesmo, e não depender dos pacotes conda-forge (eles são construídos para compatibilidade e não para desempenho). Devido ao ambiente de compilação limitado dos pacotes conda-forge, pode haver a falta de recursos importantes como XPMEM e CMA para `mpich` e `openmpi`, respectivamente.

# Instalando pacotes Apple Intel no Apple Silicon

Usando o Rosetta 2, você pode instalar pacotes compilados originalmente para computadores Mac com processadores Intel em computadores Mac com processadores Apple Silicon.

Isso pode ser habilitado por ambiente usando os seguintes comandos:

```bash
CONDA_SUBDIR=osx-64 conda create -n your_environment_name python # Cria um novo ambiente chamado your_environment_name com pacotes intel.
conda activate your_environment_name
python -c "import platform;print(platform.machine())" # Confirma se os valores corretos estão sendo usados.
conda config --env --set subdir osx-64 # Garante que os comandos conda neste ambiente usem pacotes intel.
```

Para verificar se a plataforma correta está sendo usada, execute os seguintes comandos após a ativação do ambiente:

```
python -c "import platform;print(platform.machine())" # Deve imprimir "x86_64"
echo "CONDA_SUBDIR: $CONDA_SUBDIR" # Deve imprimir "CONDA_SUBDIR: osx-64"
```

# Instalando pacotes habilitados para CUDA, como TensorFlow e PyTorch

No conda-forge, alguns pacotes estão disponíveis com suporte a GPU. Esses pacotes não só levam significativamente mais tempo para serem compilados e construídos, como também resultam em binários bastante grandes que os usuários baixam. Como um esforço para maximizar a acessibilidade para usuários com menor largura de banda de conexão e/ou armazenamento, há um esforço contínuo para limitar a instalação desnecessária de pacotes compilados para GPUs em máquinas somente com CPU por padrão. Isso é feito adicionando uma dependência de execução de pacote virtual, __cuda, que detecta se a máquina local possui uma GPU. No entanto, isso apresenta desafios para usuários que podem preferir baixar e usar pacotes habilitados para GPU, mesmo em uma máquina sem GPU. Por exemplo, nós de login em HPCs geralmente não têm GPUs e suas contrapartes de computação com GPUs geralmente não têm acesso à internet. Nesse caso, um usuário pode substituir a configuração padrão por meio da variável de ambiente CONDA_OVERRIDE_CUDA para instalar pacotes de GPU no nó de login para serem usados ​​posteriormente no nó de computação. No momento da redação deste texto (fevereiro de 2022), concluímos que este comportamento padrão seguro é o melhor para a maioria dos usuários do conda-forge, com uma opção de substituição fácil disponível e documentada. Informe-nos se tiver alguma dúvida ou problema com isso.

Para substituir o comportamento padrão, o usuário pode definir a variável de ambiente CONDA_OVERRIDE_CUDA, como abaixo, para instalar o TensorFlow com suporte a GPU, mesmo em uma máquina com apenas CPU.

```bash
CONDA_OVERRIDE_CUDA="<versão CUDA>" conda install tensorflow -c conda-forge
# OU
CONDA_OVERRIDE_CUDA="<versão CUDA>" mamba install tensorflow -c conda-forge
```

> **Observação**: Consulte os guias do usuário CUDA relevantes para obter mais informações.

Para contextualizar, a instalação da variante do TensorFlow 2.7.0 com suporte a CUDA, `tensorflow=2.7.0=cuda*`, resulta em aproximadamente 2 GB de pacotes para download, enquanto a variante com CPU, tensorflow=2.7.0=cpu*, resulta em aproximadamente 200 MB para download. Isso representa um desperdício significativo de largura de banda e armazenamento se for necessária apenas a variante com CPU!

## Usando PyPy como interpretador

O canal conda-forge suporta a criação e instalação de pacotes em ambientes usando o interpretador PyPy. Muitos pacotes já estão disponíveis. Você precisa habilitar o canal conda-forge e usar o identificador pypy ao criar seu ambiente:

```bash
conda create -c conda-forge -n my-pypy-env pypy python=3.8
conda activate my-pypy-env
```

As versões do Python atualmente suportadas são 3.8 e 3.9. O suporte para pypy3.7 foi descontinuado. Embora você ainda possa criar um ambiente Python 3.7, não receberá atualizações conforme novas versões de pacotes forem lançadas (incluindo o próprio Pypy).

> **Observação**: A partir de 8 de março de 2020, se você estiver usando `default` como um canal de baixa prioridade, precisará usar a prioridade de canal estrita, pois os metadados em defaults ainda não foram corrigidos, o que permite que pacotes de extensão do Python sejam instalados junto com o Pypy.

```bash
conda config --set channel_priority strict
```

# Usando o conda-smithy para gerenciar sua infraestrutura de integração contínua (CI)

O conda-forge, e especificamente o conda-smithy, contém diversas ferramentas para construir e implantar infraestrutura de integração contínua (CI) em diversas plataformas e arquiteturas. Não seria ótimo se você pudesse reutilizar todo esse trabalho árduo, sem precisar escrever ou gerenciar suas próprias configurações de CI?

Ao adicionar um diretório recipe/ ao seu repositório, o comando ci-skeleton do conda-smithy permite que você se conecte a uma infraestrutura de CI robusta e bem testada. Usando o comando rerender do conda-smithy, você pode manter seu repositório atualizado com quaisquer alterações necessárias.

Introdução

O comando ci-skeleton ajuda você a começar, preparando um repositório para ter a estrutura adequada, de modo que o comando rerender adicione corretamente as configurações de CI. Vejamos um exemplo!

Suponha que você tenha um repositório para um projeto chamado myproj. Na raiz do repositório, você pode executar o seguinte comando:

```bash
~/repo $ conda smithy ci-skeleton myproj
```

Isso produzirá uma saída como a seguinte:

```bash
Gerando ~/repo/conda-forge.yml
Gerando ~/repo/recipe/meta.yaml
Atualizando ~/repo/.gitignore
Um esqueleto de CI foi gerado! Siga as seguintes etapas 
para concluir o processo de configuração de CI:

1. Preencha o arquivo recipe/meta.yaml com seu código de instalação e teste
2. Confirme todas as alterações no repositório.

   git add . && git commit -m "ran conda smithy skeleton"

3. Lembre-se de registrar seu repositório com os provedores de CI.
4. Rerenderize este repositório para gerar os arquivos de configuração de CI.
   Isso pode ser feito com:

   conda smithy rerender -c auto

A qualquer momento no futuro, você poderá atualizar automaticamente a
configuração do CI executando novamente o comando rerender acima. Bons testes!
```

Como você pode ver, isso gera e atualiza alguns arquivos importantes. O primeiro arquivo criado é o conda-forge.yml. Ele foi construído especificamente para informar ao conda smithy rerender que não estamos executando o CI myproj como um arquivo de origem regular. O .gitignore foi modificado para não adicionar acidentalmente arquivos temporários indesejados do conda-smithy ao seu repositório.

Além disso, os passos que o ci-skeleton executa são muito importantes para conectar tudo corretamente. Felizmente, eles são fáceis de executar! Vamos analisá-los um por um!

### 1. Preencha o arquivo `recipe/meta.yaml`

O comando ci-skeleton emite um arquivo meta.yaml de exemplo para a compilação do myproj, daí a parte "skeleton" do nome. Se você não quiser que o esqueleto seja produzido no diretório recipe/, pode usar a opção -r para fornecer uma alternativa.

O meta.yaml se parece com:

```yml
{% set name = "myproj" %}
{% set version = environ.get('GIT_DESCRIBE_TAG', 'untagged')|string|replace('-','_') %}

pacote:
nome: {{ nome|inferior}}
versão: {{ versão}}

fonte:
git_url: {{ environ.get('FEEDSTOCK_ROOT', '..') }}

construção:
# Descomente a linha a seguir se o pacote for Python puro e a receita
# for exatamente a mesma para todas as plataformas. Não há problema se as dependências
# não forem compiladas para todas as plataformas/versões, embora seletores ainda não sejam permitidos.
# Consulte https://conda-forge.org/docs/maintainer/knowledge_base.html#noarch-python
# para mais detalhes.
# noarch: python

número: {{ environ.get('GIT_DESCRIBE_NUMBER', '0') }}
string: {{ [build_number, ('h' + PKG_HASH), environ.get('GIT_DESCRIBE_HASH', '')]|join('_') }}

# Se a instalação for complexa ou diferente entre Unix e Windows,
# use arquivos bld.bat e build.sh separados em vez desta chave. Por padrão,
# o pacote será compilado para as versões do Python suportadas pelo conda-forge
# e para todos os principais sistemas operacionais. Adicione a linha "skip: True # [py<35]" (por exemplo)
# para limitar ao Python 3.5 e mais recentes, ou "skip: True # [not win]" para limitar
# ao Windows.
script: "{{ PYTHON }} -m pip install . -vv"

requisitos:
build:
# Se o seu projeto compila código (como uma extensão C), adicione os
# compiladores necessários como entradas separadas aqui. Os compiladores são nomeados 'c', 'cxx' e 'fortran'.
- {{ compiler('c') }}
host:
- python
- pip
run:
- python

test:
# Alguns pacotes podem precisar de uma chave `test/commands` para verificar a CLI.
# Liste todos os pacotes/módulos que `run_test.py` importa.

imports:
- myproj
# Execute seus comandos de teste aqui
commands:
- myproj --help
- pytest
# declare quaisquer requisitos exclusivos para teste aqui
requires:
- pytest
# copie quaisquer arquivos de teste necessários aqui
source_files:
- tests/

# Descomente e preencha os metadados do myproj
#about:
# home: https://github.com/conda-forge/conda-smithy
# license: BSD-3-Clause
# license_family: BSD
# license_file: LICENSE

# Descomente o seguinte se este for um forge
# Remova estas linhas se este for usado apenas para integração contínua
#extra:
# recipe-maintainers:
# - BobaFett
# - LisaSimpson
```

Esta receita está configurada para obter corretamente o código-fonte e as informações de versão do git. Ela também elimina a adição de quaisquer arquivos de teste que você queira que o conda-build use ao executar o conjunto de testes
