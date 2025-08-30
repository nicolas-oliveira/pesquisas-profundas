# Guia Markitdown: Conversão Estável em GPUs de 4GB

- **Ferramenta**: MarkItDown converte PDFs e outros formatos para Markdown, ideal para análise de texto.
- **Hardware**: Otimizado para GPUs de 4GB, como NVIDIA RTX 3050, com estratégias para evitar erros de memória.
- **Ambiente**: Use Miniconda com Python 3.10 para configuração leve.
- **Cuidados**: Divida PDFs grandes e limite threads para gerenciar memória.

## Configuração do Ambiente

Instale o Miniconda ([docs.conda.io](https://docs.conda.io/en/latest/miniconda.html)) e configure:

```bash
conda create -n markitdown_env python=3.10
conda activate markitdown_env
pip install markitdown[pdf] --no-cache-dir
```

**Otimização para GPU de 4GB**:

- Forçar CPU: `export CUDA_VISIBLE_DEVICES=""`
- Limitar threads: `export OMP_THREAD_LIMIT=2`

## Uso Básico

Converta PDFs para Markdown:

```bash
markitdown input.pdf -o output.md
```

## Otimização para PDFs Grandes

Divida PDFs com `pdftk`:

```bash
pdftk input.pdf burst
for page in pg_*.pdf; do markitdown $page -o ${page%.pdf}.md; done
cat pg_*.md > output.md
```

---

# Guia Definitivo: MarkItDown para Conversão de Documentos em GPUs de 4GB

## 1. Configuração Otimizada com Miniconda

### 1.1. Por que Miniconda?

Miniconda é uma distribuição leve do Python que permite criar ambientes virtuais isolados, reduzindo conflitos de dependências e otimizando o uso de recursos em hardware limitado, como a NVIDIA RTX 3050 com 4GB de VRAM.

### 1.2. Pré-requisitos

- **Miniconda**: Baixe e instale do [site oficial](https://docs.conda.io/en/latest/miniconda.html).
- **Python 3.10**: Escolhido por ser compatível (≥3.8 e <3.11) e estável.
- **pdftk**: Para dividir PDFs grandes, instale via `sudo apt-get install pdftk` (Debian/Ubuntu) ou equivalente.

### 1.3. Configuração do Ambiente

Crie e ative um ambiente virtual:

```bash
conda create -n markitdown_env python=3.10
conda activate markitdown_env
```

Instale o MarkItDown com suporte a PDFs:

```bash
pip install markitdown[pdf] --no-cache-dir
```

A flag `--no-cache-dir` reduz o uso de disco. Instale apenas `[pdf]` para minimizar dependências.

### 1.4. Otimizações para Hardware Limitado

- **Forçar Uso da CPU**: Se a GPU causar erros (ex.: `OutOfMemoryError`), desative-a:

```bash
export CUDA_VISIBLE_DEVICES=""
```

- **Limitar Threads**: Para bibliotecas como Tesseract, reduza o uso de memória:

```bash
export OMP_THREAD_LIMIT=2
```

- **Monitoramento**: Use `htop` (CPU/RAM) ou `nvidia-smi` (GPU) para verificar o consumo de recursos.

## 2. Comandos e Parâmetros do MarkItDown

### 2.1. Comandos Básicos

| Comando                | Função                                    | Exemplo                                           |
| ---------------------- | ----------------------------------------- | ------------------------------------------------- |
| `markitdown <arquivo>` | Converte para Markdown, saída no terminal | `markitdown input.pdf`                            |
| `-o <arquivo>`         | Especifica arquivo de saída               | `markitdown input.pdf -o output.md`               |
| `--list-plugins`       | Lista plugins instalados                  | `markitdown --list-plugins`                       |
| `--use-plugins`        | Habilita plugins na conversão             | `markitdown --use-plugins input.pdf -o output.md` |

### 2.2. Conversão de PDFs Complexos

Para PDFs com tabelas ou imagens, o MarkItDown preserva a estrutura (cabeçalhos, listas, tabelas, links). No entanto, tabelas complexas podem não ser perfeitamente convertidas, conforme relatado em [GitHub Issue #41](https://github.com/microsoft/markitdown/issues/41).

**Dica**: Teste com `--use-plugins` para melhorar a extração de tabelas, se plugins estiverem disponíveis.

### 2.3. Solução para Falhas Comuns

- **Fontes Raras**: Instale pacotes de fontes adicionais (ex.: `fonts-noto` no Ubuntu) para evitar erros de renderização.
- **Erros de Memória**: Veja a seção 4 para estratégias de divisão de PDFs.

## 3. Uso Avançado com API Python

Para maior controle, use a API Python:

```python
from markitdown import MarkItDown

md = MarkItDown(enable_plugins=False)
result = md.convert("input.pdf")
with open("output.md", "w") as f:
    f.write(result.text_content)
```

Consulte a documentação em [GitHub](https://github.com/microsoft/markitdown) para opções avançadas, como integração com LLMs para descrição de imagens.

## 4. Fluxo de Trabalho para PDFs Grandes

### 4.1. Divisão de PDFs

Para PDFs grandes que consomem muita memória:

1. Instale o `pdftk`:

```bash
sudo apt-get install pdftk
```

2. Divida o PDF em páginas:

```bash
pdftk input.pdf burst
```

3. Converta cada página:

```bash
for page in pg_*.pdf; do
    markitdown $page -o ${page%.pdf}.md
done
```

4. Combine os resultados:

```bash
cat pg_*.md > output.md
```

### 4.2. Exemplo: Converter PDF com Imagens

```bash
წ

markitdown input.pdf -o output.md
```

O MarkItDown suporta OCR para PDFs com imagens, mas a divisão pode melhorar a estabilidade em GPUs de 4GB.

## 5. Solução de Problemas

| Problema                | Solução                                                                          |
| ----------------------- | -------------------------------------------------------------------------------- |
| `OutOfMemoryError`      | Divida o PDF (seção 4.1); use `CUDA_VISIBLE_DEVICES=""`; aumente swap.           |
| Tabelas mal convertidas | Teste plugins (`--use-plugins`); use Azure Document Intelligence, se disponível. |
| Erros de fontes         | Instale fontes adicionais (ex.: `fonts-noto`).                                   |

## 6. Comparação de Desempenho

| Configuração                | Tempo Estimado | Qualidade | Uso de Memória |
| --------------------------- | -------------- | --------- | -------------- |
| PDF único, CPU              | 10-30s/Página  | Boa       | Médio          |
| PDF dividido, CPU           | 5-15s/Página   | Boa       | Baixo          |
| Azure Document Intelligence | Variável       | Alta      | Baixo (nuvem)  |

**Nota**: Tempos são estimativas; dependem do tamanho do PDF e do hardware.

## 7. Conclusão

O MarkItDown é uma ferramenta eficiente para converter PDFs para Markdown, ideal para pipelines de análise de texto. Com as otimizações descritas, é possível usá-lo em GPUs de 4GB, como a RTX 3050, garantindo estabilidade e qualidade.

**Citações**:

- [MarkItDown GitHub](https://github.com/microsoft/markitdown)
- [Miniconda](https://docs.conda.io/en/latest/miniconda.html)
- [pdftk](https://www.pdflabs.com/docs/pdftk-man-page/)