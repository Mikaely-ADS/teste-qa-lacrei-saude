# teste-qa-lacrei-saude

# Bug Reports — Lacrei Saúde (Portal Paciente)

> **Ambiente:** https://paciente-staging.lacreisaude.com.br/
**Data de reporte:** Junho 2026
**Reportado por:** Mikaely Alves Dias
> 

---

## BUG-001 — Autocomplete do navegador não valida campos de Sobrenome e E-mail no Cadastro

**Módulo:** Cadastro
**Severidade:** 🟠 Alto
**Prioridade:** P2
**Status:** 🆕 Aberto
**Navegador/OS:** A confirmar (reproduzível com autocomplete nativo do navegador)

### Feature: Cadastro da Pessoa Usuária

**Scenario: Validar preenchimento de sobrenome e e-mail via autocomplete**

Given que o usuário está na tela de cadastro
When os campos "Sobrenome" e "E-mail" são preenchidos utilizando autocomplete do navegador
Then os campos são exibidos como inválidos
And o botão "Cadastrar" permanece desabilitado
When o usuário digita manualmente os valores nos campos "Sobrenome" e "E-mail"
Then os campos são validados corretamente
And o botão "Cadastrar" é habilitado

### Pré-condições

- Acessar `https://paciente-staging.lacreisaude.com.br/cadastro`
- Ter sugestões de autocomplete salvas no navegador (nome, sobrenome, e-mail)

### Passos para Reproduzir

1. Acessar a tela de cadastro
2. Preencher o campo "Nome" manualmente
3. No campo "Sobrenome", selecionar a sugestão de autocomplete do navegador
4. No campo "E-mail", selecionar a sugestão de autocomplete do navegador
5. Preencher os demais campos manualmente
6. Observar o estado do botão "Cadastrar"

### Resultado Atual

Os campos "Sobrenome" e "E-mail" exibem os valores preenchidos pelo autocomplete, porém são marcados como **inválidos** e o botão "Cadastrar" permanece desabilitado.

### Resultado Esperado

O sistema deve capturar o valor preenchido via autocomplete da mesma forma que captura a digitação manual, validando os campos corretamente e habilitando o botão "Cadastrar" quando todos os critérios forem atendidos.

### Impacto

Usuários que utilizam autocomplete do navegador (prática muito comum em mobile) são **impedidos de concluir o cadastro** sem perceber o motivo, gerando abandono do fluxo.


> Feature: Cadastro da Pessoa Usuária
> 

!erro email e sobrenome por autocomplete.png

**Erro ao validar preenchimento de sobrenome e e-mail via autocomplete**

---

## BUG-002 — Posicionamento dos critérios de senha prejudica a experiência do usuário no Cadastro

**Módulo:** Cadastro
**Severidade:** 🟡 Médio (UX)
**Prioridade:** P3
**Status:** 🆕 Aberto
**Tipo:** Melhoria de UX / Layout

### Função: Critérios de senha

**Scenario: Exibição dos critérios de senha**

Given que o usuário está na tela de cadastro
When visualiza os critérios de senha
Then os critérios são exibidos abaixo do campo "Senha"
And deveriam ser exibidos abaixo do campo "Confirme sua senha" para melhor visualização

### Pré-condições

- Acessar `https://paciente-staging.lacreisaude.com.br/cadastro`

### Passos para Reproduzir

1. Acessar a tela de cadastro
2. Localizar o bloco de campos "Senha" e "Confirme sua senha"
3. Observar a posição dos critérios de senha em relação aos campos

### Resultado Atual

Os critérios de senha estão posicionados logo abaixo do input "Senha", ficando **distantes do campo "Confirme sua senha"**, onde o usuário tem maior necessidade de consulta.

### Resultado Esperado

**Opção recomendada A:** Mover os critérios para abaixo do campo "Confirme sua senha", mantendo-os visíveis ao longo de todo o preenchimento do bloco de senha.

**Opção recomendada B:** Exibir os critérios como checklist dinâmico que atualiza em tempo real conforme o usuário digita no campo "Senha", e mantê-los visíveis até que o campo "Confirme sua senha" também seja preenchido com sucesso.

```
Layout atual:          Layout sugerido (Opção A):
┌─────────────┐        ┌─────────────┐
│ Senha       │        │ Senha       │
└─────────────┘        └─────────────┘
 • critério 1           ┌─────────────┐
 • critério 2           │ Confirme    │
┌─────────────┐         └─────────────┘
│ Confirme    │          • critério 1
└─────────────┘          • critério 2
```

### Impacto

Usuários podem não visualizar o "Confirme sua senha", pois os critérios de senha  preenchem a maior parte do campo , aumentando a taxa de erros e retrabalho no formulário.

### Evidências

> 
> 
> 
> !textos div do requesitos de senha .png
> 
> **Exibição dos critérios de senha**
> 
> !2. cadastro nova senha mobile - user daniel mesma senha com q frase deveria ser .png
> 

---

## BUG-003 — Resultados de busca por profissionais variam conforme a conta do usuário logado

**Módulo:** Busca de Profissional de Saúde
**Severidade:** 🔴 Crítico
**Prioridade:** P1
**Status:** 🆕 Aberto

### Feature: Busca de Profissional de Saúde

**Scenario: Comparar resultados de busca entre usuários distintos**

Given que existe um usuário cadastrado com o e-mail "mikaellyalvesd@gmail.com"

When realiza uma busca por "médico"

Then são exibidos dois profissionais de teste

Given que existe um usuário cadastrado com o e-mail "daniel.fer.ss@hotmail.com"

When realiza uma busca por "médico"

Then apenas o profissional "Dra. Roberta Jones" é exibido

Com a conta `mikaellyalvesd@gmail.com` no desktop dois profissionais de teste aparecem; com a conta `daniel.fer.ss@hotmail.com` mobile apenas `Dr. Robert Jones` é exibido. Após nova tentativa no dia 08/05/2026, a massa de teste Dra. Teste Pro sumiu tanto no Mobile quanto Desktop. 

### Pré-condições

- Ter duas contas de teste cadastradas no ambiente staging
- Conta A: `mikaellyalvesd@gmail.com`
- Conta B: `daniel.fer.ss@hotmail.com`

### Passos para Reproduzir

**Com Conta A:**

1. Fazer login com `mikaellyalvesd@gmail.com`
2. Acessar a busca de profissionais
3. Pesquisar por "médico ou medico"
4. Anotar os profissionais retornados

**Com Conta B:**

1. Fazer logout
2. Fazer login com `daniel.fer.ss@hotmail.com`
3. Acessar a busca de profissionais
4. Pesquisar por "médico ou medico"
5. Comparar os resultados com os da Conta A

### Resultado Atual

| Conta | Termo buscado | Profissionais retornados |
| --- | --- | --- |
| mikaellyalvesd@gmail.com | médico / medico | 2 profissionais de teste |
| daniel.fer.ss@hotmail.com | médico / medico | Apenas Dra. Roberta Jones |

### Resultado Esperado

A busca de profissionais deve retornar os **mesmos resultados** para o mesmo termo de busca, independentemente do usuário autenticado, salvo se existir lógica de negócio documentada que justifique a diferença (ex: filtragem por localização, perfil ou histórico).

### Impacto

**Impacto crítico na experiência do paciente.** Usuários podem não encontrar profissionais disponíveis dependendo de qual conta utilizaram no cadastro, gerando desconfiança na plataforma e potencial perda de acesso a cuidados de saúde.

### Evidências

> teste realizado dia 05/06/2026
> 

!Screenshot_20.png

---

!conta Mikaely mobile .png

08/06/2026 - novo teste mobile, não apresenta DRa teste pro 

!Screenshot_23.png

teste 08/06/2026 conta Daniel

## BUG-004 — Código SMS para agendamento não é entregue ao número informado

**Módulo:** Agendamento de Consulta
**Severidade:** 🔴 Crítico
**Prioridade:** P1
**Status:** 🆕 Aberto

### Feature: Recebimento do código de confirmação via SMS para confirmar agendamento

**Scenario Outline: Falha no recebimento do código de confirmação via SMS**

Given que o usuário selecionou um profissional para agendamento

When informa o número "<telefone>" para recebimento do código por SMS

Then nenhum código SMS é recebido

Examplos:

Telefone 

83 98693-2993 

71 98617-3831

### Pré-condições

- Estar logado com uma conta válida no staging
- Ter selecionado um profissional e um horário disponível para agendamento

### Passos para Reproduzir

1. Fazer login com `mikaellyalvesd@gmail.com ou daniel.fer.ss@hotmail.com` 
2. Buscar por "médico" e selecionar um profissional
3. Avançar para o fluxo de agendamento
4. Quando solicitado, informar o número `83 98693-2993`
5. Aguardar o recebimento do SMS (aguardar pelo menos 5 minutos)
6. Repetir o teste com o número `71 98617-3831`

### Resultado Atual

Nenhum código SMS é recebido em nenhum dos números testados:

- `83 98693-2993` — SMS não recebido
- `71 98617-3831` — SMS não recebido

### Resultado Esperado

O sistema deve enviar o código de verificação por SMS ao número informado em até 60 segundos, permitindo a continuidade do agendamento.

### Impacto

**O agendamento de consultas está completamente bloqueado** para usuários que dependem da verificação via SMS. Trata-se de uma funcionalidade crítica da plataforma.

---

## BUG-005 — Autocomplete de "Nova senha" marca o campo como inválido na Recuperação de Senha

**Módulo:** Recuperação de Senha
**Severidade:** 🟠 Alto
**Prioridade:** P2
**Status:** 🆕 Aberto

### Feature: Recuperação de Senha

**Scenario: Validar preenchimento da nova senha via autocomplete**

Given que o usuário está na tela de recuperação de senha

When o campo "Nova senha" é preenchido utilizando a sugestão de senha do navegador

Then o campo é exibido como inválido

And os critérios de senha são considerados não atendidos

And o botão "Cadastrar nova senha" permanece desabilitado

When o usuário digita manualmente uma nova senha

Then os critérios de senha são atendidos

And o botão "Cadastrar nova senha" é habilitado

### Pré-condições

- Acessar o fluxo de recuperação de senha
- Ter sugestão de senha salva ou gerada pelo gerenciador do navegador

### Passos para Reproduzir

1. Acessar `https://paciente-staging.lacreisaude.com.br/recuperar-senha`
2. Informar o e-mail e seguir o fluxo até a tela de nova senha
3. No campo "Nova senha", utilizar a **sugestão automática de senha do navegador** (ícone de chave/senha)
4. Observar o estado do campo e do botão "Cadastrar nova senha"

### Resultado Atual

O campo "Nova senha" exibe o valor preenchido pelo autocomplete, porém é marcado como **inválido**, e o botão "Cadastrar nova senha" permanece desabilitado.

### Resultado Esperado

O sistema deve reconhecer o valor preenchido via autocomplete/gerenciador de senhas como válido, desde que os critérios de senha sejam atendidos, habilitando o botão "Cadastrar nova senha".

### Impacto

Usuários que adotam boas práticas de segurança (uso de gerenciadores de senha) são **penalizados com uma experiência quebrada**, podendo abandonar o fluxo de recuperação de acesso.

### Evidências

> Autocomplete senha
> 

!1.1.1. Altocomplete na área de cadastro de nova senha.png

---

## BUG-006 — Mensagem de erro genérica ao tentar reutilizar a senha atual na Recuperação de Senha

**Módulo:** Recuperação de Senha
**Severidade:** 🟠 Alto
**Prioridade:** P2
**Status:** 🆕 Aberto

### Feature: Recuperação de senha

**Scenario: Informar corretamente quando a nova senha for igual à senha atual**

Given que o usuário está na tela de recuperação de senha

When informa uma nova senha igual à senha atual

And confirma a alteração da senha

Then o sistema retorna a mensagem "Os dados enviados são inválidos"

When a resposta da API é analisada

Then a mensagem retornada é "Uma nova senha não pode ser igual à atual"

And o sistema deveria exibir ao usuário a mensagem

"""

Uma nova senha não pode ser igual à atual.

"""

Instead of

"""

Os dados enviados são inválidos.

### Pré-condições

- Ter uma conta cadastrada no ambiente staging
- Estar no fluxo de recuperação de senha com o token de redefinição válido

### Passos para Reproduzir

1. Iniciar o fluxo de recuperação de senha com um e-mail válido
2. Acessar o link de redefinição recebido
3. No campo "Nova senha", digitar a **mesma senha utilizada no cadastro**
4. Confirmar a senha e clicar em "Cadastrar nova senha"
5. Observar a mensagem de erro exibida

### Resultado Atual

A interface exibe: **"Os dados enviados são inválidos."**

A API retorna (verificado via DevTools → Network):

```json
{
  "message": "Uma nova senha não pode ser igual à atual"
}
```

### Resultado Esperado

A interface deve exibir a mensagem clara e orientativa:
**"Sua nova senha não pode ser igual à senha atual. Por favor, escolha uma senha diferente."**

### Impacto

- **Usabilidade:** O usuário não consegue entender o que está errado e pode desistir do fluxo de recuperação de acesso
- **Segurança:** A mensagem genérica é menos informativa que o ideal, mas não chega a expor dados sensíveis
- **Acessibilidade:** Mensagens de erro devem ser claras e descritivas (WCAG 3.3.1 — Error Identification)

### Evidências

!1.0 cadastro nova senha campos com a mesma senha - campos obdecem ao requesito falha depois.png

!3.  cadastro nova senha frase generica - user daniel.png

!2. cadastro nova senha mobile - user daniel mesma senha com q frase deveria ser .png

## BUG-007 — Expiração prematura da sessão por inatividade

**Módulo:** Autenticação

**Severidade:** 🟡 Médio

**Prioridade:** P3

**Status:** 🆕 Aberto

### Feature: Gerenciamento de Sessão

**Scenario: Encerramento prematuro da sessão por inatividade**

Given que o usuário realizou login com sucesso

And permanece autenticado no sistema

When permanece inativo por aproximadamente 5 minutos

Then a sessão é encerrada automaticamente

And o usuário é redirecionado para a tela de login

### Pré-condições

- Usuário autenticado na plataforma
- Nenhuma interação realizada após o login

### Passos para Reproduzir

1. Acessar `https://paciente-staging.lacreisaude.com.br/`
2. Realizar login com credenciais válidas
3. Permanecer na plataforma sem realizar nenhuma interação
4. Aguardar aproximadamente 5 minutos
5. Observar o comportamento da sessão

### Resultado Atual

A sessão é encerrada automaticamente após aproximadamente 5 minutos de inatividade, redirecionando o usuário para a tela de login e causando perda de contexto durante a navegação.

### Resultado Esperado

O tempo de expiração da sessão deve ser suficiente para a utilização normal da plataforma, permitindo que o usuário retome a navegação sem ser desconectado em fluxos de uso comum.

### Impacto

Usuários que navegam com mais calma ou que pausam brevemente a utilização são **desconectados inesperadamente**, causando perda de contexto e necessidade de novo login, comprometendo a experiência geral da plataforma.

### Evidências

!image.png

Após o periodo de inatividade, o sistema volta com o login e senha da sessão anterior a que ele deslogou

---

## BUG-008 — Botão "Pesquisar" exige múltiplos cliques para executar a busca

**Módulo:** Busca de Profissionais de Saúde

**Severidade:** 🟠 Alto

**Prioridade:** P2

**Status:** 🆕 Aberto

### Feature: Busca de Profissionais de Saúde

**Scenario: Botão de pesquisa não responde ao primeiro clique**

Given que o usuário está na tela de busca de profissionais

And preenche um termo válido para pesquisa

When clica no botão "Pesquisar"

Then a busca deveria ser executada imediatamente

But a ação não é executada no primeiro clique

When o usuário realiza dois cliques consecutivos na região central do botão

Then a pesquisa é executada com sucesso

### Pré-condições

- Usuário autenticado na plataforma
- Estar na tela de busca de profissionais de saúde
- Ter um termo válido para pesquisa

### Passos para Reproduzir

1. Acessar `https://paciente-staging.lacreisaude.com.br/`
2. Realizar login com credenciais válidas
3. Navegar até a tela de busca de profissionais
4. Preencher um termo válido no campo de pesquisa
5. Clicar uma única vez no botão "Pesquisar"
6. Observar que a busca não é executada
7. Realizar dois cliques consecutivos na região central do botão

### Resultado Atual

O botão "Pesquisar" não responde ao primeiro clique. A pesquisa somente é executada após dois cliques consecutivos realizados na região central do componente, indicando possível problema no evento de clique ou na área ativa do botão.

### Resultado Esperado

O botão "Pesquisar" deve executar a busca imediatamente ao primeiro clique, independentemente da área do botão acionada, garantindo comportamento consistente e previsível para o usuário.

### Impacto

Usuários são impedidos de utilizar a funcionalidade principal da plataforma de forma intuitiva, podendo interpretar o botão como **não funcional** e abandonar o fluxo de busca por profissionais de saúde.

### Evidências

!image.png

Após clicar duas vezes no meio do icone ele processa a pesquisa

## 📊 Resumo dos Bugs Reportados

| ID | Módulo | Título resumido | Severidade | Prioridade |
| --- | --- | --- | --- | --- |
| BUG-001 | Cadastro | Autocomplete não valida Sobrenome e E-mail | 🟠 Alto | P2 |
| BUG-002 | Cadastro | Critérios de senha mal posicionados (UX) | 🟡 Médio | P3 |
| BUG-003 | Busca | Resultados variam conforme conta do usuário | 🔴 Crítico | P1 |
| BUG-004 | Agendamento | SMS de verificação não é entregue | 🔴 Crítico | P1 |
| BUG-005 | Recuperação de Senha | Autocomplete marca "Nova senha" como inválida | 🟠 Alto | P2 |
| BUG-006 | Recuperação de Senha | Mensagem de erro genérica ao reutilizar senha atual | 🟠 Alto | P2 |
| BUG- 007 | Autenticação | Sessão expira após ~5 minutos de inatividade | **🟡 Médio** | P3 |
| BUG-008 | Busca | Botão "Pesquisar" responde apenas após múltiplos cliques | **🟠 Alto** | P2 |

### Prioridade de Resolução Sugerida

```
🚨 P1 — Resolver na sprint atual
   → BUG-003: Busca retorna resultados inconsistentes por conta
   → BUG-004: SMS de verificação não entregue (agendamento bloqueado)

⚠️  P2 — Resolver na próxima sprint
   → BUG-001: Autocomplete não valida campos no Cadastro
   → BUG-005: Autocomplete marca "Nova senha" como inválida
   → BUG-006: Mensagem genérica na recuperação de senha
   > BUG-008	Busca	Botão "Pesquisar" responde apenas após múltiplos cliques	🟠 Alto	P2

📋 P3 — Backlog / Melhoria
   → BUG-002: Reposicionamento dos critérios de senha (UX)
   >  BUG- 007	Autenticação	Sessão expira após ~5 minutos de inatividade
```

---

# Registro de **Teste de Desempenho**

Teste de cenário API - Não foi possivel realizar o teste de API, pois somente a aplicação Front-end possui acesso ao Back-end. 

!image.png

Sistema Posteman API usando para o teste de desempenho;

# Teste de desempenho Lacrei Saúde

Os testes foram realizados de forma incremental para avaliar a capacidade de resposta, estabilidade e limites de escalabilidade da API de busca de profissionais sob diferentes níveis de carga (Virtual Users - VUs).

### 1. Teste de Carga Escalonada (Baseline a Stress)

- **Cenário de 1 VU (Screenshot 302):** Estabeleceu o tempo de resposta base. A aplicação respondeu de maneira estável com uma média de **71ms** e sem erros, apresentando um uso moderado de memória (73.4%).
- **Cenário de 10 VUs (Screenshot 301):** Houve um aumento no volume de requisições (~138 req/s). O tempo de resposta médio melhorou para **51ms**, mantendo 0% de erros, indicando boa eficiência do cache ou do servidor sob carga moderada.
- **Cenário de 20 VUs (Screenshot 304):** A carga subiu para ~276 req/s. O tempo de resposta se manteve estável em **50ms**, demonstrando que o sistema lida bem com o dobro da carga anterior sem degradação de performance.
- **Cenário de 30 VUs (Fase de Estabilidade - Screenshot 305):** Pico de ~337 req/s. Mesmo com o aumento de usuários, a média permaneceu em **53ms**. O uso de CPU ficou sob controle (37.2%), embora a memória tenha subido para 82.5%, sugerindo que este é o ponto ideal de operação.

### 2. Identificação de Ponto de Ruptura (Screenshot 303)

Durante um dos ciclos com **30 VUs**, foi identificado um cenário crítico de falha:

- **Erro de 100%:** O sistema atingiu um pico de uso de memória de **96.2%** e CPU de **70.1%**.
- Neste estado, apesar do tempo de resposta reportado ser baixo (1ms), a taxa de erro total indica que o servidor parou de processar internamente as requisições (possível bloqueio de segurança, firewall ou esgotamento de recursos do ambiente de teste/sandbox).

### Conclusão Técnica

- **Ponto de Eficiência:** A API demonstra excelente performance entre **1 e 30 VUs**, mantendo tempos de resposta sub-60ms (P95 de 68ms).
- **Alerta de Infraestrutura:** O sistema apresenta instabilidade crítica quando a memória ultrapassa os **95%**, resultando em interrupção total do serviço (100% de erro).
- **Recomendação:** Investigar o consumo de memória durante buscas intensas e avaliar a necessidade de escalonamento vertical ou otimização de cache para evitar o travamento identificado no pico de carga.

## Evidencias:

!image.png

!image.png

!image.png

!image.png

!image.png

Sistema Posteman API usando para o teste de desempenho após várias requisição, informando todas as métricas (% de erro, número de requisições e quantidade de usuários digitais)

## Teste de acessibilidade

No teste de acessibilidade com NVDA, percebe-se a necessidade de um botão de "voltar" após a busca por um profissional, pois caso o usuário queira pesquisar por outro profissional, fica bem dificil para ele executar essa ação. A execução do login e até a busca somente com tab foi fácil.

!image.png

# Acessibilidade (Lighthouse)

**Ambiente:** Desktop - Tela de Login

**Score Atual:** 92/100

---

### 📌 ACHADO 01: Atributos ARIA Proibidos ou Inválidos

> *No Lighthouse: "Elements use prohibited ARIA attributes"*
> 
- **O que é:** O ARIA (*Accessible Rich Internet Applications*) serve para dar contexto a leitores de tela. Este erro ocorre quando um elemento HTML recebe um atributo que não "combina" com ele (ex: colocar um atributo de menu em um campo de texto).
- **Por que ocorre:** Geralmente causado pelo uso de bibliotecas de componentes (Design System) mal configuradas ou pela tentativa de forçar acessibilidade manualmente em elementos que já têm comportamento nativo.
- **Risco/Impacto:**
    - **Usuários de Leitores de Tela:** O software pode anunciar informações confusas, travar ou não ler o estado do elemento (ex: não avisar que um campo é obrigatório ou que um botão está desabilitado).
    - **Navegação:** Pode quebrar a árvore de acessibilidade do navegador.
- **Como o QA valida:**
    1. Abrir o Console do Navegador (F12).
    2. Inspecionar o elemento apontado no relatório.
    3. Verificar se existem atributos como `aria-` em tags que não deveriam ter, ou se o `role` (papel) do elemento é incompatível com o atributo usado.
- **Resultado Esperado:** Todos os atributos ARIA devem ser válidos para o `role` do elemento, seguindo as especificações da W3C. Se o elemento for nativo (como um `<button>`), o ideal é remover o ARIA e deixar o HTML semântico resolver.

---

### 📌 ACHADO 02: Atributo de Idioma Inválido ou Ausente

> *No Lighthouse: "[lang] attributes do not have a valid value"*
> 
- **O que é:** O atributo `lang` dentro da tag `<html>` define para o sistema em qual língua a página está escrita.
- **Por que ocorre:** A tag `<html>` pode estar sem o atributo `lang`, ou o valor inserido não segue o padrão internacional (ex: usar `br` em vez de `pt-BR`, ou deixar o campo vazio/com erro de digitação).
- **Risco/Impacto:**
    - **Leitores de Tela:** O leitor pode tentar ler o texto em português usando uma pronúncia em inglês (sotaque robótico), tornando o conteúdo incompreensível.
    - **SEO e Browser:** Prejudica o ranqueamento em buscas locais e ferramentas de tradução automática do navegador podem não funcionar corretamente.
- **Como o QA valida:**
    1. No navegador, clique com o botão direito e vá em "Exibir código fonte" (ou use o Inspetor).
    2. Procure pela primeira linha do código: `<html lang="...">`.
    3. Confirme se o valor está presente e se é `pt-BR` (ou o idioma destino).
- **Resultado Esperado:** A tag HTML deve obrigatoriamente conter um atributo de idioma válido (ex: `<html lang="pt-BR">`).

---

*Bugs reportados em Junho 2026 | QA — Lacrei Saúde*
