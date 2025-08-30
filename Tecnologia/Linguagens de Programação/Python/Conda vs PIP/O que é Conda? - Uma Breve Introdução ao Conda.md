# O que é Conda? - Uma Breve Introdução ao Conda

Neste artigo, ajudaremos você a entender:

- O que é Conda?
- O que é um pacote Conda?
- Como instalar e usar pacotes Conda
- Casos de uso comuns para pacotes Conda
- Alguns recursos valiosos para Conda
- A diferença entre Anaconda e Conda

# O que exatamente é Conda?

Conda é um sistema de gerenciamento de pacotes multiplataforma de código aberto. Ele foi criado inicialmente para solucionar problemas de gerenciamento de pacotes para cientistas de dados em Python e agora é um gerenciador de pacotes popular para pacotes em Python e R, mas você também pode usá-lo para gerenciar pacotes para qualquer linguagem. Como todos os sistemas de gerenciamento de pacotes, o Conda ajuda você a criar, encontrar e instalar os pacotes necessários.

Conda também é um gerenciador de ambientes. Suponha que você precise usar um pacote que requer uma versão diferente do Python. Nesse caso, você pode facilmente configurar e alternar para um ambiente de desenvolvimento usando essa versão específica do Python sem precisar alterar a versão do Python que você usa em seu ambiente habitual.

O Conda começou como parte da Distribuição Anaconda Python, mas ganhou popularidade por conta própria para outras funções além do gerenciamento de pacotes Python e R, e foi então desmembrado como um projeto separado sob uma licença BSD.

# O que é um Pacote Conda?

Um pacote Conda normalmente é um arquivo tarball compactado (.tar.bz2) ou, a partir da versão 4.7 do Conda, também pode ser um arquivo .conda. O formato de arquivo .conda foi introduzido no Conda 4.7 como uma alternativa menor e mais eficiente a um tarball. O arquivo tarball ou .conda geralmente contém:

- Bibliotecas de nível de sistema.
- Módulos Python ou outros.
- Programas executáveis ​​e outros componentes.
- Metadados no diretório info/.
- Uma coleção de arquivos que são instalados diretamente em um prefixo de instalação.

```bash
├── bin
│ └── pyflakes
├── info
│ ├── LICENSE.txt
│ ├── files
│ ├── index.json
│ ├── paths.json
│ └── recipe
└── lib
└── python3.5
```

Onde:

- `bin` contém os binários relevantes para o pacote.
- `lib` contém os arquivos de biblioteca relevantes (por exemplo, os arquivos .py).
- `info` contém os metadados do pacote.

# Onde posso obter os pacotes do Conda?

Assim como outros gerenciadores de pacotes, coleções de pacotes Conda estão disponíveis. Para outros formatos e tipos de pacotes, termos como "repositório", "feed" ou "registro" são frequentemente usados, mas para o Conda, os pacotes são armazenados em "canais".

Um canal é um local onde os pacotes Conda são armazenados e são URLs para diretórios que contêm pacotes Conda.

O canal padrão do Conda é https://repo.anaconda.com/pkgs/, que contém milhares de pacotes Conda, incluindo aqueles mantidos pelo Anaconda. Você também pode modificar os canais nos quais deseja pesquisar pacotes e adicionar alternativas como o Conda-Forge (uma comunidade que fornece pacotes Conda para uma ampla gama de softwares) ou até mesmo seus próprios canais Conda privados ou internos, como um repositório privado do Cloudsmith.

Para obter mais informações sobre como configurar e gerenciar canais Conda, consulte a documentação do canal Conda.

# Quem usa o Conda?

Os usuários mais comuns do Conda são aqueles que desenvolvem e trabalham com aplicações de ciência de dados, análise e aprendizado de máquina em larga escala.

Por quê? Bem, esses usuários frequentemente enfrentam problemas específicos com o gerenciamento de pacotes que os gerenciadores de pacotes de uso geral não resolvem. Por exemplo, usar várias versões diferentes de um pacote.

Uma diferença fundamental entre o Conda e o gerenciador de pacotes padrão para Python, pip, está na forma como as dependências de pacotes são gerenciadas. Como o Conda também é um gerenciador de ambientes, ele pode instalar diferentes versões de quaisquer pacotes solicitados e suas dependências sem entrar em conflito com nenhum pacote instalado existente.

Em outras palavras, o pip é o gerenciador de pacotes de uso geral para pacotes Python e instala pacotes em qualquer ambiente. O Conda instala pacotes dentro de ambientes Conda.

Embora você também possa obter esse isolamento de ambientes por meio de outras soluções (como o virtualenv do Python), ele é padrão e integrado ao usar o Conda. Isso significa menos configuração.

# Como instalo um pacote Conda?

A primeira coisa a fazer é instalar o Conda, e a maneira mais fácil de fazer isso é instalar o Anaconda ou o Miniconda, que incluem o próprio Conda.

Após instalar o Conda, você pode verificar se sua instalação está funcionando corretamente com um comando como `conda info`:

`conda info` exibe todos os detalhes da sua instalação atual do Conda.

Em seguida, você pode criar um novo ambiente Conda com o comando `conda create`, como:

`conda create --name demo-env`

Você pode então ativar este ambiente com o comando Conda activate:

`conda activate demo-env`

Ativar um ambiente é vital para garantir que qualquer instalação
