# Documentação Medusa v2.4.0

A Medusa é uma **plataforma de comércio digital com uma estrutura integrada para customização**. Ela é construída tendo a customização em mente, o que significa que, ao invés de usar soluções alternativas complexas (*hacky workarounds*) que são difíceis de manter e escalar, seus esforços se concentram em construir funcionalidades que concretizam a visão do seu negócio. A Medusa é adequada para empresas e equipes de todos os tamanhos que buscam uma plataforma para implementar requisitos exclusivos que outras plataformas não suportam nativamente.

A Medusa é fornecida com três ferramentas principais:

1. Um conjunto de **módulos de comércio** com funcionalidades essenciais, como rastreamento de inventário, gerenciamento de pedidos e muito mais.
2. Uma **estrutura para a construção de funcionalidades personalizadas**, incluindo ferramentas para introduzir endpoints de API, lógica de negócios e modelos de dados; construir *workflows* e automações; e integrar com serviços de terceiros.
3. Um **painel de administração customizável** para os comerciantes configurarem e operarem suas lojas.

### Customização e Extensão

A flexibilidade do Medusa permite que você **crie recursos personalizados** usando três ferramentas principais: **Módulos**, **Workflows** e **Rotas de API**. Além disso, o Painel Administrativo do Medusa é extensível, permitindo que você personalize a interface do usuário para utilizar novos recursos.

#### Customização do Painel Admin

Você pode customizar o *dashboard* administrativo inserindo componentes, chamados **widgets**, em páginas existentes ou **adicionando novas páginas, chamadas UI Routes**.

1. **Widgets Admin** *Widgets* são componentes React que permitem aos usuários administradores realizar ações personalizadas e podem ser inseridos em zonas de injeção pré-definidas. Você cria um *widget* em um arquivo `.tsx` sob o diretório `src/admin/widgets`.

O exemplo a seguir mostra um *widget* injetado na zona `product.details.before`:

```ts
import { defineWidgetConfig } from "@medusajs/admin-sdk"
import { Container, Heading } from "@medusajs/ui"

// The widget
const ProductWidget = () => {
	return (
		<Container>Product Widget</Container>
	)
}

// The widget's configurations
export const config = defineWidgetConfig({
	zone: "product.details.before",
})
export default ProductWidget
```

2. **UI Routes Admin** As rotas de UI permitem adicionar novas páginas ao *dashboard* administrativo. Você cria uma rota de UI em um arquivo `page.tsx` sob um subdiretório de `src/admin/routes`. Para adicionar um item de barra lateral para sua rota de UI personalizada, você exporta um objeto de configuração.

O exemplo a seguir mostra uma rota de UI personalizada com configuração para exibição na barra lateral:

```ts
import { defineRouteConfig } from "@medusajs/admin-sdk"
import { ChatBubbleLeftRight } from "@medusajs/icons"
import { Container, Heading } from "@medusajs/ui"

const CustomPage = () => {

	return (
		<Container>This is my custom route</Container>
	)
}

export const config = defineRouteConfig({
	label: "Custom Route",
	icon: ChatBubbleLeftRight,
})

export default CustomPage

```

#### API Routes Personalizadas

Uma Rota de API é um *endpoint* que expõe recursos de comércio para aplicações externas, como *storefronts*, o *dashboard* administrativo ou sistemas de terceiros. Você pode criar rotas de API personalizadas para expor suas funcionalidades customizadas.

O exemplo a seguir demonstra uma Rota de API `POST` que aceita parâmetros no corpo da requisição:

```ts

import type {
	MedusaRequest,
	MedusaResponse,
} from "@medusajs/framework/http"

type HelloWorldReq = {
	name: string
}

export const POST = async (
	req: MedusaRequest<
		HelloWorldReq
	>,
	res: MedusaResponse
) => {
	res.json({
		message: `[POST] Hello ${req.body.name}!`,
	})
}

```

# UI Routes

As **UI Routes** (Rotas de Interface do Usuário) são páginas novas que você pode adicionar ao Painel Administrativo do Medusa. Elas funcionam como componentes React que exibem conteúdo personalizado, permitindo que os usuários administradores executem ações específicas para as suas customizações.

A customização do *dashboard* Admin é realizada de duas maneiras principais: adicionando novas seções a páginas existentes usando *Widgets* ou adicionando **novas páginas** usando UI Routes.

Por exemplo, você pode adicionar uma nova página para mostrar e gerenciar avaliações de produtos, algo que não está disponível nativamente no Medusa.

### Como Criar uma UI Route

Você cria uma UI Route em um arquivo `page.tsx` sob um subdiretório do diretório `src/admin/routes`. O caminho do arquivo em relação a `src/admin/routes` determina o caminho da rota no *dashboard*.

O componente React da UI Route deve ser a exportação padrão do arquivo e deve ser criado como uma *arrow function* (função de seta). É recomendado usar o pacote **Medusa UI** dentro das rotas para manter um design consistente com o *dashboard*.

Abaixo estão exemplos didáticos de como criar e configurar UI Routes:

#### Exemplo 1: Rota Simples de Visualização de Dados

Este exemplo cria uma nova página básica que é acessível através de uma URL específica.

**Caminho do Arquivo:** `src/admin/routes/custom/page.tsx`

**Acesso:** `http://localhost:9000/app/custom`

```ts
import { Container, Heading } from "@medusajs/ui" // Usando componentes Medusa UI

// O componente da UI Route deve ser uma arrow function
const CustomPage = () => {

    return (
        <Container>
            {/* O conteúdo da sua nova página */}
            <Heading>Esta é minha rota personalizada</Heading>
        </Container>
    )
}

// Exportação padrão do componente
export default CustomPage
```

#### Exemplo 2: Rota Exibida na Barra Lateral (Sidebar)

Para que a sua UI Route apareça na barra lateral de navegação do Admin, você deve exportar um objeto de configuração usando `defineRouteConfig`.

**Caminho do Arquivo:** `src/admin/routes/custom/page.tsx`

```ts
import { defineRouteConfig } from "@medusajs/admin-sdk"
import { ChatBubbleLeftRight } from "@medusajs/icons" // Um ícone do pacote Medusa UI Icons
import { Container, Heading } from "@medusajs/ui"

const CustomPage = () => {

    return (
        <Container>
            <Heading>Página de Suporte Personalizada</Heading>
        </Container>
    )
}

// A configuração define o item da barra lateral
export const config = defineRouteConfig({
    // O rótulo que aparecerá na barra lateral
    label: "Minha Rota Customizada",
    // O ícone opcional para a barra lateral
    icon: ChatBubbleLeftRight,
})

export default CustomPage
```

Este exemplo adiciona um novo item na barra lateral com o rótulo "Minha Rota Customizada".

#### Exemplo 3: Rota com Parâmetros de Caminho (Path Parameters)

Você pode criar rotas dinâmicas que aceitam parâmetros, úteis para páginas de detalhes (ex: `/app/custom/123`). Para isso, use o formato de diretório `[param]` no caminho do arquivo e acesse-o usando o *hook* `useParams` do `react-router-dom`.

**Caminho do Arquivo:** `src/admin/routes/custom/[id]/page.tsx`

```ts
import { useParams } from "react-router-dom" // Necessário para acessar parâmetros
import { Container, Heading } from "@medusajs/ui"

const CustomPage = () => {

    // Acessa o parâmetro 'id' definido no nome do diretório
    const { id } = useParams()

    return (
        <Container>
            {/* Demonstra o uso do parâmetro recebido */}
            <Heading>Detalhes do Item Customizado com ID: {id}</Heading>
        </Container>
    )
}

export default CustomPage
```

Se você executar a aplicação Medusa e for para `localhost:9000/app/custom/123`, você verá o valor `123` impresso na página.

**Observação:** UI Routes dinâmicas aninhadas (como esta) não são adicionadas à barra lateral, pois não é possível criar um *link* para uma rota dinâmica.

#### Exemplo 4: Criação de uma Página de Configurações (Settings Page)

Para adicionar uma página na seção de **configurações** do *dashboard* Admin, você deve criar a UI Route sob o caminho `src/admin/routes/settings`.

**Caminho do Arquivo:** `src/admin/routes/settings/custom/page.tsx`

```ts
import { defineRouteConfig } from "@medusajs/admin-sdk"
import { Container, Heading } from "@medusajs/ui"

const CustomSettingPage = () => {

    return (
        <Container>
            <Heading>Página de Configurações Personalizadas</Heading>
        </Container>
    )
}

export const config = defineRouteConfig({
    // O rótulo que aparecerá na barra lateral de Configurações
    label: "Custom",
})

export default CustomSettingPage
```

Isso adiciona uma página no caminho `/app/settings/custom` e um item com o rótulo `Custom` na barra lateral de configurações.


A principal finalidade de um **Widget Admin** no Medusa é permitir a **customização das páginas existentes** do Painel Administrativo.

Os *widgets* são ferramentas fundamentais para estender a interface do usuário do administrador:

- **Definição:** Um *widget* é um **componente React** que é inserido em **zonas de injeção pré-definidas** nas páginas do *dashboard* Medusa Admin.
- **Funcionalidade:** Eles são criados para permitir que os usuários administradores **realizem ações personalizadas** ou visualizem **conteúdo personalizado**.
- **Exemplos de Uso:**
  - Adicionar um *widget* na página de detalhes do produto que permita aos usuários administradores **sincronizar produtos com um serviço de terceiros**.
  - Mostrar a marca de um produto na sua página de detalhes.

### Detalhes Operacionais

- **Zonas de Injeção:** As páginas do *dashboard* Medusa Admin são personalizáveis para inserir *widgets* de conteúdo personalizado em zonas de injeção predefinidas.
- **Dados Recebidos:** Os *widgets* que são injetados em uma **página de detalhes** recebem um *prop* chamado `data`. Este *prop* contém os **dados principais** da página de detalhes. Por exemplo, um *widget* injetado na zona `product.details.before` recebe os detalhes do produto no *prop* `data`.
- **Configuração:** O arquivo do *widget* deve exportar um objeto de configuração criado com `defineWidgetConfig` que indica a `zone` (zona) onde o *widget* deve ser injetado.






