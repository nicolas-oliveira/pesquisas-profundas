# Um guia simples sobre Anaconda

Nesta série, vou guiá-lo por tudo o que envolve a vida de um cientista da computação, desde o básico das linguagens de programação até técnicas avançadas usadas por desenvolvedores experientes. Tento abordar tudo. Meu objetivo não é te afogar em jargões ou confundi-lo com teorias complexas. Em vez disso, quero tornar sua vida de desenvolvedor mais fácil, divertida e compreensível. Utilizo exemplos e exercícios práticos para reforçar seu aprendizado.

# O que é Anaconda?

Primeiro, vamos conhecer [Anaconda](https://www.anaconda.com/) 🐍 , depois podemos ir mais além e aprender como usá-lo. Anaconda é uma distribuição gratuita e de código aberto das linguagens de programação Python e R para computação científica, ciência de dados e aprendizado de máquina. Ela simplifica o gerenciamento e a implantação de pacotes, fornecendo um conjunto completo de ferramentas e bibliotecas prontas para uso. O Anaconda vem com seu próprio [gerenciador de pacotes](https://medium.com/@shb8086/tutorial-series-anaconda-7bfdfd84e2ff#d35a) chamado [**Conda**](https://conda.io/projects/conda/en/latest/user-guide/install/index.html), que facilita a instalação, o gerenciamento e a atualização de pacotes e dependências.

Até aqui, queríamos nos livrar das dificuldades do gerenciamento de pacotes, mas adicionamos um novo gerenciador de pacotes (conda)! Confuso, não é? Mas o Conda é um gerenciador de pacotes que gerencia o restante dos pacotes. 😄 No texto a seguir, abordaremos os principais recursos do Anaconda.

Alguns dos pacotes que você pode encontrar no Anaconda.

## [**Gerenciamento de Pacotes**](https://medium.com/@shb8086/tutorial-series-anaconda-7bfdfd84e2ff#d35a)**:**

Como dissemos anteriormente, o Anaconda possui um gerenciador de pacotes chamado **Conda**, que nos permite instalar, desinstalar e atualizar facilmente pacotes e suas dependências. O Conda gerencia pacotes do repositório Anaconda ou de outras fontes com fácil acesso a uma ampla gama de bibliotecas e ferramentas.

## **Gerenciamento de Ambientes:**

O Anaconda permite que os usuários criem ambientes isolados (algo como o que vimos no artigo [Ambiente Virtual Python](https://medium.com/@shb8086/a-guide-to-python-virtual-environments-200381b6cedd)) com versões específicas do Python e pacotes. Isso é útil para gerenciar dependências e evitar conflitos entre diferentes projetos.

## **Ecossistema Abrangente de Bibliotecas:**

O Anaconda vem com uma grande coleção de bibliotecas e ferramentas pré-instaladas comumente usadas em computação científica e análise de dados, como NumPy, Pandas, SciPy, Matplotlib, Scikit-learn, TensorFlow e PyTorch.

## **Suporte Multiplataforma:**

O Anaconda está disponível para Windows, macOS e Linux, o que significa que é acessível a usuários em diferentes sistemas operacionais.

## **Integração com IDEs:**

O Anaconda integra-se perfeitamente com ambientes de desenvolvimento integrado (IDEs) populares, como Jupyter Notebook, Spyder ou Visual Studio Code, e fornece um ambiente de computação interativo rico para análise e experimentação de dados.

## **Suporte da Comunidade:**

O Anaconda possui uma comunidade ativa de usuários e desenvolvedores que contribuem para o seu desenvolvimento, principalmente fornecendo suporte e compartilhando recursos como tutoriais, documentação e pacotes.

# O que é um gerenciador de pacotes?

Já falamos sobre gerenciadores de pacotes, mas o que é? Por que precisamos dele? Você já o usou sem saber o nome? Um gerenciador de pacotes é uma ferramenta de software que ajuda os usuários a instalar, gerenciar e desinstalar pacotes de software em seus computadores. Ele simplifica o processo de instalação e manutenção de software, gerenciando tarefas como resolução de dependências, gerenciamento de versões e atualizações.

## Instalação:

Gerenciadores de pacotes permitem a instalação de pacotes de software a partir de repositórios ou fontes. É possível simplesmente escolher o nome do pacote que queremos instalar, e o gerenciador de pacotes se encarrega de baixar e instalar os arquivos necessários.

## Resolução de Dependências:

Muitos pacotes de software dependem de outros pacotes ou bibliotecas para funcionar corretamente. Os gerenciadores de pacotes reconhecem e instalam automaticamente essas dependências ao instalar um pacote e garantem que todos os componentes necessários estejam disponíveis.

## Gerenciamento de Versões:

Os gerenciadores de pacotes monitoram as versões dos pacotes de software instalados e permitem que os usuários instalem versões específicas ou atualizem para a versão mais recente. Isso ajuda a garantir a compatibilidade, o software está sempre atualizado, quase não há bugs e estamos usando os recursos mais recentes.

## Desinstalação:

Quando o software não é mais necessário, os gerenciadores de pacotes nos permitem desinstalar pacotes **de forma limpa**, o que significa remover **todos** os arquivos e dependências relacionados para liberar espaço em disco e evitar conflitos futuros.

## Resolução de Conflitos:

Os gerenciadores de pacotes lidam com conflitos que podem surgir durante a instalação ou atualização de pacotes de software. Eles resolvem conflitos garantindo que todas as dependências sejam atendidas e compatíveis entre si.

Assim, os gerenciadores de pacotes facilitam a instalação, atualização e remoção de pacotes de software e garantem que o sistema esteja estável.

Como você está usando conda-forge, conda-build, etc. como seu CI, é importante executar o conjunto completo de testes aqui.

Metadados como licenças e mantenedores provavelmente são menos importantes, pois, no caso padrão, os pacotes criados aqui nunca serão enviados para um canal. Sinta-se à vontade para excluir ou ignorar esses campos.

## 2. Confirme as alterações

Depois de escrever sua receita, é importante salvar as modificações! Basta executar os seguintes comandos:

```bash
~/repo $ git add . && git commit -m "ran conda smithy skeleton"
```

## 3. Registre-se com os provedores de CI

Isso é importante! Se você ainda não fez isso, precisará acessar os provedores de CI (Travis, Circle, Azure, etc.) e habilitar o CI para o seu repositório. Cada provedor de CI que você usa terá documentação sobre como configurá-lo.

## 4. Rerender

Por último, mas não menos importante, precisamos gerar os scripts de configuração de CI! Isso se baseia no conteúdo da receita, bem como nas seleções de provedores feitas no arquivo conda-forge.yml. (Consulte esta documentação para obter uma lista completa de provedores de CI.)

Para gerar os arquivos de configuração de CI, execute:

```bash
~/repo $ conda smithy rerender -c auto
```

Enviar essas alterações para o repositório deve permitir a construção e o teste do seu pacote em CI!

Mantendo-se Atualizado

Uma grande vantagem de usar o ci-skeleton é que, uma vez configurado, é muito fácil manter seu sistema de CI atualizado. Se você modificar sua receita para habilitar novas arquiteturas, quiser executá-la em um provedor diferente ou mesmo se o sistema de CI mudar sem que você precise, voltar a funcionar é tão fácil quanto rerenderizar. Basta repetir o passo 4 acima:

```bash
~/repo $ conda smithy rerender -c auto
```

Isso gerará e substituirá os arquivos de configuração do CI para a hora e o estado atuais da receita. É muito fácil!
