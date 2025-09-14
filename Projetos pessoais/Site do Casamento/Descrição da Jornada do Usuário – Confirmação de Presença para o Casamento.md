# Descrição da Jornada do Usuário – Confirmação de Presença para o Casamento

## Dialog 1 - Confirmação de Identificação por Telefone

- Layout: Tela de identificação com campo de entrada de telefone.
- Cores: Tons neutros, botões em destaque.
- Conteúdo:
  - Número de telefone pré-preenchido ou digitado pelo usuário.
  - Botões "Cancelar" e "Continuar".
- Experiência do Usuário:
  - Confirmação do número antes de prosseguir.

---

## Dialog 1.1 - Erro de Validação por Número de Telefone

- Layout: Similar à anterior, mas com mensagem de erro.
- Cores: Fundo claro, mensagem de erro em destaque (vermelho).
- Conteúdo:
  - Solicitação do número de telefone associado ao convite.
  - Mensagem de erro: "Não consegui encontrar o número digitado!" (caso o número não esteja cadastrado).
  - Botões "Cancelar" e "Continuar".
- Experiência do Usuário:
  - Fluxo de autenticação por telefone para evitar confusões entre convidados.
  - Feedback de erro claro e instruções para corrigir.

---

## Dialog 2 – Convite para Confirmar Presença

- Layout: Tela inicial com fundo claro, tipografia elegante e discreta.
- Cores: Predominância de tons claros, com destaque em preto para o texto e botões em contraste.
- Conteúdo:
  - Saudação personalizada: "Olá, {Nome} {e Complemento}".
  - Mensagem amigável incentivando a confirmação de presença.
  - Dois botões lado a lado: "Não" e "Sim".
- Experiência do Usuário:
  - O usuário é recebido de forma calorosa e direcionado a responder rapidamente.
  - Botões bem destacados para uma decisão binária (sim/não).

---

## Dialog 3 - Formulário de Confirmação de Dados e Acompanhantes

- Layout: Formulário estruturado em seções, com campos de entrada e seleção de faixa etária.
- Cores: Fundo branco, texto em preto, [combobox](https://www.shadcn.io/components/forms/combobox) em faixas etárias.
- Conteúdo:
  - Instruções claras para preenchimento dos nomes completos e idades.
  - Campos interativos para:
    - Nome do convidado principal (obrigatório).
    - Acompanhantes 1 e 2 (Quantidade Variável), com seleção de faixa etária para cada acompanhante.
  - Botão "Continuar" no final.
- Experiência do Usuário:
  - Interface intuitiva, com campos de digitação e opções de idade por [combobox](https://www.shadcn.io/components/forms/combobox).
  - Validação provavelmente ocorre em tempo real (ex.: campo de nome obrigatório).

---

## Dialog 4 - Revisão Final dos Dados

- Layout: Lista de convidados com nomes e faixas etárias.
- Cores: Texto em preto sobre fundo branco, botão de confirmação em destaque.
- Conteúdo:
  - Lista de todos os nomes e respectivas idades.
  - Checkbox de confirmação: "Confirmo os nomes e idades estão corretos".
  - Botões "Voltar" e "Confirmar Presença", "Confirmar Presença" está bloqueado enquanto não clicar no checkbox.
- Experiência do Usuário:
  - Etapa crítica para evitar erros.
  - Checkbox obrigatória para prosseguir.
  - Alertas sobre irreversibilidade da confirmação.

---

## Dialog 4.1. Revisão Final dos Dados - Checkbox Aceito

- Layout: Similar à anterior, mas com checkbox mais aparente (Azul).
- Cores: Checkbox em azul quando marcado.
- Conteúdo:
  - Checkbox: "Confirmo que revisei e li os dados acima listados".
  - Botões "Voltar" e "Confirmar presença".
- Experiência do Usuário:
  - Maior clareza na exigência de confirmação explícita.

---

## Dialog 5. Confirmação Bem-Sucedida e Redirecionamento

- Layout: Tela de sucesso com mensagem positiva.
- Cores: Tons claros, botões em destaque.
- Conteúdo:
  - Mensagem: "Presença confirmada com sucesso!".
  - Opção para "Sair" ou "Ver a Lista de Presentes".
- Experiência do Usuário:
  - Feedback positivo e claro.
  - Chamada para ação secundária (lista de presentes).

---


