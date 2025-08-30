# Gerenciando ambientes virtuais Conda

[Documentação do Conda](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html)
---

## 1. Começando

### 1.1. Criar ambiente

Criar ambiente com Python 3.7 e Pip

```bash
conda create --name whatwhale python=3.7 pip
```

### 1.2. Ativar ambiente

Ativar por nome

```bash
conda activate whatwhale
```

### 1.3. Desativar ambiente

```bash
conda deactivate
```

### 1.4. Removendo um ambiente

Remover ambiente por nome

```bash
# remove env
conda env remove --name whatwhale
# verify
conda env list
```

----

## 2. Gerenciando pacotes

### 2.1. Visualizando pacotes

Listar pacotes no ambiente ativo

```bash
conda list
```

Listar pacotes no ambiente usando nome

```bash
conda list --name whatwhale
```

### 2.2. Instalar pacotes

Instalar o Flask no ambiente ativo

```bash
conda install flask
```

Instalar o Flask no ambiente usando nome

```bash
conda install --name whatwhale flask
```

### 2.3. Usando o pip

Recomenda-se instalar pacotes com o pip somente se o `conda install` não funcionar.
Instalar o pip no ambiente ativo

```bash
conda install pip
pip install <nome-do-pacote>
```

---

## 3. Exportando ambientes

### 3.1. Salvar dependências do ambiente em um arquivo

Produzir um arquivo de lista de especificações

```bash
conda list --explicit > requirements.txt
```

Criar um arquivo `environment.yml`

```bash
conda env export --from-history > environment.yml
```

### 3.2. Criar um ambiente idêntico na mesma máquina ou em outra:

Criar um novo ambiente a partir do arquivo de lista de especificações

```bash
conda create --name myenv -f requirements.txt
```

Recriar a partir do arquivo `environment.yml`

```bash
conda env create -f environment.yml
```

### 3.3. Atualizar ambiente existente usando um arquivo

Atualizar um ambiente existente a partir do arquivo de lista de especificações

```bash
conda install --name myenv --file requirements.txt
```

Atualizar um ambiente existente a partir do arquivo `environment.yml`

```bash
conda env update --name myenv --file environment.yml --prune
```
