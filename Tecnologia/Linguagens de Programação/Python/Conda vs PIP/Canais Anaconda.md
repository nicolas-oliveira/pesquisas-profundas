# Canais Anaconda

https://www.anaconda.com/docs/tools/working-with-conda/channels

Um canal é um local onde o Conda pode procurar [pacotes](https://www.anaconda.com/docs/tools/working-with-conda/packages/install-packages) para instalar em [ambientes](https://www.anaconda.com/docs/tools/working-with-conda/environments) na sua máquina. O Conda encontra esses canais por URL, nome ou caminho de arquivo, dependendo da sua configuração.

**Exemplos**

| URL                | `https://anaconda.org/conda-forge`      |
| ------------------ | --------------------------------------- |
| Nome               | `conda-forge`                           |
| Caminho do arquivo | `file:///local-dev-channels/my-channel` |

## Visualizando seus canais

Para ver quais canais o Conda está configurado para usar no momento, abra o Prompt do Anaconda (Terminal no macOS/Linux) e execute o seguinte comando:

```
conda config --show channels
```

**Exemplo de retorno:**

```
channels:
- defaults
- conda-forge
```

Para gerenciar a lista `channels:` no seu arquivo `.conadrc`, consulte [Gerenciando a configuração de canais](https://www.anaconda.com/docs/tools/working-with-conda/channels#managing-channel-configuration).

## Abrindo seu arquivo .condarc

Para localizar seu arquivo `.condarc`, abra o Prompt do Anaconda (Terminal no macOS/Linux) e execute o seguinte comando:

```
conda config --show-sources
```

**Exemplo de retorno:**

```
==> /Users/<USERNAME>/miniconda3/.condarc <==
channels:
- defaults
```

Embora o `.condarc` provavelmente esteja no seu diretório
Home, ele é um arquivo oculto no macOS e no Linux e não é visível em navegadores de arquivos em circunstâncias normais.

Você pode visualizar arquivos e pastas ocultos seguindo as seguintes instruções para o seu sistema operacional:

Para visualizar arquivos ocultos no macOS, use Shift+Cmd+. no Finder.

Para mais informações sobre o arquivo `.condarc`, consulte [Usando o arquivo de configuração .condarc do Conda](https://docs.conda.io/projects/conda/en/stable/user-guide/configuration/use-condarc.html) na documentação oficial do Conda.

## Gerenciando os canais padrão

Ao instalar o Conda pela primeira vez por meio da Distribuição Anaconda ou dos instaladores do Miniconda, ele vem configurado para usar um conjunto de canais padrão:

- `https://repo.anaconda.com/pkgs/main`
- `https://repo.anaconda.com/pkgs/r`
- `https://repo.anaconda.com/pkgs/msys2` (somente Windows)

Esses canais padrão são criados e hospedados pelo Anaconda e estão sujeitos aos Termos de Serviço do Anaconda.

Os canais padrão fornecidos com a Distribuição Anaconda e o Miniconda
podem ser alterados ou removidos, se necessário, editando o arquivo de configuração do Conda (`.condarc`) ou excluindo completamente o canal `defaults`.

1. [Abra](https://www.anaconda.com/docs/tools/working-with-conda/channels#opening-your-condarc-file) seu arquivo `.condarc`.
2. Adicione as seguintes linhas ao final do arquivo:

Copiar

Pergunte à IA

```
default_channels:
- URL do Canal 1
- URL do Canal 2
```

Adicionar URLs de canais à lista `default_channels:` substitui os canais codificados em `defaults`.

## Gerenciando a configuração de canais

Você pode precisar de pacotes que não estão disponíveis nos canais padrão
fornecidos pela sua Distribuição Anaconda ou instalador do Miniconda.
Alguns canais adicionais populares incluem:

- [conda-forge](https://conda-forge.org/): Um canal mantido pela comunidade com uma vasta coleção de pacotes
- [bioconda](https://bioconda.github.io/): Um canal mantido pela comunidade para pacotes especializados em bioinformática

Os canais podem ser configurados editando manualmente o arquivo `.condarc` ou usando comandos conda no Prompt do Anaconda (Terminal para macOS/Linux).

Lembre-se da [prioridade do canal](https://www.anaconda.com/docs/tools/working-with-conda/channels#channel-priority) ao configurar seus canais.

1. [Abra](https://www.anaconda.com/docs/tools/working-with-conda/channels#opening-your-condarc-file) seu arquivo `.condarc`.

2. Adicione o nome ou URL do seu canal à lista `channels:`.

Por exemplo, se você quiser adicionar o conda-forge à sua lista de canais abaixo do canal `defaults`, sua lista `channels:` ficaria assim:

```
channels:
- defaults
- conda-forge
```

### Prioridade do canal

O Conda busca os pacotes solicitados começando pela primeira entrada na lista `channels:` do seu arquivo `.condarc`. Se o pacote solicitado não estiver localizado nesse canal, o Conda continua a busca usando a próxima entrada na lista `channels:`.

Quando o Conda alcança a entrada `defaults` na lista `channels:`, ele busca os canais listados na lista `default_channels:`, na mesma ordem decrescente. Isso é chamado de "prioridade do canal"
e determina qual fonte o Conda usa primeiro ao resolver pacotes.

## Instalando pacotes de um canal específico

Você pode instalar pacotes dos canais listados em `.condarc` com a versão mais simples dos comandos `conda create`, `conda install` ou `conda update`, mas se quiser especificar um canal específico, você pode fazê-lo de duas maneiras: usando a sintaxe `channel::package` ou usando a flag `--channel`. Ambos os métodos instalam um canal
