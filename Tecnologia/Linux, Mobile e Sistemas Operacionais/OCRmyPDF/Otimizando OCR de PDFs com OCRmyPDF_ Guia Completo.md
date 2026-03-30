# Otimizando OCR de PDFs com OCRmyPDF: Guia Completo

OCRmyPDF é uma ferramenta poderosa para adicionar camadas de texto pesquisável a PDFs, mas pode exigir configurações específicas para obter resultados ótimos. Vou explicar como melhorar o OCR enquanto preserva a qualidade do seu documento original.

## Problemas comuns e soluções

O problema que você encontrou (arquivo final menor com textos incorretos) geralmente ocorre devido à compressão excessiva do PDF e configurações de OCR inadequadas. Vamos resolver isso:

### Comandos básicos para melhorar o OCR

Para um livro com boa qualidade de fonte, experimente primeiro este comando:

```bash
ocrmypdf -l por --output-type pdf --optimize 1 --pdfa-image-compression=lossless --redo-ocr input.pdf output.pdf
```

Este comando:

- Usa o idioma português (`-l por`)
- Mantém o PDF em formato normal (não PDF/A)
- Usa otimização leve sem perdas (`--optimize 1`)
- Preserva a qualidade das imagens (`--pdfa-image-compression=lossless`)
- Refaz o OCR, mantendo o texto vetorial original (`--redo-ocr`)[^5]

### Melhorando a precisão do OCR

Se os resultados ainda não forem satisfatórios, tente aprimorar a precisão do OCR:

```bash
ocrmypdf -l por --deskew --clean --oversample 300 --force-ocr --output-type pdf --optimize 1 --pdfa-image-compression=lossless input.pdf output.pdf
```

Este comando:

- Corrige inclinação das páginas (`--deskew`)[^1]
- Limpa artefatos de digitalização (`--clean`)[^1]
- Aumenta a resolução para 300 DPI (`--oversample 300`)[^1]
- Força OCR em todas as páginas (`--force-ocr`)[^5]

### Preservando a qualidade máxima

Se a qualidade do PDF é prioridade absoluta:

```bash
ocrmypdf -l por --output-type pdf --optimize 0 --pdfa-image-compression=lossless --redo-ocr --jpeg-quality 100 --clean-final input.pdf output.pdf
```

Este comando:

- Desativa otimizações (`--optimize 0`)[^3]
- Preserva a qualidade JPEG máxima (`--jpeg-quality 100`)[^12]
- Mantém a versão limpa da imagem no PDF final (`--clean-final`)[^1]

## Configurações para casos específicos

### Para PDFs multilíngues

Se seu livro contém múltiplos idiomas:

```bash
ocrmypdf -l por+eng --redo-ocr --output-type pdf --optimize 1 --pdfa-image-compression=lossless input.pdf output.pdf
```

Isto define português e inglês como idiomas do documento[^13].

### Para livros com páginas em duas colunas

Para documentos com múltiplas colunas:

```bash
ocrmypdf -l por --tesseract-pagesegmode 1 --redo-ocr --optimize 1 --pdfa-image-compression=lossless input.pdf output.pdf
```

O modo de segmentação de página 1 habilita segmentação automática com detecção de orientação e script, melhorando o reconhecimento de layouts com múltiplas colunas[^15].

## Pré-processamento para resultados superiores

Em alguns casos, o pré-processamento antes de aplicar o OCRmyPDF pode melhorar significativamente os resultados:

### Usando ScanTailor para pré-processamento

O ScanTailor é uma ferramenta excelente para preparar PDFs antes do OCR:

1. Extraia as imagens do PDF:

```bash
pdfimages -all input.pdf pasta_saida/
```

2. Processe com ScanTailor Advanced para melhorar a qualidade[^19]
3. Depois aplique o OCRmyPDF nas imagens tratadas[^18]

## Considerações sobre tamanho e qualidade

Se o tamanho do arquivo final é importante, mas você não quer comprometer a qualidade:

```bash
ocrmypdf -l por --redo-ocr --output-type pdf --optimize 1 --jpeg-quality 90 input.pdf output.pdf
```

Este comando mantém uma boa relação entre qualidade e tamanho[^12].

## Conclusão

O problema de textos incorretos geralmente está relacionado ao reconhecimento de caracteres inadequado. Certifique-se que:

1. Está usando o idioma correto do documento (`-l por` para português)
2. A qualidade da imagem é suficiente (DPI mínimo de 300)[^9]
3. Está usando o modo de OCR apropriado para seu documento (`--redo-ocr` ou `--force-ocr`)[^5]
4. As configurações de otimização não estão sacrificando qualidade por tamanho

Experimente os diferentes comandos sugeridos e ajuste os parâmetros conforme necessário para seu caso específico. O equilíbrio perfeito entre qualidade do PDF e precisão do OCR geralmente requer alguma experimentação.

<div style="text-align: center">⁂</div>

[^1]: https://www.nutrient.io/blog/how-to-ocr-pdfs-in-linux/

[^2]: https://manpages.org/ocrmypdf

[^3]: https://ocrmypdf.readthedocs.io/en/latest/optimizer.html

[^4]: https://ocrmypdf.readthedocs.io/en/latest/errors.html

[^5]: https://ocrmypdf.readthedocs.io/en/v15.4.4/advanced.html

[^6]: https://github.com/ocrmypdf/OCRmyPDF/issues/1124

[^7]: https://stackoverflow.com/questions/9480013/image-processing-to-improve-tesseract-ocr-accuracy

[^8]: https://aurigait.com/blog/how-to-increase-accuracy-of-tesseract/

[^9]: https://tesseract-ocr.github.io/tessdoc/ImproveQuality.html

[^10]: https://ocrmypdf.readthedocs.io/en/v11.7.1/installation.html

[^11]: https://ocrmypdf.readthedocs.io/en/v14.3.0/installation.html

[^12]: https://manpages.debian.org/testing/ocrmypdf/ocrmypdf.1

[^13]: https://ocrmypdf.readthedocs.io/en/v11.7.0/languages.html

[^14]: https://pyimagesearch.com/2020/08/03/tesseract-ocr-for-non-english-languages/

[^15]: https://stackoverflow.com/questions/31651071/how-to-ocr-multiple-column-in-a-document-using-tesseract

[^16]: https://www.ibm.com/docs/en/datacap/9.1.9?topic=extension-best-practices-ocr-document-processing

[^17]: https://www.iri.com/blog/data-protection/preprocessing-images-for-ocr-darkshield/

[^18]: https://github.com/Ra-Na/pdfsandwich-without-unpaper

[^19]: https://github.com/4lex4/scantailor-advanced

[^20]: https://www.reddit.com/r/DataHoarder/comments/b3xsg6/book_scanners_how_do_i_get_my_scanned_books_to/

[^21]: https://ocrmypdf.readthedocs.io

[^22]: https://ocrmypdf.readthedocs.io/en/latest/installation.html

[^23]: https://github.com/ocrmypdf/OCRmyPDF

[^24]: https://pypi.org/project/ocrmypdf/3.0/

[^25]: https://ocrmypdf.readthedocs.io/en/v15.0.2/languages.html

[^26]: https://ocrmypdf.readthedocs.io/en/latest/advanced.html

[^27]: https://ocrmypdf.readthedocs.io/en/v8.1.0/cookbook.html

[^28]: https://manpages.debian.org/unstable/ocrmypdf/ocrmypdf.1.en.html

[^29]: https://ocrmypdf.readthedocs.io/en/latest/languages.html

[^30]: https://ocrmypdf.readthedocs.io/en/latest/cookbook.html

[^31]: https://manpages.ubuntu.com/manpages/jammy/man1/ocrmypdf.1.html

[^32]: https://ocrmypdf.readthedocs.io/en/latest/introduction.html

[^33]: https://ocrmypdf.readthedocs.io/en/v8.1.0/languages.html

[^34]: https://snapcraft.io/ocrmypdf

[^35]: https://github.com/ocrmypdf/OCRmyPDF/issues/1491

[^36]: https://github.com/jbarlow83/OCRmyPDF/issues/125

[^37]: https://tesseract-ocr.github.io/tessdoc/Data-Files-in-different-versions.html

[^38]: https://ocrmypdf.readthedocs.io/en/v15.4.0/advanced.html

[^39]: https://blog.mesltd.ca/what-is-the-best-dpi-for-scanning-documents

[^40]: https://github.com/ocrmypdf/OCRmyPDF/discussions/1254

[^41]: https://github.com/jbarlow83/OCRmyPDF/issues/794

[^42]: https://github.com/jbarlow83/OCRmyPDF/issues/220

[^43]: https://ocrmypdf.readthedocs.io/en/v12.0.2/cookbook.html

[^44]: https://ocrmypdf.readthedocs.io/en/v11.7.2/advanced.html

[^45]: https://github.com/jbarlow83/OCRmyPDF/issues/91

[^46]: https://ocrmypdf.readthedocs.io/en/v11.7.2/cookbook.html

[^47]: https://github.com/ocrmypdf/OCRmyPDF/issues/1036

[^48]: https://github.com/ocrmypdf/OCRmyPDF/issues/1138

[^49]: https://ocrmypdf.readthedocs.io/en/v12.0.3/optimizer.html

[^50]: https://www.reddit.com/r/commandline/comments/h9lj6w/ocrmypdf_now_has_very_long_page_scan_times_how_to/

[^51]: https://discourse.devontechnologies.com/t/further-degradation-of-ocr-pdf-image-layer-in-3-0-2/51901?page=2

[^52]: https://github.com/ocrmypdf/OCRmyPDF/issues/912

[^53]: https://ocrmypdf.readthedocs.io/en/v13.1.1/introduction.html

[^54]: https://www.reddit.com/r/MachineLearning/comments/1f87yfg/p_tesseract_ocr_has_anybody_used_it_for_reading/

[^55]: https://ocrmypdf.readthedocs.io/en/v11.7.1/introduction.html

[^56]: https://www.mdpi.com/2073-8994/12/5/715

[^57]: https://www.cloudraft.io/blog/comprehensive-ocr-guide

[^58]: https://arxiv.org/abs/1802.10033

[^59]: https://sol.sbc.org.br/index.php/sbsi/article/view/34366

[^60]: https://groups.google.com/g/tesseract-ocr/c/5CSIYkba5Dc

[^61]: https://www.reddit.com/r/computervision/comments/dxgq7c/improve_ocr/

[^62]: https://www.adobe.com/acrobat/hub/what-to-do-when-ocr-does-not-recognize-text.html

[^63]: https://codingvision.net/improving-tesseract-4-ocr-accuracy-through-image-preprocessing

[^64]: https://www.klippa.com/en/blog/information/improve-ocr-accuracy/

[^65]: https://www.docuclipper.com/blog/ocr-limitations/

[^66]: https://gleematic.com/what-is-ocr-accuracy-how-to-improve-it/

[^67]: https://www.bakertilly.com/insights/ocr-systems-training-mistakes-avoid

[^68]: https://docparser.com/blog/improve-ocr-accuracy/

[^69]: https://pypi.org/project/ocrmypdf/4.2.4/

[^70]: https://ocrmypdf.readthedocs.io/en/v8.2.3/installation.html

[^71]: https://ocrmypdf.readthedocs.io/en/v8.0.0/installation.html

[^72]: https://forum.manjaro.org/t/snap-ocrmypdf-package-wants-old-ghostscript-how-to-fix/150649

[^73]: https://github.com/ocrmypdf/OCRmyPDF/blob/main/docs/release_notes.rst

[^74]: https://pypi.org/project/ocrmypdf/4.2.2/

[^75]: https://ocrmypdf.readthedocs.io/en/v15.0.1/installation.html

[^76]: https://forum.manjaro.org/t/no-space-left-on-device-error-with-ocrmypdf/69374

[^77]: https://ocrmypdf.readthedocs.io/en/latest/maintainers.html

[^78]: https://tesseract-ocr.github.io/tessdoc/

[^79]: https://ghostscript.com/blog/ocr.html

[^80]: https://github.com/jbarlow83/OCRmyPDF/issues/395

[^81]: https://marko.euptera.com/posts/ocrmypdf-windows.html

[^82]: https://github.com/tesseract-ocr/tesseract

[^83]: https://github.com/ocrmypdf/OCRmyPDF/issues/1367

[^84]: https://ocrmypdf.readthedocs.io/en/v9.7.1/api.html

[^85]: https://github.com/jbarlow83/OCRmyPDF/issues/541

[^86]: https://github.com/ocrmypdf/OCRmyPDF/discussions/1291

[^87]: https://github.com/jbarlow83/OCRmyPDF/issues/736

[^88]: https://linuxcommandlibrary.com/man/ocrmypdf

[^89]: https://www.reddit.com/r/macapps/comments/u3shz5/free_i_made_a_small_wrapper_for_the_ocrmypdf/

[^90]: https://ocrmypdf.readthedocs.io/en/v9.1.1/api.html

[^91]: https://ocrmypdf.readthedocs.io/en/v9.2.0/languages.html

[^92]: https://kbpdfstudio.qoppa.com/ocr-two-different-languages-at-once/

[^93]: https://docs.stirlingpdf.com/Advanced Configuration/OCR/

[^94]: https://tesseract-ocr.github.io/docs/MOCRadaptingtesseract2.pdf

[^95]: https://github.com/jbarlow83/OCRmyPDF/issues/39

[^96]: https://github.com/tesseract-ocr/docs/blob/main/das_tutorial2016/7Building%20a%20Multi-Lingual%20OCR%20Engine.pdf

[^97]: https://github.com/ocrmypdf/OCRmyPDF/blob/master/docs/installation.rst

[^98]: https://stackoverflow.com/questions/39166423/python-ocr-pdf-extraction-with-multiple-languages

[^99]: https://support.syncfusion.com/kb/article/4219/how-to-support-german-and-other-languages-in-the-ocr-processor

[^100]: https://github.com/ocrmypdf/OCRmyPDF/blob/master/docs/introduction.rst

[^101]: https://stackoverflow.com/questions/47533875/how-to-extract-a-table-as-text-from-the-pdf

[^102]: https://ocrmypdf.readthedocs.io/en/v13.6.0/introduction.html

[^103]: https://jonathansoma.com/everything/pdfs/ocr-tools/

[^104]: https://www.reddit.com/r/Python/comments/1awc0hh/extracting_information_text_tables_layouts_from/

[^105]: https://www.youtube.com/watch?v=Zk_39dSXRWg

[^106]: https://github.com/jbarlow83/OCRmyPDF/issues/77

[^107]: https://github.com/jbarlow83/OCRmyPDF/issues/316

[^108]: https://github.com/ocrmypdf/OCRmyPDF/discussions/1457

[^109]: https://wiki.ifpe.edu.br/books/ti---infraestrutura-e-serviços-de-rede/page/instalação-da-solução-ocr

[^110]: https://news.ycombinator.com/item?id=40612190

[^111]: https://www.truenas.com/community/threads/what-is-the-best-way-to-run-a-ocr-tool-such-as-ocrmypdf.114685/

[^112]: https://www.mindee.com/blog/create-ocrized-pdfs-in-2-steps

[^113]: https://ocrmypdf.readthedocs.io/en/v15.2.0/advanced.html

[^114]: https://pitt.libguides.com/ocr/bestpractices

[^115]: https://ocrmypdf.readthedocs.io/en/v15.4.0/optimizer.html

[^116]: https://www.reddit.com/r/DataHoarder/comments/13j7rk6/ocr_for_large_books/

[^117]: https://ocrmypdf.readthedocs.io/en/v11.6.1/advanced.html

[^118]: https://ocrmypdf.readthedocs.io/en/latest/performance.html

[^119]: https://github.com/ocrmypdf/OCRmyPDF/issues/1366

[^120]: https://github.com/ocrmypdf/OCRmyPDF/issues/1137

[^121]: https://www.reddit.com/r/LocalLLaMA/comments/192i8ew/ocr_techniques_for_rag_pdf_extraction/

[^122]: https://ocrmypdf.readthedocs.io/en/v8.0.1/advanced.html

[^123]: https://www.nutrient.io/blog/how-to-use-tesseract-ocr-in-python/

[^124]: https://www.youtube.com/watch?v=mdLBr9IMmgI

[^125]: https://github.com/ocrmypdf/OCRmyPDF/issues/1374

[^126]: http://www.tobias-elze.de/pdfsandwich/

[^127]: https://undatas.io/blog/posts/how-to-process-unstructured-pdfs-with-python-in-2025/

[^128]: https://pyimagesearch.com/2021/11/22/improving-ocr-results-with-basic-image-processing/

[^129]: https://www.restack.io/p/ai-tools-linux-developers-answer-ocr-software-cat-ai

[^130]: https://askubuntu.com/questions/396437/how-can-i-remove-the-gray-scale-page-background-of-a-pdf-document-scan-while-pre

[^131]: https://www.reddit.com/r/LocalLLaMA/comments/16mkyyf/whats_the_best_approach_for_pdf_text_extraction/

[^132]: https://www.youtube.com/watch?v=ADV-AjAXHdc

[^133]: https://unstract.com/blog/guide-to-optical-character-recognition-with-tesseract-ocr/

[^134]: https://fransdejonge.com/2014/10/fixing-up-scanned-pdfs-with-scan-tailor/

[^135]: https://scantailor.org

[^136]: https://digitalorientalist.com/2020/03/09/scantailor-installation-instructions-and-impressions/

[^137]: https://programminghistorian.org/en/lessons/retired/OCR-with-Tesseract-and-ScanTailor

[^138]: https://github.com/ImageProcessing-ElectronicPublications/scantailor-experimental

[^139]: https://www.ocr4all.org/guide/user-guide/workflow

[^140]: https://docs.easyrpa.eu/help/docs/Data-Analyst-Guide/OCR-Tuning-Guide/Evaluating-and-Enhancing-Image-Quality-for-OCR

[^141]: https://flathub.org/apps/com.github._4lex4.ScanTailor-Advanced

[^142]: http://www.imagemagick.org/discourse-server/viewtopic.php?t=33953

[^143]: https://launchpad.net/~alex-p/+archive/ubuntu/scantailor

[^144]: https://pdf.co/resources/comparisons/pdfco-vs-ocrmypdf

[^145]: https://rescribe.xyz

[^146]: https://www.imagemagick.org/discourse-server/viewtopic.php?t=32263

[^147]: https://www.reddit.com/r/DataHoarder/comments/cu71n0/scanning_for_ocr_straight_to_pdf_or_images_first/

[^148]: https://updf.com/ocr/linux-ocr/

[^149]: https://www.youtube.com/watch?v=qYeYZqZO6RQ

[^150]: https://research.google.com/pubs/archive/37481.pdf

[^151]: https://hyperverge.co/blog/ocr-automation/

[^152]: https://aclanthology.org/2021.findings-emnlp.356/

[^153]: https://community.adobe.com/t5/acrobat-discussions/newbie-workflow-optimize-amp-ocr-newspaper-scans/m-p/12835375

[^154]: https://www.reddit.com/r/libgen/comments/xe7nu3/how_to_digitize_a_book/

[^155]: https://www.zora.uzh.ch/id/eprint/54277/1/Volk_Furrer_Sennrich_2011.pdf

[^156]: https://community.adobe.com/t5/acrobat-discussions/what-is-the-workflow-when-scanning-and-ocr-for-typesetting/m-p/14604883

[^157]: https://python.plainenglish.io/how-i-used-gpt-4-to-correct-ocr-errors-in-a-pdf-book-e0bc53518f51
