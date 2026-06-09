# README — Roteiro de Testes QA | Lacrei Saúde (Portal Paciente)
Objetivo
Este documento tem como objetivo servir como roteiro de execução dos testes funcionais, acessibilidade e desempenho realizados no Portal do Paciente da Lacrei Saúde, permitindo a reprodução dos cenários identificados durante a homologação do ambiente de staging.

Ambiente Testado
URL: https://paciente-staging.lacreisaude.com.br/
Ambiente: Staging
Data de Execução: Junho/2026
Responsável pelos testes: Mikaely Alves Dias
Escopo dos Testes
Foram executados os seguintes tipos de teste:

Testes Funcionais
Testes de Usabilidade (UX)
Testes de Acessibilidade
Testes de Desempenho
Testes de Compatibilidade com Autocomplete do Navegador
Testes de Sessão
Testes de Fluxos Críticos do Paciente
1. Testes Funcionais
1.1 Cadastro de Usuário
Objetivo
Validar o fluxo completo de cadastro de pacientes.

Cenários Executados
Cenário 1 — Cadastro utilizando autocomplete do navegador
Objetivo
Validar se o sistema reconhece corretamente valores preenchidos pelo autocomplete nativo do navegador.

Passos
Acessar a página de cadastro.
Preencher o campo Nome manualmente.
Selecionar uma sugestão do navegador para Sobrenome.
Selecionar uma sugestão do navegador para E-mail.
Preencher os demais campos.
Verificar o comportamento do botão Cadastrar.
Resultado Esperado
Campos devem ser considerados válidos.
Botão Cadastrar deve ser habilitado.
Resultado Encontrado
Sobrenome e E-mail permanecem inválidos.
Botão Cadastrar continua desabilitado.
Evidência
BUG-001

Cenário 2 — Exibição dos critérios de senha
Objetivo
Avaliar a experiência do usuário durante a criação da senha.

Passos
Acessar a tela de cadastro.
Localizar os campos Senha e Confirmar Senha.
Observar a posição dos critérios de senha.
Resultado Esperado
Os critérios devem permanecer visíveis durante todo o preenchimento da senha e confirmação.

Resultado Encontrado
Os critérios ficam posicionados abaixo do primeiro campo, dificultando sua consulta durante a confirmação da senha.

Evidência
BUG-002

2. Busca de Profissionais
Objetivo
Validar a consistência dos resultados apresentados para diferentes usuários.

Cenário 1 — Comparação de resultados entre contas
Contas utilizadas
Conta A:

mikaellyalvesd@gmail.com
Conta B:

daniel.fer.ss@hotmail.com
Passos
Fazer login com a Conta A.
Pesquisar por "médico".
Registrar os resultados.
Fazer logout.
Fazer login com a Conta B.
Repetir a pesquisa.
Comparar os resultados.
Resultado Esperado
A mesma pesquisa deve retornar os mesmos profissionais para ambos os usuários.

Resultado Encontrado
Resultados diferentes para cada conta.

Evidência
BUG-003

Cenário 2 — Funcionamento do botão Pesquisar
Objetivo
Validar a responsividade do botão de busca.

Passos
Fazer login.
Acessar Busca de Profissionais.
Informar um termo válido.
Clicar em Pesquisar apenas uma vez.
Resultado Esperado
A busca deve ser executada imediatamente.

Resultado Encontrado
A busca somente é realizada após múltiplos cliques.

Evidência
BUG-008

3. Agendamento de Consulta
Objetivo
Validar o recebimento do código SMS para confirmação do agendamento.

Cenário — Recebimento do SMS
Telefones utilizados
83 98693-2993
71 98617-3831
Passos
Fazer login.
Selecionar um profissional.
Iniciar agendamento.
Informar um dos números de telefone.
Aguardar até 5 minutos.
Resultado Esperado
Recebimento do código SMS em até 60 segundos.

Resultado Encontrado
Nenhum SMS foi recebido.

Evidência
BUG-004

4. Recuperação de Senha
Objetivo
Validar o fluxo de redefinição de senha.

Cenário 1 — Utilização de autocomplete para nova senha
Passos
Iniciar recuperação de senha.
Chegar à tela de nova senha.
Utilizar a sugestão automática do navegador.
Resultado Esperado
A senha deve ser validada normalmente.

Resultado Encontrado
A senha é considerada inválida.

Evidência
BUG-005

Cenário 2 — Reutilização da senha atual
Passos
Iniciar recuperação de senha.
Informar a mesma senha já utilizada.
Confirmar a alteração.
Resultado Esperado
Exibição da mensagem:

Sua nova senha não pode ser igual à senha atual.

Resultado Encontrado
Exibição da mensagem:

Os dados enviados são inválidos.

Evidência
BUG-006

5. Gerenciamento de Sessão
Objetivo
Validar o comportamento da autenticação após período de inatividade.

Cenário — Expiração da Sessão
Passos
Realizar login.
Permanecer inativo.
Aguardar aproximadamente 5 minutos.
Resultado Esperado
A sessão deve permanecer ativa por um período compatível com a utilização normal da plataforma.

Resultado Encontrado
Logout automático após aproximadamente 5 minutos.

Evidência
BUG-007

6. Testes de Acessibilidade
Ferramentas Utilizadas
Lighthouse
NVDA
Navegação por teclado (TAB)
6.1 Teste com Leitor de Tela (NVDA)
Objetivo
Validar a navegação utilizando tecnologias assistivas.

Cenário Executado
Realizar login utilizando apenas teclado.
Navegar pela busca de profissionais.
Tentar retornar para uma nova busca.
Resultado Encontrado
Foi identificada dificuldade para retornar ao fluxo de busca após acessar os resultados.

Recomendação
Adicionar mecanismo claro de retorno para facilitar a navegação de usuários que utilizam leitores de tela.

6.2 Lighthouse
Resultado Geral
Métrica	Score
Performance	45
Accessibility	92
Best Practices	73
SEO	82
Achado 1 — Uso incorreto de atributos ARIA
Descrição
O Lighthouse identificou elementos utilizando atributos ARIA incompatíveis.

Impacto
Leitores de tela podem interpretar incorretamente os elementos.
Possíveis falhas de navegação assistiva.
Recomendação
Utilizar HTML semântico sempre que possível e revisar atributos ARIA implementados.

Achado 2 — Atributo lang inválido
Descrição
O atributo lang da página não possui valor válido.

Impacto
Pronúncia incorreta em leitores de tela.
Possíveis impactos em SEO e tradução automática.
Recomendação
Configurar corretamente:

html
Copy
<html lang="pt-BR">
7. Testes de Desempenho
Ferramenta Utilizada
Postman Performance Testing
Limitações Encontradas
Não foi possível executar testes diretamente na API do backend, pois o acesso às rotas é restrito à aplicação frontend.

Cenários Executados
Cenário 1 — 1 Usuário Virtual
Métrica	Resultado
Tempo médio	71 ms
Erros	0%
Cenário 2 — 10 Usuários Virtuais
Métrica	Resultado
Tempo médio	51 ms
Erros	0%
Cenário 3 — 20 Usuários Virtuais
Métrica	Resultado
Tempo médio	50 ms
Erros	0%
Cenário 4 — 30 Usuários Virtuais
Métrica	Resultado
Tempo médio	53 ms
Erros	0%
CPU	37.2%
Memória	82.5%
Cenário Crítico Identificado
Ponto de Ruptura
Durante um ciclo de execução com 30 usuários virtuais:

Métrica	Resultado
CPU	70.1%
Memória	96.2%
Taxa de erro	100%
Conclusão
Ao ultrapassar aproximadamente 95% de utilização de memória, o ambiente apresentou indisponibilidade total.

Recomendação
Investigar consumo de memória.
Revisar mecanismos de cache.
Avaliar escalabilidade da infraestrutura.
Resumo Executivo
ID	Módulo	Severidade
BUG-001	Cadastro	Alto
BUG-002	Cadastro	Médio
BUG-003	Busca	Crítico
BUG-004	Agendamento	Crítico
BUG-005	Recuperação de Senha	Alto
BUG-006	Recuperação de Senha	Alto
BUG-007	Autenticação	Médio
BUG-008	Busca	Alto
Prioridades Recomendadas
P1 — Correção Imediata
BUG-003 — Busca inconsistente entre usuários
BUG-004 — SMS não recebido para agendamento
P2 — Próxima Sprint
BUG-001 — Problema com autocomplete
BUG-005 — Autocomplete na redefinição de senha
BUG-006 — Mensagem genérica de erro
BUG-008 — Botão Pesquisar exige múltiplos cliques
P3 — Backlog
BUG-002 — Critérios de senha
BUG-007 — Expiração prematura da sessão
QA Report — Lacrei Saúde (Portal Paciente)
Junho/2026
Autor: Mikaely Alves Dias
Ambiente: Staging (paciente-staging.lacreisaude.com.br)
