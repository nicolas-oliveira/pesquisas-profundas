# RESUMO COMPLETO: Análise do Tablet Jlinksz T1000 (MediaTek) e Uso de ADB para Debugging de Hardware

---

## 1. POR QUE USAR ADB PARA DEBUGAR E ENCONTRAR O HARDWARE

### O ADB (Android Debug Bridge) como Ferramenta de Diagnóstico

O ADB é o método mais confiável para extrair informações de hardware em dispositivos Android porque:

**Acesso Direto ao Sistema**: O ADB conecta ao daemon `adbd` rodando no Android, permitindo acesso shell direto ao kernel e sistema de arquivos sem passar por camadas de abstração de UI que podem esconder informações.

**Propriedades do Sistema em Tempo Real**: O comando `adb shell getprop` acessa `ro.*` (read-only) properties que são carregadas do `build.prop` durante o boot. Essas properties refletem o que o fabricante e o ROM builder definiram como identificação do hardware[121].

**Acesso a Procfs (Virtual Filesystems)**: Via `adb shell cat /proc/cpuinfo`, `/proc/meminfo`, `/proc/partitions`, você acessa informações reportadas pelo kernel em tempo real, que são mais fiáveis que a UI (a UI geralmente exibe versões "marketing" ou resumidas).

**Dumpsys para Componentes de Sistema**: Comandos como `adb shell dumpsys SurfaceFlinger` extraem dados da pilha gráfica real em execução, revelando qual GPU está sendo usado (Mali-400, Adreno, etc.), OpenGL ES version, e estado de drivers em operação.

**Sem Necessidade de Root**: A maioria dos comandos diagnostic não exige root, funcionando com ADB simples ativado. Informações críticas como CPU, RAM, GPU, partições são acessíveis em modo user.

---

## 2. SOBRE O TABLET JLINKSZ T1000 E MEDIATEK

### Quem é Jlinksz?

**Jlinksz é um fabricante chinês genérico** de tablets e dispositivos de baixo custo. Não é uma marca OEM tradicional como Lenovo, Samsung ou Apple[119]. 

Características típicas de fabricantes como Jlinksz:

- Reutilizam designs de hardware idênticos entre modelos "diferentes"
- Alteram apenas especificações de marketing (RAM, storage) sem mudar o SoC real
- Compram SoCs MediaTek em volume (mais baratos que Qualcomm)
- Usam ROMs AOSP customizadas genéricas com múltiplos device configs reutilizados
- Atualizam "modelos" anualmente mas reutilizam carcaça e eletrônica interna[116]

Seu T1000 é um exemplo textbook: a mesma PCB/chassis foi provavelmente vendida em 2017-2021 sob diferentes nomes (ZH960, T1000, etc.) com especificações falsas.[116][119]

### Por Que Valores Estão Vazios ou em Lugares Diferentes

**Razão 1: Propriedades Opcionais**
Nem todo SoC ou ROM preenche todas as properties padrão. Por exemplo:

- `ro.board.platform`: retorna vazio (linha bruta: `[ro.board.platform]: []`)
- `ro.hardware.gpu`: vazio (não é property padrão no Android 4.4)
- `ro.hardware.keystore`: vazio

Isso é normal. O Android **não força** OEMs a preencher todas as properties. Jlinksz não preencheu essas específicas.[145]

**Razão 2: Duplicação Intencional de Hardware Identifiers**
MediaTek permite que um SoC tenha múltiplos identificadores em diferentes campos:

- `ro.hardware`: identifica a variante específica do hardware device-tree
- `ro.mediatek.platform`: identifica a **plataforma SoC** (não device-specific)
- `ro.mtk.hardware`: espelhamento de `ro.hardware` (redundância MediaTek)
- `ro.product.board`: identifica a placa do tablet

Esses são **campos separados com propósitos diferentes**. Não é "bug", é design[121].

**Razão 3: Build.prop Cacheado vs Dinâmico**
`getprop` lê do cache de propriedades em memória (carregado no boot). Se a ROM foi construída com certos campos vazios ou a customização Jlinksz não preencheu-os, eles permanecem vazios. Isso diferencia de verificar `/system/build.prop` (arquivo estático), que você já extraiu.

---

## 3. DISCREPÂNCIAS ENCONTRADAS (DADOS BRUTOS COMO VERDADE ABSOLUTA)

### Discrepância 1: `ro.hardware: mt6592t` vs `ro.mediatek.platform: MT6582`

| Property                 | Valor            | O Que Significa                                 |
| ------------------------ | ---------------- | ----------------------------------------------- |
| `ro.hardware`            | mt6592t          | Device-tree identifier para hardware específico |
| `ro.mediatek.platform`   | MT6582           | Plataforma SoC MediaTek base                    |
| `ro.mtk.hardware`        | mt6582           | Redundância MediaTek de hardware                |
| `/proc/cpuinfo Hardware` | MT6592T          | Kernel reporta MT6592T                          |
| `ro.jlink.cpu_info`      | "octa core 2.0G" | Marketing string Jlinksz                        |

**Causa Real**: Isso indica que o dispositivo foi inicialmente desenvolvido para um chipset, mas o firmware foi reutilizado ou adaptado com device-tree heterogênea.

**Explicação técnica**: MediaTek MT6592T (octa-core, 2.0 GHz, 2013) e MT6582 (quad-core, 1.3 GHz, 2013) compartilham arquitetura **similar** (ambos ARMv7, ambos Mali-400 MP). Jlinksz provavelmente:

1. Comprou tablets com MT6582 (mais barato, real)
2. Reutilizou device-tree de build anterior que tinha MT6592T
3. Ou fez spoof no `ro.hardware` para parecer mais potente[118][121][130]

**Dados que suportam MT6582 como real**:

- `mediatek.wlan.chip`: CONSYS_MT6582 (Wi-Fi+BT integrado, específico MT6582)
- `persist.mtk.wcn.combo.chipid`: 0x6582
- `ro.mtk.hardware`: mt6582
- GPU: Mali-400 MP (MT6582 tem Mali-400, MT6592T teria Mali-450)

**Conclusão**: O hardware **provavelmente é MT6582**, mas device-tree repousa spoof como MT6592T em `ro.hardware`. Kernel vê a verdade (MT6592T em `/proc/cpuinfo`), mas é apenas o que device-tree reporta.

---

### Discrepância 2: `ro.build.version.release: 5.1` vs `ro.build.version.sdk: 19`

| Property                      | Valor              | Significado                                    |
| ----------------------------- | ------------------ | ---------------------------------------------- |
| `ro.build.version.release`    | 5.1                | Versão Android reportada ao usuário (Lollipop) |
| `ro.build.version.sdk`        | 19                 | API Level (KitKat = 19)                        |
| `ro.build.id`                 | KOT49H             | Build ID oficial Android 4.4.2                 |
| `ro.com.google.gmsversion`    | 4.4_r5             | Google Mobile Services para Android 4.4        |
| `ro.mediatek.version.branch`  | KK1.MP1            | MediaTek branch KitKat                         |
| `ro.mediatek.version.release` | ALPS.KK1.MP1.V2.10 | MediaTek release baseado em KitKat             |
| `ro.build.date`               | 2017-12-13         | Data de build (dezembro 2017)                  |

**O Conflito**:

- SDK 19 = Android 4.4 KitKat (2013)
- Release 5.1 = Android 5.0/5.1 Lollipop (2014-2015)
- Lollipop deveria ser SDK 21, não 19[146]

**Causa**: Jlinksz/ROM builder alterou `ro.build.version.release` manualmente para "5.1" para parecer mais moderna, mas deixou `ro.build.version.sdk` como 19 (valor real). Essa incompatibilidade ocorre porque:

1. **Build.prop é editável**: Qualquer pessoa pode editar `/system/build.prop` (ou vendor/build.prop em Android 8+) e alterar propriedades[117][132]
2. **Compatibilidade para trás**: O Android 4.4 (SDK 19) roda em cima de 5.1 (SDK 21), mas o oposto não é sempre possível[136]
3. **Marketing vs. Realidade**: Fabricantes chineses frequentemente falsificam versão de Android reportada. O tablet roda como Android 4.4 internamente (kernel, frameworks), mas relata 5.1 para parecer mais atual[133]

**Dados que comprovam ser realmente Android 4.4**:

- `ro.build.id: KOT49H` (tag oficial AOSP Android 4.4.2)
- `ro.mediatek.version.branch: KK1.MP1` (branch KitKat)
- `ro.com.google.gmsversion: 4.4_r5` (GMS stack 4.4)
- `/proc/cpuinfo Hardware: MT6592T` e ARMv7 (tipicamente KitKat era última versão estável para MT6582)

**Conclusão**: O tablet roda **Android 4.4.2 KitKat realmente**, mas foi customizado para reportar "5.1". Aplicações que checam `ro.build.version.release` verão "5.1" (marketing), mas o `ro.build.version.sdk` (19) revela a verdade.

---

### Discrepância 3: Vantagens do Tablet (Apesar das Inconsistências)

Apesar de ser genérico e ter properties conflitantes, o T1000 tem características sólidas:

| Componente        | Valor                                  | Avaliação                                  |
| ----------------- | -------------------------------------- | ------------------------------------------ |
| **RAM**           | 3,871 GB (~4 GB)                       | ✅ Bom para 2017, tablet rodará fluido      |
| **Storage**       | 12.9 GB disponíveis em /data           | ✅ Razoável para tablet genérico            |
| **GPU**           | Mali-400 MP, OpenGL ES 2.0             | ✅ Adequado para 2D/3D leve, jogos leves    |
| **CPU**           | ARMv7 Quad-core 1.3 GHz (MT6582)       | ⚠️ Lento para padrões 2024, OK para 2017   |
| **Dual SIM 3G**   | Sim (Gemini support)                   | ✅ Recurso premium em genéricos             |
| **Wi-Fi 802.11n** | Sim (CONSYS_MT6582)                    | ✅ Wifi rápida (padrão da época)            |
| **Tela**          | 2560×1600 Retinal (marketing), 210 DPI | ✅ Excelente resolução (provavelmente real) |
| **Conectividade** | Bluetooth, FM Radio, GPS               | ✅ Completo                                 |

**Por que funciona bem apesar do spoof de versão**:

- O sistema operacional real (KitKat) é estável e otimizado para hardware MediaTek antigo
- A falsificação de versão não afeta funcionamento, apenas relatórios
- Mali-400 é suficiente para Android 4.4 (foi contemporânea)

---

## 4. POR QUE PROPRIEDADES VAZIAS E DISTRIBUÍDAS DIFERENTEMENTE

### Pattern de Distribuição em Tablets Genéricos MediaTek

MediaTek fornece templates de `build.prop` genéricos:

```
# Template MediaTek genérico (simplificado)
ro.mediatek.platform=MT6582
ro.mediatek.version.branch=KK1.MP1
ro.mediatek.version.release=ALPS.KK1.MP1.V2.10
# (ro.board.platform pode ser deixado vazio por simplicidade)
ro.hardware=<device-specific, preenchido por OEM>
ro.mtk.hardware=<cópia de ro.hardware>
```

Jlinksz/ROM builder deixa vazio:

- `ro.board.platform` (propriedade Qualcomm, não padrão MediaTek)
- `ro.hardware.gpu` (Google não obriga)
- `ro.hardware.keystore` (Android 6.0+ requirement, KitKat ignora)

Isso é **válido e comum**[135][145].

### Locais Diferentes para Mesma Informação

```
ro.hardware: mt6592t           # Device-tree naming
ro.mtk.hardware: mt6582        # MediaTek redundancy
/proc/cpuinfo Hardware: MT6592T # Kernel reportage
```

Todos coexistem porque servem **propósitos diferentes**:

1. Device tree matching (ro.hardware) para init.rc scripts
2. Kernel command line (proc/cpuinfo)
3. Vendor identification (ro.mtk.hardware)

Isso não é erro; é design de separação de concerns[121].

---

## 5. RESUMO FINAL ESTRUTURADO

### Hardware Real (Dados Brutos)

```
Marca:              Jlinksz (Fabricante Chinês Genérico)
Modelo:             T1000
SoC Real:           MediaTek MT6582 (provavelmente)
SoC Reportado:      MT6592T (spoof em ro.hardware)
CPU:                ARMv7 Quad-core @ 1.3 GHz (Cortex-A7)
GPU:                ARM Mali-400 MP, OpenGL ES 2.0
RAM:                ~4 GB (3,871,264 kB)
Storage:            ~13 GB /data + ~886 MB /system
Tela:               2560×1600 (210 DPI)
Conectividade:      3G/HSPA, Wi-Fi 802.11n (CONSYS_MT6582), Bluetooth, GPS, FM Radio
Dual SIM:           Sim (Gemini support)
Android Real:       4.4.2 KitKat (SDK 19, KOT49H build)
Android Reportado:  5.1 (spoof em ro.build.version.release)
Build Date:         2017-12-13
Kernel:             Linux 3.10.x (ALPS.KK1.MP1 branch)
```

### Por Que as Inconsistências Existem

| Inconsistência                                                | Razão                                                                                           |
| ------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| `ro.hardware: mt6592t` vs `ro.mediatek.platform: MT6582`      | Device-tree reutilizada de build anterior; pode ser spoof de marketing ou incompletude          |
| `ro.build.version.release: 5.1` vs `ro.build.version.sdk: 19` | Falsificação manual de release version em build.prop; tablet roda 4.4 internamente              |
| Propriedades vazias                                           | Android não força preenchimento de todas as properties; Jlinksz não preencheu opcionais         |
| Propriedades em locais diferentes                             | Design intencional: ro.hardware (device-tree), ro.mtk.hardware (vendor), /proc/cpuinfo (kernel) |

### Implicação para LineageOS

**LineageOS NÃO oferece suporte oficial** para MT6582 ou MT6592T em versões 14.1+. O máximo seria:

- LineageOS 14.1 (Android 6.0) - suporte de comunidade
- ROM AOSP customizadas não-oficiais
- Buscar "Jlinksz T1000 LineageOS" em XDA Developers ou GitHub

O tablet é **muito antigo** (2013 SoC, 2017 build) para custom ROMs modernas.

---

## CONCLUSÃO

O Jlinksz T1000 é um tablet genérico chinês com **propriedades conflitantes intencionais** (spoof de versão e SoC). As propriedades vazias são **normais** e não indicam erro. O ADB revelou a verdade: é Android 4.4.2 com MT6582 (ou MT6592T), Mali-400, ~4GB RAM, tela excelente. Perfeitamente funcional para 2017, inadequado para LineageOS moderno.

---

## REFERÊNCIAS

[116] SMOOREZ. "The $126 JLINKSZ ZH960 Generic Tablet from AliExpress" (YouTube, 2021)
[117] How to Edit Build Prop File on Android (Without/With Root) (YouTube, 2023)
[118] ROM Android: Porting A80A (onnocenter.or.id, 2015)
[119] Guidance on rooting a china tablet (XDA Forums, 2020)
[121] GitHub - rohantaneja/android_device_mediatek_mt6582
[133] MT6572 (MT6589 is Fake!) Hardware, Root & Flash (chinamobiles.org, 2025)
[135] [Guide]Build.prop Wiki (XDA Forums, 2015)
[136] apk compiled for targetsdk 19(kitkat), works on sdk v21 ... (StackOverflow, 2017)
[145] Property {props[0]} could not be found in build.prop #217 (GitHub, twrpdtgen)
[146] SDK Platform release notes (Android Studio, 2025)
