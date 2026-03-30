# Grok: Visão geral de repositórios para conversão de PDF para Markdown

A pesquisa identificou vários repositórios, com foco naqueles explicitamente projetados para a conversão de PDF em Markdown. Veja abaixo uma análise detalhada:   

- **Marker** por VikParuchuri ( [https://github.com/VikParuchuri/marker](https://github.com/VikParuchuri/marker)): Uma ferramenta baseada em Python que converte PDFs e outros tipos de documentos (imagem, PPTX, DOCX, XLSX, HTML, EPUB) em Markdown, JSON e HTML. Ela suporta extração estruturada e formata tabelas, equações e muito mais, com a opção de usar LLMs para maior precisão.   
- **Docling** by docling-project ( [https://github.com/docling-project/docling](https://github.com/docling-project/docling)): Uma ferramenta Python para analisar vários formatos de documentos, inclusive PDFs, com compreensão avançada de layout, ordem de leitura e estrutura de tabela. Exporta para Markdown, HTML e JSON, com integrações para estruturas de IA como LangChain e LlamaIndex.   
- **MarkItDown** da Microsoft ( [https://github.com/microsoft/markitdown](https://github.com/microsoft/markitdown)): Um utilitário Python leve para converter vários arquivos, inclusive PDFs, em Markdown, com foco na preservação da estrutura do documento para LLMs e análise de texto.   
- **Nougat** by facebookresearch ( [https://github.com/facebookresearch/nougat](https://github.com/facebookresearch/nougat)): Um modelo de OCR baseado em Python para documentos acadêmicos, com saída no formato .mmd (Mathpix Markdown), otimizado para PDFs científicos com matemática e tabelas LaTeX.   
- **pdf-to-markdown** by jzillmann ( [https://github.com/jzillmann/pdf-to-markdown](https://github.com/jzillmann/pdf-to-markdown)): Uma ferramenta JavaScript para conversão de PDFs em Markdown, com uma versão on-line disponível em [http://pdf2md.morethan.io](http://pdf2md.morethan.io).   
- **pdf-to-markdown** por iamarunbrahma ( [https://github.com/iamarunbrahma/pdf-to-markdown](https://github.com/iamarunbrahma/pdf-to-markdown)): Uma ferramenta baseada em Python para converter PDFs em Markdown estruturado, otimizada para tarefas de RAG (Retrieval Augmented Generation) e NLP, extraindo texto, tabelas e imagens.   
- **pdf-to-markdown** by johnlinp ( [https://github.com/johnlinp/pdf-to-markdown](https://github.com/johnlinp/pdf-to-markdown)): Uma ferramenta Python específica para documentos de planejamento urbano em Taiwan, convertendo PDFs em Markdown com possíveis limitações para uso geral.   

### Comparação baseada em critérios

A comparação está estruturada de acordo com os critérios especificados pelo usuário: número de estrelas (popularidade), compatibilidade com idiomas como o português, benchmarking e compatibilidade de hardware com 4 GB de VRAM.   

### Popularidade (número de estrelas)

A contagem exata de estrelas foi um desafio para recuperar programaticamente, mas com base nos resultados de pesquisa e na visibilidade do repositório, **Marker** e **Docling** parecem populares, com menções frequentes em benchmarks e comparações. O **Nougat**, por ser do Meta, provavelmente também tem um interesse significativo da comunidade, embora não tenham sido obtidos números exatos. Outros repositórios, como o pdf-to-markdown de jzillmann, têm menos estrelas, o que sugere uma menor adoção pela comunidade.   

### Suporte a idiomas

- **Marker**: Afirma ter suporte para todos os idiomas, o que o torna adequado para o português e outros idiomas.   
- \***Docling**: Provavelmente oferece suporte a vários idiomas, embora a extração de metadados para o idioma seja mencionada como “em breve” na documentação, sugerindo amplos recursos de manipulação de texto.   
- **MarkItDown**: Focado na preservação da estrutura, provavelmente lida com vários idiomas devido ao seu foco em LLM.   
- **Nougat**: Melhor para trabalhos acadêmicos em inglês, com suporte limitado para outros idiomas baseados em latim e não oferece suporte a scripts não latinos, como chinês ou russo.   
- **pdf-to-markdown** fork de jzillmann\*\*: Como uma ferramenta JavaScript, provavelmente lida com texto Unicode, com suporte ao português.   
- **pdf-to-markdown**: de iamarunbrahma e johnlinp\*\*: Provavelmente suporta vários idiomas, dada sua base em Python e o foco na extração de documentos.   

Para o português, todas as ferramentas parecem viáveis, mas o **Marker** e o **Docling** reivindicam explicitamente um amplo suporte a idiomas.   

## Benchmark

Os dados de benchmarking estavam disponíveis para **Marker** e **Docling**, com comparações com outras ferramentas:   

- **Marker**: Compara favoravelmente com serviços de nuvem como Llamaparse e Mathpix, com uma taxa de transferência projetada de 122 páginas/segundo em uma GPU H100 (0,18 segundos por página em 22 processos). O uso de VRAM é de 5 GB de pico por trabalhador, com média de 3,5 GB, o que sugere alto desempenho, mas possíveis problemas de VRAM.   
- Documentação\*\*: Os benchmarks de desempenho de um relatório técnico ( [https://arxiv.org/html/2408.09869v1](https://arxiv.org/html/2408.09869v1)) mostram velocidades de processamento na CPU, por exemplo, 2,18 páginas/segundo no MacBook Pro M3 Max com 2,56 GB de memória usando o backend pypdfium. As métricas de precisão incluem reconhecimento de estrutura de tabela de última geração com o TableFormer, processando de 2 a 6 segundos por tabela na CPU.   
- **Nougat**: Comparado com os benchmarks do Marker, observado como 10 vezes mais lento e menos preciso fora do arXiv, com uso de 3,3 GB de VRAM em um A6000.   
- Outras ferramentas não têm benchmarks específicos, o que sugere possíveis lacunas de desempenho em comparação com o Marker e o Docling.   

### Compatibilidade de hardware (NVIDIA 3050, 4GB VRAM)

Devido ao limite de 4 GB de VRAM, as ferramentas que exigem aceleração de GPU precisam ser consideradas com cuidado:   

- **Marker**: Requer até 5 GB de VRAM por trabalhador, o que excede seu limite. No entanto, ele pode ser executado na CPU, embora o desempenho possa ser mais lento. O uso médio de VRAM é de 3,5 GB, o que pode ser adequado com uma configuração cuidadosa, mas o pico de uso pode causar problemas.   
- Documentação\*\*: Baseado principalmente na CPU, com uso de memória em torno de 2,5 GB em benchmarks, o que se encaixa bem em seu hardware. O suporte à GPU está em andamento, portanto, o uso atual seria focado na CPU e eficiente para sua configuração.   
- **MarkItDown**: Baseado na CPU, sem requisitos específicos de VRAM, leve e adequado.   
- **Nougat**: Requer GPU, com uso de 3,3 GB de VRAM, próximo ao seu limite, potencialmente viável, mas pode ter dificuldades com vários documentos ou processamento complexo.   
- Ferramentas de pdf paramarkdown\*\*: Provavelmente baseadas em CPU, sem necessidade significativa de VRAM, adequadas ao seu hardware, mas possivelmente com menos recursos.   

### Recursos detalhados e vantagens e desvantagens

Para oferecer uma visão holística, aqui está uma tabela que resume os principais recursos e as compensações:  

| Repositório                     | Idioma | Principais recursos                                   | Suporte a idiomas      | Necessidades de VRAM | Compensações                                             |
|:------------------------------- |:------ |:----------------------------------------------------- |:---------------------- |:-------------------- |:-------------------------------------------------------- |
| Marker                          | Python | Extração estruturada, tabelas, equações, uso de LLM   | Todos os idiomas       | Pico de 5 GB         | Alta necessidade de VRAM, opção de CPU mais lenta        |
| Docling                         | Python | Layout avançado, integrações de IA, eficiência de CPU | Provavelmente vários   | ~2,5 GB              | Suporte a GPU em breve, pode ser complexo de configurar  |
| MarkItDown                      | Python | Preservação de estrutura, foco em LLM                 | Provavelmente vários   | Somente CPU          | Menos avançado para PDFs complexos                       |
| Nougat                          | Python | Foco acadêmico, matemática LaTeX, tabelas             | Melhor em inglês       | 3,3 GB               | Idiomas limitados, saída .mmd pode precisar de conversão |
| pdf-to-markdown (jzillmann)     | JS     | Versão simples e on-line                              | Provavelmente múltipla | Somente CPU          | Menos recursos, menos robusto para PDFs complexos        |
| pdf-to-markdown (iamarunbrahma) | Python | RAG/NLP otimizado, extrai tabelas, imagens            | Provavelmente vários   | Somente CPU          | Menos conhecido, potencialmente com menos recursos       |
| pdf-to-markdown (johnlinp)      | Python | Foco em planejamento urbano                           | Provavelmente vários   | Somente CPU          | Não é de uso geral, escopo limitado                      |

### Recomendações e considerações

Considerando seu hardware, o **Docling** se destaca pela eficiência da CPU e pelo baixo consumo de memória (~2,5 GB), o que o torna uma opção confiável sem preocupações com a VRAM. Se você precisar da utilização da GPU, o **Marker** e o **Nougat** são opções, mas suas necessidades de VRAM (média de 3,5 GB, pico de 5 GB para o Marker; 3,3 GB para o Nougat) estão próximas ou excedem seu limite, o que pode exigir um fallback da CPU. O **Marker** oferece suporte mais amplo a documentos e melhores benchmarks para PDFs em geral, enquanto o **Nougat** é excelente para documentos acadêmicos, mas é menos versátil.   
Para o português e outros idiomas, todas as ferramentas parecem viáveis, sendo que o **Marker** e o **Docling** oferecem suporte explícito a conjuntos de idiomas amplos. Se os seus PDFs forem acadêmicos, explore o **Nougat**, mas observe que seu formato de saída (.mmd) pode exigir conversão. Para simplificar, considere o pdf-to-markdown de jzillmann, especialmente com sua versão on-line, embora possa não ter recursos avançados.   
As compensações incluem o equilíbrio entre velocidade (GPU vs. CPU), precisão (recursos avançados vs. simplicidade) e facilidade de uso (complexidade de configuração vs. funcionalidade). Considerando sua configuração, começar com o **Docling** para obter eficiência da CPU e explorar o **Marker** na CPU para obter recursos mais amplos pode ser uma abordagem estratégica.   
Traduzido com a versão gratuita do tradutor - DeepL.com   

### Referências:

- [Marker Convert PDF to markdown quickly with high accuracy](https://github.com/VikParuchuri/marker)   
- [Docling Get your documents ready for gen AI](https://github.com/docling-project/docling)   
- [MarkItDown Python tool for converting files to Markdown](https://github.com/microsoft/markitdown)   
- [Nougat Implementation of Neural Optical Understanding](https://github.com/facebookresearch/nougat)   
- [pdf-to-markdown A PDF to Markdown converter](https://github.com/jzillmann/pdf-to-markdown)   
- [pdf-to-markdown Conversion for RAG and NLP tasks](https://github.com/iamarunbrahma/pdf-to-markdown)   
- [pdf-to-markdown Convert for urban planning documents](https://github.com/johnlinp/pdf-to-markdown)   
- [Docling Technical Report performance benchmarks](https://arxiv.org/html/2408.09869v1)   
