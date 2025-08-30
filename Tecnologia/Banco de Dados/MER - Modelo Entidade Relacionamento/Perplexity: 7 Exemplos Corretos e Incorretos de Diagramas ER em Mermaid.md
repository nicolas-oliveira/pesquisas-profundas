# Perplexity: 7 Exemplos Corretos e Incorretos de Diagramas ER em Mermaid

## 1. Relacionamento Um-para-Muitos

### ❌ **INCORRETO** - Símbolos de cardinalidade errados

```mermaid
erDiagram
    CLIENTE -->> PEDIDO : faz
    CLIENTE {
        int id PK
        string nome
    }
    PEDIDO {
        int id PK
        int cliente_id FK
    }
```

### ✅ **CORRETO** - Cardinalidade correta para um-para-muitos

```mermaid
erDiagram
    CLIENTE ||--o{ PEDIDO : faz
    CLIENTE {
        int id PK
        string nome
    }
    PEDIDO {
        int id PK
        int cliente_id FK
    }
```

***

## 2. Relacionamento Muitos-para-Muitos

### ❌ **INCORRETO** - Relacionamento direto sem entidade associativa

```mermaid
erDiagram
    ESTUDANTE ||--|| CURSO : estuda
    ESTUDANTE {
        int id PK
        string nome
    }
    CURSO {
        int id PK
        string titulo
    }
```

### ✅ **CORRETO** - Relacionamento muitos-para-muitos adequado

```mermaid
erDiagram
    ESTUDANTE }o--o{ MATRICULA : possui
    CURSO }o--o{ MATRICULA : possui
    ESTUDANTE {
        int id PK
        string nome
    }
    CURSO {
        int id PK
        string titulo
    }
    MATRICULA {
        int estudante_id FK
        int curso_id FK
        date data_matricula
    }
```

***

## 3. Sintaxe de Atributos

### ❌ **INCORRETO** - Sintaxe de atributos mal formada

```mermaid
erDiagram
    PRODUTO {
        id: integer PRIMARY KEY,
        nome: varchar(100),
        preco: decimal(10,2)
    }
```

### ✅ **CORRETO** - Sintaxe adequada para atributos

```mermaid
erDiagram
    PRODUTO {
        int id PK
        string nome
        decimal preco
        date criado_em
    }
```

***

## 4. Relacionamento com Linha Pontilhada

### ❌ **INCORRETO** - Uso inadequado de linha pontilhada

```mermaid
erDiagram
    FUNCIONARIO ||..o{ DEPARTAMENTO : trabalha
    FUNCIONARIO {
        int id PK
        string nome
        int departamento_id FK
    }
    DEPARTAMENTO {
        int id PK
        string nome
    }
```

### ✅ **CORRETO** - Relacionamento identificativo vs não-identificativo

```mermaid
erDiagram
    FUNCIONARIO }o--|| DEPARTAMENTO : pertence
    FUNCIONARIO ||..|{ PROJETO : trabalha_em
    FUNCIONARIO {
        int id PK
        string nome
        int departamento_id FK
    }
    DEPARTAMENTO {
        int id PK
        string nome
    }
    PROJETO {
        int id PK
        string nome
    }
```

***

## 5. Entidade com Chaves Múltiplas

### ❌ **INCORRETO** - Múltiplas chaves primárias incorretas

```mermaid
erDiagram
    ITEM_PEDIDO {
        int pedido_id PK
        int produto_id PK
        int quantidade
        decimal preco_unitario
    }
```

### ✅ **CORRETO** - Chave primária composta adequada

```mermaid
erDiagram
    ITEM_PEDIDO {
        int pedido_id FK
        int produto_id FK
        int quantidade
        decimal preco_unitario
    }
    PEDIDO ||--o{ ITEM_PEDIDO : contem
    PRODUTO ||--o{ ITEM_PEDIDO : incluido_em
```

***

## 6. Relacionamento Um-para-Um

### ❌ **INCORRETO** - Cardinalidade errada para um-para-um

```mermaid
erDiagram
    PESSOA ||--o{ PASSAPORTE : possui
    PESSOA {
        int id PK
        string nome
    }
    PASSAPORTE {
        string numero PK
        int pessoa_id FK
    }
```

### ✅ **CORRETO** - Relacionamento um-para-um adequado

```mermaid
erDiagram
    PESSOA ||--|| PASSAPORTE : possui
    PESSOA {
        int id PK
        string nome
    }
    PASSAPORTE {
        string numero PK
        int pessoa_id FK
    }
```

***

## 7. Labels de Relacionamento

### ❌ **INCORRETO** - Labels ausentes ou inadequados

```mermaid
erDiagram
    AUTOR ||--o{ LIVRO
    AUTOR {
        int id PK
        string nome
    }
    LIVRO {
        int id PK
        string titulo
        int autor_id FK
    }
```

### ✅ **CORRETO** - Labels descritivos nos relacionamentos

```mermaid
erDiagram
    AUTOR ||--o{ LIVRO : escreve
    EDITORA ||--o{ LIVRO : publica
    AUTOR {
        int id PK
        string nome
    }
    LIVRO {
        int id PK
        string titulo
        int autor_id FK
        int editora_id FK
    }
    EDITORA {
        int id PK
        string nome
    }
```
