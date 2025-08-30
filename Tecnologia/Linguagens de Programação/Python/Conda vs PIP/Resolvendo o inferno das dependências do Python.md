# Resolvendo o inferno das dependências do Python

Se eu executar uma consulta "inferno das dependências do Python" no Google, obtenho 1.940.000 resultados. É muito! É claramente um problema. No entanto, nos últimos 2 anos, mais ou menos, nunca tive problemas ao implantar serviços Python em produção. Bem, pelo menos não quando segui o processo.

## Ferramentas

Eu uso três ferramentas para gerenciar ambientes — pyenv, pip-tools e make.

Vamos analisar cada uma delas mais detalhadamente e como elas contribuem para um gerenciamento de ambientes sem complicações.

[pyenv](https://github.com/pyenv/pyenv) — Um Gerenciamento Simples de Versões em Python. Esta é uma maneira simples de gerenciar diferentes versões do Python. Eu também uso o plugin [pyenv-virtualenv](https://github.com/pyenv/pyenv-virtualenv) para gerenciar e criar meus ambientes. É facilmente instalável com `brew`

```bash
brew update
brew install pyenv pyenv-virtualenv
```

Após a instalação, precisamos apenas adicionar algumas linhas ao nosso `.zshrc` (se você tiver o bash, elas podem ser um pouco diferentes; consulte o arquivo README).

```bash
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
```

[pip-tools](https://github.com/jazzband/pip-tools) — Este pacote Python é o nosso salvador. É uma ferramenta simples que recebe uma
lista de pacotes dos quais nosso projeto depende e a compila em uma
lista de pacotes que contém todas as versões corrigidas e compatíveis entre si.

Fonte: [GitHub - jazzband/pip-tools: Um conjunto de ferramentas para manter suas dependências Python fixadas atualizadas.](https://github.com/jazzband/pip-tools)

Precisamos instalar o pip-tools em todos os ambientes virtuais, assim como qualquer outro pacote, para usá-lo.

Enquanto escrevia este post, encontrei um [post médio](https://towardsdatascience.com/end-python-dependency-hell-with-pip-compile-multi-56eea0c55ffe) sobre uma iteração do pip-tools com alguns benefícios adicionais interessantes — [pip-compile-multi](https://github.com/peterdemin/pip-compile-multi)

[GNU Make](https://www.gnu.org/software/make/) — A ferramenta de automação. Algo que ajudará você a esquecer todos os comandos necessários para configurar o ambiente, executar testes, etc. Que bom que você terá seu `Makefile` para atualizar seus conhecimentos.

## Fluxo de Trabalho

As três ferramentas acima me ajudam a não me preocupar com dependências. Vamos passar pelo processo.

1 - Configurar o ambiente

```bash
pyenv install -s 3.11
pyenv virtualenv 3.11 demo-venv
pyenv local demo-venv
```

Os comandos acima instalarão a versão do Python necessária (e pularão esta etapa se já estiver disponível). Em seguida, informando a versão, ele criará um ambiente virtual chamado `demo-venv`. O último comando criará um arquivo `.python-version`, que permitirá que seu shell saiba qual Python executar a partir deste diretório.

2 - Em seguida, criaremos os arquivos `requirements.in` e `dev-requirements.in`:

```bash
# requirements.in
pandas
scikit-learn

# dev-requirements.in
-r requirement.txt
pytest
```

Como você pode ver, no arquivo `dev-requirements.in` dependemos do arquivo `requirements.txt`, que ainda não está disponível. Para obtê-lo, executaremos `pip-compile requirements.in`. Este comando produzirá um arquivo `requirements.txt`:

```bash
#
# Este arquivo é gerado automaticamente pelo pip-compile com Python 3.10
# pelo seguinte comando:
#
# pip-compile requirements.in
#
joblib==1.2.0
# via scikit-learn
numpy==1.24.2
# via
# pandas
# scikit-learn
# scipy
pandas==1.5.3
# via -r requirements.in
python-dateutil==2.8.2
# via pandas
pytz==2022.7.1
# via pandas
scikit-learn==1.2.1
# via -r requirements.in
scipy==1.10.1
# via scikit-learn
six==1.16.0
# via python-dateutil
threadpoolctl==3.1.0
# via scikit-learn
```

E se você compilar `dev-requirements.in`, obterá um arquivo semelhante a este:

```
#
# Este arquivo é gerado automaticamente pelo pip-compile com Python 3.10
# pelo seguinte comando:
#
# pip-compile dev-requirements.in
#
attrs==22.2.0
# via pytest
exceptiongroup==1.1.0
# via pytest
iniconfig==2.0.0
# via pytest
joblib==1.2.0
# via
# -r requirements.txt
# scikit-learn
numpy==1.24.2
# via
# -r requirements.txt
# pandas
# scikit-learn
# scipy
packaging==23.0
# via pytest
pandas==1.5.3
# via -r requirements.txt
pluggy==1.0.0
# via pytest
pytest==7.2.1
# via -r dev-requirements.in
python-dateutil==2.8.2
# via
# -r requirements.txt
# pandas
pytz==2022.7.1
# via
# -r requirements.txt
# pandas
scikit-learn==1.2.1
# via -r requirements.txt
scipy==1.10.1
# via
# -r requirements.txt
# scikit-learn
six==1.16.0
# via
# -r requirements.txt
# python-dateutil
threadpoolctl==3.1.0
# via
# -r requirements.txt
# scikit-learn
tomli==2.0.1
# via pytest
```

3 - Execute o comando `pip-sync dev-requirements.txt` para instalar as dependências atualizadas no seu ambiente de desenvolvimento.

4 - Normalmente, adiciono o conteúdo do arquivo `requirements.txt` ao `setup.py` por meio do argumento `install_requires`.

5 - Automatize o processo com `make`.

6 - Se você estiver realmente empolgado com a automação, adicione um modelo padronizado. Criamos um repositório para cada unidade de funcionalidade desenvolvida (endpoint para expor o modelo) e, portanto, muitas coisas precisaram ser copiadas e coladas de um repositório para o outro. Inevitavelmente, cometemos erros e precisamos perder tempo com depuração. Descobrimos que ter uma maneira automatizada de criar um repositório com todos os arquivos principais (incluindo o gerenciamento do ambiente) economizou tempo.

E é isso! Toda vez que você atualizar seus pacotes, `pip-compile` resolverá as dependências para você e, se houver versões de pacotes conflitantes, você saberá antes de executar `pip install` em produção.

***Observação:*** Há também um `pre-commit hook` [disponível](https://github.com/jazzband/pip-tools#version-control-integration) para garantir que os pacotes sejam sempre compilados.

## Conclusão

Após adotar as ferramentas que descrevi acima, não tive mais problemas com incompatibilidade de dependências. Bem, uma vez, quando tentei instalar um pacote adicional durante a construção do contêiner e depois que todas as dependências resolvidas foram instaladas, foi um bom lembrete para manter minhas dependências em um só lugar!

Como você lidou com as dependências do Python?

Você achou outras ferramentas/processos úteis?
