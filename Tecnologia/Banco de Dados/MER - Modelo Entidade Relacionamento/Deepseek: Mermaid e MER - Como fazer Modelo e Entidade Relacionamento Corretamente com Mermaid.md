# Deepseek: Mermaid e MER - Como fazer Modelo e Entidade Relacionamento Corretamente com Mermaid

Aqui estão 10 exemplos de diagramas de entidade-relacionamento em Mermaid, com versões incorretas (❌) e corretas (✅):

### 1. Relacionamento Binário Básico

**❌ Errado (sintaxe de cardinalidade incorreta)**

```mermaid
erDiagram
    CLIENTE ||--o{ PEDIDO : "faz"
    PEDIDO {
        int id
        date data
    }
```

**✅ Correto**

```mermaid
erDiagram
    CLIENTE ||--o{ PEDIDO : "faz"
    CLIENTE {
        int id
        string nome
    }
    PEDIDO {
        int id
        date data
    }
```

---

### 2. Relacionamento com Atributo

**❌ Errado (atributo em relacionamento não suportado)**

```mermaid
erDiagram
    ALUNO }|--|| CURSO : "matriculado_em" {
        date data_matricula
    }
```

**✅ Correto (usar entidade associativa)**

```mermaid
erDiagram
    ALUNO }o--o{ CURSO : "matricula"
    MATRICULA {
        date data_matricula
    }
    ALUNO ||--o{ MATRICULA : "possui"
    CURSO ||--o{ MATRICULA : "inclui"
```

---

### 3. Relacionamento Ternário

**❌ Errado (Mermaid não suporta n-ários diretamente)**

```mermaid
erDiagram
    PROJETO ||--|{ FUNCIONARIO : "aloca"
    PROJETO ||--|{ DEPARTAMENTO : "envolvido"
```

**✅ Correto (usar entidade associativa)**

```mermaid
erDiagram
    ALOCACAO {
        string papel
    }
    PROJETO ||--o{ ALOCACAO : "tem"
    FUNCIONARIO ||--o{ ALOCACAO : "participa"
    DEPARTAMENTO ||--o{ ALOCACAO : "coordena"
```

---

### 4. Cardinalidade Múltipla

**❌ Errado (cardinalidade ambígua)**

```mermaid
erDiagram
    LIVRO }|--|{ AUTOR : "escrito_por"
```

**✅ Correto (definir cardinalidade corretamente)**

```mermaid
erDiagram
    LIVRO }o--o{ AUTOR : "escrito_por"
    LIVRO {
        string titulo
    }
    AUTOR {
        string nome
    }
```

---

### 5. Entidade sem Atributos

**❌ Errado (entidade vazia)**

```mermaid
erDiagram
    PRODUTO {
    }
    CATEGORIA {
    }
    PRODUTO }o--|| CATEGORIA : "pertence"
```

**✅ Correto (adicionar atributos)**

```mermaid
erDiagram
    PRODUTO {
        int id
        string nome
    }
    CATEGORIA {
        int id
        string nome
    }
    PRODUTO }o--|| CATEGORIA : "pertence"
```

---

### 6. Sintaxe de Relacionamento Invertida

**❌ Errado (ordem das entidades incorreta)**

```mermaid
erDiagram
    "faz" }o--|| CLIENTE : PEDIDO
```

**✅ Correto**

```mermaid
erDiagram
    CLIENTE ||--o{ PEDIDO : "faz"
```

---

### 7. Herança (Generalização)

**❌ Errado (sintaxe não suportada)**

```mermaid
erDiagram
    PESSOA <|-- FUNCIONARIO
    PESSOA <|-- CLIENTE
```

**✅ Correto (usar notação do Mermaid)**

```mermaid
erDiagram
    PESSOA {
        string nome
    }
    FUNCIONARIO ||--|| PESSOA : "é"
    CLIENTE ||--|| PESSOA : "é"
    FUNCIONARIO {
        float salario
    }
    CLIENTE {
        string categoria
    }
```

---

### 8. Chave Estrangeira

**❌ Errado (chave estrangeira na entidade errada)**

```mermaid
erDiagram
    CLIENTE {
        int id_pedido
    }
    PEDIDO {
        int id
    }
    CLIENTE ||--o{ PEDIDO : "faz"
```

**✅ Correto (chave estrangeira no pedido)**

```mermaid
erDiagram
    CLIENTE {
        int id
    }
    PEDIDO {
        int id
        int cliente_id
    }
    CLIENTE ||--o{ PEDIDO : "faz"
```

---

### 9. Relacionamento Recursivo

**❌ Errado (autorrelacionamento mal definido)**

```mermaid
erDiagram
    FUNCIONARIO }|--|| FUNCIONARIO : "gerencia"
```

**✅ Correto**

```mermaid
erDiagram
    FUNCIONARIO {
        int id
        int id_gerente
    }
    FUNCIONARIO }o--|| FUNCIONARIO : "gerencia"
```

---

### 10. Entidade Associativa com Atributos

**❌ Errado (atributos na linha de relacionamento)**

```mermaid
erDiagram
    ALUNO }o--o{ CURSO : "nota: int"
```

**✅ Correto (usar entidade separada)**

```mermaid
erDiagram
    MATRICULA {
        int nota
    }
    ALUNO ||--o{ MATRICULA : "faz"
    CURSO ||--o{ MATRICULA : "tem"
```

---

### Observações:

1. Mermaid não suporta atributos diretamente em relacionamentos (exemplo 2 e 10).
2. Relacionamentos n-ários (exemplo 3) exigem entidades associativas.
3. Sempre declare atributos dentro de chaves `{}` para entidades.
4. A ordem das entidades na definição do relacionamento segue `ENTIDADE1 <cardinalidade>--<cardinalidade> ENTIDADE2 : "rótulo"`.
