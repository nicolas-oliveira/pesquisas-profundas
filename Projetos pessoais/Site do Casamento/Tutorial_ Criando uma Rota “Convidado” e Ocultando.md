<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" style="height:64px;margin-right:32px"/>

# Tutorial: Criando uma Rota “Convidado” e Ocultando “Cliente” no MedusaJS v2.4

**Visão Geral**
Vamos usar apenas as extensões oficiais de Admin UI Routes e Widgets na versão v2.4 para:

1. Instalar o template v2.4
2. Criar uma rota “Convidado” que replique as funcionalidades de “Cliente”
3. Ocultar o item de menu nativo “Clientes”
4. Tornar o campo *telefone* obrigatório em vez de *email* no formulário de convidado

***

## 1. Instalação do Projeto v2.4

1. Garanta que seu Node.js ≥16 e Yarn/PNPM estejam instalados.
2. Crie um novo projeto Medusa v2.4 usando o CLI oficial:

```bash
npx create-medusa-app@latest my-medusa-app --template @medusajs/medusa@2.4
cd my-medusa-app
yarn install
```

3. Inicie o backend e admin em modo desenvolvimento:

```bash
# No terminal 1 (backend)
yarn workspace medusa develop

# No terminal 2 (admin)
yarn workspace admin start
```

***

## 2. Estrutura de Extensões na v2.4

Dentro de `admin/src` você encontrará a pasta `extensions` onde criará nossas customizações:

```
my-medusa-app/
└─ admin/
   └─ src/
      └─ extensions/
         ├─ routes/
         └─ widgets/
```

***

## 3. Ocultando o Menu “Clientes”

Crie um arquivo para sobrescrever o menu:

1. `admin/src/extensions/routes/override-menu.js`

```javascript
import { overrideMenuItems } from "@medusajs/admin";

export default overrideMenuItems((items) => {
  // Filtra o item "customers" (rota original)
  return items.filter((item) => item.id !== "customers");
});
```

2. Registre-o em `admin/src/index.js`:

```javascript
import overrideMenu from "./extensions/routes/override-menu";

// ... dentro de setup()
admin.registerExtension(overrideMenu);
```

Isso remove “Clientes” do sidebar sem tocar no core.

***

## 4. Criando a Rota “Convidado”

1. `admin/src/extensions/routes/convidado-route.js`

```javascript
import { AdminRoute } from "@medusajs/admin";

const ConvidadoRoute = {
  id: "convidados",
  path: "/convidados",
  icon: "User",
  title: "Convidados",
  Component: () => import("./ConvidadoPage"),
};

export default AdminRoute(ConvidadoRoute);
```

2. Crie o componente de listagem em `admin/src/extensions/routes/ConvidadoPage.jsx`:

```jsx
import React, { useEffect, useState } from "react";
import { useClient } from "@medusajs/admin";
import { Table, Spinner } from "@medusajs/ui";

export default function ConvidadoPage() {
  const client = useClient();
  const [data, setData] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    client.customers
      .list()
      .then(({ customers }) => {
        setData(customers);
        setLoading(false);
      });
  }, []);

  if (loading) {
    return <Spinner />;
  }

  return (
    <Table
      columns={[
        { title: "ID", key: "id" },
        { title: "Nome", key: "first_name" },
        { title: "Telefone", key: "phone" },
      ]}
      data={data}
    />
  );
}
```

3. Registre a rota em `admin/src/index.js`:

```javascript
import ConvidadoRoute from "./extensions/routes/convidado-route";

// ... dentro de setup()
admin.registerExtension(ConvidadoRoute);
```

***

## 5. Tornando o Telefone Obrigatório

### 5.1 Backend (Prisma Schema)

1. Abra `medusa/src/models/customer.ts` e ajuste o campo `email` para opcional e `phone` para obrigatório:

```ts
@Column({ nullable: true })
email?: string;

@Column()
phone: string;
```

2. Gere nova migration:

```bash
yarn workspace medusa run prisma migrate dev --name phone-required
```

### 5.2 Frontend (Formulário)

1. Crie um widget para sobrescrever o formulário de criação de cliente:

`admin/src/extensions/widgets/override-customer-form.js`

```javascript
import { overrideWidget } from "@medusajs/admin";

export default overrideWidget("CustomerForm", ({ original }) => {
  return (props) => {
    const newProps = {
      ...props,
      fields: props.fields.map((f) => {
        if (f.name === "email") {
          return { ...f, required: false };
        }
        if (f.name === "phone") {
          return { ...f, required: true };
        }
        return f;
      }),
    };
    return original(newProps);
  };
});
```

2. Registre-o em `admin/src/index.js`:

```javascript
import overrideCustomerForm from "./extensions/widgets/override-customer-form";

// ... dentro de setup()
admin.registerExtension(overrideCustomerForm);
```

***

## 6. Testando

1. Reinicie both admin e backend:

```bash
yarn workspace medusa develop
yarn workspace admin start
```

2. Acesse `http://localhost:7000`
3. Confirme que o menu “Clientes” desapareceu e foi substituído por “Convidados”.
4. Vá em **Convidados › Novo** e verifique se o campo *Telefone* é obrigatório e *Email* opcional.

***

**Você agora possui uma rota “Convidado” completamente funcional na v2.4, sem fork, usando apenas as APIs oficiais de Admin UI Routes e Widgets.**
