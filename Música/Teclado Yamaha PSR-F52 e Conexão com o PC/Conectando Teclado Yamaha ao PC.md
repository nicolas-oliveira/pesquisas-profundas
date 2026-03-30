

# **Guia Técnico Definitivo: Conectando o Yamaha PSR-F52 ao Computador para Gravação de Áudio de Alta Fidelidade**

## **Análise da Saída de Áudio do Yamaha PSR-F52: Sinal e Conector**

### **Identificação do Conector PHONES/OUTPUT**

O teclado Yamaha PSR-F52, projetado como um instrumento de entrada, consolida suas capacidades de saída de áudio em uma única porta multifuncional. A análise das especificações do produto confirma que esta porta é identificada como "Headphones jack" (saída para fones de ouvido).1 Conforme os padrões da indústria para equipamentos de áudio de consumo e semi-profissionais, este conector é uma **mini-jack estéreo de 3,5 mm**, popularmente conhecida como **P2**. Esta porta foi projetada para uma dupla finalidade: permitir a audição privada através de fones de ouvido e servir como a principal saída de áudio para conexão com equipamentos externos, como amplificadores, mesas de som ou, neste caso, um computador.

### **A Natureza do Sinal de Saída: O Conceito Crítico de Nível de Linha vs. Nível de Microfone**

Um dos aspectos técnicos mais cruciais para uma conexão bem-sucedida é a compreensão da natureza do sinal elétrico emitido pela porta PHONES/OUTPUT. Esta saída fornece um sinal de áudio de **nível de linha (line-level)**. Este não é um detalhe trivial; é o fator determinante para todas as decisões de conexão subsequentes. Um sinal de nível de linha é um padrão de voltagem robusto, projetado para a interconexão de equipamentos de áudio, como teclados, sintetizadores, mixers e reprodutores de CD.2 Tecnicamente, sua voltagem nominal é de aproximadamente 1 volt.4  
Em contrapartida, um sinal de **nível de microfone (mic-level)** é exponencialmente mais fraco. Gerado pela cápsula de um microfone ao captar ondas sonoras, este sinal mede apenas alguns milésimos de volt e, portanto, requer uma pré-amplificação substancial para se tornar utilizável em um sistema de áudio.4 A magnitude da diferença é fundamental: um sinal de nível de linha é aproximadamente **1.000 vezes mais forte** que um sinal de nível de microfone.4 A consequência direta dessa disparidade é que as entradas de áudio são projetadas especificamente para um ou outro tipo de sinal. Conectar uma fonte de nível de linha (como o PSR-F52) a uma entrada projetada para nível de microfone resultará em uma sobrecarga massiva do circuito de pré-amplificação, causando distorção digital severa (conhecida como *clipping*) e tornando o áudio gravado completamente inutilizável.

### **Distinção Essencial: Áudio vs. MIDI**

É imperativo diferenciar a conexão de áudio, que é o foco deste guia, da conexão MIDI. A interface **MIDI (Musical Instrument Digital Interface)** não transmite som. Em vez disso, ela transmite dados de performance: informações sobre qual nota foi tocada, com que intensidade (velocidade), por quanto tempo foi sustentada, entre outros parâmetros.6 Esses dados são então utilizados para controlar instrumentos virtuais ou outros equipamentos MIDI dentro do computador.7  
O Yamaha PSR-F52, sendo um modelo de entrada, não possui portas MIDI dedicadas nem funcionalidade de áudio/MIDI via USB. Isso significa que a única maneira de capturar e gravar os sons característicos do instrumento — seus timbres de piano, cordas, órgãos, etc. — é através da conexão de áudio analógico pela sua saída de fones de ouvido. Esta limitação eleva a importância da qualidade da captura de áudio; não se trata apenas de uma opção, mas da única metodologia para preservar e utilizar o caráter sônico do instrumento em um ambiente de produção digital.

## **Mapeamento e Análise das Entradas de Áudio do Computador**

### **Inventário das Portas de Entrada de Áudio Comuns**

A maioria dos computadores, especialmente os desktops, está equipada com um conjunto de portas de áudio localizadas nos painéis traseiro e/ou frontal.9 Para evitar erros de conexão, estas portas seguem um código de cores padronizado 10:

* **Entrada de Linha (Line In):** Geralmente identificada pela cor **azul claro**, esta porta é projetada especificamente para receber os sinais de nível de linha mais fortes, provenientes de dispositivos como teclados, mixers ou reprodutores de áudio.2 Esta é a entrada ideal para conectar o Yamaha PSR-F52.  
* **Entrada de Microfone (Microphone In):** Comumente identificada pela cor **rosa**, esta porta contém um pré-amplificador integrado, projetado para amplificar os sinais de nível de microfone, que são muito fracos.11 Como detalhado anteriormente, conectar o teclado a esta entrada resultará em distorção severa.  
* **Portas Combo (em Laptops):** Muitos laptops modernos simplificam o design ao incluir uma única porta P2 que funciona tanto como saída de fone de ouvido quanto entrada de microfone. Essas portas são, por padrão, configuradas para esperar um sinal de microfone e podem não ser adequadas para um sinal de nível de linha sem configurações de software específicas ou hardware adicional.

A tendência da indústria de eletrônicos de consumo em remover portas dedicadas, como a Entrada de Linha, em favor de portas combo ou da sua eliminação completa, representa um desafio crescente para músicos. Para muitos usuários de laptops modernos, a placa de som integrada pode ser não apenas de baixa qualidade, mas funcionalmente inadequada para a tarefa de gravar um instrumento de nível de linha. Essa realidade fortalece significativamente a recomendação de se utilizar uma interface de áudio externa, transformando-a de uma melhoria de qualidade para uma necessidade funcional.

### **Tabela Comparativa de Entradas de Áudio do PC**

Para solidificar a compreensão e servir como uma ferramenta de referência rápida, a tabela abaixo detalha as características e implicações de cada porta de entrada.

| Característica | Entrada de Linha (Line In) | Entrada de Microfone (Mic In) |
| :---- | :---- | :---- |
| **Cor Padrão** | Azul Claro 10 | Rosa 10 |
| **Tipo de Sinal Esperado** | Nível de Linha 2 | Nível de Microfone 11 |
| **Voltagem Típica** | Aprox. \+4 dBu / \-10 dBV (\~1V) 4 | Aprox. \-60 a \-40 dBu (milivolts) 4 |
| **Pré-amplificação** | Não (ou mínima) | Sim (significativa) |
| **Uso Ideal** | Teclados, mixers, players de áudio 2 | Microfones 11 |
| **Resultado da Conexão do PSR-F52** | Sinal limpo e com nível adequado | Sinal distorcido e com *clipping* |

## **Metodologias de Conexão Física: Cenários e Equipamentos**

### **Cenário A: Conexão Direta via Placa de Som Integrada (Método Básico)**

Este método é o mais simples e de menor custo, utilizando a placa de som que já vem no computador.

* **Equipamento Necessário:** Um único **cabo de áudio estéreo P2 para P2 (3,5 mm TRS)**.  
* **Procedimento de Conexão:**  
  1. Conecte uma extremidade do cabo na saída "PHONES/OUTPUT" do Yamaha PSR-F52.  
  2. Conecte a outra extremidade na porta "Entrada de Linha" (azul claro) do computador.  
* **Análise de Desempenho:** Apesar da sua simplicidade, esta abordagem apresenta desvantagens técnicas significativas que comprometem a qualidade final da gravação.  
  * **Qualidade de Conversão A/D:** As placas de som integradas utilizam conversores Analógico-Digital (A/D) de baixo custo, que resultam em menor fidelidade de áudio, uma faixa dinâmica mais restrita e um nível de ruído de fundo mais elevado.12  
  * **Interferência Eletromagnética (EMI):** A placa de som está localizada dentro do gabinete do computador, um ambiente eletricamente "ruidoso" devido à atividade da CPU, GPU, ventoinhas e fonte de alimentação. Esse ruído elétrico pode ser induzido no sinal de áudio, manifestando-se como zumbidos, chiados ou outros artefatos indesejados na gravação.  
  * **Latência:** A latência é o atraso perceptível entre o momento em que uma nota é tocada no teclado e o momento em que o som é ouvido de volta através dos alto-falantes do computador. Os drivers de áudio padrão do sistema operacional não são otimizados para produção musical em tempo real, resultando em uma latência elevada que pode tornar a gravação e o monitoramento impraticáveis.13

### **Cenário B: Conexão Profissional via Interface de Áudio Externa (Método Recomendado)**

Para qualquer aplicação de gravação séria, a utilização de uma interface de áudio externa é a abordagem tecnicamente correta e recomendada. Uma interface de áudio é, essencialmente, uma placa de som externa de alta qualidade, projetada especificamente para as demandas da produção musical.14 Ela resolve sistematicamente todas as deficiências do método de conexão direta.

* **Vantagens Técnicas:**  
  * **Conversores A/D Superiores:** Oferecem gravação de alta resolução (tipicamente 24 bits) com maior fidelidade e um piso de ruído drasticamente mais baixo, capturando o som do teclado com clareza e detalhe.12  
  * **Isolamento de Ruído:** Ao mover o processo de conversão A/D para fora do ambiente ruidoso do gabinete do PC, a interface isola o sinal analógico sensível da interferência eletromagnética, resultando em uma gravação limpa.  
  * **Drivers de Baixa Latência:** Interfaces de áudio utilizam drivers especializados (como ASIO no Windows ou Core Audio no macOS) que fornecem um caminho de sinal mais direto, resultando em uma latência quase imperceptível, o que é essencial para gravar e monitorar a performance em tempo real.17  
  * **Controle de Nível de Entrada (Ganho):** Permitem um ajuste preciso do nível do sinal de entrada antes que ele seja convertido para digital, garantindo um nível de gravação ideal.  
* **Equipamento Necessário:**  
  1. **Interface de Áudio USB:** Um modelo de entrada com pelo menos duas entradas de linha mono (geralmente conectores P10 de 6,35 mm).  
  2. **Cabo Apropriado:** O cabo mais comum para esta aplicação é um **cabo P2 estéreo para dois P10 mono (TRS 3,5 mm para 2x TS 6,35 mm)**. Este cabo divide o sinal estéreo da saída de fone de ouvido do teclado em seus canais esquerdo (L) e direito (R), permitindo que sejam conectados a duas entradas mono separadas na interface para uma gravação estéreo fiel. Adaptadores P2 para P10 também são uma opção viável em conjunto com cabos P10 padrão.19  
* **Fluxo de Sinal:** PSR-F52 → Cabo P2 para 2x P10 → Entradas de Linha da Interface de Áudio → Cabo USB → Computador.

A decisão de investir em uma interface de áudio transcende a simples conexão de um teclado. Ela estabelece a fundação de um home studio, servindo como um hub central que permitirá futuras expansões, como a conexão de microfones profissionais (que requerem os pré-amplificadores e a alimentação *phantom power* fornecidos pela interface) e monitores de estúdio para uma audição crítica.

### **Cenário C: A Solução Híbrida \- Cabo-Interface USB**

Existem no mercado cabos especializados que integram um conversor A/D e uma conexão USB diretamente em sua estrutura, com um conector P10 em uma ponta e USB na outra.21 Este dispositivo funciona como uma interface de áudio miniaturizada. Embora ofereça a vantagem do isolamento de EMI em comparação com a conexão direta, a qualidade de seus conversores e a falta de controle de ganho geralmente o tornam inferior a uma interface de áudio de desktop dedicada.

## **Configuração de Software e Calibração do Nível de Entrada**

Após a conexão física, é necessário configurar o sistema operacional do computador para reconhecer e utilizar o novo dispositivo de entrada de áudio.

### **Configuração no Ambiente Windows (11/10)**

1. **Acessar as Configurações de Som:** Navegue até Configurações \> Sistema \> Som. Em versões mais antigas do Windows 10, pode ser necessário acessar o Painel de Controle \> Som.  
2. **Selecionar o Dispositivo de Entrada:** Na seção "Entrada", localize o menu suspenso "Escolha seu dispositivo de entrada".  
   * **Para conexão direta:** Selecione "Entrada de Linha (Line In)", que geralmente estará associada ao chipset de áudio integrado (ex: Realtek High Definition Audio).24  
   * **Para conexão com interface de áudio:** Selecione o nome da sua interface (ex: "Focusrite Scarlett Solo", "Behringer UMC22", etc.) na lista.25 É fundamental ter instalado previamente o driver ASIO específico do fabricante para garantir o desempenho de baixa latência.18  
3. **Ajustar o Nível de Entrada (Gain Staging):** Este é um passo crítico. Toque o teclado na intensidade mais forte que pretende usar durante a gravação. Observe o medidor de nível de entrada ("Testar seu microfone") nas configurações de som. Ajuste o controle de volume de entrada (ou o botão de ganho na interface de áudio) para que os picos mais altos atinjam aproximadamente 75-80% do medidor, garantindo que ele nunca chegue a 100%. Um sinal que atinge 100% resulta em *clipping* digital, uma distorção irrecuperável.

### **Configuração no Ambiente macOS (Sonoma/Ventura)**

1. **Acessar os Ajustes de Som:** Abra os Ajustes do Sistema (anteriormente "Preferências do Sistema") e clique em Som.26  
2. **Selecionar o Dispositivo de Entrada:** Clique na aba "Entrada".27  
   * **Para conexão direta** (em modelos de Mac mais antigos com uma porta de entrada de linha dedicada): Selecione "Entrada de Linha".  
   * **Para conexão com interface de áudio:** Selecione o nome da sua interface na lista de dispositivos de som.  
3. **Ajustar o Nível de Entrada:** Utilize o controle deslizante "Volume de Entrada" enquanto toca o teclado. O princípio é idêntico ao do Windows: obter um sinal forte e claro, mas com uma margem de segurança (*headroom*) para evitar que os picos mais altos causem distorção.

A calibração correta do nível de entrada, conhecida como *gain staging*, é o elo final para garantir uma gravação de alta fidelidade. Negligenciar este passo pode anular os benefícios de um hardware de qualidade, pois um sinal gravado muito baixo terá uma má relação sinal-ruído, enquanto um sinal gravado muito alto será permanentemente distorcido.

## **Conclusões e Próximos Passos na Produção Musical**

### **Sumário das Recomendações Técnicas**

A conexão bem-sucedida do teclado Yamaha PSR-F52 a um computador para fins de gravação de áudio depende fundamentalmente da compreensão de um princípio central: a saída do teclado fornece um sinal de **nível de linha**, que deve ser conectado a uma entrada de áudio compatível com este nível de sinal.  
A análise técnica conclui que, embora a conexão direta à porta de "Entrada de Linha" da placa de som integrada de um computador seja funcional, ela é uma solução tecnicamente comprometida. As limitações inerentes a esta abordagem — qualidade inferior dos conversores A/D, suscetibilidade à interferência eletromagnética e alta latência — degradam a qualidade do áudio capturado.  
Portanto, a recomendação inequívoca para qualquer aplicação de gravação que vise a alta fidelidade é a utilização de uma **interface de áudio externa**. Este dispositivo é projetado especificamente para superar todas as deficiências da placa de som integrada, proporcionando uma conversão de áudio limpa, gravação de baixa latência e controle profissional sobre os níveis de sinal.

### **Rumo à Gravação: Integrando com uma DAW**

Com a conexão física estabelecida e a configuração do sistema operacional concluída, o teclado está pronto para ser utilizado no ambiente de produção musical. O próximo passo é integrar o sinal de áudio em uma **Digital Audio Workstation (DAW)**. Softwares como Reaper, Ableton Live, Logic Pro X, FL Studio ou Cubase são os centros de comando para gravação, edição e mixagem de música.6  
Dentro da DAW, o procedimento geral consiste em:

1. Configurar as preferências de áudio para usar a interface de áudio (ou a entrada de linha) como o dispositivo de entrada.  
2. Criar uma nova trilha de áudio (estéreo).  
3. "Armar" a trilha para gravação, o que permitirá monitorar o som do teclado através do software.  
4. Iniciar a gravação para capturar a performance.

Uma vez gravado, o áudio do Yamaha PSR-F52 existe como uma trilha digital dentro do projeto, pronta para ser editada, processada com efeitos e mixada com outros instrumentos, transformando um procedimento técnico em um ponto de partida para a expressão criativa.

#### **Referências citadas**

1. Yamaha PSR-F52 "How to use the \[SHIFT\] button and Functions" \- YouTube, acessado em outubro 14, 2025, [https://www.youtube.com/watch?v=6xs3Xti4VaU](https://www.youtube.com/watch?v=6xs3Xti4VaU)  
2. Quais são algumas aplicações comuns de linha em portas? | Lenovo Portugal, acessado em outubro 14, 2025, [https://www.lenovo.com/pt/pt/glossary/line-in/](https://www.lenovo.com/pt/pt/glossary/line-in/)  
3. Quais são algumas aplicações comuns de linha em portas? | Lenovo Brasil, acessado em outubro 14, 2025, [https://www.lenovo.com/br/pt/glossary/line-in/](https://www.lenovo.com/br/pt/glossary/line-in/)  
4. What's the Difference Between Line and Mic Levels? \- Shure USA, acessado em outubro 14, 2025, [https://www.shure.com/en-US/insights/whats-the-difference-between-line-and-mic-levels](https://www.shure.com/en-US/insights/whats-the-difference-between-line-and-mic-levels)  
5. Artigo \- MIC IN e LINE IN em mesas de som: uma tremenda confusão, acessado em outubro 14, 2025, [https://somaovivo.org/forum/threads/mic-in-e-line-in-em-mesas-de-som-uma-tremenda-confusao.851/](https://somaovivo.org/forum/threads/mic-in-e-line-in-em-mesas-de-som-uma-tremenda-confusao.851/)  
6. MONTAGE M Manual de operação | Uso do MONTAGE M | Conexão de instrumentos MIDI externos | Conexão e configuração de um computador \- Yamaha, acessado em outubro 14, 2025, [https://manual.yamaha.com/mi/synth/montage\_m/pt/om01basicoperation0310.html](https://manual.yamaha.com/mi/synth/montage_m/pt/om01basicoperation0310.html)  
7. \#02 Como conectar o teclado no computador (USB e Cabo MIDI) \- YouTube, acessado em outubro 14, 2025, [https://www.youtube.com/watch?v=RIn6fmC9Rvo](https://www.youtube.com/watch?v=RIn6fmC9Rvo)  
8. Piano híbrido: Como eu faço uma conexão MIDI para o meu computador?, acessado em outubro 14, 2025, [https://faq.yamaha.com/br/s/article/000001195](https://faq.yamaha.com/br/s/article/000001195)  
9. Informática Básica: Parte posterior do gabinete \- GCFGlobal, acessado em outubro 14, 2025, [https://edu.gcfglobal.org/pt/informatica-basica/parte-posterior-do-gabinete/1/](https://edu.gcfglobal.org/pt/informatica-basica/parte-posterior-do-gabinete/1/)  
10. Um guia sobre portas e conectores externos em um computador Dell, acessado em outubro 14, 2025, [https://www.dell.com/support/kbdoc/pt-pt/000132029/um-guia-sobre-portas-e-conectores-externos-em-um-computador-dell](https://www.dell.com/support/kbdoc/pt-pt/000132029/um-guia-sobre-portas-e-conectores-externos-em-um-computador-dell)  
11. Eu e meu Dell Para computadores Inspiron, Série G, XPS e ..., acessado em outubro 14, 2025, [https://www.dell.com/support/manuals/pt-br/inspiron-15-3593-laptop/inspiron-3593\_me-and-my-dell/tipos-de-portas-de-%C3%A1udio?guid=guid-4ee500d3-543a-4266-8381-ea8fcde80f31\&lang=pt-br](https://www.dell.com/support/manuals/pt-br/inspiron-15-3593-laptop/inspiron-3593_me-and-my-dell/tipos-de-portas-de-%C3%A1udio?guid=guid-4ee500d3-543a-4266-8381-ea8fcde80f31&lang=pt-br)  
12. Afinal, para que serve uma interface de áudio? : r/musicproduction \- Reddit, acessado em outubro 14, 2025, [https://www.reddit.com/r/musicproduction/comments/18woxyk/what\_the\_hell\_is\_an\_audio\_interface\_really\_for/?tl=pt-br](https://www.reddit.com/r/musicproduction/comments/18woxyk/what_the_hell_is_an_audio_interface_really_for/?tl=pt-br)  
13. Melhores Interfaces de Áudio 2025: 7 Opções Custo Benefício, acessado em outubro 14, 2025, [https://melhoresdosom.com.br/melhores-interfaces-de-audio/](https://melhoresdosom.com.br/melhores-interfaces-de-audio/)  
14. Interface de Áudio ou Mesa de Som: Qual é a Melhor Opção para Você? \- Saramonic Brasil, acessado em outubro 14, 2025, [https://blog.saramonic.com.br/interface-de-audio-ou-mesa-de-som/](https://blog.saramonic.com.br/interface-de-audio-ou-mesa-de-som/)  
15. Interface de áudio: entenda o que é e para que serve \- Só Som, acessado em outubro 14, 2025, [http://blog.sosom.com.br/interface-de-audio-entenda-o-que-e-e-para-que-serve/](http://blog.sosom.com.br/interface-de-audio-entenda-o-que-e-e-para-que-serve/)  
16. Interface de Áudio vs. Mesa de Som: O ideal pra você \- Assinatura e venda de instrumentos musicais \- BATS, acessado em outubro 14, 2025, [https://batsss.co/blog/producao-e-gravacao/interface-de-audio-vs-mesa-de-som-qual-a-diferenca-na-gravacao/](https://batsss.co/blog/producao-e-gravacao/interface-de-audio-vs-mesa-de-som-qual-a-diferenca-na-gravacao/)  
17. Como escolher uma interface de áudio? \- Soundclass, acessado em outubro 14, 2025, [https://soundclass.com.br/dicas/como-escolher-uma-interface-de-audio/](https://soundclass.com.br/dicas/como-escolher-uma-interface-de-audio/)  
18. Como Ligar um Teclado Midi \- O Guia Definitivo \#1/2 \- YouTube, acessado em outubro 14, 2025, [https://www.youtube.com/watch?v=zrPLl85YbLM](https://www.youtube.com/watch?v=zrPLl85YbLM)  
19. Adaptador P2 P10 | MercadoLivre, acessado em outubro 14, 2025, [https://lista.mercadolivre.com.br/adaptador-p2-p10](https://lista.mercadolivre.com.br/adaptador-p2-p10)  
20. Kit Plug Áudio Estéreo P2 para P10 – Ideal para Mesa de Som, Teclado e Fone Profissional, acessado em outubro 14, 2025, [https://shopee.com.br/Kit-Plug-%C3%81udio-Est%C3%A9reo-P2-para-P10-%E2%80%93-Ideal-para-Mesa-de-Som-Teclado-e-Fone-Profissional-i.551238513.22298773364](https://shopee.com.br/Kit-Plug-%C3%81udio-Est%C3%A9reo-P2-para-P10-%E2%80%93-Ideal-para-Mesa-de-Som-Teclado-e-Fone-Profissional-i.551238513.22298773364)  
21. CABO P10 X USB INTERFACE GUITARRA COMPUTADOR GUITAR LINK \- Alysson\! eShop, acessado em outubro 14, 2025, [https://www.alyssoneshop.com.br/instrumentos-musicais/acessorios-para-instrumentos/cabo-p10-x-usb-interface-guitarra-computador-guitar-link](https://www.alyssoneshop.com.br/instrumentos-musicais/acessorios-para-instrumentos/cabo-p10-x-usb-interface-guitarra-computador-guitar-link)  
22. Cabo Usb 2.0 Instrumental P10 Ativo 3 metros Profissional, acessado em outubro 14, 2025, [https://www.jccabos.com.br/cabo-usb-para-captura-instrumental-via-p10-jccabos](https://www.jccabos.com.br/cabo-usb-para-captura-instrumental-via-p10-jccabos)  
23. Cabo P10 para USB Interface 2.0, acessado em outubro 14, 2025, [https://www.edcabos.com/cabos/cabo-audio/cabo-p10/cabo-p10-para-usb-interface-3-metros](https://www.edcabos.com/cabos/cabo-audio/cabo-p10/cabo-p10-para-usb-interface-3-metros)  
24. Entradas de microfone e linha \- Microsoft Q\&A, acessado em outubro 14, 2025, [https://learn.microsoft.com/pt-br/answers/questions/3300975/entradas-de-microfone-e-linha](https://learn.microsoft.com/pt-br/answers/questions/3300975/entradas-de-microfone-e-linha)  
25. COMO CONECTAR E CONFIGURAR INTERFACE DE ÁUDIO? \[ATUALIZADO\] Como ligar a placa de som USB \- YouTube, acessado em outubro 14, 2025, [https://www.youtube.com/watch?v=pVWci2ETDR0](https://www.youtube.com/watch?v=pVWci2ETDR0)  
26. Como configurar o som de entrada e saída do Mac \- MacMagazine, acessado em outubro 14, 2025, [https://macmagazine.com.br/post/2021/07/07/como-configurar-o-som-de-entrada-e-saida-do-mac/](https://macmagazine.com.br/post/2021/07/07/como-configurar-o-som-de-entrada-e-saida-do-mac/)  
27. Alterar as definições de entrada de som no Mac \- Suporte Apple (PT), acessado em outubro 14, 2025, [https://support.apple.com/pt-pt/guide/mac-help/mchlp2567/mac](https://support.apple.com/pt-pt/guide/mac-help/mchlp2567/mac)  
28. Como Usar o Teclado do PC como Controlador MIDI (REAPER) \- YouTube, acessado em outubro 14, 2025, [https://www.youtube.com/watch?v=B8gQDKjWcmg](https://www.youtube.com/watch?v=B8gQDKjWcmg)