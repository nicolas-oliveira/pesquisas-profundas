<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

# Guia Passo a Passo: Iniciando Projeto React + Next.js + Shadcn UI com TypeScript

## 1. Criar o projeto inicial com Next.js e TypeScript

Use o comando `npx create-next-app` que já gera um template configurado com React, Next.js e TypeScript:

```bash
npx create-next-app@latest my-app \
  --typescript \
  --eslint \
  --src-dir \
  --app
```

Explicação das flags:

- `--typescript` ativa TypeScript.
- `--eslint` configura ESLint.
- `--src-dir` coloca o código fonte em `src/`.
- `--app` usa o App Router (pasta `app/`) em vez do Pages Router.


## 2. Navegar até a pasta do projeto

```bash
cd my-app
```


## 3. Instalar o Shadcn UI CLI

O Shadcn UI fornece componentes estilizados e um gerador de componentes. Instale globalmente ou como dependência de desenvolvimento:

```bash
npm install -D shadcn-ui
# ou, se preferir yarn:
# yarn add -D shadcn-ui
```


## 4. Inicializar o Shadcn UI no projeto

Dentro de `my-app`, execute:

```bash
npx shadcn-ui init
```

Isso vai:

1. Criar pasta `components/ui/` com estilos e configurações.
2. Gerar arquivo `tailwind.config.ts` (caso use Tailwind CSS).
3. Ajustar arquivos de configuração (`tsconfig.json`, `next.config.js`).

> **Observação:** O Shadcn UI pressupõe Tailwind CSS. Se ainda não tiver, execute:

```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

E configure `tailwind.config.ts` contendo:

```ts
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./src/app/**/*.{ts,tsx,js,jsx}",
    "./src/components/**/*.{ts,tsx,js,jsx}",
  ],
  theme: { extend: {} },
  plugins: [],
}
```


## 5. Instalar dependências do Tailwind CSS (se necessário)

Caso o `shadcn-ui init` não tenha instalado:

```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```


## 6. Configurar estilos globais

Em `src/app/globals.css` (ou `src/styles/globals.css`), adicione:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

E importe em `src/app/layout.tsx`:

```tsx
import './globals.css';
export const metadata = { title: 'My App' };
export default function RootLayout({ children }: { children: React.ReactNode }) {
  return <html lang="pt-BR"><body>{children}</body></html>;
}
```


## 7. Gerar e usar componentes Shadcn UI

Para criar um componente (por exemplo, um botão estilizado):

```bash
npx shadcn-ui add button
```

Isso vai:

- Gerar `src/components/ui/button.tsx` com variantes já estilizadas.
- Atualizar tipos e exportações.

Use em sua página:

```tsx
import { Button } from '@/components/ui/button';

export default function Home() {
  return (
    <main className="p-8">
      <Button variant="primary">Clique Aqui</Button>
    </main>
  );
}
```


## 8. Executar em modo de desenvolvimento

```bash
npm run dev
# ou
# yarn dev
```

Acesse `http://localhost:3000` para ver seu app rodando.

Pronto! Você agora tem um projeto Next.js com React, TypeScript e Shadcn UI totalmente configurado e pronto para desenvolvimento.

