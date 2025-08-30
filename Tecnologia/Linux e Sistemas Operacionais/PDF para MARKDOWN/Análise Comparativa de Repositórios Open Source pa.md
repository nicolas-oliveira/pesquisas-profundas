<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

# Análise Comparativa de Repositórios Open Source para Conversão de PDF para Markdown

A conversão de documentos PDF para formato Markdown representa um desafio técnico significativo no processamento de documentos, especialmente considerando a preservação de estruturas complexas como tabelas, imagens e formatação. Esta análise examina os principais repositórios open source disponíveis no GitHub e GitLab para esta tarefa, avaliando suas capacidades, compatibilidade linguística e adequação para hardware com recursos limitados, especificamente uma NVIDIA GeForce RTX 3050 com 4GB de VRAM.

## Repositórios Principais e Suas Características

### Marker (VikParuchuri/marker)

O Marker se destaca como uma das soluções mais robustas disponíveis, com aproximadamente 18.000 estrelas no GitHub[^8]. Esta ferramenta utiliza um pipeline de modelos de deep learning que inclui extração de texto, OCR quando necessário, detecção de layout de página e formatação de blocos[^5]. O Marker suporta uma ampla gama de documentos, sendo otimizado especificamente para livros e artigos científicos[^5].

A arquitetura do Marker é estruturada em etapas distintas: extração de texto com OCR heurístico usando Surya e Tesseract, detecção de layout de página e ordem de leitura através do Surya, limpeza e formatação de cada bloco usando heurísticas e Texify, e combinação de blocos com pós-processamento completo do texto[^5]. O sistema funciona em GPU, CPU ou MPS, oferecendo flexibilidade para diferentes configurações de hardware[^5].

Uma das principais vantagens do Marker é sua capacidade de remover cabeçalhos, rodapés e outros artefatos automaticamente, além de formatar adequadamente tabelas e blocos de código[^5]. O sistema também extrai e salva imagens junto com o markdown e converte a maioria das equações para LaTeX[^5]. A versão mais recente inclui um novo modelo OCR que é mais preciso, suporta matemática inline e é mais rápido em GPU[^11].

### Microsoft MarkItDown

O MarkItDown, desenvolvido pela Microsoft, ganhou impressionantes 25.000 estrelas no GitHub em apenas duas semanas após seu lançamento[^4]. Esta ferramenta oferece suporte robusto para uma ampla variedade de formatos de arquivo, incluindo formatos do Office (Word, PowerPoint, Excel), arquivos de mídia (imagens com dados EXIF e descrições, áudio com suporte de transcrição), formatos web e de dados (HTML, JSON, XML, CSV) e arquivos compactados (ZIP)[^4].

A utilização do MarkItDown é extremamente simples, requerendo apenas quatro linhas de código para operação básica[^4]. O sistema utiliza OCR e reconhecimento de fala para extrair conteúdo de imagens e arquivos de áudio, destacando-se pela capacidade de processar dados multimodais[^4]. Para funcionalidades avançadas como descrição de imagens, o MarkItDown requer integração com um cliente LLM, como o GPT-4o da OpenAI[^4].

### IBM Docling

O Docling, desenvolvido pela IBM, alcançou 20.000 estrelas no GitHub em aproximadamente seis meses[^9]. Este framework open source facilita o parsing de documentos com recursos avançados baseados em IA, suportando múltiplos formatos incluindo PDF, DOCX, XLSX, HTML, imagens e outros[^9].

As características principais do Docling incluem parsing multi-formato, compreensão avançada de PDF com análise de layout de página, ordem de leitura, tabelas e fórmulas, formato DoclingDocument unificado para trabalhar com dados de documentos de forma estruturada, múltiplas opções de exportação (Markdown, HTML e JSON sem perdas), suporte para execução local mantendo dados sensíveis seguros, e integrações plug-and-play com LangChain, LlamaIndex, Crew AI e Haystack[^9].

O Docling é alimentado por modelos de IA especializados de última geração para análise de layout (DocLayNet) e reconhecimento de estrutura de tabelas (TableFormer), executando eficientemente em hardware comum com um pequeno orçamento de recursos[^12]. A ferramenta foi aceita no AAAI 25: Workshop on Open-Source AI for Mainstream Use, demonstrando sua relevância acadêmica[^12].

## Repositórios Especializados e Alternativos

### PDF-to-Markdown (iamarunbrahma)

Este repositório foca especificamente na extração de conteúdo formatado em markdown de arquivos PDF, sendo projetado para tarefas downstream como Retrieval Augmented Generation (RAG)[^1]. O sistema preserva vários elementos markdown incluindo tabelas, imagens, links, texto em negrito e itálico, blockquotes, blocos de código e outras sintaxes específicas do markdown[^1].

A implementação utiliza bibliotecas Python como PyMuPDF (fitz), pdfplumber, pytesseract e outras para alcançar extração e conversão precisas[^1]. O sistema suporta layouts complexos incluindo texto multi-coluna, realiza OCR em imagens para extrair texto, gera legendas de imagens usando um modelo pré-treinado, e produz markdown limpo e estruturado adequado para recuperação de informações e tarefas de geração de texto[^1].

### Soluções Baseadas em APIs Externas

Alguns repositórios utilizam APIs de modelos de linguagem externos para conversão. O repositório pdf2md (zyocum/pdf2md) converte PDF para Markdown via modelo multi-modal de texto/visão da OpenAI[^18]. Este sistema requer uma chave de API da OpenAI e utiliza o poppler para conversão de PDF para imagem[^18].

Similarmente, o repositório pdf-to-markdown (mattlgroff/pdf-to-markdown) utiliza o GPT-4 Vision e GPT-4 Turbo para reconhecimento e conversão de conteúdo baseado em imagem[^20]. Este sistema processa a conversão em múltiplas etapas para maior precisão e escalabilidade[^20].

## Análise de Compatibilidade e Recursos Computacionais

### Requisitos de Hardware e VRAM

Para uma NVIDIA GeForce RTX 3050 com 4GB de VRAM, é crucial considerar os requisitos computacionais de cada solução. O Marker, embora poderoso, utiliza múltiplos modelos de deep learning que podem demandar recursos significativos de GPU[^5]. No entanto, o sistema oferece flexibilidade para execução em CPU quando necessário[^5].

O MarkItDown apresenta uma abordagem mais leve em termos de recursos computacionais locais, especialmente quando usado sem integração LLM para descrição de imagens[^4]. Para funcionalidades completas que requerem processamento de imagens, depende de APIs externas, reduzindo a carga computacional local[^4].

O Docling foi especificamente projetado para executar eficientemente em hardware comum com um pequeno orçamento de recursos[^12]. Esta característica o torna particularmente adequado para configurações com recursos limitados como a RTX 3050 com 4GB de VRAM[^12].

### Suporte a Idiomas e Localização

O suporte ao português e outros idiomas varia significativamente entre as soluções. O Marker oferece suporte a todos os idiomas[^5], o que sugere compatibilidade robusta com texto em português. O Docling também demonstra capacidades multilíngues, sendo desenvolvido por uma equipe internacional e testado em diversos contextos linguísticos[^12].

As soluções baseadas em APIs externas como pdf2md e outros repositórios que utilizam modelos da OpenAI herdam as capacidades linguísticas desses modelos, que incluem suporte robusto ao português[^18][^20].

## Comparação de Trade-offs

### Performance vs Recursos

O Marker oferece a melhor qualidade de conversão com suporte abrangente para elementos complexos de documento, mas requer mais recursos computacionais devido ao seu pipeline de múltiplos modelos de deep learning[^5][^8]. Para uma RTX 3050 com 4GB de VRAM, pode ser necessário executar em modo CPU para evitar limitações de memória.

O MarkItDown fornece uma solução equilibrada com boa qualidade de conversão e menor demanda de recursos locais[^4]. Sua simplicidade de uso (apenas quatro linhas de código) o torna atrativo para implementações rápidas[^4]. No entanto, funcionalidades avançadas requerem conectividade com APIs externas[^4].

O Docling representa um meio-termo ideal, oferecendo alta qualidade de conversão com otimização específica para hardware com recursos limitados[^12]. Sua integração com frameworks populares como LangChain e LlamaIndex adiciona valor para aplicações downstream[^9].

### Facilidade de Implementação vs Funcionalidades

As soluções mais simples como pdf-to-markdown (jzillmann) oferecem implementação JavaScript direta com ferramenta online disponível[^13]. No entanto, podem carecer de recursos avançados para documentos complexos[^13].

Repositórios especializados como o de iamarunbrahma oferecem foco específico em aplicações RAG, mas podem ter escopo mais limitado para casos de uso gerais[^1].

## Conclusão

Para uma configuração com NVIDIA RTX 3050 e 4GB de VRAM, o **Docling** emerge como a escolha mais equilibrada, oferecendo alta qualidade de conversão, eficiência de recursos e suporte multilíngue robusto[^9][^12]. Sua otimização para hardware comum e integração com frameworks populares o tornam ideal para desenvolvimento de aplicações.

O **MarkItDown** representa uma excelente alternativa para casos que requerem processamento de múltiplos formatos de arquivo além de PDF, especialmente quando recursos computacionais locais são limitados[^4]. Para projetos que necessitam da mais alta qualidade de conversão e possuem flexibilidade para execução em CPU quando necessário, o **Marker** permanece como a opção mais robusta[^5][^8].

A escolha final deve considerar o equilíbrio entre qualidade de conversão desejada, recursos computacionais disponíveis e requisitos específicos do projeto, como suporte a idiomas e integração com outros sistemas.

<div style="text-align: center">⁂</div>

[^1]: https://github.com/iamarunbrahma/pdf-to-markdown

[^2]: https://openl.io/pt/pdf-to-markdown

[^3]: https://github.com/Grayda/gitlab-doc-builder

[^4]: https://dev.to/leapcell/deep-dive-into-microsoft-markitdown-4if5

[^5]: https://pypi.org/project/marker-pdf/0.3.2/

[^6]: https://www.mindfiretechnology.com/blog/archive/docling-for-pdf-to-markdown-conversion/

[^7]: https://github.com/iamarunbrahma/pdf-to-markdown/blob/main/extract.py

[^8]: https://www.youtube.com/watch?v=moGsPRCcivk

[^9]: https://www.linkedin.com/posts/peter-w-j-staar-7b261373_docling-hits-20k-github-stars-just-activity-7294688688733605888-itpU

[^10]: https://github.com/microsoft/markitdown/blob/main/src/markitdown/_markitdown.py

[^11]: https://github.com/VikParuchuri/marker/releases

[^12]: https://arxiv.org/abs/2501.17887

[^13]: https://github.com/jzillmann/pdf-to-markdown

[^14]: https://notegpt.io/pdf-to-markdown-converter

[^15]: https://gitlab.com/gitlab-org/gitlab/-/issues/13932

[^16]: https://www.avonture.be/blog/markitdown/

[^17]: https://www.linkedin.com/posts/armand-ruiz_this-project-got-more-than-29000-stars-on-activity-7329098701493194752-X2Uj

[^18]: https://github.com/zyocum/pdf2md

[^19]: https://pypi.org/project/markitdown/

[^20]: https://github.com/mattlgroff/pdf-to-markdown

[^21]: https://github.com/VikParuchuri/marker

[^22]: https://github.com/opendatalab/MinerU

[^23]: https://github.com/microsoft/markitdown

[^24]: https://apidog.com/pt/blog/markitdown-mcp/

[^25]: https://github.com/iamarunbrahma/pdf-to-markdown/blob/main/README.md

[^26]: https://github.com/iamarunbrahma/vision-parse

[^27]: https://github.com/drmingler/docling-api

[^28]: https://github.com/docling-project/docling-mcp

[^29]: https://github.com/microsoft/markitdown/labels?sort=count-desc

[^30]: https://jiasu.xzqcsaa.nyc.mn/topics/document-conversion

[^31]: https://www.obsidianstats.com/plugins/marker-api

[^32]: https://daeckel.com/lander/daeckel.com/?_=%2Fjzillmann%2Fpdf-to-markdown%2Fpulls%23XwKUwjkJUt2E4uhiLodCRGn7

[^33]: https://markitdown.pro

[^34]: https://pypi.org/project/marker-pdf/0.2.4/

[^35]: https://github.com/iamarunbrahma/pdf-to-markdown/releases

[^36]: https://github.com/KorigamiK/markitdown_mcp_server

[^37]: https://github.com/zyocum/pdf2md/issues

[^38]: https://github.com/laiso/pdf-to-markdown-gpt

[^39]: https://gitstar-ranking.com

[^40]: https://github.com/docling-project/docling

[^41]: https://github.com/docling-project

[^42]: https://research.ibm.com/blog/docling-generative-AI

[^43]: https://github.com/docling-project/docling-core

[^44]: https://github.com/docling-project/docling-ibm-models

[^45]: https://github.com/microsoft/markitdown/issues/12

[^46]: https://github.com/stn1slv/markdown-github-stars-updater

[^47]: http://janvitek.org/events/NEU/6050/Ps/star.pdf

[^48]: https://www.ndss-symposium.org/wp-content/uploads/madweb2024-4-paper.pdf

[^49]: https://www.reddit.com/r/MachineLearning/comments/1hg5d3p/p_vision_parse_parse_pdf_documents_into_markdown/

[^50]: https://daeckel.com/lander/daeckel.com/?_=%2Ftopics%2Fpdf-to-markdown%23XwKUwjkJUt2E4uhiLodCRGn7

[^51]: https://github.com/cuuupid/cog-marker

[^52]: https://github.com/adithya-s-k/marker-api

[^53]: https://www.linkedin.com/posts/govindatotla_this-microsoft-project-hit-almost-30k-stars-activity-7281882180631347201-mKQZ

[^54]: https://github.com/swswsws583/marker-solo

[^55]: https://github.com/jzillmann/pdf-to-markdown/blob/master/package.json

[^56]: https://huggingface.co/collections/Praise2112/pdf-to-markdown-67c9ca399cdf264c435d05fd

[^57]: https://www.reddit.com/r/ObsidianMD/comments/1j77tbd/convert_entire_pdfs_to_markdown_new_mistral_ocr/

[^58]: https://community.openai.com/t/converting-pdf-to-markdown-with-ocr/762476

[^59]: https://github.com/zyocum/pdf2md/blob/main/pdf2md.py

[^60]: https://pdfbox.pro/pdf-to-markdown

[^61]: https://www.npmjs.com/package/md-to-pdf

[^62]: https://products.aspose.com/words/python-net/conversion/pdf-to-markdown/

[^63]: https://www.reddit.com/r/ObsidianMD/comments/1jlbo74/new_webpage_for_pdfs_to_markdown_conversion/
