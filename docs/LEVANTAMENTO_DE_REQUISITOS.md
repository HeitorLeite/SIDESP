# Levantamento de Requisitos — SIDESP

> **SIDESP — Sistema Integrado de Desenvolvimento Esportivo Público**  
> Documento de requisitos do produto completo, aplicável ao frontend, ao backend e às integrações.

## Identificação do documento

| Campo | Valor |
| --- | --- |
| Projeto | SIDESP — Sistema Integrado de Desenvolvimento Esportivo Público |
| Órgão demandante | Secretaria de Esportes de Guaratinguetá |
| Responsável de negócio | Secretaria de Esportes de Guaratinguetá — representante nominal e contato **pendentes** |
| Product Owner | Lívia Andrade — contato **pendente** |
| Responsável técnico | Heitor Leite (Tech Lead) — contato **pendente** |
| Segurança | Responsável **pendente** |
| Privacidade/Encarregado | Encarregado da Prefeitura/Secretaria — nome e contato **pendentes de confirmação** |
| Equipe | Diego Henrico, Heitor Leite, Kauã Raphael, Lívia Andrade, Luís Mario e Micael Phillipini |
| Versão | `0.1.0` |
| Data da revisão | 12/08/2026 |
| Classificação | Interna |
| Status | Rascunho — não aprovado |
| Próxima revisão | Após validação das questões bloqueadoras da seção 18 ou quando escopo/regra mudar |

## Aprovações

| Papel | Responsável | Situação | Data |
| --- | --- | --- | --- |
| Responsável de negócio | Pendente | Não aprovado | — |
| Product Owner | Lívia Andrade | Pendente de revisão | — |
| Responsável técnico | Heitor Leite | Pendente de revisão | — |
| Segurança | Pendente | Não avaliado | — |
| Privacidade/Encarregado | Pendente | Não avaliado | — |
| QA | Micael Phillipini | Pendente de revisão dos critérios de aceite | — |

## 1. Finalidade e fontes

Este documento registra o que deve ser construído, para quem, sob quais regras e como o comportamento será aceito. Ele descreve o **produto completo**.

### 1.1 Fontes e hierarquia

1. **Documento de Visão — SIDESP, versão 1.0:** fonte primária do objetivo, problemas, atores, regras `RN001` a `RN025` e requisitos `SE001` a `SE035`.
2. Entrevistas com representantes da Secretaria e questionário aplicado a professores, citados no Documento de Visão: origem de negócio a ser preservada e, quando possível, anexada ao projeto.
3. Guia de Documentação e Governança Antes do Desenvolvimento: fonte do formato e dos controles mínimos deste documento.


Em caso de conflito, a decisão aprovada pelo responsável de negócio e registrada no histórico deste documento prevalece. Regras não aprovadas permanecem como `Pendente`.

### 1.2 Convenções

- **DEVE / OBRIGATÓRIO:** requisito indispensável.
- **NÃO DEVE / PROIBIDO:** comportamento vedado.
- **DEVERIA / RECOMENDADO:** comportamento esperado, cuja ausência exige justificativa.
- **PODE:** alternativa permitida após avaliação.
- **Proposto:** planejado, ainda não implementado no produto alvo.
- **Pendente:** exige informação ou decisão externa.
- Prioridades deste rascunho são propostas e devem ser ratificadas pelo Product Owner.
- Os IDs `RF-*`, `RNF-*` e `RN-*` são estáveis. Os códigos `SE###` permanecem registrados como origem para não perder a rastreabilidade com o Documento de Visão.

## 2. Resumo executivo

A Secretaria de Esportes de Guaratinguetá utiliza processos majoritariamente manuais e descentralizados para inscrições, controle de vagas, chamadas e relatórios. O cenário provoca registros duplicados, retrabalho, inconsistências, dificuldade de localizar informações de alunos e pouca rastreabilidade das turmas e listas de espera.

O SIDESP centralizará cadastros, inscrições, turmas, chamadas, justificativas, notícias, notificações e informações gerenciais. Alunos acompanharão suas inscrições e frequência; professores trabalharão apenas com as turmas às quais estão vinculados; administradores gerenciarão a operação e analisarão dados. O sistema também prevê comunicação por WhatsApp, relatórios, exportações e mapas de calor.

O produto tratará CPF, dados de contato, dados de menores, possíveis dados de saúde, frequência e documentos comprobatórios. A baseline não poderá ser aprovada antes da validação de privacidade, segurança, retenção, papéis de acesso e fornecedor de mensageria.

## 3. Problema atual e impactos

| Problema | Pessoas afetadas | Impacto observado | Resultado esperado |
| --- | --- | --- | --- |
| Informações de alunos dispersas e de difícil acesso | Professores e administradores | Atendimento lento e dificuldade de gestão; risco em situações emergenciais | Cadastro centralizado, acesso rápido e autorização por perfil |
| Controle manual e repetitivo | Professores e administradores | Retrabalho, inconsistência, cansaço operacional e maior probabilidade de erro | Registro único de chamada alimentando históricos, indicadores e relatórios |
| Falta de controle de vagas e inscrições | Alunos, professores e administradores | Turmas desorganizadas e lista de espera pouco confiável | Limites automáticos, fila ordenada e oferta rastreável de vagas |
| Falta de vínculo estruturado entre aluno, turma e professor | Todos os perfis internos | Acompanhamento e responsabilização prejudicados | Vínculos explícitos e consultas coerentes por perfil |
| Comunicação descentralizada | Alunos, responsáveis, professores e administradores | Avisos tardios ou sem comprovação de envio | Notificações acionadas por evento com histórico de entrega |
| Dados gerenciais não consolidados | Gestores | Decisões tomadas sem visão integrada | Relatórios, exportações e visualizações com critérios conhecidos |
| Internet insuficiente em alguns polos | Professores | Possível falha ou atraso no registro durante a aula | Estratégia de operação em conectividade instável **pendente de decisão** |

## 4. Objetivos de negócio e indicadores

| ID | Objetivo | Evidência de alcance | Meta |
| --- | --- | --- | --- |
| `OBJ-01` | Centralizar a gestão das atividades esportivas | Cadastros, vínculos, inscrições e chamadas consultáveis em uma única solução | Cobertura de processos e data de corte pendentes |
| `OBJ-02` | Reduzir registros repetidos de presença | Uma chamada salva alimenta o histórico do aluno e os relatórios autorizados | Presença registrada uma única vez; redução percentual pendente de baseline |
| `OBJ-03` | Tornar vagas e filas rastreáveis | Capacidade, inscrições, posição, ofertas, confirmações e expirações possuem histórico | 100% das turmas do escopo com capacidade controlada |
| `OBJ-04` | Aumentar confiabilidade das informações | Restrições, unicidade, validações e trilha de alterações críticas verificadas | Meta de qualidade e método de medição pendentes |
| `OBJ-05` | Melhorar a comunicação operacional | Avisos e notificações possuem destinatário, motivo, horário e resultado | Meta de entrega e canal alternativo pendentes |
| `OBJ-06` | Apoiar decisões da gestão | Relatórios e mapas reproduzíveis a partir dos mesmos filtros e dados | Indicadores e periodicidade de uso pendentes |

## 5. Escopo

### 5.1 Incluído

- página pública de notícias, consulta de polos/modalidades e criação de conta;
- autenticação, autorização por perfil, recuperação de acesso e gestão do próprio perfil;
- cadastro e gestão de alunos, responsáveis legais, professores e administradores;
- cadastro e gestão de polos, modalidades, turmas, aulas, horários, capacidade e vínculos de professores;
- inscrições, cancelamentos, lista de espera e oferta automática de vagas;
- fluxo de processo seletivo para turmas configuradas dessa forma;
- chamada, conteúdo da aula, observações, frequência e correção administrativa justificada;
- envio e análise de justificativas de falta com documento comprobatório;
- avisos internos e integração com WhatsApp;
- notícias com publicação imediata ou agendada;
- relatórios de participação, frequência e evasão, exportados em Excel e PDF;
- mapa de polos/modalidades e mapa de calor com filtros autorizados;
- registros de auditoria e histórico de notificações indispensáveis aos fluxos anteriores;
- interfaces web responsivas para os perfis definidos e API de suporte ao produto.

### 5.2 Explicitamente excluído desta baseline

- folha de pagamento, contratos, finanças, compras públicas e gestão de orçamento;
- cobrança, pagamento ou mensalidade de aluno;
- prontuário médico, diagnóstico, prescrição ou atendimento clínico;
- gestão de competições, placares, arbitragem e equipes profissionais;
- redes sociais, chat livre entre usuários e videoconferência;
- controle de acesso físico aos polos;
- aplicativos móveis nativos; a necessidade de PWA ou suporte offline está pendente;
- substituição de sistemas oficiais da Prefeitura que não tenham sido formalmente identificados;
- inteligência artificial ou decisões automatizadas de elegibilidade além das regras determinísticas aprovadas;
- migração de dados legados, até que fontes, qualidade e autorização sejam mapeadas.

Qualquer item excluído exige nova avaliação de requisitos, dados, arquitetura, segurança e prazo antes de entrar no escopo.

## 6. Partes interessadas e atores

| Parte/ator | Responsabilidade ou objetivo | Acesso esperado |
| --- | --- | --- |
| Secretaria/Gestão de Esportes | Patrocinar, aprovar regras, indicadores e operação | Visões gerenciais conforme papel atribuído |
| Usuário sem cadastro | Conhecer notícias, polos e modalidades; iniciar cadastro | Somente conteúdo público |
| Aluno | Manter perfil permitido, inscrever-se, acompanhar aulas/frequência e justificar falta | Próprios dados e próprias inscrições |
| Responsável legal | Receber comunicações referentes a aluno menor; demais acessos estão pendentes | Notificações; eventual portal próprio não está definido |
| Professor | Consultar turmas vinculadas, alunos necessários à aula, realizar chamada e enviar avisos | Somente turmas vinculadas e dados mínimos necessários |
| Administrador parcial | Executar funções administrativas explicitamente concedidas | Matriz granular pendente |
| Administrador total | Administrar cadastros, inscrições, correções e configurações autorizadas | Acesso amplo, ainda sujeito a auditoria e segregação |
| Product Owner | Esclarecer, priorizar e aceitar requisitos | Gestão do produto, sem acesso produtivo implícito |
| QA | Verificar critérios com dados sintéticos | Ambientes não produtivos e acessos mínimos |
| Encarregado/Privacidade | Avaliar tratamento de dados e direitos de titulares | Evidências e inventário; acesso a dados pessoais somente se necessário |
| Operação/Suporte | Manter disponibilidade e tratar incidentes | Acesso técnico mínimo e auditado |
| Provedor de WhatsApp | Entregar mensagens aprovadas | Somente dados necessários à mensagem, após avaliação do fornecedor |
| Serviço de mapas/geocodificação | Exibir localização ou converter endereço, se adotado | Dados e fornecedor pendentes; não deve receber dados de aluno sem necessidade |

## 7. Glossário do domínio

| Termo | Definição |
| --- | --- |
| SIDESP | Sistema Integrado de Desenvolvimento Esportivo Público. |
| Polo | Local onde a Secretaria oferece atividades esportivas. |
| Modalidade | Tipo de atividade esportiva, com regras como faixa etária e limite de faltas. |
| Turma | Oferta concreta de uma modalidade, vinculada a polo, horário, capacidade e professor(es). |
| Aula | Ocorrência de uma turma em data e horário determinados. |
| Inscrição | Vínculo do aluno com uma turma/modalidade, com estado e histórico. |
| Lista de espera | Fila ordenada de alunos que aguardam vaga em uma turma. |
| Oferta de vaga | Convite temporário enviado ao primeiro elegível da lista de espera. |
| Chamada | Registro de presença/falta dos alunos e conteúdo ministrado em uma aula. |
| Justificativa de falta | Solicitação do aluno, acompanhada de comprovante, sujeita à decisão administrativa. |
| Processo seletivo | Avaliação administrativa necessária antes da confirmação de vaga em certas turmas. |
| Administrador parcial/total | Níveis administrativos previstos na visão; permissões concretas ainda serão detalhadas. |
| Inativação | Desativação lógica que preserva histórico e impede novo uso quando aplicável. |
| Mapa de calor | Visualização agregada de concentração ou indicador, sem exposição indevida de indivíduos. |
| Evasão | Encerramento ou abandono de participação segundo fórmula ainda pendente de aprovação. |
| Notificação | Comunicação gerada pelo sistema por canal interno, WhatsApp ou canal futuro aprovado. |

## 8. Premissas e dependências

- A Secretaria validará regras, cadastros obrigatórios, indicadores e responsáveis por aprovação.
- O produto alvo é novo; todos os requisitos deste documento estão como `Proposto`.
- O frontend consumirá contratos publicados pelo backend; regras críticas e autorização serão aplicadas no servidor.
- CPF será tratado como identificador pessoal, mas **não** como segredo ou prova suficiente de identidade.
- O sistema usará dados sintéticos em desenvolvimento e testes, salvo processo formal de anonimização e autorização.
- Horários de negócio serão interpretados no fuso `America/Sao_Paulo`; a decisão deve ser ratificada.
- Integração com WhatsApp depende de fornecedor, contrato, modelos de mensagem, consentimento/base aplicável, custo e aprovação pública.
- Mapas dependem da disponibilidade e das condições de um provedor ainda não selecionado.
- Exportação depende da definição dos campos, filtros, limites e permissões de cada relatório.
- Operação em polos depende de estratégia aprovada para conectividade instável.
- Hospedagem e serviços pagos dependem do rito de contratação/aprovação da Secretaria.

## 9. Requisitos funcionais

Todos os requisitos abaixo estão no estado **Proposto**. A prioridade é proposta para revisão pelo Product Owner.

### 9.1 Acesso público, identidade e perfil

| ID | Ator e requisito | Critério de aceite e erros esperados | Origem | Prioridade | Dependências/impactos |
| --- | --- | --- | --- | --- | --- |
| `RF-PUB-001` | Usuário sem cadastro DEVE visualizar notícias publicadas em ordem cronológica definida. | Apenas notícias cujo instante de publicação já chegou são listadas; empate e ordenação exata devem ser definidos; ausência retorna lista vazia sem erro. | `SE004`, `RN021` | Média | Fuso horário; `RF-ADM-006` |
| `RF-PUB-002` | Usuário sem cadastro DEVE abrir o conteúdo completo de uma notícia publicada. | ID válido e publicado exibe título, conteúdo e data; notícia inexistente, futura ou inativa não é exposta. | `SE005` | Média | Privacidade do conteúdo público |
| `RF-PUB-003` | Usuário sem cadastro DEVE consultar mapa/lista de polos e modalidades com filtros. | Filtros válidos retornam somente itens ativos; filtro sem resultado retorna coleção vazia; endereço interno ou dado não público não é exibido. | `SE006` | Média | Provedor de mapas; `RF-ADM-001/002` |
| `RF-IDN-001` | Usuário sem cadastro DEVE solicitar cadastro de aluno com os dados obrigatórios aprovados. | Cadastro válido cria uma única conta; CPF duplicado ou inválido e campos inconsistentes são rejeitados sem indicar dados da conta existente. | `SE001`, `RN016` | Alta | Dados obrigatórios, menor e responsável pendentes; `RNF-PRI-*` |
| `RF-IDN-002` | Usuário cadastrado DEVE autenticar-se com CPF **ou** e-mail e senha. | Credenciais válidas iniciam sessão com o perfil correto; inválidas recebem resposta genérica; tentativas são limitadas e auditadas sem registrar senha. | `SE002`, `RN017` | Crítica | Política de sessão, senha e MFA pendente |
| `RF-IDN-003` | Usuário DEVE recuperar acesso por um fluxo seguro iniciado com CPF ou e-mail. | O sistema sempre responde de modo a não revelar existência da conta; redefinição exige token de uso único, curto e entregue por canal previamente verificado. CPF isolado não redefine senha. | `SE003` | Crítica | Canal verificado, expiração e suporte pendentes |
| `RF-IDN-004` | Aluno autenticado DEVE editar somente campos permitidos do próprio perfil e alterar senha após verificação. | Campos autorizados são atualizados e auditados; CPF, papel e vínculos não mudam por esse fluxo; senha atual incorreta ou validação inválida bloqueia a ação. | `SE013`, `RN017` | Alta | Lista de campos editáveis pendente |

### 9.2 Inscrições, turmas e lista de espera

| ID | Ator e requisito | Critério de aceite e erros esperados | Origem | Prioridade | Dependências/impactos |
| --- | --- | --- | --- | --- | --- |
| `RF-INS-001` | Aluno autenticado DEVE solicitar inscrição em turma elegível. | Com conta ativa, idade compatível, menos de dois vínculos simultâneos, vaga e ausência de processo seletivo pendente, a inscrição é confirmada uma única vez; duplicidade ou inelegibilidade é rejeitada com motivo seguro. | `SE007`, `RN001`, `RN008`, `RN012` | Crítica | Definição de simultaneidade e conflito de horários pendentes |
| `RF-INS-002` | Ao faltar vaga, o sistema DEVE inserir o aluno elegível na lista de espera, por ordem de chegada. | Solicitações concorrentes produzem uma ordem única e auditável; o mesmo aluno não ocupa duas posições na mesma turma; posição é informada conforme política de privacidade. | `SE008`, `RN009` | Crítica | Regra de reentrada e prioridade pendente |
| `RF-INS-003` | Aluno DEVE cancelar a própria inscrição permitida. | Cancelamento idempotente altera o estado, preserva histórico e libera a vaga uma única vez; inscrição inexistente, já encerrada ou não pertencente ao aluno não é alterada. | `SE009` | Alta | `RF-INS-004`, retenção e prazo de cancelamento pendentes |
| `RF-INS-004` | Ao surgir vaga, o sistema DEVE oferecê-la ao primeiro aluno elegível da fila. | Somente uma oferta ativa ocupa a vaga; o aluno é notificado; confirmação no prazo efetiva a inscrição; recusa/expiração passa a vaga ao próximo sem duplicidade. | `RN010`, `RN011`, `SE034` | Crítica | Prazo, canal alternativo e indisponibilidade do WhatsApp pendentes |
| `RF-INS-005` | Aluno autenticado DEVE consultar suas turmas, horários e informações públicas/permitidas do professor. | Somente inscrições do próprio aluno são exibidas; alterações de horário são refletidas; turma inexistente ou não autorizada não vaza dados. | `SE012` | Alta | Campos do professor e fuso pendentes |
| `RF-INS-006` | Administrador autorizado DEVE gerenciar a lista de espera. | Ações permitidas e justificadas preservam ordem/histórico; alteração manual não pode ser silenciosa; permissões insuficientes bloqueiam a ação. | `SE024` | Alta | Limites da intervenção administrativa pendentes |
| `RF-INS-007` | Turma configurada com processo seletivo DEVE encaminhar solicitações a um painel Kanban. | Inscrição não vira vaga confirmada antes da aprovação; cada transição válida registra autor, horário e justificativa quando exigida; estados inválidos são rejeitados. | `SE025`, `RN018` | Alta | Estados, critérios, avaliadores e SLA pendentes |
| `RF-INS-008` | Administrador autorizado PODE criar ou cancelar inscrição em exceção operacional justificada. | A operação exige justificativa, registra regra excepcionada, autor e data, e preserva histórico; a ação sem permissão ou justificativa é bloqueada. | `RN023` | Alta | Escopo da exceção e dupla aprovação pendentes |

### 9.3 Frequência, aulas e justificativas

| ID | Ator e requisito | Critério de aceite e erros esperados | Origem | Prioridade | Dependências/impactos |
| --- | --- | --- | --- | --- | --- |
| `RF-FRQ-001` | Aluno autenticado DEVE consultar presença, faltas e histórico das próprias aulas. | Dados correspondem às chamadas válidas; nenhuma informação de outro aluno é retornada; período sem aulas apresenta estado vazio. | `SE010`, `RN017` | Alta | Definição de aula cancelada e frequência pendente |
| `RF-FRQ-002` | Professor DEVE visualizar somente turmas às quais está vinculado. | Listagem e acesso direto recusam turmas sem vínculo ativo; alterações de vínculo passam a valer segundo regra temporal definida. | `SE014`, `RN013` | Crítica | Data de vigência do vínculo pendente |
| `RF-FRQ-003` | Professor vinculado DEVE registrar presença/falta dos alunos de uma aula. | Cada aluno elegível recebe um estado válido; salvamento é atômico e idempotente; turma não vinculada, aula inválida ou chamada já encerrada bloqueia alteração. | `SE015`, `RN013`, `RN019` | Crítica | Conectividade instável; janela de chamada pendente |
| `RF-FRQ-004` | Professor DEVE registrar conteúdo da aula antes de salvar a chamada e PODE incluir observações permitidas. | Conteúdo vazio impede o salvamento; observações respeitam tamanho e proibição de dados excessivos; chamada e conteúdo ficam vinculados. | `SE016`, `RN014` | Alta | Limites e finalidade das observações pendentes |
| `RF-FRQ-005` | Professor DEVE consultar dados mínimos e frequência de alunos das próprias turmas. | Apenas campos aprovados e alunos vinculados são exibidos; dados de saúde exigem decisão de necessidade e permissão específica; acesso indevido é bloqueado e auditado. | `SE018`, `RN013` | Crítica | Minimização e acesso emergencial pendentes |
| `RF-FRQ-006` | Administrador autorizado DEVE corrigir chamada salva somente mediante justificativa. | Estado anterior e novo, autor, data e justificativa são imutavelmente registrados; professor não consegue corrigir nem excluir chamada salva. | `RN019` | Crítica | Prazo e papéis habilitados pendentes |
| `RF-JUS-001` | Aluno elegível DEVE enviar justificativa para a falta permitida, com comprovante. | Falta pertencente ao aluno, elegível e no prazo aceita um arquivo válido; ausência de arquivo, duplicidade, tipo/tamanho inválido ou falta inelegível é rejeitada. | `SE011`, `RN003`, `RN004` | Crítica | Conflito de regras de faltas; arquivo/retenção pendentes |
| `RF-JUS-002` | Administrador autorizado DEVE analisar e aceitar ou recusar justificativa. | Decisão exige motivo quando recusada, registra autor/data e não pode ser executada por professor; reanálise segue regra ainda a definir. | `SE026`, `RN024` | Alta | Permissão administrativa e recurso pendentes |
| `RF-JUS-003` | O sistema DEVE notificar o aluno sobre a decisão da justificativa. | Uma decisão consolidada gera no máximo uma notificação por canal/evento; falha fica registrada e sujeita à política de nova tentativa. | `RN025` | Alta | Canal e conteúdo aprovados pendentes |

### 9.4 Administração de cadastros

| ID | Ator e requisito | Critério de aceite e erros esperados | Origem | Prioridade | Dependências/impactos |
| --- | --- | --- | --- | --- | --- |
| `RF-ADM-001` | Administrador autorizado DEVE cadastrar, consultar, alterar e inativar polos. | Código/identificador é único; polo referenciado não é excluído fisicamente; novo uso de polo inativo é bloqueado. | `SE019`, `RN015` | Alta | Campos/endereço pendentes |
| `RF-ADM-002` | Administrador autorizado DEVE cadastrar, consultar, alterar e inativar modalidades, incluindo idade e limite de faltas. | Faixas e limites válidos são persistidos; modalidade referenciada não é excluída; mudança de regra não reescreve histórico. | `SE020`, `RN002`, `RN008`, `RN015` | Crítica | Vigência das mudanças pendente |
| `RF-ADM-003` | Administrador autorizado DEVE cadastrar/inativar professores e gerenciar vínculos com turmas. | CPF permanece único; vínculo possui vigência e não apaga histórico; professor inativo não recebe novo vínculo nem autentica. | `SE021`, `RN013`, `RN015`, `RN016` | Alta | Campos obrigatórios e reativação pendentes |
| `RF-ADM-004` | Administrador autorizado DEVE criar e gerenciar turmas, aulas, horários, capacidade, polo, modalidade e processo seletivo. | Turma inválida não é salva; redução de capacidade abaixo das inscrições ativas exige tratamento explícito; exclusão física é proibida. | `SE022`, `RN012`, `RN015`, `RN018` | Crítica | Conflito de horário e recorrência pendentes |
| `RF-ADM-005` | Administrador autorizado DEVE consultar e gerenciar alunos dentro de suas permissões. | Busca e alteração respeitam minimização; ações críticas são auditadas; inativação não elimina histórico; acesso em massa exige permissão. | `SE023`, `RN015` | Alta | Matriz de campos/ações e direitos do titular pendentes |
| `RF-ADM-006` | Administrador autorizado DEVE criar, editar, agendar, publicar e inativar notícias. | Publicação futura aparece automaticamente no instante configurado; notícia inativa/futura não aparece ao público; autoria e mudanças ficam registradas. | `SE030`, `RN021` | Média | Fluxo de aprovação editorial pendente |
| `RF-ADM-007` | Administrador total autorizado DEVE cadastrar novos administradores e atribuir nível total ou parcial. | Somente papel autorizado cria administrador; CPF é único; privilégios são explícitos e auditados; o usuário não pode elevar o próprio acesso. | `SE031`, `RN016`, `RN022` | Crítica | Matriz de permissões e aprovação pendentes |

### 9.5 Comunicação, relatórios e análises

| ID | Ator e requisito | Critério de aceite e erros esperados | Origem | Prioridade | Dependências/impactos |
| --- | --- | --- | --- | --- | --- |
| `RF-COM-001` | Professor vinculado DEVE enviar aviso de cancelamento ou mudança de local aos alunos da turma pelo sistema e WhatsApp. | Destinatários são calculados pelo servidor; envio registra conteúdo/modelo, evento, canal, horário e resultado; turma alheia ou mensagem inválida é rejeitada. | `SE017`, `RN020` | Alta | Fornecedor, consentimento/base, templates e fallback pendentes |
| `RF-COM-002` | O sistema DEVE notificar aluno na segunda falta mensal e também o responsável quando o aluno for menor. | Contagem aprovada dispara uma única notificação por destinatário/evento; maioridade e vínculo do responsável são avaliados na data definida. | `SE032`, `RN006`, `RN007` | Crítica | Conflito com limite por modalidade; canal pendente |
| `RF-COM-003` | O sistema DEVE cancelar inscrição ao ultrapassar o limite aplicável de faltas sem justificativa válida. | Reprocessamento não duplica cancelamento; justificativa pendente e regra mensal recebem tratamento definido; histórico e motivo são preservados e a vaga é liberada. | `SE033`, `RN002`, `RN005` | Crítica | Ordem dos eventos e conflitos de regra pendentes |
| `RF-COM-004` | O sistema DEVE manter histórico das tentativas de notificação via WhatsApp. | Cada tentativa contém evento, destinatário referenciado, template, horário, resultado e identificador do provedor, sem segredo ou conteúdo sensível desnecessário. | `SE035` | Alta | Retenção e acesso ao histórico pendentes |
| `RF-REL-001` | Administrador autorizado DEVE gerar relatórios de participação, frequência e evasão com filtros aprovados. | Mesmos dados/filtros produzem resultados reproduzíveis; fórmula, período e data de geração aparecem; filtros sem resultado retornam relatório vazio. | `SE027` | Alta | Fórmulas e permissões por relatório pendentes |
| `RF-REL-002` | Administrador autorizado DEVE exportar relatórios permitidos em Excel e PDF. | Arquivo corresponde à prévia/filtros, usa dados autorizados, evita fórmula executável em planilha e falha de geração não produz arquivo parcial como sucesso. | `SE028` | Alta | Limites, campos, retenção e marca d'água pendentes |
| `RF-REL-003` | Administrador autorizado DEVE visualizar mapa de calor com filtros de concentração e indicadores aprovados. | Visualização usa dados agregados, informa filtro/período e não permite reidentificar indivíduo em grupos pequenos. | `SE029` e objetivo da visão | Média | Métrica, granularidade, limiar de anonimização e geocodificação pendentes |

## 10. Regras de negócio

As descrições normativas vêm do Documento de Visão. Os critérios abaixo as tornam verificáveis, mas não resolvem os conflitos listados na seção 18.

| ID | Regra | Verificação mínima | Status |
| --- | --- | --- | --- |
| `RN-001` | Cada aluno pode manter no máximo duas modalidades simultaneamente. | A terceira inscrição simultânea é rejeitada, salvo exceção administrativa autorizada e justificada. | Proposto; conceito de simultaneidade pendente |
| `RN-002` | Cada modalidade define seu limite mensal de faltas. | Cadastro exige limite válido e a apuração usa a regra vigente no período. | Proposto; conflita potencialmente com `RN-003/006` |
| `RN-003` | Somente a terceira falta do mês é elegível para justificativa. | Outras faltas não aceitam justificativa pelo fluxo comum. | Proposto; justificativa da regra e conflito pendentes |
| `RN-004` | Toda justificativa exige documento comprobatório. | Solicitação sem arquivo aprovado não é registrada como enviada. | Proposto; tipos, tamanho e retenção pendentes |
| `RN-005` | Exceder limite sem justificativa válida cancela automaticamente a inscrição. | Job/evento cancela uma vez, registra motivo e libera vaga conforme ordem aprovada. | Proposto; tratamento de análise pendente |
| `RN-006` | A segunda falta do mês gera alerta de risco de cancelamento. | Um alerta é criado ao consolidar a segunda falta aplicável. | Proposto; conflito com limites variáveis pendente |
| `RN-007` | Para menor de 18 anos, alertas de faltas também vão ao responsável cadastrado. | Menoridade e responsável válido geram destinatário adicional; ausência de responsável é tratada como pendência operacional. | Proposto |
| `RN-008` | Modalidade possui idade mínima e máxima; aluno fora da faixa não se inscreve. | Idade calculada na data de referência aprovada bloqueia inscrição incompatível. | Proposto; data de referência pendente |
| `RN-009` | Sem vagas, aluno elegível entra na lista de espera por ordem de chegada. | Concorrência resulta em sequência única e persistida. | Proposto |
| `RN-010` | Uma vaga é oferecida primeiro ao aluno da frente da fila daquela turma. | Nenhum aluno posterior recebe oferta enquanto o anterior estiver elegível e dentro do prazo. | Proposto |
| `RN-011` | Oferta deve ser confirmada em prazo definido; expirada, segue ao próximo. | Relógio, confirmação, recusa e expiração ficam registrados. | Pendente: prazo inexistente |
| `RN-012` | Turma possui capacidade máxima; inscrição direta é bloqueada no limite. | Inscrições confirmadas nunca excedem capacidade, exceto override explicitamente permitido por `RN-023`. | Proposto |
| `RN-013` | Professor só consulta e chama turmas vinculadas pelo administrador. | A autorização é validada no backend por turma e vigência do vínculo. | Proposto |
| `RN-014` | Conteúdo da aula é obrigatório ao salvar chamada. | Operação atômica falha quando o conteúdo está vazio/inválido. | Proposto |
| `RN-015` | Polos, modalidades, professores e turmas não são excluídos; são inativados. | Operação comum não remove fisicamente registro nem rompe referências históricas. | Proposto |
| `RN-016` | CPF é único no sistema. | Restrição de unicidade normalizada impede duplicidade inclusive em concorrência. | Proposto |
| `RN-017` | Usuário acessa apenas funcionalidades do perfil autorizado. | Backend nega acesso vertical e horizontal independentemente da interface. | Proposto |
| `RN-018` | Turma pode exigir processo seletivo; solicitação depende de aprovação administrativa via Kanban. | Vaga só é confirmada após transição aprovada. | Proposto; fluxo pendente |
| `RN-019` | Professor não altera/exclui chamada salva; administrador corrige com justificativa. | Tentativa do professor falha; correção autorizada mantém antes/depois e justificativa. | Proposto |
| `RN-020` | Aviso do professor sobre cancelamento/mudança de local é imediato e enviado via WhatsApp a todos os alunos da turma. | Evento cria tentativas para todos os destinatários elegíveis e registra falhas. | Proposto; significado de “imediato” e fallback pendentes |
| `RN-021` | Administrador pode agendar notícia para publicação automática. | Antes do horário a notícia não é pública; no instante aprovado passa a ser exibida. | Proposto |
| `RN-022` | Existem ao menos dois níveis administrativos: total e parcial. | Conta parcial não obtém operações não concedidas mesmo por acesso direto à API. | Proposto; matriz pendente |
| `RN-023` | Administrador pode criar/cancelar inscrição fora dos limites por necessidade operacional justificada. | Exceção exige permissão, justificativa e auditoria da regra ignorada. | Proposto; alcance excessivamente amplo pendente |
| `RN-024` | Apenas administrador aceita/rejeita justificativa; professor não acessa a função. | Backend nega leitura/decisão ao professor e registra decisor autorizado. | Proposto |
| `RN-025` | Aluno é notificado quando justificativa é aceita ou recusada. | Cada decisão final gera notificação rastreável. | Proposto |

## 11. Requisitos não funcionais

O Documento de Visão não apresenta uma lista explícita e mensurável de RNFs. Os itens abaixo são **propostos a partir das restrições, dos dados tratados e do Guia Mestre**. Valores não conhecidos permanecem pendentes em vez de serem inventados.

| ID | Requisito e critério de aceite | Prioridade | Status |
| --- | --- | --- | --- |
| `RNF-SEG-001` | Todo acesso não público DEVE exigir autenticação, e toda operação DEVE aplicar autorização no backend por papel, objeto e vínculo. Testes devem cobrir acesso horizontal e vertical. | Crítica | Proposto |
| `RNF-SEG-002` | Credenciais e dados pessoais em trânsito DEVEM usar HTTPS; produção NÃO DEVE aceitar HTTP para fluxos sensíveis. | Crítica | Proposto |
| `RNF-SEG-003` | Senhas DEVEM ser armazenadas apenas com algoritmo de hash de senha forte e parâmetros atuais; senha, token e segredo NÃO DEVEM aparecer em código, logs ou respostas. | Crítica | Proposto |
| `RNF-SEG-004` | Sessões DEVEM possuir expiração, revogação e proteção contra roubo/replay; o frontend NÃO DEVE tratar `localStorage` ou `sessionStorage` como cofre de credenciais. Critérios exatos dependem da arquitetura. | Crítica | Proposto; parâmetros pendentes |
| `RNF-SEG-005` | Login, recuperação, cadastro e integrações DEVEM ter limites contra abuso. Limites e comportamento de bloqueio serão definidos com base no risco e na capacidade. | Alta | Pendente: valores |
| `RNF-SEG-006` | Uploads DEVEM validar tipo real, extensão, tamanho, nome e conteúdo; devem ser armazenados fora de caminho executável, com varredura de malware quando aplicável. | Crítica | Proposto; formatos/limites pendentes |
| `RNF-SEG-007` | Ações administrativas, autenticação, chamada/correção, justificativa, inscrição excepcional, exportação e mudanças de permissão DEVEM gerar auditoria com ator, ação, alvo, instante e resultado, sem dado sensível excessivo. | Crítica | Proposto; retenção pendente |
| `RNF-SEG-008` | As APIs DEVEM aplicar limitação de requisições por usuário, endereço IP, credencial e endpoint, conforme o risco, para reduzir abuso e indisponibilidade. Ao exceder o limite, DEVEM responder com HTTP 429, sem processar a operação, e registrar o evento para monitoramento. | Crítica | Proposto; limites e intervalos pendentes |
| `RNF-PRI-001` | O sistema DEVE coletar apenas dados necessários, classificar CPF, contato, saúde, menores, frequência e documentos, e registrar finalidade, hipótese legal, acesso, compartilhamento, retenção e descarte antes da produção. | Crítica | Pendente e bloqueador |
| `RNF-PRI-002` | Dados de saúde e comprovantes DEVEM ser restritos a papéis formalmente aprovados; a interface e os relatórios NÃO DEVEM expô-los por padrão. | Crítica | Proposto; papéis pendentes |
| `RNF-PRI-003` | Mapas de calor e relatórios agregados DEVEM aplicar limiar que impeça reidentificação de grupos pequenos. O limiar será aprovado por negócio/privacidade. | Alta | Pendente: limiar |
| `RNF-DSP-001` | As telas de consulta comuns e operações interativas DEVEM cumprir metas de tempo de resposta percentiladas sob carga definida. | Alta | Pendente: p95, carga e volume |
| `RNF-CAP-001` | O produto DEVE suportar os volumes de alunos, turmas, aulas, notificações, arquivos e exportações aprovados sem violar `RNF-DSP-001`. | Alta | Pendente: volumes e crescimento |
| `RNF-DIS-001` | Disponibilidade, janela de manutenção, RPO e RTO DEVEM ser aprovados antes da arquitetura e contratação da hospedagem. Backups só contam como controle após restauração testada. | Alta | Pendente e bloqueador de produção |
| `RNF-RES-001` | Falhas de WhatsApp, mapas ou exportação NÃO DEVEM corromper a operação principal. Retentativas DEVEM ser limitadas e idempotentes, com estado visível para suporte. | Alta | Proposto |
| `RNF-CON-001` | Deve existir comportamento definido para conexão instável durante chamada. Nenhuma confirmação visual pode ocorrer antes da persistência local/remota correspondente. | Crítica | Pendente: offline, rascunho ou nova tentativa |
| `RNF-ACE-001` | Interfaces web DEVERIAM atender WCAG 2.2 nível AA nos fluxos críticos, incluindo teclado, foco, contraste, rótulos e mensagens de erro. | Alta | Proposto para validação |
| `RNF-UX-001` | A interface DEVE ser responsiva e fornecer retorno claro de sucesso, vazio, validação, indisponibilidade e autorização negada. Dispositivos e navegadores suportados serão definidos. | Alta | Proposto; matriz pendente |
| `RNF-OBS-001` | Serviços DEVEM produzir logs estruturados, métricas, health checks e identificador de correlação, sem senha, token, documento ou dado de saúde desnecessário. Alertas e retenção serão definidos no plano operacional. | Alta | Proposto |
| `RNF-MAN-001` | Contratos da API DEVEM ser versionados e documentados em OpenAPI; regras críticas DEVEM possuir testes automatizados e rastreabilidade para `RF`/`RN`. | Alta | Proposto |
| `RNF-COM-001` | Datas e horários DEVEM usar formato inequívoco e preservar instante/fuso; apresentação inicial proposta: português do Brasil e `America/Sao_Paulo`. | Média | Proposto para ratificação |
| `RNF-EXP-001` | Exportações NÃO DEVEM permitir injeção de fórmula em planilhas, expor campos não autorizados nem permanecer acessíveis além do prazo aprovado. | Alta | Proposto; prazo pendente |
| `RNF-CUS-001` | Infraestrutura e fornecedores DEVEM permanecer dentro do orçamento aprovado e do processo de contratação pública. Custos recorrentes e limites de uso devem ser estimados antes da escolha. | Alta | Pendente: teto e rito formal |
| `RNF-POR-001` | Dados e arquivos DEVEM ser exportáveis em formato documentado e fornecedor deve possuir estratégia de saída, evitando dependência irreversível. | Média | Proposto |

## 12. Dados e arquivos

### 12.1 Inventário inicial

| Categoria/entidade | Exemplos | Titular | Classificação inicial | Acesso mínimo proposto | Decisões pendentes |
| --- | --- | --- | --- | --- | --- |
| Identidade e contato | nome, CPF, e-mail, telefone, data de nascimento | Aluno, responsável, professor, administrador | Pessoal/confidencial | Próprio titular e administração autorizada | Campos, validação, finalidade, base e retenção |
| Credenciais | hash de senha, sessão, tokens de recuperação | Usuários | Restrita | Serviço de identidade; suporte não vê senha | Política de senha, MFA, sessão e expiração |
| Menor e responsável | vínculo, contatos, comprovação | Aluno menor e responsável | Pessoal/restrita | Administração e serviços de notificação necessários | Comprovação, atualização, consentimento/base e acesso do responsável |
| Saúde/emergência | informações citadas no Documento de Visão | Aluno | Pessoal sensível/restrita | Ainda não aprovado | Campos estritamente necessários, finalidade, base, acesso emergencial e retenção |
| Polo/modalidade/turma | endereço público, horários, capacidade, regras | Secretaria | Pública ou interna por campo | Público para oferta; administração para gestão | Campos internos versus públicos |
| Inscrição/lista/processo | estados, posição, decisões e justificativas | Aluno | Pessoal/confidencial | Próprio aluno, professor no necessário, administração autorizada | Retenção e visibilidade da posição |
| Aula/frequência | presença, falta, conteúdo, observações, correções | Aluno/professor | Pessoal/confidencial | Aluno próprio, professor vinculado e administração | Período de correção e retenção |
| Comprovante de falta | documento anexado e decisão | Aluno | Potencialmente sensível/restrita | Aluno próprio e administrador aprovador | Tipos, tamanho, malware, base, retenção e descarte |
| Notificação | canal, destinatário, template, resultado | Usuários | Pessoal/confidencial | Sistema, suporte mínimo e usuário quando aplicável | Conteúdo, consentimento/base e retenção |
| Notícia | título, conteúdo, autoria e agenda | Secretaria/autor | Pública após publicação; interna antes | Público após publicação; administração antes | Aprovação editorial e retenção |
| Auditoria | ator, alvo, ação, instante, resultado | Usuários/Secretaria | Restrita | Segurança, suporte autorizado e auditoria | Retenção, integridade e revisão |
| Relatório/exportação | combinações dos dados acima | Conforme conteúdo | Herda maior classificação dos campos | Papel explicitamente autorizado | Campos, limiar, prazo e descarte |

### 12.2 Requisitos para arquivos

- Tipos MIME, extensões, tamanho máximo, quantidade e prazo de envio de comprovantes são **pendentes**.
- O nome fornecido pelo usuário não será usado como caminho físico e não poderá causar sobrescrita.
- Arquivos serão validados pelo conteúdo, armazenados fora da área pública e entregues somente após autorização por objeto.
- Comprovantes não serão copiados para ambientes de desenvolvimento/teste.
- Exportações usarão dados sintéticos em testes e neutralizarão células iniciadas por caracteres de fórmula.
- Retenção, descarte, backup e eventual criptografia em repouso dependem da avaliação de privacidade e arquitetura.

## 13. Interfaces e integrações

| Interface | Finalidade | Dados principais | Comportamento em falha | Situação |
| --- | --- | --- | --- | --- |
| Frontend web ↔ API SIDESP | Todos os fluxos autenticados e públicos | Dados de tela e comandos autorizados | Erro seguro, identificador de correlação e nenhuma confirmação falsa | Proposto; contrato OpenAPI futuro |
| Provedor de WhatsApp | Alertas, avisos, decisões e oferta de vaga | Telefone, template e parâmetros mínimos | Registrar falha, aplicar retentativa limitada e canal alternativo aprovado | Fornecedor e regras pendentes |
| E-mail/serviço de identidade | Recuperação de acesso e possível notificação | Endereço e token opaco de curta duração | Resposta antienumeração; suporte conforme fluxo aprovado | Provedor pendente |
| Mapas/geocodificação | Exibir polos e apoiar visualizações geográficas | Endereço/coordenadas de polo; agregados autorizados | Exibir lista ou estado degradado sem bloquear outros módulos | Provedor pendente |
| Gerador Excel/PDF | Exportar relatórios | Resultado autorizado e metadados de filtro | Falha atômica, sem arquivo parcial como sucesso | Tecnologia pendente |

Nenhuma integração pode receber segredo no frontend, dado além do necessário ou informação pessoal antes da avaliação contratual e de privacidade.

## 14. Cenários de erro e exceção

| ID | Cenário | Resposta esperada |
| --- | --- | --- |
| `ERR-001` | Credenciais inválidas ou conta inexistente | Mensagem genérica, limitação de tentativas e nenhum indício de existência da conta |
| `ERR-002` | Usuário tenta acessar recurso de outro aluno/turma | Negar no backend, não retornar conteúdo e registrar evento quando relevante |
| `ERR-003` | Duas inscrições disputam a última vaga | Uma única inscrição confirmada; demais entram na fila na ordem transacional correta |
| `ERR-004` | Solicitação repetida por duplo clique/rede | Resultado idempotente; não duplicar inscrição, chamada, decisão ou notificação |
| `ERR-005` | CPF duplicado | Rejeitar de modo seguro; suporte trata possível conta existente sem expor dados |
| `ERR-006` | Arquivo inválido, excessivo ou malicioso | Rejeitar antes do processamento/armazenamento definitivo e informar restrição sem detalhe perigoso |
| `ERR-007` | WhatsApp indisponível | Operação principal preservada quando possível; tentativa marcada como falha e fallback aplicado conforme política |
| `ERR-008` | Conexão cai durante chamada | Não perder silenciosamente dados nem mostrar sucesso não confirmado; estratégia final pendente |
| `ERR-009` | Administrador reduz capacidade abaixo das inscrições | Bloquear ou exigir plano explícito; nunca cancelar alunos silenciosamente |
| `ERR-010` | Justificativa ainda está em análise no momento do limite | Não cancelar até aplicar ordem de eventos aprovada; regra é bloqueadora e está pendente |
| `ERR-011` | Oferta de vaga expira enquanto aluno confirma | Processamento transacional produz um único vencedor e resposta clara ao aluno |
| `ERR-012` | Exportação excede limite | Rejeitar ou processar assincronamente segundo limite aprovado; não degradar todo o serviço |
| `ERR-013` | Provedor de mapas falha | Exibir lista/estado degradado e manter demais funções disponíveis |
| `ERR-014` | Professor tenta alterar chamada salva | Negar; orientar fluxo administrativo de correção sem apagar histórico |
| `ERR-015` | Conta/polo/modalidade/turma inativa é referenciada | Preservar consultas históricas e impedir novos vínculos quando a regra exigir |

## 15. Critérios de aceite do produto

A baseline de requisitos será aceita quando:

- todos os requisitos críticos tiverem ator, entrada, saída, erro, permissão e aceite definidos ou estiverem marcados como pendência bloqueadora;
- o responsável de negócio resolver os conflitos de faltas, lista de espera, exceção administrativa e processo seletivo;
- a matriz de papéis e permissões for aprovada e testável;
- os dados pessoais/sensíveis tiverem finalidade, hipótese legal, acesso, retenção e descarte aprovados pelos responsáveis competentes;
- os requisitos não funcionais críticos receberem valores mensuráveis ou justificativa formal;
- cada requisito funcional estiver ligado a pelo menos um caso de uso e a um cenário de teste;
- diagramas e protótipos não contradisserem o comportamento aprovado;
- Product Owner, responsável de negócio, Tech Lead, QA, segurança e privacidade registrarem aprovação nas suas competências.

Para uma funcionalidade individual ser aceita, o caminho principal, estados vazios, validações, falta de autorização, concorrência/repetição relevante, auditoria e efeitos de privacidade devem ser demonstrados com dados sintéticos.

## 16. Riscos e lacunas

| ID | Risco/lacuna | Impacto | Tratamento inicial | Responsável |
| --- | --- | --- | --- | --- |
| `RIS-001` | Regras de faltas usam limite variável e também segunda/terceira faltas fixas | Cancelamento/notificação incorretos | Resolver `Q-001` antes de modelar frequência | Negócio/PO |
| `RIS-002` | Dados de saúde, menores e comprovantes sem governança definida | Dano ao titular e não conformidade | Inventário, necessidade, base, acesso, retenção e avaliação de impacto | Privacidade/Negócio |
| `RIS-003` | Permissão administrativa parcial não detalhada | Escalada de privilégio ou bloqueio operacional | Criar matriz de permissões e segregação | Negócio/Segurança |
| `RIS-004` | `RN-023` permite ignorar restrições de forma ampla | Inconsistência e abuso administrativo | Limitar regras excepcionáveis e exigir auditoria/aprovação | Negócio/Segurança |
| `RIS-005` | Dependência de WhatsApp sem fornecedor/fallback | Mensagens não entregues e custo desconhecido | Avaliar fornecedor, contrato, templates, SLA e canal alternativo | Negócio/Técnico |
| `RIS-006` | Internet insuficiente nos polos | Chamada perdida ou duplicada | Decidir estratégia offline/rascunho/idempotência | Técnico/Negócio |
| `RIS-007` | RNFs sem metas de carga, desempenho e disponibilidade | Arquitetura e aceite subjetivos | Levantar volumes e SLOs antes da arquitetura final | Técnico/Negócio |
| `RIS-008` | Métricas de evasão, satisfação e mapas não definidas | Relatórios divergentes ou discriminatórios | Aprovar dicionário de indicadores e limiar de agregação | Negócio/Privacidade |
| `RIS-009` | Recuperação “por CPF ou e-mail” pode ser interpretada como validação fraca | Tomada de conta | Adotar token em canal verificado e resposta antienumeração | Segurança/Técnico |
| `RIS-010` | Protótipo usa CSV e não representa requisitos de produção | Decisões técnicas inadequadas por herança acidental | Tratar protótipo apenas como referência conceitual | Tech Lead |
| `RIS-011` | Orçamento público e contratação ainda incertos | Interrupção ou troca de fornecedor | Estimar custos e alternativas reversíveis antes do compromisso | Secretaria/PO |
| `RIS-012` | Migração de registros atuais não mapeada | Perda/duplicidade na entrada em operação | Inventariar fontes e qualidade; decidir corte e reconciliação | Negócio/Dados |

## 17. Decisões já registradas

| ID | Decisão | Fonte | Estado |
| --- | --- | --- | --- |
| `DEC-001` | A documentação de requisitos representa o produto completo, não apenas o backend. | Orientação Tech Lead | Aceita para este documento |
| `DEC-002` | O backend alvo será desenvolvido em Java/Spring Boot. | Orientação do Tech Lead | Diretriz técnica; detalhamento pertence à Arquitetura/ADR |
| `DEC-003` | O README será elaborado posteriormente, quando houver código relevante. | Orientação do Tech Lead | Aceita para a sequência de trabalho |
| `DEC-004` | O protótipo Java básico não define o escopo nem a arquitetura do produto. | Orientação do Tech Lead | Aceita |
| `DEC-005` | O frontend alvo será desenvolvido em Angular/TypeScript. | Orientação do Tech Lead | Diretriz técnica; detalhamento pertence à Arquitetura/ADR |
| `DEC-006` | O nome oficial do produto é **Sistema Integrado de Desenvolvimento Esportivo Público**, mantendo a sigla SIDESP. | Decisão da equipe em 13/08/2026 | Aceita; deve ser usada em toda a documentação |

## 18. Questões pendentes

### 18.1 Bloqueadoras para aprovar a baseline

| ID | Questão | Por que bloqueia | Decisor sugerido |
| --- | --- | --- | --- |
| `Q-001` | Como `RN-002` (limite por modalidade), `RN-003` (só 3ª falta justificável) e `RN-006` (alerta na 2ª) funcionam quando o limite não é três? | Altera cancelamento, justificativa e notificação | Secretaria + PO |
| `Q-002` | “No máximo 2 modalidades” significa duas modalidades distintas, duas turmas ou duas inscrições ativas? O que define simultaneidade e há bloqueio por choque de horário? | Altera elegibilidade e modelo de dados | Secretaria + PO |
| `Q-003` | Qual é o prazo de confirmação da vaga, quando começa, em quais horários corre e qual canal alternativo existe? | `RN-011` está incompleta | Secretaria + PO |
| `Q-004` | Qual data calcula a idade: pedido, início da turma ou cada aula? O que ocorre se o aluno ultrapassa a faixa durante a turma? | Altera elegibilidade e permanência | Secretaria + PO |
| `Q-005` | Como faltas mensais são contadas, inclusive chamada corrigida, aula cancelada e justificativa pendente? | Evita cancelamento incorreto | Secretaria + PO |
| `Q-006` | Quais restrições o administrador pode excepcionar em `RN-023`? Ação exige administrador total ou segunda aprovação? | Regra atual permite contornar controles críticos | Secretaria + Segurança |
| `Q-007` | Quais dados pessoais e de saúde são realmente necessários, quem os acessa, com qual finalidade, hipótese legal e retenção? | Tratamento sensível e de menores | Controlador/Encarregado |
| `Q-008` | Como o responsável legal é comprovado, atualizado e autenticado? Ele terá portal próprio ou somente receberá alertas? | Acesso e comunicação de menores | Secretaria + Privacidade |
| `Q-009` | Qual fornecedor/canal de WhatsApp será usado, quem paga, quais templates, base/consentimento, opt-out e fallback? | Integração obrigatória sem contrato | Secretaria + Privacidade + Técnico |
| `Q-010` | Qual política de autenticação: criação de senha, verificação de e-mail/telefone, MFA administrativo, sessão, bloqueio e recuperação? | Controle de acesso crítico | Segurança + Negócio + Técnico |
| `Q-011` | Quais permissões compõem administrador total e parcial? Quem concede, revisa e revoga? | `RN-022` não é testável | Secretaria + Segurança |
| `Q-012` | Quais estados, transições, critérios, documentos, avaliadores e prazos formam o processo seletivo Kanban? | `RN-018` não é testável | Secretaria + PO |
| `Q-013` | Como são calculados participação, frequência, evasão e satisfação; quais filtros e campos cada papel pode ver/exportar? | Relatórios podem divergir e expor dados | Secretaria + Privacidade |
| `Q-014` | Qual granularidade e limiar mínimo serão usados no mapa de calor e quais dados geográficos existem? | Risco de reidentificação e escopo indefinido | Secretaria + Privacidade |
| `Q-015` | Quais são retenção, descarte, direitos dos titulares, backup e migração para cada categoria de dado? | Banco e segurança não podem ser fechados | Controlador/Encarregado + Técnico |
| `Q-016` | Quais volumes, p95, disponibilidade, RPO, RTO, navegadores/dispositivos e limites de arquivo/exportação são aceitáveis? | RNFs não mensuráveis | Negócio + Técnico |
| `Q-017` | O WhatsApp é obrigatório ou opcional? A abrangência diz “poderá integrar”, enquanto regras e requisitos o tornam obrigatório. | Contradição de escopo | Secretaria + PO |
| `Q-018` | O produto precisa funcionar offline/parcialmente offline durante chamadas? | Internet insuficiente foi registrada como problema real | Secretaria + Técnico |

### 18.2 Importantes, não bloqueadoras do primeiro refinamento

| ID | Questão |
| --- | --- |
| `Q-019` | Notícias exigem revisão/aprovação antes da publicação? Quem pode publicar? |
| `Q-020` | Professor pode ter múltiplas turmas/professores por turma e substituição temporária? |
| `Q-021` | Aluno pode sair e voltar à lista; mantém ou perde posição? Como tratar duplicidade entre turmas da mesma modalidade? |
| `Q-022` | Haverá QR Code para presença? O termo está no glossário da visão, mas não há requisito funcional correspondente. |
| `Q-023` | Quais dados atuais serão migrados, de quais planilhas/sistemas e qual será a data de corte? |
| `Q-024` | Quais notificações também aparecem no próprio sistema e por quanto tempo ficam visíveis? |

## 19. Rastreabilidade

Os IDs abaixo correspondem ao catálogo oficial definido em `CASOS_DE_USO.md`. Qualquer inclusão, remoção ou renumeração deve ser atualizada simultaneamente nos dois documentos.

| Módulo | Requisitos | Regras | Casos de uso oficiais | Testes esperados | Código alvo |
| --- | --- | --- | --- | --- | --- |
| Público/notícias/mapas | `RF-PUB-*`, `RF-ADM-006` | `RN-021` | `UC-PUB-01` Consultar notícias; `UC-PUB-02` Consultar detalhe de notícia; `UC-PUB-03` Consultar polos e modalidades; `UC-ADM-11` Gerenciar notícias; `UC-AUT-05` Publicar notícia agendada | `CT-PUB-*`, `CT-ADM-NOT-*` | Pendente |
| Identidade/perfil | `RF-IDN-*`, `RF-ADM-007` | `RN-016/017/022` | `UC-IDN-01` Cadastrar aluno; `UC-IDN-02` Autenticar usuário; `UC-IDN-03` Recuperar acesso; `UC-IDN-04` Atualizar perfil; `UC-IDN-05` Alterar senha; `UC-ADM-12` Gerenciar administradores | `CT-IDN-*`, `CT-AUT-*` | Pendente |
| Inscrição/fila/seleção | `RF-INS-*` | `RN-001`, `RN-008` a `RN-012`, `RN-018`, `RN-023` | `UC-INS-01` Consultar ofertas esportivas; `UC-INS-02` Solicitar inscrição; `UC-INS-03` Entrar na lista de espera; `UC-INS-04` Cancelar inscrição; `UC-INS-05` Consultar posição na espera; `UC-INS-06` Consultar turmas e aulas; `UC-INS-07` Confirmar oferta de vaga; `UC-ADM-07` Gerenciar inscrições e lista de espera; `UC-ADM-08` Gerenciar processo seletivo; `UC-ADM-13` Executar exceção de inscrição; `UC-AUT-01` Ofertar vaga da lista de espera | `CT-INS-*`, incluindo concorrência/idempotência | Pendente |
| Chamada/frequência | `RF-FRQ-*` | `RN-002`, `RN-005/006`, `RN-013/014/019` | `UC-FRQ-01` Consultar frequência; `UC-PRF-01` Consultar turmas vinculadas; `UC-PRF-02` Realizar chamada; `UC-PRF-03` Registrar diário da aula; `UC-PRF-04` Consultar alunos da turma; `UC-ADM-10` Corrigir chamada | `CT-FRQ-*`, autorização por turma | Pendente |
| Justificativa | `RF-JUS-*` | `RN-003/004/024/025` | `UC-JUS-01` Enviar justificativa; `UC-JUS-02` Anexar comprovante; `UC-ADM-09` Analisar justificativa; `UC-AUT-04` Notificar decisão de justificativa | `CT-JUS-*`, upload/privacidade | Pendente |
| Cadastros operacionais | `RF-ADM-001` a `RF-ADM-005` | `RN-008`, `RN-012/013/015/016/018` | `UC-ADM-01` Gerenciar polos; `UC-ADM-02` Gerenciar modalidades; `UC-ADM-03` Gerenciar professores; `UC-ADM-04` Gerenciar vínculos de professor; `UC-ADM-05` Gerenciar turmas; `UC-ADM-06` Gerenciar alunos | `CT-ADM-*`, inativação/histórico | Pendente |
| Comunicação | `RF-COM-*` | `RN-005` a `RN-007`, `RN-010/011/020/025` | `UC-COM-01` Enviar aviso à turma; `UC-COM-02` Enviar mensagem via WhatsApp; `UC-COM-03` Consultar histórico de notificações; `UC-AUT-02` Alertar limite de faltas; `UC-AUT-03` Cancelar por excesso de faltas | `CT-COM-*`, falha/replay/fallback | Pendente |
| Relatórios/análises | `RF-REL-*` | Regras de dados aplicáveis | `UC-REL-01` Gerar relatório; `UC-REL-02` Exportar relatório; `UC-REL-03` Visualizar mapa de calor | `CT-REL-*`, autorização/agregação/formula injection | Pendente |

Quando a implementação começar, cada PR deverá citar os IDs afetados e os testes deverão usar os padrões acima ou registrar mapeamento equivalente. O estado de requisito só muda de `Proposto` para `Parcial` ou `Atual` após evidência em código e teste aceito.

## 20. Checklist para aprovação deste documento

- [ ] Responsável de negócio nominal e contatos foram preenchidos.
- [ ] Prioridades foram ratificadas pelo Product Owner.
- [ ] Questões bloqueadoras `Q-001` a `Q-018` foram decididas ou formalmente aceitas como risco.
- [ ] Regras e requisitos não apresentam contradições.
- [ ] Matriz de permissões foi anexada ou referenciada.
- [ ] Inventário de dados, finalidade, base legal, retenção e descarte foram aprovados.
- [ ] Critérios mensuráveis de desempenho, capacidade, disponibilidade e recuperação foram definidos.
- [ ] Casos de uso receberam IDs e foram alinhados a esta rastreabilidade.
- [ ] QA validou se os critérios são testáveis.
- [ ] Segurança e privacidade registraram revisão.
- [ ] Responsável de negócio e Product Owner aprovaram a baseline.

## 21. Histórico de versões

| Versão | Data | Autor | Alterações | Situação |
| --- | --- | --- | --- | --- |
| `0.1.0` | 12/08/2026 | Equipe SIDESP, com consolidação documental assistida por IA | Primeira consolidação do Documento de Visão no padrão do Guia Mestre; inclusão de critérios de aceite, dados, erros, riscos, pendências e rastreabilidade | Rascunho |

