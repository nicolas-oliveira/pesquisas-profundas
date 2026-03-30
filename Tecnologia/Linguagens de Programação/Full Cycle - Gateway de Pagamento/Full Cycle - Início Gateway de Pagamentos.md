# Full Cycle - Início Gateway de Pagamentos

## Introdução e Visão Geral do Projeto

E falando em aprender, nessa imersão vamos colocar muitos conceitos, tecnologias e abordagens na prática. Vamos desenvolver um projeto prático simulando um gateway de pagamento, onde teremos endpoints necessários para criarmos uma nova conta para uma empresa poder realizar as transações, criando seus invoices e o processamento do pagamento.

Além disso, quando realizarmos uma transação com o valor acima de um determinado parâmetro, nosso sistema enviará uma transação para um microsserviço de antifraude, utilizando o **Apache Kafka** e receberá em seguida se a transação poderá ou não ser aprovada. Também você poderá visualizar todas as transações em tempo real através de um dashboard que vamos desenvolver.

O mais bacana é que em grande parte desse processo utilizaremos IA (Inteligência Artificial) para nos ajudar a sermos mais produtivos. E para realizar tudo isso, trabalharemos com:

* **Linguagem GO**: Para desenvolver o back end do gateway de pagamento.
* **NextJS**: Para criarmos o dashboard.
* **NestJS**: Para o sistema antifraude.
* **Apache Kafka**: Para a comunicação entre microsserviços.

Também a gente vai trabalhar com **Docker** para que a gente possa ser mais produtivo utilizando contêiners e mantermos todo o nosso ambiente de desenvolvimento extremamente produtivo. Além disso, também usaremos a IDE **Cursor** para nos auxiliar com o desenvolvimento utilizando IA.

Sendo que a cereja do bolo estará também na utilização de **MCPs (Model Context Protocol)**, que fará com que a nossa IA possa se comunicar com as nossas ferramentas para acelerar o desenvolvimento do projeto.

Esse projeto vai te mostrar na prática como todas essas tecnologias e conceitos se conectam, te preparando para criar soluções complexas de alto desempenho, como as exigidas pelo mercado. Então, se você quer se destacar, aprender a trabalhar com sistemas complexos e ser capaz de desenvolver soluções para grandes empresas, sem dúvidas, o que você vai aprender nessa semana vai te ajudar bastante.

Agora chegou a hora de colocar a mão na massa. Bora pra tela do meu computador.

## Apresentação do Projeto na Prática

Bom, pessoal, então agora aqui na tela do meu computador, eu quero trazer para vocês e explicar o projeto que a gente vai desenvolver aqui nessa semana, tá?

A nossa ideia é a gente simular um gateway de pagamento. E a ideia desse gateway de pagamento, na realidade, ela é bem simples, né? Nós vamos ter aqui um front-end onde ele vai ser o nosso gerenciador do gateway, onde a gente vai poder ver as faturas aprovadas, a gente vai poder ver as nossas transações, mas também, nesse gateway de pagamento, nós vamos ter uma API que ela vai ficar disponível aqui pra gente. Então, a gente pode fazer requisições aqui, fazer chamadas de API para que a gente consiga realizar as transações. Fechou?

E o que que acontece? Esse nosso sistema é um sistema distribuído. Então, a nossa ideia principal aqui é ter um front-end, ter o gateway API, né? Ou seja, o sistema principal aqui que ele vai ser desenvolvido na linguagem GO. E se você não tem nenhuma experiência com a linguagem, fique totalmente tranquilo, porque eu vou aos pouquinhos aqui introduzindo e eu não tenho dúvidas que se você já tiver uma base de programação, você vai conseguir acompanhar completamente tudo isso.

E uma coisa interessante dessa nossa aplicação é que ela vai ter uma condição bem interessante: toda transação que tiver um valor maior que 10.000, legal, ela vai cair para um sistema antifraude que a gente vai colocar. Então a nossa ideia é: se uma transação acontecer maior que 10.000, essa nossa transação, ela vai ficar pendente e a gente vai enviar uma mensagem para um sistema de mensageria, um sistema de streaming de dados nesse caso, que é o **Apache Kafka**.

Então a nossa ideia é a gente manda uma mensagem para esse sistema e a gente vai ter um outro microsserviço aqui, um sistema antifraude que o Luiz Carlos vai desenvolver utilizando o **NestJS**. Ele vai pegar essa transação, analisar essa transação aqui para ver se ela é suspeita ou não e depois ela vai retornar o resultado pra aplicação aqui em GO falar, atualizar o status como aprovado ou rejeitado aqui pra gente.

Tanto a aplicação em GO quanto a aplicação em NestJS têm os seus próprios bancos de dados separados. Então, é isso que a gente vai desenvolver durante essa semana aqui. E eu não tenho dúvidas que vai ser uma experiência bem bacana.

## Arquitetura do Sistema em GO (Gateway API)

Agora que a gente tem uma ideia geral aqui do nosso sistema, né? Eu queria falar especificamente desse sistema aqui, que é o que a gente vai começar a desenvolver hoje, tá? Que é o Gateway API, que é desenvolvido em GO, tá?

### Entidades

Como que vai ser essa arquitetura aqui? O que que a gente vai ter e o que que você precisa entender pra gente começar a trabalhar nesse sistema aqui? Basicamente nós vamos ter aqui algumas entidades.

Uma entidade aqui principal é chamada de `Account`. Essa entidade basicamente vai ser a conta da empresa que tá utilizando o gateway de pagamento. Vamos imaginar, eu tenho a Full Cycle, eu quero cobrar os meus alunos, então eu tenho a minha conta aqui nesse gateway de pagamento. Então eu vou ter um nome da minha conta, um e-mail e aqui eu vou ter dois campos super importantes, né? Um campo é chamado de `balance`, onde cada transação que é aprovada eu vou adicionar valor nesse balance. Então, se aprovou uma transação de 100, uma de 200, o meu balance é 300. E um outro campo aqui interessante pra gente é chamado de `API Key`. Toda vez que a gente criar uma conta pela primeira vez, a gente vai ter uma API Key, ou seja, uma chave especial que nós vamos passar no cabeçalho de toda requisição para identificar que é essa conta que tá realizando a transação aqui pra gente, tá? Então essa que é a ideia dessa nossa entidade.

Nós temos uma outra entidade aqui que ela é super importante, que é a `Invoice`, que é onde a gente emite a fatura e faz o processamento da transação. Então esse invoice ele vai ter uma ligação diretamente com essa conta. Então a gente vai falar que esse invoice é baseado nessa conta dessa determinada empresa. O valor, o `amount`, ou seja, o quanto que é o valor da transação, o `status` da transação. Por padrão, toda a transação, ela começa como `pending` e depois ela vai `approved` ou `rejected`. E caso ela seja maior que R$ 10.000, por exemplo, o que que vai acontecer? Ela vai ficar como `pending` e irá para o Apache Kafka. E aí o sistema, o NestJS vai falar se ela vai ter que ser aprovada ou rejeitada pra gente. Uma descrição do invoice, o `payment type`, a gente colocou muito mais ali para constar, porque só vai ser cartão de crédito. E a gente tem aqui `card last digits`, que vai ser basicamente os últimos dígitos do cartão de crédito, apenas para identificar qual cartão que fez a transação.

A gente tem aqui uma estrutura de dados que a gente vai criar. Ela serviria muito como um value object, mas eu não quero dizer que ele é porque a gente pode digitar os valores, mas a ideia principal aqui a gente ter uma estrutura onde a gente recebe os dados do cartão, né, e passa junto na transação com invoice, mas a gente não vai guardar esses números no banco de dados, tá? Então, somente para você saber que a gente vai ter essa estrutura de cartão de crédito aqui pra gente.

### Endpoints

Bom, dito isso, nós temos os endpoints que nós vamos ter que desenvolver ao longo desses dias, tá? Quais são os endpoints aqui pra gente? Vão ser cinco endpoints aqui pra gente.

1. Um endpoint é pra gente criar uma nova conta.
2. Um segundo endpoint é pra gente pegar os dados de uma determinada conta, ou seja, eu consigo ver todas as contas para eu conseguir pegar essa informação.
3. E depois disso eu tenho os meus endpoints realmente para eu realizar a minha transação, ou seja, o meu invoices para eu criar uma nova transação, onde eu já passo o cartão de crédito, ele aprova ou não.
4. Eu tenho `get invoices`, onde eu posso listar todos os invoices.
5. E eu preciso passar em qualquer um desses endpoints a minha API Key para dizer de qual empresa, de qual conta esse invoice ele tá sendo relacionado. Inclusive é uma chave ali secreta, vamos dizer aqui pra gente. E se eu quiser pegar especificamente um invoice, o que que eu vou fazer no final das contas? Eu vou passar um `get/invoices/{id}`, onde eu vou ter essa informação.

Então é isso aqui que a gente precisa para trabalhar nessa aplicação. Maravilha.

### Design da Aplicação (Camadas)

Agora, para você entender um pouco melhor o design da aplicação que a gente vai desenvolver, né? E quando eu tô falando a aplicação aqui nesse momento é a aplicação em Go, que é a aplicação principal aqui desse nosso projeto, eu fiz um desenho aqui para você conseguir entender um pouco melhor como que esse camarada vai funcionar, tá?

Então, basicamente nós temos alguns caminhos que a gente segue.

Primeiro caminho aqui que você vai perceber é que nós temos uma interface pra web, né? Ou seja, eu posso bater aqui uma requisição num web server. Esse web server ele vai cair aqui pra gente em `handlers`, ou seja, são controladores, onde eu posso executar operações de contas ou operações de invoice. Por exemplo, `create a new account`, listar um invoice, ou criar um novo invoice. Então, as funções aqui nesse caso que são executadas quando a gente chama uma API, elas ficam aqui nesses handlers, são os controllers aí pra gente. Legal.

O invoice handler, ele vai ter um `middleware`. O que que o middleware faz? Ele passa por uma camada aqui de autenticação baseado nesse caso na API Key. Então, antes da gente conseguir fazer qualquer transação com um invoice, eu preciso fazer uma validação aqui da minha API Key. E esse middleware ele vai fazer exatamente isso.

Bom, dito isso, o que que acontece? A gente tem aqui desse outro lado direito a nossa aplicação. E é assim que a gente vai organizar, vamos dizer assim, as camadas dessa nossa aplicação, né?

Então, o que que acontece é o seguinte. Aqui a gente recebe um JSON e a gente vai precisar pegar uma informação que a gente recebe nesse JSON e mandar aqui pra nossa aplicação. Mas os dados e as entidades aqui da nossa aplicação é uma coisa e o JSON que a gente recebe aqui é outra coisa. Então para isso nós vamos precisar de algo que a gente chama de `DTO (Data Transfer Object)`. É como se fosse um objeto, uma classe, vamos dizer assim, que não tem absolutamente nada. Ela só recebe dados, por exemplo, de um JSON e transforma isso num objeto de domínio e o inverso, a mesma coisa. Ela pode pegar um objeto da nossa aplicação aqui, um account, um invoice e transformar ele de volta ali preparado para ele retornar como um JSON, por exemplo, tá? Então é basicamente isso que acontece aqui pra gente.

Então quando a gente recebe essa requisição, nós temos algumas camadas aqui na nossa aplicação. E é isso que eu quero explicar, porque a gente vai desenvolver tudo isso no final das contas.

* **Camada de Domínio**: Aqui vão ficar as nossas regras de negócio. Por exemplo, Eu vou ter uma entidade de `Account`, onde eu vou ter minhas regras de account. Por exemplo, quando eu crio uma nova conta, ele vai, por exemplo, gerar uma API Key. Ah, toda vez que eu mudo alguma coisa, eu consigo adicionar um valor no balance. Ou, por exemplo, toda vez que eu tenho um invoice, eu consigo fazer aquela regra dos 10.000. Se ele for maior que 10.000, o status fica pendente ou coisas desse tipo.
* **Services (Account Service e Invoice Service)**: Aqui nesse caso, ele orquestra o fluxo da aplicação, ou seja, ele usa o account, ele usa o invoice para aplicar as regras de negócio, onde tem todas as informações que a gente precisa e ao mesmo tempo ele consegue acessar o nosso banco de dados através de algo que a gente chama de repositórios.
* **Repositórios**: É um padrão aqui onde a gente tem acesso aos dados. Então, toda hora que eu quiser salvar os dados de uma conta, eu preciso pegar o objeto da minha account aqui no meu service e passar pro repositório e o repositório consegue salvar essa informação.
* **Apache Kafka (Produtor/Consumidor)**: Nós temos uma camada aqui que é do Apache Kafka, que a gente separa ele em duas partes, consumidor e produtor. O que que significa? Vamos imaginar que a gente recebeu uma transação maior que 10.000. Esse invoice service aqui, ele vai criar um invoice, vai ver que a transação vai ficar pendente, esse cara aqui vai falar que olha, é maior que 10.000 essa aplicação. Então o que que ele vai fazer? Ele vai mandar, vai usar o Apache Kafka aqui para mandar uma mensagem para ele falando: "Olha, o sistema NestJS, por exemplo, tá entrando uma transação suspeita, faz a verificação antifraude." Então aqui é onde o Kafka ele vai enviar, o service vai mandar um evento aqui pro Apache Kafka produzir. Beleza? Então aqui a gente tem o produtor.

Por outro lado, se você olhar aqui no nosso sistema, junto ao nosso servidor web, nós temos algo que eu tô chamando aqui de `consumer`, que fica rodando em paralelo ao nosso servidor web. Então aqui eu tenho um processo, uma thread, vamos dizer assim, onde eu fico escutando a minha porta 80, onde eu consigo receber as chamadas de API. Por outro lado, aqui eu tenho um outro serviço que ele é um consumidor. Ele fica lendo uma fila, um tópico do Apache Kafka para ver se ele recebe alguma transação com resultado do antifraude, aprovado ou rejeitado. Então ele vai ficar lendo aqui esse tópico quando ele recebe uma nova mensagem. Essa mensagem aqui a gente vai receber um JSON, a gente converte aqui através do usando o DTO. E aí o nosso serviço vai dar baixa nessa mensagem falando que nesse invoice falando se ele tá aprovado ou não porque ele passou aqui pelo sistema antifraude.

Então esse aqui é o desenho do nosso sistema. Eu não sei o quão de experiência você tem com desenvolvimento de software, mas de uma forma ou de outra, olhando dessa forma pode assustar um pouco. Mas são esses, e essa estrutura de forma geral que a gente vai trabalhar com Gol.

### Divisão em Etapas

A gente vai dividir isso em três etapas:

1. **Primeira Etapa (Account)**: Fazer tudo referente a account. Então, a gente vai criar um account, um account service, o account repository, a gente vai criar um account handler, a gente vai criar o servidor web e também o DTO do account. Então, a gente vai cuidar primeiro dessa parte de account.
2. **Segunda Etapa (Invoice)**: Vai ser a parte de invoice, onde a gente vai criar uma transação, a gente vai aprovar a transação. A gente vai salvar a transação no banco de dados. A gente vai ter os invoice handlers, que no final das contas ele vai ser o camarada responsável para receber a informação da API aqui para mandar o processamento. A gente vai ter o middleware para conseguir fazer a autenticação desses nossos handlers. Essa segunda etapa já vai permitir que a área administrativa feita em NextJS já consiga funcionar, porque a gente vai ter todos os accounts, a gente vai ter o account e o handler pra gente poder verificar todas as transações.
3. **Terceira Etapa (Apache Kafka)**: É fazer essa parte do Apache Kafka, onde ele vai ficar consumindo as mensagens que a gente vai receber do sistema antifraude para que ela seja processada, aprovada ou não. E também a gente vai ter o produtor. Toda vez que a gente fizer uma conta que seja maior que 10.000, por exemplo, ele vai mandar pro produtor e vai publicar no Apache Kafka.

Então são três etapas. Account que a gente vai fazer, depois a gente vai fazer o invoice e depois a gente vai fazer a parte do Apache Kafka pra integração do nosso sistema. E obviamente são, vamos dizer assim, dias diferentes, porque tem bastante coisa aqui pra gente codificar.

Maravilha. Outra coisa, pra gente alinhar as expectativas, eu não tô esperando que você entenda 100% de tudo que eu coloquei nesse momento, ou até mesmo de 100% do código que eu vou produzir aqui junto com você. Mas eu quero que você entenda a ideia principal para que esses conceitos fiquem claros na sua cabeça que vão te ajudar aí criar cada dia mais uma carreira mais sólida. Ah, aí e obviamente, se você quiser vir estudar com a gente aqui da Full Cycle, eu vou ficar mais do que feliz para conseguir te ajudar a te aprofundar em cada um desses assuntos, desde design de aplicações, a parte de patterns, design patterns, mensageria, Docker e muita coisa que a gente tem aqui na Full Cycle. Legal?

## Configuração do Ambiente de Desenvolvimento

Então, a nossa ideia principal é ter esse design dessa nossa aplicação. E para isso, como eu falei para vocês, a gente vai fazer isso desenvolvendo através da linguagem Go, tá? E aí que é o grande ponto, nem todo mundo tá acostumado com essa linguagem. Então, a gente vai começar bem do zero aqui. Obviamente a gente vai ter um ritmo forte, acelerado, né? Vamos dizer um intensivo aqui para você, mas você vai perceber que se você já programa em qualquer linguagem de programação, não vai ser nada tão complexo para você entender como é que o Go funciona, porque ele é uma linguagem bem simples, de forma geral, tá?

### Instalação do Go

Aqui, tá, só para você entender, é o site da linguagem Go, tá? Então é `Go.dev`. Você entra no site, clica em download, faz o download da linguagem pro seu sistema operacional que você vai precisar. E aí em cima dessa parte você vai instalar next, next aí na sua linguagem de programação no seu sistema operacional. Basicamente é isso. Você vai ter o Go instalado, não tem segredo.

### IDE (Cursor / VS Code)

Aqui um outro ponto aqui que eu vou usar durante o nosso processo de desenvolvimento é o **Cursor**, tá? Então o cursor ele é um editor de desenvolvimento de texto. Ele é baseado totalmente em cima do VS Code. Então se você usa o VS Code, você pode acompanhar essa inteira utilizando o VS Code também. Mas o cursor, pelo menos o momento da gravação desse vídeo, é a ferramenta mais bacana que você pode utilizar recursos de inteligência artificial, onde ele consegue programar para você, ele consegue fazer análise do seu código, ele tem muita coisa bacana e eu tenho utilizado ele como editor principal nos meus últimos tempos aqui.

Maravilha. Mas nada impede de você utilizar o VS Code. O que você vai precisar de uma forma ou de outra, tá? Quando você for no seu VS Code, você vai precisar em relação à parte das extensions, você vai digitar `go` aqui, tá? Você vai digitar `go` aqui nas extensions e vai instalar a extensão do GO, que é a extensão oficial da linguagem aqui.

Maravilha. Você fazendo essa instalação dessa extensão, você vai ter que seguir o seguinte passo em seguida. Você vai dar um `Ctrl+Shift+P` ou um `Command+Shift+P`, dependendo se você usa Mac ou se você usa Windows. E quando você fizer isso, você vai digitar `Go` e você vai perceber aqui que vai ter `install/update tools`. Quando você fizer isso, você seleciona todas essas opções, dá um OK e acabou. Você vai ter tudo, exatamente tudo para trabalhar com Gol aí na sua aplicação, no seu dia a dia. Fechou?

## Iniciando o Projeto em Go

Então é dessa forma, tá aqui feita uma introdução pra gente e agora vamos começar a programar. Então vamos colocar a mão na massa aqui.

### Inicializando o Módulo Go

Primeira coisa que você tem que entender do Gol é que o Gol ele também tem um sistema de gerenciador de dependências, como um npm da vida para JavaScript. E no nosso caso aqui, o que que a gente vai fazer com o GO, tá? Nós vamos precisar inicializar esse gerenciador de dependências aqui pra gente. E para isso, a gente vai digitar um comando interessante aqui, que vai ser:

```bash
go mod init github.com/devfullcycle/imersao22/gateway
```

Por que que eu tô colocando esse endereço inteiro? Porque aqui é como se fosse o meu namespace, né? Então, se você trabalha com Java, C#, qualquer linguagem que trabalha com namespace, você garante que isso aqui vai ser único. E outra coisa é que se você um dia criar uma biblioteca em GO, você pode utilizar essas bibliotecas com o seu namespace do GitHub. Se alguém quiser baixar essa biblioteca, ele dá um `go get` e ele baixa para utilizar essa biblioteca. Você não precisa registrar isso como um pacote no npm, por exemplo. Aí fechou?

Então eu vou dar um enter aqui e você vai perceber que ele vai gerar aqui um arquivo chamado `go.mod` e esse `go.mod` vai falar que é o nome do nosso módulo aqui e a versão da linguagem que a gente tá trabalhando, no caso GO 1.24, pelo menos no momento dessa gravação. Fechou?

### Estrutura de Pastas e Arquivo Main

Uma outra coisa que é importante, só para você entender, eu vou criar uma pasta aqui chamada `cmd/app`. Legal? Essa pasta app vai ser o nome da minha aplicação ou poderia chamar de gateway. Eu tô aqui colocando a app apenas como um nome genérico. E aqui eu vou criar um arquivo chamado `main.go`. Nesse `main.go` eu vou criar um pacote chamado `main` e uma função chamada `main` aqui pra gente.

Para que que eu tô fazendo isso? Vai ser nesse momento muito mais para demonstração, tá? Por quê? Porque toda aplicação GO, ela precisa de um entry point, ou seja, uma função principal, um local onde ela começa a executar a aplicação. E da mesma forma que no Java, por exemplo, a gente tem o método `main`, a gente tem aqui no go uma função `main`.

Então, se eu chegar aqui e der, por exemplo, um `println("hello world")` e executar aqui no terminal `go run cmd/app/main.go`, você vai ver que a gente vai ter o nosso Hello World. E se eu quiser compilar, porque Go é uma linguagem compilada, você compila para executar a aplicação, você vai dar um `go build -o main cmd/app/main.go`. E ele vai gerar aqui pra gente o nosso executável. E eu posso gerar esse executável para qualquer sistema operacional. Isso aqui é só para você entender uma ideia de como que o Gol funciona, beleza?

### Convenções de Projeto (internal, domain)

Normalmente, por convenção, não é uma obrigação e tem nem todo mundo segue, mas de forma geral cada dia é mais comum você encontrar projetos em GO dessa forma. Normalmente você vai ver uma pasta aqui chamada `internal`. E dentro dessa pasta vai ter todo o conteúdo da sua aplicação. Por que `internal`? Porque dentro dessa pasta vai ser tudo que a sua aplicação precisa para ela executar e que não é compartilhável com mais nenhuma outra aplicação. E se você um dia quiser criar uma biblioteca, você pode criar uma pasta chamada `pkg`, onde essa pasta ela sim fica compartilhada e outros sistemas que quiserem dar um `go get` vão lá e vão baixar a sua biblioteca para poder utilizar. Beleza? Então isso aqui é para mostrar que é para ser utilizado como módulo interno.

No nosso caso aqui dentro da `internal` eu vou criar uma pasta chamada `domain`, né? Onde nesse domain aqui a gente vai voltar aqui pra nossa entidade aqui nesse caso de `account`, onde a gente vai ter as nossas regras de negócio.

### Criando a Entidade Account (Domain)

Então o que que eu vou fazer? Eu vou criar um arquivo chamado `account.go`, né? E aqui eu tenho que colocar o nome do package. E o nome do package sempre vai ser o nome da pasta que você tá, a não ser se você tiver no main, né, que você vai ver que o nome do package ele tem que ser chamado de `main` aqui para gente, beleza?

Bom, o GO ele não é orientado a objetos, mas ele é muito orientado a dados. Então, ele trabalha com estrutura de dados que a gente pode correlacionar como uma classe, por exemplo. Então, por exemplo, nesse nosso caso, a gente precisa da entidade `Account`. Então, o que que eu vou fazer? Eu vou criar um tipo aqui chamado `Account`. E esse tipo é uma `struct`, tá? O que que é uma `struct`? É uma estrutura de dados onde eu posso escolher o que eu vou colocar dentro desses dados, como que eu vou agrupar esses dados, mais ou menos como se fosse uma classe.

Então, eu vou colocar, por exemplo, eu vou ter um `ID` que vai ser uma `string`. Eu vou ter um `Name` que vai ser uma `string`. Eu vou ter um `Email` que vai ser uma `string`. Eu vou ter aquele meu `APIKey`, que também vai ser uma `string`. Eu vou precisar do `Balance` aqui pra gente, que vai ser um `float64` para eu guardar os valores. E eu vou criar também aqui para mim um `CreatedAt`, que vai ser um `time.Time`. Legal. E eu vou ter um `UpdatedAt`, que vai ser um `time.Time`. Você vai perceber que ele já importa, faz a importação automática aqui pra gente. Beleza?

#### Função Construtora (NewAccount)

Com isso aqui, pessoal, o que que eu vou fazer? Eu vou criar uma função construtora. No GO a gente não tem método construtor, mas normalmente por convenção, a gente tem uma função construtora pra gente poder criar um novo objeto aqui pra gente, pra gente poder construir essa nossa `struct`.

Então, nesse nosso caso, o que que vai acontecer? Eu vou criar minha função chamada `func NewAccount`. E nesse caso, Eu vou precisar do `name` e do `email`, que vai ser do tipo `string`. E ele vai retornar aqui para mim uma `*Account` (um ponteiro para Account). Essa account que ele tá retornando, se você perceber, tem um asterisco aqui. Isso significa que é um ponteiro. Na realidade, eu não quero que você se preocupe com isso, principalmente se você tem traumas do C. Mas o ponteiro, na realidade, a única coisa que ele diz é que o valor dessa `struct` vai ficar na memória e esse aqui vai fazer um apontamento na memória. Ou seja, em qualquer local do sistema onde você mudar esse objeto, ele vai ser alterado em todos os lugares, porque a gente tá falando apenas de um apontamento. Eu não quero que você fique preocupado aqui com ponteiros, simplesmente abstraia isso e se depois, se você quiser, você vai poder acessar qualquer vídeo nosso da Full Cycle no YouTube, onde a gente fala de Gol. Tem um vídeo específico sobre ponteiro, se você quiser saber mais. Beleza?

Agora, o que que eu vou fazer aqui? Eu vou construir esse meu objeto. Então, eu vou fazer o seguinte, eu vou fazer aqui `account := Account{}`. Toda vez que você vai criar uma variável pela primeira vez você coloca um `:` na frente porque ele cria, declara variável e atribui o valor, tá? Basicamente é isso. Ao invés de você declarar `var account Account`, você coloca um `account := Account{}` e ele já vai entender qual é o tipo da variável que você vai criar. Se eu colocar `s := ""`, ele já consegue inferir que é uma string. Se eu colocar `i := 0`, ele sabe que é um inteiro. Se eu colocar `f := 0.0`, ele sabe que é um float, né? E se eu colocar a dessa forma aqui, ele vai saber que a gente tá falando da nossa `struct`. E se eu tô retornando um ponteiro de `Account` aqui, eu tenho que colocar um `&` (e comercial) para dizer que isso aqui é um apontamento pro `account`, ou seja, referência na memória onde a gente tá trabalhando. Mas novamente, galera, abstraiam se você para não te atrapalhar caso você seja novo com Gol ou qualquer coisa desse tipo. Beleza?

Então, o negócio é o seguinte. No `ID` aqui, tá? Onde eu tenho que falar qual que vai ser o valor do ID que eu vou inicializar, eu vou falar que ele vai ser um UUID, que ele vai ser gerado automaticamente aqui pra gente, tá? O grande ponto é que o Gol nativamente ele não trabalha com UUID, mas existe um pacote bem interessante feito pela Google, tá? Que é chamado de `uuid`. Então, olha só como é que eu importo. Lembra que eu falei que é importante ter um namespace? Então, o Google tem um repositório no GitHub que tem um pacote chamado `uuid`. E se você utilizar, fazer a chamada desse pacote, ó, `uuid.New()`, ele vai criar um ID.

Por que que ele tá vermelhinho aqui embaixo? Porque a gente não baixou esse pacote. Então, se eu der um `go mod tidy`, ele vai ver todos os pacotes que eu tô utilizando externo, vai identificar e vai baixar essa dependência que ele acabou de fazer. Se você olhar agora o meu `go.mod`, você vai ver aqui que eu já tenho essa dependência e ele gerou esse arquivo `go.sum` aqui, exatamente para quê? Para manter a integridade, e saber que a gente tá utilizando aquele pacote. É um checksum ali para garantir a integridade desse pacote pra gente. Isso acontece inclusive no `package.json` no node e coisas desse tipo. Beleza?

Então agora que a gente tem um `uuid`, eu vou ter o `Name`, tá? E esse `Name` vai ser basicamente o nome que a gente tá esperando, o que a gente tá recebendo aqui. Legal. `Email` é o e-mail e o `APIKey` aqui pra gente, eu não vou colocar ele por enquanto porque a gente vai fazer uma funçãozinha para ele gerar. O `Balance` vai ser `0` e o `CreatedAt` e o `UpdatedAt` vai ser `time.Now()` aqui pra gente.

E agora o que que eu vou fazer? Eu vou dar um `return &account`. E agora ele vai retornar o dado. O ponto é que eu preciso do `APIKey` para ele gerar a primeira vez.

#### Gerando a API Key

Então, nesse caso, o que que eu vou fazer? Eu vou criar uma funçãozinha bem simples aqui, chamado de `generateAPIKey`, que ele vai retornar uma `string` para mim. Só para você saber o que eu vou fazer, eu vou gerar randomicamente uma string de 16 posições e vai ser no formato de hexadecimal. Então, eu vou fazer assim, ó. Vou criar uma variável aqui chamada de `b`, por exemplo, né? Ou por conta que são um slice de bytes. Então vou criar aqui como se fosse um array, tá? Aqui para mim de 16 posições. Legal. Mas somente para você saber, isso aqui no Gol é um slice. Um slice é como se fosse uma lista dinâmica. É um array dinâmico. Todo o array no go ele é fixo. Então eu se eu colocar um valor 4 aqui, 44, por exemplo, isso aqui é um array e ele não muda de tamanho. O slice ele consegue mudar de tamanho. Eu já tô criando ele desse tamanho aqui para ele já fazer a locação na memória, eu não precisaria necessariamente fazer dessa forma.

Mas o que que eu tô fazendo agora? Eu quero gerar randomicamente um valor aqui pra gente. Então, eu vou utilizar uma função, tá? Que o go tem chamada `rand.Read`. Que que ele vai fazer? Baseado nisso, ele vai acessar esse nosso slice. E agora eu posso executar a seguinte chamada `hex.EncodeToString(b)`. Então, que ele vai fazer ele vai pegar esse nosso slice aqui de `b`, e ele vai pegar e vai gerar esse número randômico no formato de string aqui pra gente. Então, a gente vai ter um hexadecimal aqui para string. E aqui são os pacotes que ele importou. O seu editor, ele vai fazer isso automaticamente.

E agora que nós temos isso, o que que eu posso fazer? Eu posso colocar aqui para mim, né, o `account.APIKey = generateAPIKey()` aqui para mim. E agora eu já tenho minha account.

#### Método para Atualizar o Balance (AddBalance)

Segunda coisa que eu vou fazer, que o meu account vai precisar, é pra gente poder adicionar o valor na nossa account, ou seja, o `balance`, né? Eu preciso mudar o valor de acordo com uma transação, ela vai acontecendo. Então, eu vou criar uma função chamada de `AddBalance`. Como é que ela vai funcionar? Vai ser uma função, mas nesse caso, essa função, ela vai estar atribuída a essa `struct`. Então, nesse caso, a gente chama isso de método (como se fosse um método de uma classe). Então, imagina que isso aqui é como se fosse uma classe e eu vou criar um método para essa classe. Como é que eu crio aqui? A gente cria no formato de função. A única coisa que eu vou fazer é que nesse caso, antes do nome da função, eu vou falar o nome da minha `struct` (`a *Account`) para que eu consiga acessar as propriedades aqui dessa minha `struct`. Então isso aqui, quando eu falo que isso aqui é um método, é porque eu tô falando de qual `struct` eu tô fazendo a referência.

E eu vou criar um método chamado de `AddBalance`, onde eu vou ter aqui para mim o nosso `amount`, onde eu passo um `float64` aqui para mim, não vou retornar erro aqui, nada disso. E agora com isso, o que que eu posso fazer? Eu simplesmente vou pegar o valor do meu `Balance`, vou somar com o meu `amount` aqui para mim, para eu conseguir trabalhar.

Uma outra coisa que eu vou fazer aqui também, né? Eu não preciso retornar nada porque essa função só tá fazendo essa alteração. Se eu quisesse ser um pouquinho mais criterioso aqui por relações à parte de concorrência, eu vou fazer isso somente para você saber, tá? O que que eu poderia fazer aqui na minha `struct`, né? Chamada `mu` de `mutex`, né? Então isso vem de um pacote chamado `sync/mutex`. Para que que isso aqui serve? Imagina que eu tenho um monte de transação acontecendo nessa mesma account, em várias dessas transações, querendo adicionar, mudar o valor do `balance` ali pra gente. O que que eu poderia fazer para garantir que ninguém tá mudando o `balance` simultaneamente para não dá um erro de cálculo? Ou seja, a gente pode ter um problema de condição de corrida, ou `race conditions`. Então, o que que eu posso fazer? Eu posso bloquear. E nesse caso, eu vou colocar um `sync.RWMutex`. O que que ele vai fazer? Eu consigo bloquear a escrita desse valor enquanto o meu `balance` tá sendo adicionado. Então, eu posso fazer a seguinte, olha só, eu vou colocar assim, ó, `a.mu.Lock()`. Nesse momento, ninguém consegue mudar o valor aqui do meu `balance`, porque eu estou, eu dei um lock, ninguém consegue trabalhar, tem que esperar eu realizar essa transação. E depois aqui embaixo eu posso dar um `a.mu.Unlock()`. E agora a partir de aqui, qualquer transação pode tentar fazer a alteração do `balance`. O Gol tem um recurso bem interessante que ele chama de `defer`. O que que o `defer` faz? Você declara ele logo aqui no começo, né? Por exemplo, `defer a.mu.Unlock()`. O que que ele vai fazer? Ele vai esperar tudo executar e por último ele vai executar essa função. É muito bom porque você não tem perigo de esquecer. Imagina, sabe quantas vezes você esqueceu de fechar a conexão com o banco de dados? Lá embaixo você tem que dar um close. Aqui com `defer` não. Você pode dar um close em cima, né? Mas ele vai ser executado somente depois que tudo da função aqui acontecer. Então essa seria uma forma de a gente evitar a condição de corrida. Obviamente, só para você saber, eu teria que deixar esse `balance` sem acesso público, para ninguém poder mudar ele diretamente pela propriedade, né? Mas isso é uma outra história. Só queria dar um exemplo aqui para você.

### Definindo a Interface do Repositório (Domain)

Uma vez que a gente tem esse nosso `AddBalance` aqui certinho, o que que eu vou fazer? Eu já fiz uma primeira parte do que eu queria trabalhar, que no final das contas é eu poder ter a minha regrinha de negócio. Nesse caso aqui é criar um account, fazer um `AddBalance` e aqui tudo mais.

Um outro ponto que eu preciso fazer agora aqui é também trabalhar com a parte de acesso a dados. Então, o que que eu recomendo aqui nesse momento? A gente vai fazer, se você olhar para eu acessar dados, né? Eu preciso de um repositório para eu acessar meu banco de dados. Então, o que que eu vou fazer aqui nesse meu caso para eu acessar meu banco de dados? Eu vou criar aqui dentro, dentro de `domain` mesmo, uma definição. Então, vou criar um arquivo chamado `repository.go`, que vai ser uma interface onde o meu domínio tá falando como o acesso a dados deve ser realizado, tá?

Então, nesse meu caso, o que que eu vou fazer? Eu vou ter um `type AccountRepository interface`. Por que uma interface? Porque depois essa camada aqui implementa essa interface para fazer o acesso aos dados. Então, nesse momento aqui, eu vou falar que eu vou precisar ter um método `Save`, onde eu vou passar um `account` aqui para mim, né? Então, eu vou passar um `account` que eu vou precisar aqui e eu Posso retornar um erro caso alguma coisa dê errado. Ou senão, se eu não retornar um erro, eu vou retornar um erro em branco, um `error = nil`.

Eu vou precisar também de um `FindByAPIKey`, que vai receber a `APIKey`, e vai retornar uma conta ou um erro. Na realidade, vai retornar um `*Account` e um `error`, mas esse erro pode ser nulo, ou seja, ele pode ser um erro em branco. Então, se o erro for em branco, quer dizer que a minha account deu certo. Agora, se a minha account vier em branco e o meu erro vier com alguma coisa, eu sei que a minha transação não deu certo. E a mesma coisa, eu preciso de um `FindByID`. E por último, eu preciso de um `UpdateBalance` aqui para mim, para eu poder fazer a alteração do meu `balance` no meu banco de dados quando eu trabalhar.

### Implementando o Repositório (Repository)

Agora que eu tenho essa minha definição de como que eu vou trabalhar com banco de dados, eu vou fazer o seguinte, eu vou agora criar aqui dentro da minha pasta `internal` uma pasta chamada `repository`. E aqui, esse `repository` sim, é onde a gente vai fazer o acesso à nossa camada de dados mesmo, tá? Então é aqui é onde a gente vai ter o nosso `AccountRepository`, por exemplo. Então vou criar aqui um `account_repository.go`. Aqui o nosso pacote vai ser `repository`. E aqui vai ser a parte onde a gente vai implementar todos aqueles métodos que eu falei ali para você.

Então, nesse nosso caso aqui, primeira coisa que eu preciso, eu preciso criar uma `struct`, um `type` chamado `AccountRepositoryDB`. E para esse cara funcionar, eu preciso de uma conexão com o banco de dados. Então eu vou colocar `DB *sql.DB`, que é um pacote que o Google tem para trabalhar com o banco de dados. Legal. Basicamente é uma interface para você conseguir falar com qualquer tipo de banco de dados e depois você só implementa o driver aqui pra gente.

Da mesma forma, eu preciso de uma função construtora. Então, basicamente eu tô dando um `NewAccountRepository` e ele tá retornando um `AccountRepositoryDB` com o valor do banco de dados. É como se fosse um construtor aqui pra gente.

#### Método Save

E aqui a gente tem o nosso primeiro método que é o `Save`, né? Eu poderia até ter chamado ele de `Create`, esse `Save` aqui, ele só vai inserir no banco de dados uma nova `account`. Então, para isso, o que que eu vou fazer? Eu vou definir um statement, `stmt` ou um `error` e eu vou fazer o seguinte, ó, `r`, né, de do meu repositório `r.DB.Prepare`. E aqui, basicamente, eu vou fazer colocar qual vai ser a minha query no banco de dados que eu vou trabalhar. Então, nesse caso aqui, vai ser, eu vou passar um `INSERT INTO accounts (id, name, email, api_key, balance, created_at, updated_at) VALUES ($1, $2, $3, $4, $5, $6, $7)`. Isso aí evita inclusive SQL injection e tudo mais. Provavelmente se você programa em outra linguagem de programação, isso aqui não é nada estranho aí para você. Legal.

E uma vez que ele fez esse `Prepare`, lembra que eu vou fazer, eu vou dar um `defer stmt.Close()` para depois que executar toda essa nossa aplicação ele fechar esse `Prepare` para evitar leak de memória e coisas desse tipo. E agora o que eu vou precisar apenas é dar um `stmt.Exec`, tá? O que que esse meu `Exec` ele vai fazer? Ele vai executar essa transação aí no banco de dados ali pra gente.

Por que que eu tenho um `_` (underline) aqui, galera? Porque esse `Exec` ele retorna dois valores. Ele retorna a quantidade de linha que foi inserida nesse nosso caso. E ele retorna também um `error`. Então, para que que eu preciso saber a quantidade de linhas? Nesse caso não precisa, eu tô inserindo um registro só, mas eu preciso saber se deu erro ou não. Então, como eu não preciso desse primeiro valor, eu coloco um `_`. Por quê? Porque o Go obriga a gente usar todas as variáveis que a gente define. E como eu não vou usar esse valor, se eu colocasse assim aqui como `a`, pode ver, enquanto o `a` não tiver sendo utilizado, o Gol não vai compilar. Então, como eu não vou utilizar o valor, eu coloco o `_`. Significa `blank identifier`, tá? É, eu vou ignorar, eu não vou utilizar esse valor que eu tô recebendo aqui.

E o `Exec`, então, ele vai substituir o `$1` pelo `account.ID`, o `$2` pelo `account.Name`, o `$3` pelo `account.Email`, etc. A ordem que eu passo os dados na função `Exec` vai ser a ordem de substituição que acontece aqui. E aqui o que que ele vai verificar aqui no final? Ele vai verificar se esse `error` que a gente recebeu aqui tá vazio. Se ele não tiver vazio, é porque a gente teve um erro. Então, ele vai retornar um erro aqui pra gente. Agora, caso esse erro *seja* vazio (nil), ele não vai retornar nada e a gente vai retornar `nil`. Ou seja, eu vou falar que o erro que a gente tá retornando é um erro em branco, é um erro nulo. Logo, ele vai ser ignorado na hora que a gente for fazer qualquer validação.

Então, o Gol não tem `try/catch`. Tudo que a gente utiliza para fazer validações de erro é assim: você verifica se o erro tá em branco. Se o erro não tiver em branco, você toma ação. Nesse nosso caso é retornar o erro aqui pra gente. Então aqui a gente fez a nossa primeira, o nosso primeiro método `Save`.

#### Métodos FindByAPIKey e FindByID

A gente tem vários outros métodos que a gente precisa fazer aqui, tá? E eu não acho que vale a pena a gente ficar digitando item por item. Então, alguns desses métodos aqui eu vou trazer pronto para facilitar um pouco a nossa vida, mas obviamente eu vou explicar linha por linha aqui para você. Lembre-se só, a gente precisa do `Save`, do `FindByID` e do `FindByAPIKey`.

Então, eu vou trazer aqui pra gente o nosso `FindByAPIKey` para que a gente consiga olhar e entender. Então, o seguinte, `FindByAPIKey` aqui a gente vai passar uma `APIKey` e eu vou retornar uma `struct` de `Account`, ou seja, o nosso objeto do domínio de `Account`. E eu também vou retornar um `error`, onde esse erro pode ser vazio ou não.

Então o que que eu vou fazer aqui? Como eu vou ter que fazer uma consulta no banco de dados, o que que eu estou fazendo aqui? Eu estou declarando uma variável chamada `account`. Então, nesse momento, ela é nula, mas a gente já identificou, a gente já reservou ali na memória falando que esse `account` aqui vai ser essa variável. E eu também já deixei bonitinho aqui um `CreatedAt` com `UpdatedAt` como `time.Time` aqui pra gente.

Então o que que a gente vai fazer aqui nesse momento? Eu vou fazer uma consulta no banco de dados. Então eu vou dar um `SELECT id, name, email, api_key, balance, created_at, updated_at FROM accounts WHERE api_key = $1`. Perceba que aqui, ó, `$1` é um parâmetro que eu tô passando, beleza? E agora que eu tô passando esse parâmetro, eu já vou emendar isso aqui com um outro método aqui chamado de `Scan`. O que que o `Scan` faz no final das contas? Ele acessa essa nossa variável direto lá na memória e pega o ID e coloca no ID do `account`. Ele pega o e-mail e pega, coloca no Email do `account`. Ele pega o API Key no API Key do `account` e o balance, o `CreatedAt`, `UpdatedAt` aqui pra gente. Então é basicamente isso que o `Scan` ele acaba fazendo aqui. Legal.

Então por que que ele tem o `&` (e comercial) na frente, porque o `Scan` ele precisa alterar lá na memória o valor dessa variável. Perceba que só de eu fazer essa função, essa variável aqui, ela está sendo alterada e isso só é possível porque ele tá fazendo essa chamada por referência. Ou seja, ele vai lá na memória, vê aonde esse dado tá guardado e faz alteração.

E aqui a gente tem o seguinte, a gente pode ter um erro que é o que é chamado de `sql.ErrNoRows`. O que que significa? Pode ser que a gente dê uma busca e não encontrou nenhum resultado. Nesse caso aqui, eu vou retornar um `account` em branco (`nil`), mas eu quero retornar um erro, tá? Mas esse erro aqui que eu tô retornando é um `ErrAccountNotFound`, somente pra gente deixar isso mais claro. E obviamente, se a gente tiver outro erro diferente de vazio, eu vou retornar o erro e a nossa `account` em branco, e o erro aqui pra gente. E obviamente eu vou pegar o meu `account.CreatedAt` como `createdAt` e o `UpdatedAt` como `updatedAt`, né? Então eu só tô atribuindo o valor que a gente já passou.

Agora você deve estar pensando aonde tá esse bendito erro, `ErrAccountNotFound`. Basicamente eu não criei ele ainda. Então eu vou aqui no meu domínio, tá? E vou criar um arquivo chamado `errors.go`. E nesse `errors.go` eu vou definir alguns erros que eu sei que eu vou utilizar, tá? E aqui pra gente é importante a gente saber quais vão ser os erros.

```go
var (
    ErrAccountNotFound   = errors.New("account not found")
    ErrDuplicatedAPIKey  = errors.New("duplicated api key")
    ErrInvoiceNotFound   = errors.New("invoice not found")
    ErrUnauthorized      = errors.New("unauthorized")
)
```

- `ErrAccountNotFound`: é retornado quando uma conta não é encontrada.
- `ErrDuplicatedAPIKey`: quando uma API key já existe.
- `ErrInvoiceNotFound`: quando o invoice não for encontrado.
- `ErrUnauthorized`: é quando o cara ele não tem acesso a um determinado recurso, ou seja, ele tá tentando acessar o invoice que não é dele, por exemplo, tá?

Então, é assim que a gente cria erros, objetos de erro no GO, `errors.New`, que é no pacote `errors`, e ele cria aqui pra gente. Então, quando eu volto aqui no meu repositório, ó, e `domain.ErrAccountNotFound`, agora eu não tenho mais problema nenhum. Maravilha.

Então, é basicamente essa pegada que a gente tá trabalhando aqui. Fechou?

Uma outra coisa importante aqui pra gente é que agora nós já temos esse `FindByAPIKey`, mas eu preciso de um outro camarada, né? E quem é ele? Ah, é o nosso `FindByID`, né? Então, a gente tem o `FindByID`. E ele é muito parecido, né? Ele é quase uma duplicação desse `FindByAPIKey`. A única diferença é que ele vai mudar a busca. Ao invés de ser `WHERE api_key = $1`, vai ser `WHERE id = $1`. Então, eu vou colar ele aqui porque esse cara aqui pra gente ele é importante. Então, vamos lá. Eu vou colar ele aqui. Você vai perceber que ele faz exatamente a mesma coisa: `FindByID`. E aqui é um `id` ao invés do `api_key`. O resto é tudo igual. A única mudança é nesse `id` aqui, né? A gente obviamente poderia refatorar, a gente poderia fazer um monte de coisa, mas é assim que a gente definiu no nosso repositório lá na nossa interface. Então ele é basicamente a mesma coisa.

#### Método UpdateBalance

Agora, e por último, nós temos um dos métodos aqui mais importantes e ele é um pouco mais crítico, que ele é o nosso famoso `UpdateBalance`. O `UpdateBalance` a gente tem que tomar cuidado porque não é só dar um `UPDATE` no banco de dados, a gente tem que tomar cuidado com concorrência, lembra? Imagina se alguém tá fazendo alteração no banco de dados enquanto outro processo está rodando também. A gente pode ter problema de concorrência. E para evitar esse problema de concorrência, a gente tem um tipo de lock que a gente pode dar no banco de dados para que enquanto eu estou realizando uma transação, ninguém possa mexer naquela linha ali para naquele momento, tá? E isso a gente vai, eu vou te mostrar como que a gente faz aqui, beleza?

Então, primeira ponto que eu vou fazer aqui, então eu tenho um `UpdateBalance`, eu vou chamar `UpdateBalance` aqui para mim, onde eu tenho um `account` e retorno um `error`. Mas agora o que que vai acontecer? Eu preciso fazer algo um pouco diferente. Eu preciso iniciar uma transação para eu fazer essa mudança no banco de dados da forma para eu evitar a concorrência. Então, eu vou colocar chamar de `tx`, que é um padrão, né? É um padrão comum de `transaction`. `tx, err := r.DB.Begin()`. Legal. Agora que eu fiz isso, eu vou ver se eu tive algum tipo de erro para trabalhar com isso. Se eu não tive, o que que eu vou fazer? Eu posso colocar um `defer tx.Rollback()` caso o sistema, caso a gente retorne um erro e acabe a aplicação inteira, ele vai dar um `rollback` nessa minha chamada.

E agora o que que a gente vai fazer. Primeira coisa, pra gente bloquear a nossa linha, para ninguém poder fazer alteração naquele momento, eu vou fazer algo chamado de `SELECT FOR UPDATE`. Primeiro eu vou criar uma variável `currentBalance`. E o ponto que eu vou fazer agora aqui vai ser o seguinte, eu vou dar um `err = tx.QueryRow("SELECT balance FROM accounts WHERE id = $1 FOR UPDATE", account.ID).Scan(&currentBalance)`. Opa, pera um pouquinho. Eu quero dar um `SELECT`. A minha IA, ela tá passando do ponto nesse momento para eu trabalhar. Pera aí.

Então, eu vou dar um `SELECT`, mas esse `SELECT` aqui ele é um pouco diferente. Eu vou dar um `SELECT balance FROM accounts WHERE id = $1 FOR UPDATE`. Quando eu dou um `FOR UPDATE`, eu tô dizendo o seguinte, ó. Eu tô selecionando essa linha, esse cara aqui, porque eu vou fazer uma alteração nessa linha. Então, quando eu fizer isso, ninguém nesse momento, a partir de agora, consegue fazer um `UPDATE` nesse recurso, nesse ID aqui pra gente. Então, agora que eu dei um `FOR UPDATE`, eu tenho que falar o `account.ID` que eu vou trabalhar. Então, vou colocar `account.ID` para ele fazer a substituição aqui no final das contas. Qual substituição? Substituir o `$1` pelo `account.ID`. E agora que eu tenho isso, eu posso dar um `.Scan` para ele pegar. E no `.Scan`, eu simplesmente vou pegar o valor que ele vai receber, que é o meu `currentBalance` aqui para mim. É basicamente isso que tá acontecendo. Deixa eu só fazer o seguinte, colocar aqui para ele não dar erro, tá? Então eu tenho esse `currentBalance`, ele vai dar um `Scan` aqui para mim para eu conseguir trabalhar.

Fechou? Então é `currentBalance` e aqui é `&currentBalance`. Tinha escrito errado. Maravilha. Então agora eu bloqueei a minha linha.

Uma vez que eu fiz isso, agora eu vou verificar se eu achei algum recurso, né? Se eu achei algum erro. Então vou colocar `if err == sql.ErrNoRows` que a gente tinha feito, ele vai dar um `return domain.ErrAccountNotFound`, que é o que a gente já tava fazendo antes. Caso contrário, se `err != nil`, ele vai retornar um erro aqui pra gente.

E agora que eu já bloqueei a minha linha e vi que esse recurso existe, eu vou mudar finalmente o saldo da minha conta. Então como que eu vou fazer? Eu vou dar um `_, err = tx.Exec("UPDATE accounts SET balance = $1, updated_at = $2 WHERE id = $3", account.Balance, time.Now(), account.ID)`. E agora eu vou passar o `account.Balance`, que é o que eu quero alterar, o `time.Now()`, que é o meu `updated_at` e o `account.ID` (`WHERE id = $3`). Aqui para mim, uma vez que eu fiz isso, eu vou verificar se eu tive algum erro. Se eu não tive nenhum erro, o que que eu vou fazer? Se eu tive algum erro, eu retorno o erro. Agora, caso eu não tenha tido nenhum erro, o que que eu vou fazer? Eu vou dar um `tx.Commit()` nessa transação e simplesmente o que vai acontecer é que ele vai realizar o update. Uma vez que ele der esse `commit`, aí essa linha é liberada para alteração.

Então com isso, galera, a gente já também nós já temos acesso ao nosso banco de dados.

### Criando o Service

Uma vez que nós já temos acesso ao nosso banco de dados aqui, o que que a gente precisa fazer, né? Eu tenho o meu `AccountRepository`, eu tenho o meu `Account` e agora eu preciso do meu `Service` para ele orquestrar essas minhas chamadas aqui. Lembre-se que a gente precisa criar novas contas e conseguir pegar dados da nossa conta. E para isso eu preciso organizar o fluxo dessa minha aplicação.

Então, como que eu vou fazer isso aqui nesse momento? Eu vou aqui dentro criar um novo folder chamado de `service`. Legal. E dentro desse `service` eu vou criar um arquivo chamado de `account_service.go` aqui para mim.

#### Struct e Construtor do Service

O que que esse `account_service.go` ele vai fazer aqui nesse meu caso, né? O que que ele vai, no que que ele vai me ajudar? Ele simplesmente ele vai permitir com que eu tenha aqueles métodos para que eu consiga criar uma nova conta e para eu conseguir pegar os dados de uma conta. Então eu vou criar uma `struct` dele aqui falando `AccountService`, vai ser uma `struct`. E essa `struct` o que ela precisa? Ela precisa acessar, pegar os dados do meu banco de dados. Então, o que que eu vou passar aqui para ela? Um repositório. E o repositório que eu vou passar aqui, eu vou passar a minha interface, porque daí eu posso substituir por qualquer tipo de repositório de banco de dados que eu quiser depois, tá? Então, eu trabalho aqui com injeção, a inversão de controle, a parte aqui do SOLID, do `Dependence Inversion Principle` aqui para mim.

Então, o que que eu vou fazer aqui nesse momento? Eu vou falar que eu vou precisar de um `repository`. E esse `repository`, ele tá definido em `domain.AccountRepository`. Beleza? E aqui eu vou precisar de uma função construtora, para eu ter um `NewAccountService`, que ele vai retornar um `AccountService` para mim, atribuindo o valor do meu repositório.

### Criando os DTOs (Data Transfer Objects)

E agora que eu tenho isso, o que que eu preciso fazer, pessoal? Eu preciso criar uma função para eu criar uma nova conta. Agora, lembra que eu falei aqui para vocês, para eu criar uma nova conta, eu preciso de dados de entrada, para eu fazer a criação. Esses dados eles podem vir daqui, por exemplo, no formato de um JSON. Então, o que que eu preciso? Eu preciso criar aqui uma camada intermediária que a gente chama de `Data Transfer Object (DTO)`. E no nosso caso ele vai ser uma `struct`, para no formato de entidade e vice-versa.

Então, o que que eu vou fazer aqui para facilitar a nossa vida? Eu vou criar uma pastinha aqui dentro, tá? Chamada de `dto`. E dentro dessa pasta `dto`, eu vou criar um arquivo chamado de `account.go`. E dentro desse `account.go`, eu tenho que pensar nas operações que eu quero fazer.

Por exemplo, toda vez que eu quiser criar uma nova `account`, eu vou precisar de quais informações? Nome e e-mail. Então, eu posso fazer o seguinte, eu tenho um `type`, vou colocar aqui `CreateAccountInput`, por exemplo, e a minha `struct`. E eu vou precisar de ter um `Name` e um `Email`, que são `string`, mas lembra que eu falei que esse dado ele pode vir via JSON? Então o Go ele tem um recurso muito interessante que é chamado de `tags`. Essas `tags` aqui elas funcionam como se fossem `annotations`. E significa o seguinte: toda vez que eu pegar um dado em JSON e querer converter em `struct`, ele vai pegar o campo do JSON chamado `name` e vai popular nesse `Name` aqui. Ele vai pegar o campo do JSON chamado `email` e vai popular esse `Email` aqui. E vice-versa, se eu tiver essa `struct` e quiser transformar ela em JSON, esse campo `Name` no JSON vai se chamar `name` e o campo `Email` vai se chamar `email`. Se eu colocar `email` `json:"email_x"`, né, ele vai buscar um campo `email_x` para popular. E se eu tiver essa `struct` e quiser transformar isso para JSON, o dado desse `Email`, no JSON vai se chamar `email_x`. É basicamente isso.

```go
type CreateAccountInput struct {
    Name  string `json:"name"`
    Email string `json:"email"`
}
```

Uma outra coisa que eu preciso agora é eu vou criar um outro DTO que é pro meu output. Depois que eu criar uma conta ou toda vez que eu retornar uma conta aqui para a minha API, eu preciso ter a estrutura de dados que eu quero, né? E aqui eu vou chamar então de `AccountOutput`, que é toda vez que eu quiser retornar os dados de uma conta.

```go
type AccountOutput struct {
    ID        string    `json:"id"`
    Name      string    `json:"name"`
    Email     string    `json:"email"`
    Balance   float64   `json:"balance"`
    APIKey    string    `json:"api_key,omitempty"`
    CreatedAt time.Time `json:"created_at"`
    UpdatedAt time.Time `json:"updated_at"`
}
```

Então vai ser basicamente isso. Agora, uma coisa que é interessante somente para você saber, né? Toda vez que eu quiser listar dados das minhas contas, eu quero esconder o API Key, caso ele não exista aqui pra gente quando ele tá vazio. Eu não quero nem que apareça o API Key quando ele tiver vazio. Então, se eu quiser, eu posso até colocar assim, ó, `omitempty` e quando ele não existir, ele nem vai aparecer. Mas no nosso caso, ele sempre vai aparecer mesmo. Mas eu tô colocando aqui para você aprender mais uma coisinha que pode te ajudar no Gol.

Agora, uma coisa que eu não sei se você se tocou aqui, eu só tenho a estrutura de dados, mas lembre-se que o meu DTO ele pode receber um dado de JSON, ou seja, ele vai pegar o dado nessa estrutura e ele tem que converter esse cara numa entidade de domínio, que é esse `Account` aqui. Ou ele pode pegar um `Account` de domínio e ele tem que transformar esse `Account` e reconverter esse `Account` no nosso DTO. Ou seja, isso aqui pode ser convertido numa classe de domínio ou uma classe dessa ou uma classe de domínio, um objeto de domínio, um `Account` de domínio pode ser transformado em DTO.

Então, nesse nosso caso, eu vou criar duas funções. Por exemplo, eu vou criar uma função chamada de `ToAccount`, tá? `ToAccount`. O que que o `ToAccount` faz? Ele tem um input, tá? Que ele vai receber um `CreateAccountInput` aqui para mim, tá? E aqui `CreateAccountInput`. Eu vou chamar esse cara aqui, `CreateAccountInput`. E ele vai retornar o quê pra gente? Um objeto de domínio aqui para mim, né? Então, toda vez que eu receber um `CreateAccountInput`, onde eu sei que eu tenho nome, e-mail, eu tenho que pegar esse dado e transformar num objeto de domínio. Então, nesse caso, eu simplesmente vou dar um `return NewAccount(input.Name, input.Email)`. Então agora eu tenho um dado que eu posso receber em JSON para fazer com que esse dado vire uma entidade. Aí eu posso dar um `ToAccount` passando o dado desse meu DTO.

Mas eu posso ter ao contrário, né? Quando depois que eu processo tudo, eu vou ter esse `Account` aqui em domínio, mas eu tenho que transformar ele num formato de DTO. Então o que que eu vou fazer? Eu vou chamar ele de `FromAccount`, ou seja, quando eu tenho um objeto de domínio e eu quero retornar ele no formato de DTO, certo? Então, nesse caso aqui, eu vou dar um `return AccountOutput` com todas as informações aqui que eu que ele quer. Então, nesse caso, `ID: a.ID, Name: a.Name, Email: a.Email, Balance: a.Balance, APIKey: a.APIKey, CreatedAt: a.CreatedAt, UpdatedAt: a.UpdatedAt`. Então, dessa forma a gente consegue trabalhar tanto pegando um objeto de DTO e transformar num objeto de domínio, como pegar um objeto de domínio, aqui no caso meu `Account`, e transformar nele num DTO.

Então agora eu consigo fazer as conversões dessa minha camada aqui. Então é basicamente essa parte aqui que a gente construiu.

### Implementando o Service

Feito isso, agora a gente volta naquele nosso `service` que a gente estava brincando aqui no começo. Como é que vai funcionar esse nosso `service`? A gente vai ter uma função chamada de `CreateAccount`, certo? Então vamos lá.

#### Método CreateAccount

`CreateAccount`. Como é que eu crio uma nova account aqui para mim, galera? Para eu criar uma nova account, deixa eu esconder aqui para ficar maior para você. Ah, eu preciso de um DTO, né? Ou seja, eu vou receber de uma camada aqui do web, por exemplo, pra minha aplicação inteira e eu vou, para eu fazer isso, eu vou ter que ter o meu DTO. Então, ele vai receber um `input` aqui daquele meu `CreateAccountInput`, certo? Então, esse é um ponto. E o que que ele vai retornar depois que ele criar um account? Um DTO `output`, para ele pegar o dado que foi criado aqui e retornar, por exemplo, pro meu web server. Então, ele recebe o `input` e retorna um `output` aqui para mim.

E agora eu preciso criar o meu objeto de domínio. Então, vou colocar `account := dto.FromCreateAccountInput(input)`. Agora eu já tenho aquele meu objeto e agora eu vou verificar, tá? Por exemplo, se essa account já existe, né? Ou alguma coisa desse tipo, né? Por que que eu tenho que fazer isso? Porque aqui, galera, eu tô dando a no nosso domínio Eu estou simplesmente criando um número randômico do meu API Key, mas se esse API Key já existir, eu tô enrolado. Então o que que eu posso fazer? Eu posso fazer uma verificação se uma API Key existe. E aí facilita um pouco mais a minha vida aqui. Então eu posso fazer o seguinte, eu posso colocar assim, ó, `existingAccount, err := s.repository.FindByAPIKey(account.APIKey)`. E o que que é acontece.

Agora eu vou fazer o seguinte, eu vou fazer uma verificação. Se vier alguma coisa no erro, ou seja, se o erro for diferente de vazio, né? E se o erro não for que ele encontrou, que ele não encontrou uma nova account, significa que realmente a gente teve um erro aí no nosso sistema, a gente teve um problema aqui na hora de fazer essa operação. Então, nesse caso, eu vou retornar o nosso account em branco e eu vou retornar o error aqui pra gente. Basicamente é isso que eu tô fazendo aqui. Maravilha.

E aqui eu tenho que retornar no formato de ponteiro aqui para mim. Então essa aqui é uma é uma forma que eu tô fazendo a verificação, ou seja, eu tô verificando se já existe esse cara. Agora para garantir caso a gente não tenha essa situação onde deu um erro, mas ele não encontrou nenhum uma conta, caso, se esse `existingAccount` for diferente de vazio, significa o que aqui pra gente? Significa que ele já encontrou uma chave duplicada, né? Então, é basicamente isso que a gente tá fazendo. Ou aqui ele encontra um erro comum, ou aqui, se ele tiver algum erro, é porque ele encontrou esse objeto duplicado e a gente não vai poder fazer a criação. Aí eu poderia fazer uma retentativa para ele gerar de novo um objeto enquanto ele não encontrar uma chave duplicada. Mas aqui é apenas uma verificação, tá pessoal? Eu não quero chegar nesse nível de detalhe, a gente poderia trabalhar.

Agora que eu tenho isso, o que que eu posso fazer? Simplesmente salvar esse `account` no meu banco de dados. Então eu posso colocar `err = s.repository.Save(account)`. Beleza? Uma vez que eu fiz isso, eu vou verificar se aconteceu algum erro. Caso não teve nenhum erro, o que que eu vou fazer? Eu vou retornar o quê? `dto.FromAccount(account)` e o erro em branco aqui para mim também, tá?

Agora tem um ponto importante. Aqui que a gente tem que se ligar, tá, pessoal? Aqui como eu tô retornando num formato de ponteiro, deixa eu só ver uma coisa. E aqui ele tá dando um erro aqui pra gente. `invalid operation: cannot assign values to dto.AccountOutput struct field APIKey in map`, ou seja, ele não tô podendo atribuir nenhum valor para esse cara. Deve ter alguma coisa errada no nosso repositório. Pera aí, vou aqui no `Save`. Ah, olha aqui o meu `Save` no meu account. Provavelmente eu tinha que poder retornar um error, né? Então, Basicamente, provavelmente é esse cara aqui. Maravilha.

E aqui, esse segundo erro aqui pra gente é que a gente tem que retornar aqui no final das contas o nosso DTO, ou seja, a gente tem que retornar esse `account` no formato de DTO. E aqui ele tá dando um erro para mim falando que esse `dto.FromAccount`, né, ele tá sendo do tipo `AccountOutput`. Ah, mas né? A gente precisa retornar aqui para ele num formato de ponteiro, tá? Por que que a gente tem que colocar num formato de ponteiro, principalmente aqui? Porque se ele for retornado em branco, a gente pode retornar um ponteiro apontando para nenhum lado. Agora, senão a gente ia ter que alocar um espaço numa variável e a gente não pode retornar uma variável `nil` nesse caso. Então, somente esquecendo esses níveis de detalhe, o que eu vou fazer simplesmente é uma forminha bem bobinha, `output := dto.FromAccount(account)` e eu posso retornar `&output` aqui para mim. Então, com isso eu tenho o meu `CreateAccount`.

#### Métodos UpdateBalance, FindByAPIKey e FindByID

Feito o meu `CreateAccount`, eu posso fazer o meu `UpdateBalance`, o `FindByAPIKey` a gente tem que ter e o `FindByID`, né? Então vamos fazer o `UpdateBalance`. Ele no final das contas ele acaba sendo bem simples aqui pra gente, né? Então aqui tá a assinatura dele, que no final das contas é, eu recebo um `UpdateBalance`, eu preciso do `APIKey` e eu preciso do `amount` que eu vou fazer a alteração do meu `balance` e ele vai retornar um `AccountOutput` pra gente, né? Mesmo esquema que a gente tem que retornar o objeto do nosso account.

Então, primeira coisa que eu vou fazer é verificar e buscar essa minha conta pelo `APIKey`. Então, vou colocar `account, err := s.FindByAPIKey(apiKey)`. Se ele der algum erro aqui pra gente, a gente retorna o erro. Caso contrário, eu adiciono o `amount` aqui para mim `account.AddBalance(amount)`. E agora eu vou dar o meu `UpdateBalance`, que vai ser o `s.repository.UpdateBalance(account)`. E se eu não tenho `UpdateBalance`, é porque tá escrito errado aqui. Beleza? Então eu tô dando um `UpdateBalance`. E se der algum erro, eu retorno o meu erro aqui, normal, mesmo esquema do GO, como sempre. E agora, se ele não der um erro, eu vou retornar o meu `account` no formato de DTO. Então, eu vou fazer aquele mesmo esquema que eu fiz aqui, ó, né? Eu pego o meu `account` e faço o meu retorno `dto.FromAccount(account)`. Então, eu já fiz o meu `UpdateBalance`.

E agora eu tenho o meu `FindByAPIKey`. E nesse caso ele é bem simples. né? Eu vou colocar aqui `func (s *AccountService) FindByAPIKey(apiKey string) (*dto.AccountOutput, error)`. Vamos ver se ele consegue completar aqui para mim. Então vamos lá. Eu eu pego o meu `account, err := s.repository.FindByAPIKey(apiKey)`, busco via API e retorno aqui para mim, né? Se tiver algum erro, ele traz aqui. Caso contrário, eu tenho aquele mesmo esquema aqui para eu retornar, `return dto.FromAccount(account), nil`. Aqui eu tenho que retornar ele no formato aqui de ponteiro. Maravilha.

Eu tenho `FindByAPIKey` agora e eu preciso fazer a mesma coisa com `FindByID`, né? Então aqui ele já o meu cursor ele já tá recomendando os meus comandos e aqui ele vai dar basicamente a mesma coisa `FindByID`. Então agora eu tenho `FindByID`, o `FindByAPIKey`, eu tenho meu `UpdateBalance` e eu tenho o meu `CreateAccount`.

Com isso aqui, galera, a gente já fez os services que a gente precisava de account. E agora a gente tem que começar ir para um outro lado, que é os nossos `handlers`, pra gente ir pro lado agora do nosso servidor web. Beleza?

## Implementando os Handlers HTTP

Então, como que a gente vai fazer isso para ir pro lado do nosso servidor web? Nesse momento nós vamos fazer o seguinte aqui dentro, deixa eu fechar todas as nossas pastinhas para você ver melhor. Eu vou criar uma pastinha aqui chamada de `web`. E nessa pasta aqui chamada `web`, o que que eu vou fazer? Eu vou criar uma pasta aqui chamada `handlers` e dentro dessa pasta `handlers` eu vou criar um arquivo chamado de `account_handler.go`. Beleza? E o que que esse meu `account_handler` ele vai fazer? Ele vai ter que implementar. Lembra que eu falei aqui pra gente? Esses endpoints aqui para `POST` e `GET` accounts aqui pra gente? Então vamos lá. Vamos implementar.

### Struct do Handler e Construtor

Então eu tenho um `type AccountHandler struct`. E o `AccountHandler` para ele conseguir conversar, ele vai precisar acessar o nosso `service`, né? Esse `Handler` aqui, ele precisa acessar o nosso `service` e provavelmente ele vai receber essa informação num JSON, converter para DTO para fazer a execução desse nosso `account service`. Então, para que o `account service` funcione, eu preciso ter o `account service` aqui para mim, como dependência. Então, vou colocar `accountService service.AccountService` aqui para mim. Vou ter a minha função construtora, como a gente sempre fez. E agora a gente vai fazer a coisa funcionar da seguinte forma, tá, pessoal? Deixa eu tirar isso aqui. Ele importou à toa.

### Método Create (Handler)

E agora o que que eu vou querer fazer? Eu vou criar aqueles meus endpoints. É como se fosse os nossos controladores, os nossos handlers. Ou seja, eu recebo uma `request`, uma `response`, né? Ele tem uma `response`, uma `request`. E aí eu faço a minha execução. Então eu vou fazer o seguinte, beleza? Então eu vou colocar assim, ó, `func (h *AccountHandler) Create(w http.ResponseWriter, r *http.Request)`. Mas em vez de `CreateAccount`, eu vou chamar apenas de `Create`. E aqui eu vou colocar no padrão do go, que é `w`, que é de `ResponseWriter`, quer dizer, que é o meu response. E o `r` é o meu `http.Request` aqui para mim. Então, usando o pacote `HTTP` do GO, eu tenho esse aqui é a minha interface para eu conseguir receber e enviar dados HTTP aqui no GO.

Então, a primeira coisa que eu preciso fazer é vou trabalhar com o meu DTO. Então, vou colocar assim, ó, `var input dto.CreateAccountInput`. Os dados que eu tô recebendo `dto.CreateAccountInput` aqui para mim. Então, eu declarei esse cara. Agora, lembra que eu vou receber os dados via JSON? Então, eu quero pegar o dado que eu retorno em JSON e converter ele e preencher essa variável aqui `input` com os dados do JSON, ou seja, eu vou fazer uma hidratação, um `hydrate`. Legal? Então, para isso, o que que eu vou fazer? Eu vou colocar `err := json.NewDecoder(r.Body).Decode(&input)`. Por que que ele tá colocando um `&` (e comercial)? Porque ele tá acessando por referência aonde esse `input` está guardado na memória para fazer alteração nele. Então, perceba que a essa função consegue alterar essa variável. Isso só é possível porque ele vai direto na memória onde essa variável está guardada. É basicamente isso.

E agora ele vai tentar, agora que ele vai tentar fazer isso, a gente vai verificar se ele conseguiu, né? Então eu vou fazer uma verificação de erro, tá? Então o erro vai ser o seguinte aqui pra gente. Se der um erro para converter o JSON no nosso DTO, ele vai retornar um `http.Error` e vai dar aqui um `http.StatusBadRequest` e vai retornar e acabou a nossa requisição agora.

Agora, caso não dê erro, a gente vai criar essa nossa account chamando quem? O nosso `account service`. Então eu vou fazer o seguinte, ó. Eu vou fazer assim, ó, o nosso `output` e o `error`, eu vou colocar `h`, né, de `AccountHandler` `h.accountService.CreateAccount(input)`. Da onde que tá vindo o `accountService`? Aqui, ó, esse cara aqui. `accountService.CreateAccount`. Passando o meu `input`. Esse `input` é o quê? É o cara que veio como JSON e ele acabou virando o nosso `input` que a gente tem aqui, ó, do nosso `CreateAccountInput`, beleza? E ele vai tentar fazer a criação. Deu certo isso? Então eu vou verificar erro, tá? Se ele deu algum erro aqui pra gente, eu vou retornar um `http.StatusInternalServerError`, porque realmente deu um problema no sistema nessa operação. Basicamente é isso que eu vou fazer agora.

Caso eu não tenha dado esse erro, eu quero retornar o resultado da criação da minha `account`. Então, nesse caso aqui, o que que ele vai fazer? Eu vou falar que o meu header é `application/json` `w.Header().Set("Content-Type", "application/json")` e vou pegar o dado aqui desse meu `output` e transformar ele de DTO para JSON, fazendo um `json.NewEncoder(w).Encode(output)` aqui pra gente. E como que eu sei o formato que ele vai trabalhar? Seguinte, lembra o nosso `AccountOutput`? Esse `output`, ele vai falar que esse `D` maiúsculo vai ficar esse `name` o `balance` desse jeito bonitinho que a gente acabou colocando aqui. Beleza? Então é assim que ele vai transformar.

Como ele tá criando um recurso aqui, eu também vou colocar um writer status created aqui para mim no meu header, só pra gente dar um `201` ali falando que criou um recurso `w.WriteHeader(http.StatusCreated)`.

Então com isso aqui nós já temos o nosso endpoint para criar uma nova conta.

### Método Get (Handler)

E agora eu preciso um outro endpoint onde eu consiga pegar os dados de uma conta para eu ter as informações dela, para eu pegar uma informação dessa conta, né? Então, é basicamente isso que eu vou precisar. Mas para eu conseguir pegar informação dessa conta, eu preciso do quê? Do API Key, né? Por quê? Porque na hora que eu for querer pegar o dado da minha conta, para eu fazer esse meu `GET`, eu preciso ter o API Key para eu saber de qual conta que eu quero pegar. Eu não quero listar aqui todas as contas, eu quero pegar uma conta em específico, mas eu não vou passar o API Key como ID, como recurso, porque ele é uma chave protegida. Então o que que eu vou fazer? Eu vou fazer o seguinte, vamos lá criar o nosso próximo handler aqui pra gente, né? Que é pra gente fazer o nosso `GET` aqui.

Como é que eu vou fazer? Eu vou precisar verificar o API key. O API key eu vou pegar no header da minha aplicação, da minha requisição. Então eu vou fazer assim, ó. `apiKey := r.Header.Get("x-api-key")`. E aqui eu vou verificar se ele não tiver, se tiver em branco esse valor, eu vou dar um `unauthorized` porque ele não tem acesso, ele não pode ter acesso a esse endpoint. Então eu vou fazer `if apiKey == ""`. Prontinho, ó. Ele já até me completou meu cursor aqui me ajudando. `err := errors.New("api key is required")`. Vou colocar aqui, ó, `"API key is required"`. E eu vou retornar aqui um status `http.StatusUnauthorized` e retorno. Caso agora ele tenha API key, o que que eu vou fazer aqui no final das contas? Eu vou chamar o meu service e o meu service tem aquele método `FindByAPIKey`, lembra? Então o que que eu vou fazer no final das contas? Eu vou fazer aqui o meu `output, err := h.accountService.FindByAPIKey(apiKey)`. Onde eu vou passar a API key que veio por parâmetro para mim. Se ele não encontrar, se der algum problema, ele vai dar um `http.StatusInternalServerError`. Se ele não encontrar o API key, eu poderia dar até um status `http.StatusNotFound` ali para ele. Eu poderia fazer isso, inclusive caso eu quisesse, mas eu vou deixar aqui bonitinho, está um `http.StatusInternalServerError` aqui, não tem problema, tá?

Ah, uma outra coisa que eu posso fazer agora, né? Caso não tenha dado nenhum erro, eu simplesmente vou retornar o resultado aqui para mim, né? Que é o `application/json`. `w.Header().Set("Content-Type", "application/json")`, o status `200`, ele já tá meio que incluso no pacote. Por padrão, ele sempre vai retornar `200`. Então eu não preciso trabalhar aqui para mim, tá? `json.NewEncoder(w).Encode(output)`. Por que que ele não tá me completando? Na hora que é para ele me ajudar a me completar, ele não completa. Deixa aqui.

Eu não preciso dar o `UpdateBalance`. O `UpdateBalance` a gente não dá de acordo com o no handler, né? A gente faz quando a gente for trabalhar com o `invoice`, porque quando o `invoice` é aprovado, aí ele faz o `UpdateBalance` ali pra gente.

Mas agora, galera, nós já temos no final das contas os nossos endpoints que a gente queria ter, que a gente estava precisando ali pra gente. Então, uma vez que eu tenho esses meus dois endpoints aqui importantes para mim, eu já resolvi essa parte aqui do meu sistema, né?

## Criando o Web Server

Então, eu tenho `AccountHandler`, já fiz esses accounts aqui para mim e agora eu preciso do web server trabalhando aqui comigo. Como que eu consigo fazer esse meu web server trabalhar aqui comigo, galera? Eu vou aqui dentro da minha pasta `web` e eu vou criar uma pasta chamada de `server`. Eu posso fazer isso? Posso. Então, vou criar uma pasta aqui chamada de `server` e criei aqui o meu server com letra maiúscula. `Server` aqui. E eu vou criar um arquivo chamado `server.go` bonitão aqui para mim. Pastinha `server`. E dentro do meu `server` agora, o que que eu vou fazer? Eu vou criar um `type` chamado `Server`. E nesse `Server` eu vou precisar de algumas coisas aqui para me ajudar.

### Configurando o Roteador (Chi Router)

Uma coisa que eu vou precisar, se você olhar o coloquei aqui, é um roteador. O Gol, ele tem um roteador básico que vem com ele. Ele até melhorou nos últimos tempos, mas esse roteador ele não deixa eu agrupar URLs, ele não deixa eu aplicar middlewares tudo de uma vez. Então ele não é um roteador mais completo, mas existe um pacote chamado `go-chi/chi`, tá? E esse roteador, ele tem diversos recursos. Então eu vou usar ele. Então vou chamar isso aqui de `router` e vou falar que ele vai ter o `*chi.Mux`. Mux vem de `multiplexer`, galera. `Multiplexer` quando você tem diversas rotas e ele escolhe para onde a cada item tem que ir. É basicamente o que a gente faz com a nossa rota. Cada rota aponta para um handler diferente.

Aí eu tenho que ter o meu servidor, né? que é o `http.Server` do Go, que é padrão. A gente tem que ter o `accountService` que a gente vai precisar aqui pra gente trabalhar aqui para mim. E eu vou colocar aqui também um `port`, que é a porta do servidor que ele vai rodar aqui para mim.

E agora o que que eu vou fazer? Eu preciso a minha função construtora, né? Então, basicamente ele vai retornar dessa forma aqui. Eu passo um `accountService`, passo a minha porta e ele retorna aqui um novo roteador, o meu `accountService` que eu passei e a minha porta bonitinha.

E agora eu vou criar um método aqui para mim chamado de `ConfigureRoutes`, tá? Então vou colocar aqui, ó, `func (s *Server) ConfigureRoutes()`. E essas rotas, o que que eu vou fazer agora? Eu preciso ter o meu `AccountHandler`. Lembra que a gente acabou de criar meu `AccountHandler`, onde eu tenho esses meus endpoints? Então o que que eu posso fazer aqui? Eu posso ter `accountHandler := handlers.NewAccountHandler(s.accountService)`. Onde eu preciso do meu `accountService` aqui pra mim. E agora o que que eu vou fazer? Simplesmente eu vou configurar o meu roteador `s.router.Post("/accounts", accountHandler.Create)`. É basicamente isso que ele vai fazer. Então toda vez que alguém mandar um `POST` para `/accounts`, ele vai chamar o método `Create` do meu `AccountHandler` aqui para mim. Beleza? E a gente precisa também do `GET /accounts`. Então, vou colocar o `Get /accounts`, que ele vai pegar o `accountHandler.Get` aqui para mim `s.router.Get("/accounts", accountHandler.Get)`. Então, com isso, eu já tenho as minhas rotas configuradas.

### Método Start

E agora, por último, o que que eu vou fazer? Eu vou criar o meu `Start` pro meu servidor, né? Então, vamos lá. Ele já até meio que colocou aqui para eu trabalhar, né? Então, `server`, onde eu vou retornar um `http.Server` ou um erro, porque o meu servidor pode tem algum problema para inicializar, mas aqui no meu servidor eu quero passar qual é a porta que ele vai rodar. Então vou colocar porta vai ser `":" + s.port` que eu passei lá no meu servidor. E eu preciso também aqui falar qual que é o handler meu, né? Qual que vai ser o roteador que eu vou utilizar. Então aqui eu vou colocar `Handler: s.router`, que é o meu `chi` ali que eu coloquei. E agora eu retorno o quê? O `http.ListenAndServe(addr, handler)`. O `ListenAndServe` significa que sobe o servidor aqui para mim no final das contas. Maravilha.

Então agora eu tenho já o meu servidor. Se você olhar aqui, tá? Eu tenho o `chi` na versão 5 que eu não posso esquecer, senão a gente já ia ter um probleminha, mas mesmo assim você vai ver que ele tá vermelhinho aqui embaixo. Por quê? Porque eu não baixei a dependência. Então se eu chegar aqui, dar um `go mod tidy`, você vai ver que ele vai baixar esse meu camarada. E agora eu não tenho mais erro aqui no meu sistema. Se tem algum erro é porque tem algo errado e não tem essa referência aqui passada. Agora não tô tendo mais nenhum erro aqui para mim.

Então, nesse momento, galera, a gente já implementou o nosso web server. Aqui a gente já implementou tudo que a gente precisava. Agora eu tenho que juntar as minhas peças. E para juntar as minhas peças, eu preciso ir no meu `main.go`.

## Configurando a Aplicação Principal (main.go)

O meu `main.go` é onde eu vou conseguir consolidar tudo que eu fiz até agora. Beleza? Mas o que que acontece? Uma das coisas que eu preciso fazer para facilitar a minha vida é, por exemplo, eu vou ter que ter credenciais de banco de dados, né? Então, o que que eu posso fazer aqui para mim? Eu tenho, tá, eu posso criar aqui para mim um arquivo chamado `.env`, onde eu vou guardar as minhas variáveis de ambiente. E nessas minhas variáveis de ambiente, eu posso guardar as minhas variáveis que são importantes pro meu banco de dados e pro meu servidor web, que é assim, ó:

```bash
PORT=8080
DB_HOST=localhost
DB_PORT=5432
DB_USER=postgres
DB_PASSWORD=postgres
DB_NAME=gateway
SSL_MODE=disable
```

Eu tô falando que a minha aplicação vai rodar na porta `8080`. O DB vai ser esse host. A gente vai trabalhar com Docker. Você vai ver logo logo como é que a gente vai fazer, tá? A porta `5432` postgres. Aqui vai ser o nome do meu banco de dados. Aqui vai ser o SSL mode disabled. Então aqui vai ser as minhas variáveis de ambiente.

Agora que eu tenho aqui isso aqui como variável de ambiente, o que que eu vou fazer aqui nesse meu `main.go`? Eu vou usar um pacote, tá? Que o Go tem, que ele é chamado de `github.com/joho/godotenv`. Onde ele lê esse arquivo `.env`, por exemplo, ou arquivos de variáveis de ambiente e carrega isso como variável de ambiente pra gente poder acessar. Então o que que eu vou fazer? Eu vou fazer o seguinte, eu vou colocar `if err := godotenv.Load(); err != nil { log.Fatal("Error loading .env file") }`. Por que que eu tô dizendo isso aqui, galera? Porque ele vai tentar executar e carregar meu arquivo `.env`. Se ele não conseguir, ele vai falar que deu um erro e vai matar a minha aplicação aqui para mim. Fechou? Então essa é a primeira coisa. Mas você vai ver que ele já pediu para importar o pacote do `godotenv`. Então eu vou dar um `go mod tidy` aqui para mim. E prontinho, ele importou esse meu pacote.

### String de Conexão com o Banco

Ah, agora que a gente tem isso, o que que eu vou precisar, galera? Eu vou precisar criar uma string de conexão para eu usar para chamar o meu banco de dados. Eu vou usar o Postgres para fazer isso. Então eu já tenho aqui meio que uma string de conexão pra gente concatenar ela, tá? E ela vai funcionar da seguinte forma.

```go
connStr := fmt.Sprintf("host=%s port=%s user=%s password=%s dbname=%s sslmode=%s",
    getEnv("DB_HOST", "localhost"),
    getEnv("DB_PORT", "5432"),
    getEnv("DB_USER", "postgres"),
    getEnv("DB_PASSWORD", "postgres"),
    getEnv("DB_NAME", "gateway"),
    getEnv("SSL_MODE", "disable"),
)
```

Olha só, tá vendo aqui que ele tá dando um `sprintf`, ou seja, ele vai pegar o host e aqui é uma string, porta string, user string, password string e ele vai substituir pelo `DB_HOST`, pelo `DB_PORT`, pelo `DB_USER`, pelo `DB_PASSWORD`, `DB_NAME` e `SSL_MODE`. Se ele não encontrar essas variáveis de ambiente, ele vai usar esses valores como padrão. Mas se você olhar aqui, ó, esse `getEnv` é uma função que não existe. Então a gente precisa criar essa função. E ela é uma função muito básica, ela verifica se existe essa variável de ambiente no GO. Se ela não existir, ela retorna esse valor padrão. Então, basicamente é uma função chamada assim, ó:

```go
func getEnv(key, defaultValue string) string {
    value := os.Getenv(key)
    if value == "" {
        return defaultValue
    }
    return value
}
```

Ele implementou de outra forma, mas eu quero implementar assim, ó. `getEnv` aqui para mim. Então, ele vai buscar se existe uma variável de ambiente, se existir, beleza, ele retorna a variável. Se não existir, ele vai trazer esse valor padrão que é o `defaultValue` aqui para mim.

Então, agora eu consegui nessa string ter a minha string de conexão com o banco de dados.

### Abrindo a Conexão com o Banco

Agora eu preciso configurar a minha conexão com o banco de dados. Então, vamos fazer isso agora.

```go
db, err := sql.Open("postgres", connStr)
if err != nil {
    log.Fatal("Error connecting to the database: ", err)
}
defer db.Close()
```

E eu tô falando que o que eu vou utilizar de banco de dados vai ser o `postgres` aqui para mim. Maravilha. E agora ele vai tentar abrir a minha conexão. Se ele der algum erro, ele vai dar erro `connecting to the database`. E lembra que eu falei para vocês do `defer close`? Ou seja, depois que executar tudo no sistema, aí ele vai encerrar a conexão com o banco de dados. Por isso que a gente já dá o `Close` aqui em cima, mas tem o `defer` aqui na frente.

### Inicializando a Árvore de Dependências

E agora que a gente fez isso, a gente precisa começar a montar a nossa árvore de objetos. Por exemplo, para eu precisar ter um `service`, eu preciso de um `repository`. Para eu ter um `handler`, eu preciso do `service`. Então, a gente tem que começar inicializando os nossos caras.

Então, para eu ter um repositório, eu preciso de um banco de dados. Por isso que eu criei o banco de dados aqui. Então, vamos lá. Vou colocar aqui, ó:

```go
accountRepository := repository.NewAccountRepository(db)
accountService := service.NewAccountService(accountRepository)
port := getEnv("PORT", "8080")
server := server.NewServer(accountService, port)
server.ConfigureRoutes()
```

Account repository, né? É igual a `NewAccountRepository`, recebendo um `db`. Maravilha. Tenho o meu banco de dados. Agora eu preciso do meu `accountService`. `AccountService`, eu tenho um `NewAccountService` que recebe um `repositório`. Então, vamos lá. O repositório recebe o `DB`, o `accountService` recebe o `repositório`. É basicamente isso que acaba acontecendo aqui com a gente.

E agora que a gente tem isso, eu vou fazer o seguinte, eu vou colocar assim, ó, `port := getEnv("PORT", "8080")`, que eu preciso aqui. E vou fazer o seguinte, ó. O meu `server` é igual `server.NewServer`, onde eu recebo o meu `accountService` e o meu `port`, né? A gente criou o nosso servidor web, lembra? Vou executar o meu `server.ConfigureRoutes()` aqui para mim. Eu poderia até já executar esse `ConfigureRoutes` aqui na hora que eu crio o no o nosso novo server, né? A gente poderia até fazer isso aqui, né? Quando eu configuro esse cara, ele já até executa a esse cara aqui pra gente. A gente poderia ter feito isso, mas beleza. Vou executar a configuração da minha rota.

E agora que eu executei a configuração da minha rota, eu simplesmente executo o meu servidor. Como que eu executo o meu servidor? Eu coloco `server.Start()` aqui para mim. E eu posso até verificar se ele tem um erro, né? Porque ele pode retornar. Então eu posso ver e se se tiver `error` no meu `server.Start()`, ele vai retornar o erro que ele teve aqui para mim.

E agora a minha aplicação está entre aspas pronta. Por que que eu tô dizendo que ela está entre aspas pronta, galera? Ela está entre aspas pronta porque eu preciso fazer a parte de banco de dados, né? A gente não tem o banco de dados, a gente não tem tabela de banco de dados, a gente não tem absolutamente nada nesse momento.

## Configurando o Banco de Dados com Docker

Então, é nesse momento que a gente vai utilizar um cara que a gente chama de **Docker**, né? O Docker é um sistema de containerização. E o container, o que que ele faz no final das contas? Ele consegue criar processos na sua máquina como se fosse uma máquina virtual. Beleza? Eu recomendo que você assista os materiais que a gente tem de Docker no nosso canal do YouTube, onde a gente fala muito sobre isso, mas a ideia é que com simples comandos a gente consegue subir banco de dados, a gente consegue subir as nossas aplicações, a gente consegue fazer o que a gente quiser, basicamente com qualquer tipo de sistema, sem ter que ficar instalando um monte de coisa na nossa máquina. A gente trabalha com contêiners, que são processos isolados no nosso computador, que são criados através de um container runtime, que nesse caso é o Docker, tá?

Então, se você não tem o Docker no seu computador, entre em `docker.com` e você pode baixar o Docker Desktop e você vai fazer a instalação. Se você tá instalando, usando o Mac, entre e instale o Docker Desktop. Ponto. Se você tá usando Windows e tiver um computador razoável, entre, instale o Docker Desktop, mas tenha o WSL2 instalado no seu Windows, porque você tem o Linux dentro do seu Windows e aí o Docker tem integração e daí fica tudo muito mais fácil para você aí trabalhar com o Docker Desktop, que é uma ferramenta fantástica para você trabalhar com Docker. Legal.

Nesse meu caso aqui, tá? Eu instalei o Docker Desktop no meu Mac aqui, somente para você ver a cara dele. Aqui eu tenho dois contêiners rodando, mas faça isso no Windows, tá? Se você tá usando Windows, ou se você não quiser instalar o Docker Desktop no Windows, você pode usar o Docker Engine, né? ou Docker Community Edition e instalá-lo no WSL2 do seu Windows normalmente. E se você tá utilizando o Linux, você pode instalar facilmente também o Docker Community Edition, ou o Docker Engine. É a mesma coisa, basicamente. Teve um, vamos dizer assim, renomearam o Docker Engine para Docker Community Edition. Basicamente é isso, tá? Então, dúvidas em relação a isso. A gente vai deixar disponível também um tutorial se você usa Windows para você instalar o WSL2 no seu computador e depois em seguida instalar o Docker. O Docker aqui vai ser uma peça fundamental. Por quê? Porque quando nós formos grudar todos os nossos sistemas para chegar nessa configuração aqui, tudo isso a gente vai fazer utilizando o Docker. Legal.

### Docker Compose para o PostgreSQL

Então essa que é a nossa, o nosso primeiro ponto aqui. Mas o que que acontece para eu rodar aqui, nesse meu caso, o Docker, a minha ideia é eu conseguir no final das contas rodar o meu Postgres, tá? Eu quero um Postgres bonitão aqui para eu trabalhar. Então eu vou criar aqui para mim um arquivo chamado de `docker-compose.yml`. Nesse arquivo `docker-compose.yml` o que que vai acontecer? Existe um serviço que eu vou criar bonitinho aqui, somente para você ver, ó:

```yaml
version: '3.8'
services:
  db:
    image: postgres:16-alpine
    ports:
      - "5432:5432"
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: gateway
    volumes:
      - postgres_data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 10s
      timeout: 5s
      retries: 5
    restart: unless-stopped

volumes:
  postgres_data:
```

Services. E o nome desse service vai ser `db`, onde ele vai pegar o Postgres, uma imagem do Postgres, certo? Na versão `16-alpine`, vai falar que a porta que vai ser utilizada é a `5432` aqui no meu Postgres. E por que que ele tá usando esse aqui `5432:5432`? Porque eu vou ser capaz também de acessar o meu banco de dados da minha máquina sem eu precisar entrar no meu contêiner ou dentro da rede do docker. Então eu consigo pela minha máquina local mesmo normal eu consigo acessar aqui variável de ambiente usuário e senha a Postgres e o nome do banco de dados é `gateway`. Aqui é onde ele vai guardar os dados do Postgres e ele vai usar um volume do docker, ou seja, para quando eu deletar, vamos dizer assim, ou quando eu quando eu destruí esse contêiner, né? Se em algum momento esse contêiner cair. Quando eu criar um novo, ele vai ainda achar os dados aqui do Postgres aqui para mim através desse volume chamado `postgres_data`. Então tudo que for tiver do os dados do banco de dados nessa pasta do Postgres, que é onde ele guarda as tabelas, os esquemas, tudo bonitinho, ele vai guardar na minha máquina local dentro desse volume. E aqui é um `healthcheck`, galera, para verificar se o banco de dados tá de pé aqui para mim. E se o sistema cair, ele vai tentar restartar aqui para mim. E por último, aqui embaixo, eu preciso dos meus volumes, né? que é o meu `postgres_data`, porque eu tô definindo ele aqui em cima.

Eu fazendo isso aqui vai me ajudar a fazer o que no final das contas? A subir o meu banco de dados. Então você vai perceber aqui que nesse momento eu vou rodar um comando chamado de `docker-compose up -d`. E ele vai subir o meu Postgres agora aqui para mim todo bonitão.

### Migrations do Banco de Dados

Uma vez que eu consegui subir o meu Postgres tudo bonitão aqui para mim. Ah, obviamente eu preciso criar as tabelas do banco de dados, né? Não tem muito para onde eu escapar. Eu preciso ter as tabelas do meu banco de dados. Eu tenho, tá, uma pastinha que eu chamei aqui de `migrations`, que eu vou colar aqui dentro do nosso projeto. Essa pastinha `migrations`, ela tem dois arquivos. Um arquivo chamado `001_create_accounts_table.up.sql`, que ele vai criar uma tabela aqui de `accounts` aqui para mim. E o `down.sql` é quando eu quiser aqui deletar essas minhas tabelas. O `account` e o `invoice` estão aqui mesmo. Aqui já estão tudo aqui dentro do mesmo cara, tá? A gente já deixa tudo aqui dentro dessa tabela mesmo, não tem problema. Ele vai criar as duas tabelas no banco.

E aí você deve estar pensando como que eu aplico essas migrações, né? Ou seja, como que eu executo isso no banco de dados. Eu poderia copiar esse SQL e executar lá no Postgres para fazer isso, não teria nenhum problema. Mas o Gol, ele tem um pacote que a gente que é que a gente chama de `golang-migrate/migrate`. E esse cara aí ele ajuda a gente a executar migrações, tá?

#### Instalando o Go-Migrate

Então o que que você pode fazer aqui? Tá? Você vai dar um `go install -tags 'postgres' github.com/golang-migrate/migrate/v4/cmd/migrate@latest`, que é basicamente o pacote que a gente tá que eu tô falando aqui para você. Como que você faz isso? No final das contas, eu vou deixar no README para você poder instalar na sua máquina, mas basicamente é esse o comando ele é grande, mas na realidade ele é bem simples. Eu tô dando um `go install` já instalar esse cara com driver do Postgres. E aqui é o caminho da onde esse pacote, do GitHub de quem criou esse pacote, tá? E daí quando eu faço isso, eu tenho o `migrate` instalado na minha máquina. Se eu digitar aqui `migrate`, você vai ver que eu tenho ele aqui na minha máquina.

#### Executando as Migrations

E aí para eu executar essas minhas migrações, eu tenho um outro comando grandão, né? Ah, porque eu tenho que passar a string inteira do meu banco de dados, tá? Inclusive, eu vou fazer uma alteração temporária aqui no meu `.env`, eu vou falar que o `DB_HOST` por enquanto é `localhost` (no próximo vídeo a gente vai executar tudo dentro do Docker, inclusive o GO, e aí vai ser o nome do nosso contêiner, da nossa do nosso serviço que é `db`. Por enquanto eu tô deixando aqui como `localhost`, tá?).

E aí o que que eu posso fazer? Pedir para ele executar essa minha migração, que é um comando gigante. Uma dica, se você tiver usando o cursor, talvez funcione isso aqui, ó. Eu posso até colocar assim, ó. Eu dou um comando de `Ctrl+K` e falar assim, ó. `Execute a migrate usando o golang-migrate`. Ah, para o Postgres. Vamos ver o que que ele vai aparecer aqui para mim. Olha só, `migrate -database postgres://postgres:postgres@localhost:5432/gateway?sslmode=disable -path migrations up`. Se você olhar aqui, ó, tem `-path migrations`, onde que ele vai precisar executar e o comando que é de `up`. Então, é esse, é o comandão que ele coloca aqui, ó, o cursor ele já logo entende pra gente trabalhar aqui com a parte de IA nos ajudando. Então, eu vou dar um enter. Vamos ver se essa parada aqui vai funcionar. Vou dar um enter. E prontinho, ele criou a nossa tabela do banco de dados. Bonitona.

## Testando a Aplicação

Agora que a gente já tem o nosso banco de dados criado, arquivos de conexão. A nossa aplicação em tese, ela tá pronta. O que que a gente faz aqui no meio da história aqui pra gente? Galera, a gente pode trabalhar da seguinte forma. Eu posso dar um `go run cmd/app/main.go`. E aqui ele tá dizendo que ele não encontrou o driver do Postgres. Por que que ele tá falando isso, galera? Porque o seguinte, aqui eu tô falando que eu vou usar o Postgres, mas eu não importei o driver. Então aqui em cima no import, eu vou colocar o driver do Postgres `_ "github.com/lib/pq"`. E por que que eu tô colocando o underline `_` aqui antes? Eu tô colocando esse underline exatamente para dizer o seguinte: eu não vou usar diretamente esse driver, eu não vou usar esse pacote diretamente, mas esse `SQL Open` vai utilizar ele. Então eu coloco underline pro Gol não ficar bravo comigo e deixar eu compilar o meu programa. Vou dar um `go mod tidy`. Ele vai baixar o driver e eu vou tentar executar minha aplicação novamente.

E agora a minha aplicação tá rodando aí na minha porta `8080`. E aí chega no momento onde a gente vai ter que testar tudo isso aí, né? Ah, e pra gente testar esse cara, como é que nós vamos fazer? Nós podemos utilizar uma biblioteca chamada `REST Client` do VS Code, né? E o REST Client do VS Code ele permite que a gente crie, no final das contas um arquivo chamado, eu vou criar, por exemplo, `teste.http`. E nesse `teste.http` eu posso fazer o seguinte, ó. Eu vou até copiar e colar dois caras aqui que eu sei que são importantes, tá? Só para você entender o que que ele faz aqui, ó.

```http
### Create Account
POST http://localhost:8080/accounts
Content-Type: application/json

{
  "name": "John Doe",
  "email": "john@doe.com"
}

### Get Account
GET http://localhost:8080/accounts
x-api-key: {{apiKey}}
```

Um é, tô falando qual é a minha URL, que é `http://localhost:8080/accounts`. Tô falando que que a minha API key, ela vai vir do meu `Create Account`, que é dessa minha rota `response.api_key`. Legal. E aqui, ó, é onde eu posso dar um `POST` passando `John Doe`. E aqui é para eu conseguir dar um `GET` passando a minha API key que ele pegou daqui dessa minha requisição anterior. Então, não sei se a gente consegue fazer a nossa aplicação rodar de primeiro. É muito difícil, mas eu vou dar um `Send Request` aqui pra gente. Vamos ver se vai funcionar.

Galera, rodou de primeira, hein? Olha só, né? Quase um milagre. Então, ó, criamos a nossa aplicação, né? Retornou o nosso `balance` e agora aqui eu vou dar um `accounts` no `GET` aqui pra gente, né? Ah, o `GET` ele vai pegar os dados da minha conta. Então, vou dar um `Send Request` `GET` e ele conseguiu pegar, né? `200 OK`, conseguiu pegar os dados.

## Conclusão e Próximos Passos

Então, a nossa missão aqui, ela está cumprida nessa aula, galera. A gente conseguiu fazer exatamente o que nós precisávamos, criar toda a parte referente a `account`. Nosso próximo passo depois disso vai ser criar tudo referente a `invoice` e depois no final de tudo a gente vai fazer a integração com o Kafka.

Então galera, tivemos aí uma longa jornada aí, mas eu estou muito feliz da gente ter conseguido fazer rodar e ainda de primeira aqui parece um milagre acontecendo, mas a gente conseguiu fazer essa parada funcionar.

Lembrando que o repositório desse projeto vai ficar disponível no nosso GitHub, vai tá na descrição do vídeo. Inclusive todas essas informações da imersão. Então dê uma olhada aí nas descrições para você ter acesso ao repositório aí desse nosso primeiro dia aqui de projeto. Fechou?

Bom pessoal, espero que vocês tenham gostado. Então até o nosso próximo vídeo e lembre-se que aqui na Full Cycle a nossa ideia é conseguir acompanhar você em todos os momentos da sua carreira. A gente tem diversos cursos, pós-graduações e se você tiver interesse de saber, entra aí no nosso site, saiba mais sobre o curso Full Cycle, onde ele vai te ensinar muito, mas muito além de tudo isso que eu tô te mostrando aqui durante essa semana. Fechou? Um grande abraço e é isso aí.
