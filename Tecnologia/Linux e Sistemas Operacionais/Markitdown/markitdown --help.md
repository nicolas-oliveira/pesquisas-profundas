# Comando markitdown - Ajuda

## Sintaxe

```bash
markitdown <OPCIONAL: NOME_DO_ARQUIVO>
```

Se NOME_DO_ARQUIVO estiver vazio, markitdown lê da entrada padrão (stdin).

## Exemplos de Uso

| Comando                                | Descrição                                    |
| -------------------------------------- | -------------------------------------------- |
| `markitdown exemplo.pdf`               | Converte um arquivo PDF diretamente          |
| `cat exemplo.pdf \| markitdown`        | Lê o arquivo via pipe                        |
| `markitdown < exemplo.pdf`             | Lê o arquivo via redirecionamento de entrada |
| `markitdown exemplo.pdf -o exemplo.md` | Salva a saída em um arquivo específico       |
| `markitdown exemplo.pdf > exemplo.md`  | Redireciona a saída para um arquivo          |

## Descrição

Converte vários formatos de arquivo para markdown.

## Argumentos Posicionais

| Argumento  | Descrição                        |
| ---------- | -------------------------------- |
| `filename` | Nome do arquivo a ser convertido |

## Opções

| Opção          | Formato Longo           | Descrição                                                                                                                    |
| -------------- | ----------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| `-h`           | `--help`                | Mostra esta mensagem de ajuda e sai                                                                                          |
| `-v`           | `--version`             | Mostra o número da versão e sai                                                                                              |
| `-o OUTPUT`    | `--output OUTPUT`       | Nome do arquivo de saída. Se não fornecido, a saída é escrita na saída padrão (stdout)                                       |
| `-x EXTENSION` | `--extension EXTENSION` | Fornece uma dica sobre a extensão do arquivo (ex: ao ler da entrada padrão)                                                  |
| `-m MIME_TYPE` | `--mime-type MIME_TYPE` | Fornece uma dica sobre o tipo MIME do arquivo                                                                                |
| `-c CHARSET`   | `--charset CHARSET`     | Fornece uma dica sobre o conjunto de caracteres do arquivo (ex: UTF-8)                                                       |
| `-d`           | `--use-docintel`        | Usa Document Intelligence para extrair texto em vez de conversão offline. Requer um Endpoint válido do Document Intelligence |
| `-e ENDPOINT`  | `--endpoint ENDPOINT`   | Endpoint do Document Intelligence. Obrigatório se usando Document Intelligence                                               |
| `-p`           | `--use-plugins`         | Usa plugins de terceiros para converter arquivos. Use `--list-plugins` para ver plugins instalados                           |
|                | `--list-plugins`        | Lista plugins de terceiros instalados. Plugins são carregados ao usar a opção `-p` ou `--use-plugin`                         |
|                | `--keep-data-uris`      | Mantém URIs de dados (como imagens codificadas em base64) na saída. Por padrão, URIs de dados são truncadas                  |

## Funcionalidade Principal

O `markitdown` é uma ferramenta de linha de comando que converte diversos formatos de arquivo para markdown, oferecendo flexibilidade na entrada e saída de dados, além de suporte a plugins e integração com serviços de inteligência de documentos.
