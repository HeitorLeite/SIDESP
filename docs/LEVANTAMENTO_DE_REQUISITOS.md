# Levantamento de Requisitos — SIDESP

> **SIDESP — Sistema Integrado de Desenvolvimento Esportivo Público**  
> Documento de requisitos do produto completo, aplicável ao frontend, ao backend e às integrações.

## Identificação do documento

| Campo | Valor |
| --- | --- |
| Projeto | SIDESP — Sistema Integrado de Desenvolvimento Esportivo Público |
| Órgão demandante | Secretaria de Esportes de Guaratinguetá |
| Responsável de negócio / Scrum Master | Kauãn Raphael |
| Product Owner | Livia Andrade |
| Responsável técnico | Heitor Leite (Tech Lead) |
| Segurança | Heitor Leite |
| Responsável interno por privacidade | Heitor Leite |
| QA | Micael Phillipini |
| Equipe | Diego Henrico, Heitor Leite, Kauãn Raphael, Livia Andrade, Luís Mario e Micael Phillipini |
| Versão | `0.2.0` |
| Data da revisão | 17/08/2026 |
| Classificação | Interna |
| Status | Pronto para revisão — não aprovado |
| Próxima revisão | Após revisão formal da equipe ou quando escopo/regra mudar |

## Aprovações

| Papel | Responsável | Situação | Data |
| --- | --- | --- | --- |
| Responsável de negócio / Scrum Master | Kauãn Raphael | Pendente de revisão | — |
| Product Owner | Livia Andrade | Pendente de revisão | — |
| Responsável técnico | Heitor Leite | Pendente de revisão | — |
| Segurança | Heitor Leite | Pendente de revisão | — |
| Privacidade do projeto | Heitor Leite | Pendente de revisão | — |
| QA | Micael Phillipini | Pendente de revisão dos critérios de aceite | — |

## 1. Finalidade e fontes

Este documento registra o que deve ser construído, para quem, sob quais regras e como o comportamento será aceito. Ele descreve o **produto completo**.

### 1.1 Fontes e hierarquia

1. **Documento de Visão — SIDESP, versão 1.0:** fonte primária do objetivo, problemas, atores, regras `RN001` a `RN025` e requisitos `SE001` a `SE035`.
2. Entrevistas com representantes da Secretaria e questionário aplicado a professores, citados no Documento de Visão: origem de negócio a ser preservada e, quando possível, anexada ao projeto.
3. Guia de Documentação e Governança Antes do Desenvolvimento: fonte do formato e dos controles mínimos deste documento.
4. Protótipos de telas no [Figma do SIDESP](https://www.figma.com/design/41evldxEaPMcPxX15Z5CLd/Untitled?node-id=0-1&m=dev&t=7mEnpP0Kx9jTrD0K-1): referência visual inicial, sujeita às regras e decisões aprovadas neste documento.


Em caso de conflito, a decisão aprovada pelo responsável de negócio e registrada no histórico deste documento prevalece. Regras não aprovadas permanecem como `Pendente`.

### 1.2 Convenções

- **DEVE / OBRIGATÓRIO:** requisito indispensável.
- **NÃO DEVE / PROIBIDO:** comportamento vedado.
- **DEVERIA / RECOMENDADO:** comportamento esperado, cuja ausência exige justificativa.
- **PODE:** alternativa permitida após avaliação.
- **Proposto:** planejado, ainda não implementado no produto alvo.
- **Pendente:** exige informação ou decisão externa.
- Prioridades deste documento são propostas e devem ser ratificadas pelo Product Owner durante a revisão.
- Os IDs `RF-*`, `RNF-*` e `RN-*` são estáveis. Os códigos `SE###` permanecem registrados como origem para não perder a rastreabilidade com o Documento de Visão.

### 1.3 Como ler este documento

O texto usa palavras obrigatórias para deixar claro o que o sistema precisa fazer. Os termos mais técnicos são explicados no glossário da seção 7. Quando uma decisão ainda não foi tomada, ela aparece como **Pendente**; isso significa que a equipe não deve tratá-la como regra definitiva.

Este é um projeto desenvolvido pelo grupo acadêmico. Kauãn Raphael representa o negócio e o contato com o problema apresentado pelo cliente. Heitor Leite responde internamente por técnica, segurança e privacidade. Isso não atribui a nenhum integrante o cargo legal de encarregado de dados da Prefeitura. Se o SIDESP vier a operar com dados reais para um órgão público, as responsabilidades legais e institucionais deverão ser validadas pelo próprio órgão antes da implantação.

## 2. Resumo executivo

A Secretaria de Esportes de Guaratinguetá utiliza processos majoritariamente manuais e descentralizados para inscrições, controle de vagas, chamadas e relatórios. O cenário provoca registros duplicados, retrabalho, inconsistências, dificuldade de localizar informações de alunos e pouca rastreabilidade das turmas e listas de espera.

O SIDESP centralizará cadastros, inscrições, turmas, chamadas, justificativas, notícias, notificações e informações gerenciais. Alunos acompanharão suas inscrições e frequência; professores trabalharão apenas com as turmas às quais estão vinculados; administradores gerenciarão a operação e analisarão dados. A comunicação por WhatsApp, os relatórios e as exportações específicas estão planejados para versões posteriores, após as definições externas necessárias.

O produto tratará CPF, dados de contato, dados de menores, possíveis dados de saúde, frequência e documentos comprobatórios. A baseline não poderá ser aprovada antes da validação de privacidade, segurança e papéis de acesso. A política inicial de retenção está na seção 12.3; antes do uso real, ela dependerá de validação institucional. O fornecedor de WhatsApp pode ser escolhido depois, pois a notificação interna será o canal obrigatório.

## 3. Problema atual e impactos

| Problema | Pessoas afetadas | Impacto observado | Resultado esperado |
| --- | --- | --- | --- |
| Informações de alunos dispersas e de difícil acesso | Professores e administradores | Atendimento lento e dificuldade de gestão; risco em situações emergenciais | Cadastro centralizado, acesso rápido e autorização por perfil |
| Controle manual e repetitivo | Professores e administradores | Retrabalho, inconsistência, cansaço operacional e maior probabilidade de erro | Registro único de chamada alimentando históricos, indicadores e relatórios |
| Falta de controle de vagas e inscrições | Alunos, professores e administradores | Turmas desorganizadas e lista de espera pouco confiável | Limites automáticos, fila ordenada e oferta rastreável de vagas |
| Falta de vínculo estruturado entre aluno, turma e professor | Todos os perfis internos | Acompanhamento e responsabilização prejudicados | Vínculos explícitos e consultas coerentes por perfil |
| Comunicação descentralizada | Alunos, responsáveis, professores e administradores | Avisos tardios ou sem comprovação de envio | Notificações acionadas por evento com histórico de entrega |
| Dados gerenciais não consolidados | Gestores | Decisões tomadas sem visão integrada | Relatórios, exportações e visualizações com critérios conhecidos |
| Internet insuficiente em alguns polos | Professores | Possível falha ou atraso no registro durante a aula | Rascunho parcialmente offline, indicação de não sincronizado e envio após o retorno da conexão |

## 4. Objetivos de negócio e indicadores

| ID | Objetivo | Evidência de alcance | Meta |
| --- | --- | --- | --- |
| `OBJ-01` | Centralizar a gestão das atividades esportivas | Cadastros, vínculos, inscrições e chamadas consultáveis em uma única solução | Primeira versão começa sem migração; cobertura será medida a partir dos novos registros |
| `OBJ-02` | Reduzir registros repetidos de presença | Uma chamada salva alimenta o histórico do aluno e os relatórios autorizados | Presença registrada uma única vez; percentual de melhoria será medido após projeto piloto |
| `OBJ-03` | Tornar vagas e filas rastreáveis | Capacidade, inscrições, posição, ofertas, confirmações e expirações possuem histórico | 100% das turmas do escopo com capacidade controlada |
| `OBJ-04` | Aumentar confiabilidade das informações | Restrições, unicidade, validações e trilha de alterações críticas verificadas | Erros e correções serão medidos no piloto antes de definir percentual de melhoria |
| `OBJ-05` | Melhorar a comunicação operacional | Avisos e notificações possuem destinatário, motivo, horário e resultado | Entrega interna é requisito inicial; melhoria percebida será medida no piloto |
| `OBJ-06` | Apoiar decisões da gestão | Relatórios e mapas reproduzíveis a partir dos mesmos filtros e dados | Funcionalidade fica para versão posterior; indicadores serão definidos pela Secretaria |

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
- avisos internos obrigatórios; integração com WhatsApp planejada para depois da escolha do fornecedor;
- notícias com publicação imediata ou agendada;
- relatórios e exportações planejados para uma versão posterior, depois que a Secretaria definir campos, filtros, fórmulas e permissões;
- lista pública de polos e modalidades; mapas interativos e mapa de calor ficam para versão posterior;
- registros de auditoria e histórico de notificações indispensáveis aos fluxos anteriores;
- interfaces web responsivas em Angular para os perfis definidos;
- rascunho parcialmente offline da chamada, com sincronização após o retorno da internet;
- API própria em Java/Spring Boot, responsável pelas regras do sistema e pela comunicação com o banco MySQL;
- banco de dados MySQL 8.x, preferencialmente em versão LTS homologada antes do desenvolvimento.

### 5.2 Explicitamente excluído desta baseline

- folha de pagamento, contratos, finanças, compras públicas e gestão de orçamento;
- cobrança, pagamento ou mensalidade de aluno;
- prontuário médico, diagnóstico, prescrição ou atendimento clínico;
- gestão de competições, placares, arbitragem e equipes profissionais;
- redes sociais, chat livre entre usuários e videoconferência;
- controle de acesso físico aos polos;
- aplicativos móveis nativos, PWA instalável e funcionamento totalmente offline; somente o rascunho de chamada terá suporte parcial sem conexão;
- integração real com WhatsApp na primeira versão; ela entrará em versão posterior após a escolha e aprovação do fornecedor;
- presença por QR Code está excluída da primeira versão; a ideia poderá ser reavaliada como melhoria futura;
- mapa interativo, geocodificação e mapa de calor na primeira versão; entrarão depois da escolha do fornecedor e dos indicadores;
- substituição de sistemas oficiais da Prefeitura que não tenham sido formalmente identificados;
- inteligência artificial ou decisões automatizadas de elegibilidade além das regras determinísticas aprovadas;
- migração de dados legados na primeira versão; o banco começará vazio e uma eventual migração será uma atividade separada.

Qualquer item excluído exige nova avaliação de requisitos, dados, arquitetura, segurança e prazo antes de entrar no escopo.

## 6. Partes interessadas e atores

| Parte/ator | Responsabilidade ou objetivo | Acesso esperado |
| --- | --- | --- |
| Secretaria/Gestão de Esportes | Patrocinar, aprovar regras, indicadores e operação | Visões gerenciais conforme papel atribuído |
| Usuário sem cadastro | Conhecer notícias, polos e modalidades; iniciar cadastro | Somente conteúdo público |
| Aluno | Manter perfil permitido, inscrever-se, acompanhar aulas/frequência e justificar falta | Próprios dados e próprias inscrições |
| Responsável legal | Responder pelo cadastro do aluno menor e receber suas comunicações | Não possui conta própria nem portal; seus dados ficam vinculados à conta do aluno até a maioridade |
| Professor | Consultar turmas vinculadas, alunos necessários à aula, realizar chamada, enviar avisos e avaliar seleção quando designado | Somente turmas vinculadas; processo seletivo exige designação explícita como avaliador |
| Administrador parcial | Executar somente as funções administrativas concedidas por um administrador total | Matriz inicial da seção 9.4.1 |
| Administrador total | Administrar cadastros, inscrições, correções e configurações autorizadas | Acesso amplo, ainda sujeito a auditoria e segregação |
| Product Owner | Esclarecer, priorizar e aceitar requisitos | Gestão do produto, sem acesso produtivo implícito |
| QA | Verificar critérios com dados sintéticos | Ambientes não produtivos e acessos mínimos |
| Responsável interno por privacidade | Avaliar minimização, acesso, retenção e proteção dos dados no projeto; sinalizar o que exigirá validação institucional antes de uso real | Evidências e inventário; acesso a dados pessoais somente se necessário |
| Operação/Suporte | Manter disponibilidade e tratar incidentes | Acesso técnico mínimo e auditado |
| Prefeitura de Guaratinguetá e Embrass | Fornecer, por meio da parceria informada pela equipe, hospedagem, banco de dados e infraestrutura para implantação | Acesso técnico conforme responsabilidades e autorizações que serão formalizadas antes da implantação |
| Provedor de WhatsApp | Entregar mensagens aprovadas | Somente dados necessários à mensagem, após avaliação do fornecedor |
| Serviço de mapas/geocodificação futuro | Exibir localização ou converter endereço em versão posterior | Não integra a primeira versão; não deve receber dados de aluno sem necessidade |

## 7. Glossário do domínio e de termos técnicos

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
| Justificativa de falta | Solicitação do aluno com descrição obrigatória e comprovante opcional, sujeita à decisão administrativa. |
| Processo seletivo | Avaliação humana feita por professor designado ou administrador autorizado antes da confirmação de vaga em certas turmas. |
| Administrador parcial/total | O administrador total possui todas as permissões e concede acessos específicos ao administrador parcial conforme a matriz da seção 9.4.1. |
| Inativação | Desativação lógica que preserva histórico e impede novo uso quando aplicável. |
| Mapa de calor | Visualização agregada de concentração ou indicador, sem exposição indevida de indivíduos. |
| Evasão | Encerramento ou abandono de participação segundo fórmula ainda pendente de aprovação. |
| Notificação | Comunicação gerada pelo sistema por canal interno, WhatsApp ou canal futuro aprovado. |
| Frontend | Parte visual usada no navegador. No SIDESP, será desenvolvida em Angular. |
| Backend | Parte executada no servidor, responsável pelas regras, segurança e acesso aos dados. No SIDESP, será desenvolvida em Java/Spring Boot. |
| Angular | Tecnologia usada para construir as telas e a navegação do frontend. |
| TypeScript | Linguagem usada com Angular. É baseada em JavaScript e acrescenta verificação de tipos para reduzir erros. |
| API | Conjunto de endereços e regras que permite ao frontend solicitar dados ou operações ao backend. |
| Endpoint | Endereço de uma operação da API, como autenticar um usuário ou solicitar uma inscrição. |
| Spring Boot | Tecnologia Java usada para criar a API e os serviços do backend. |
| MySQL | Sistema de banco de dados relacional escolhido para armazenar os dados estruturados do SIDESP. |
| LTS | Versão de uma tecnologia mantida e corrigida por um período mais longo. |
| OpenAPI | Arquivo que documenta as operações, entradas, respostas e erros da API. |
| Transação | Conjunto de alterações no banco que deve ser concluído por inteiro; se uma parte falhar, nenhuma alteração é confirmada. |
| Idempotência | Proteção que impede uma solicitação repetida de produzir efeitos duplicados, como duas inscrições por causa de um duplo clique. |
| Rate limiting | Limite de requisições em um intervalo para evitar abuso ou sobrecarga da API. |
| HTTP 429 | Resposta da API informando que o limite de requisições foi excedido e que o cliente deve aguardar antes de tentar novamente. |
| Fallback | Alternativa usada quando o meio principal falha, como uma notificação interna quando o WhatsApp estiver indisponível. |
| p95 | Medida de desempenho: 95% das respostas devem ocorrer dentro do tempo definido. |
| RPO | Quantidade máxima de dados que a equipe aceita perder após uma falha, medida em tempo. |
| RTO | Tempo máximo esperado para restaurar o serviço após uma falha. |
| Rastreabilidade | Ligação entre requisito, regra, caso de uso, tarefa, teste e implementação. |
| Ficha de saúde | Conjunto restrito de informações usadas para segurança, emergência e adaptação da atividade física. Não é um prontuário médico. |
| MFA | Segunda verificação além da senha. No SIDESP, o administrador informa um código enviado ao e-mail em todos os logins. |
| Código de uso único | Código temporário que só pode ser usado uma vez para confirmar uma ação ou identidade. |
| Hash de senha | Resultado de uma proteção matemática que permite verificar a senha sem armazená-la de forma legível. |
| Rascunho offline | Registro temporário mantido no navegador enquanto não há internet. Ele ainda não é considerado salvo no servidor. |
| Sincronização | Envio ao servidor de um rascunho criado ou alterado enquanto a conexão estava indisponível. |
| Anonimização | Alteração que impede relacionar um dado a uma pessoa identificada. |
| Quarentena de arquivo | Área isolada onde um arquivo permanece indisponível até ser verificado contra conteúdo malicioso. |
| Infraestrutura | Servidores, rede, armazenamento, banco de dados e demais recursos necessários para executar o SIDESP. |

## 8. Premissas e dependências

- A Secretaria validará regras, cadastros obrigatórios, indicadores e responsáveis por aprovação.
- O produto alvo é novo; todos os requisitos deste documento estão como `Proposto`.
- O frontend em Angular consumirá a API própria em Java/Spring Boot. As regras críticas, a autorização e o acesso ao MySQL serão aplicados no backend; o frontend não acessará o banco diretamente.
- O banco será MySQL 8.x, preferencialmente em uma versão LTS homologada antes do desenvolvimento. A ferramenta de controle das alterações do banco ainda será escolhida.
- Os protótipos no Figma são referência visual inicial. Se houver conflito, prevalecem as regras e os fluxos aprovados na documentação.
- CPF será tratado como identificador pessoal, mas **não** como segredo ou prova suficiente de identidade.
- O sistema usará dados sintéticos em desenvolvimento e testes, salvo processo formal de anonimização e autorização.
- Horários de negócio serão interpretados no fuso `America/Sao_Paulo`; a decisão deve ser ratificada.
- Integração com WhatsApp depende de fornecedor, contrato, modelos de mensagem, consentimento/base aplicável, custo e aprovação pública.
- Mapas interativos e mapa de calor ficam fora da primeira versão e dependem de fornecedor e indicadores futuros.
- Exportação depende da definição dos campos, filtros, limites e permissões de cada relatório.
- Operação em polos depende de estratégia aprovada para conectividade instável.
- A Prefeitura, em parceria com a Embrass, fornecerá hospedagem, banco de dados e infraestrutura; capacidade, ambientes, acesso técnico e responsabilidades serão confirmados antes da implantação.

## 9. Requisitos funcionais

Todos os requisitos abaixo estão no estado **Proposto**. A prioridade é proposta para revisão pelo Product Owner.

### 9.1 Acesso público, identidade e perfil

| ID | Ator e requisito | Critério de aceite e erros esperados | Origem | Prioridade | Dependências/impactos |
| --- | --- | --- | --- | --- | --- |
| `RF-PUB-001` | Usuário sem cadastro DEVE visualizar notícias publicadas da mais recente para a mais antiga. | Apenas notícias cujo instante de publicação já chegou são listadas. Em empate do horário de publicação, aparece primeiro a criada mais recentemente. Ausência retorna lista vazia sem erro. Datas usam `America/Sao_Paulo`. | `SE004`, `RN021` | Média | Definido em 14/08/2026; `RF-ADM-006` |
| `RF-PUB-002` | Usuário sem cadastro DEVE abrir o conteúdo completo de uma notícia publicada. | ID válido e publicado exibe título, conteúdo e data; notícia inexistente, futura ou inativa não é exposta. | `SE005` | Média | Privacidade do conteúdo público |
| `RF-PUB-003` | Usuário sem cadastro DEVE consultar uma lista de polos e modalidades com filtros. | Filtros válidos retornam somente itens ativos; filtro sem resultado retorna coleção vazia; endereço interno ou dado não público não é exibido. Mapa interativo fica fora da primeira versão. | `SE006` | Média | `RF-ADM-001/002`; mapas serão refinados em versão posterior |
| `RF-IDN-001` | Usuário sem cadastro DEVE solicitar cadastro de aluno com nome completo, CPF, nascimento, e-mail, telefone, bairro, cidade, contato de emergência e senha. | A conta começa como `PENDENTE_CONFIRMACAO`, torna-se `ATIVO` após as confirmações obrigatórias e pode ser `INATIVO`; falhas de login não criam estado bloqueado. Dados de saúde são opcionais, exceto contato de emergência. Menor também exige nome, CPF, vínculo, e-mail e WhatsApp do responsável, que não possui conta e pode estar vinculado a vários alunos. | `SE001`, `RN016` | Alta | WhatsApp para confirmação fica em versão futura; `RN-026`; `RNF-PRI-*` |
| `RF-IDN-002` | Usuário cadastrado DEVE autenticar-se com CPF **ou** e-mail e senha. | A senha possui no mínimo 8 caracteres, pelo menos uma letra maiúscula e pelo menos um número. Letras minúsculas e símbolos são permitidos, mas não obrigatórios. A 3ª falha consecutiva exige espera de 30 segundos; as falhas seguintes exigem 1, 2, 5 e depois 15 minutos, que é o limite máximo. Um login válido encerra a contagem. Administrador também informa um código enviado ao e-mail confirmado, válido por 10 minutos e por no máximo 5 tentativas. A resposta de erro não revela se a conta existe. | `SE002`, `RN017` | Crítica | Definido em 14/08/2026 |
| `RF-IDN-003` | Usuário DEVE recuperar acesso por link enviado exclusivamente ao e-mail confirmado. | O sistema sempre responde de modo a não revelar a existência da conta. O link é válido por 30 minutos, só pode ser usado uma vez e é invalidado após o uso; CPF isolado não redefine senha. A recuperação concluída revoga todas as sessões existentes. Se o usuário perder acesso ao e-mail, somente administrador total poderá alterá-lo após conferir CPF e documento oficial com foto. A troca também revoga todas as sessões e exige recuperação da senha pelo novo e-mail confirmado. | `SE003` | Crítica | Definido em 14/08/2026; procedimento manual deve evitar guardar cópia desnecessária do documento |
| `RF-IDN-004` | Aluno autenticado DEVE consultar e corrigir telefone, bairro, cidade, contato de emergência e dados de saúde do próprio perfil, alterar a senha e solicitar inativação ou exclusão da conta. | Uma pessoa é única pelo CPF e pode ter vários perfis, com no máximo uma conta. Mudança de e-mail exige nova confirmação. Quando contato confirmado do responsável compartilhado mudar, o novo valor passa a todos os vínculos dessa pessoa e os alunos vinculados são avisados. Nome, CPF e nascimento só são corrigidos por administrador autorizado com documento. | `SE013`, `RN017` | Alta | Definido em 14/08/2026 |

### 9.2 Inscrições, turmas e lista de espera

| ID | Ator e requisito | Critério de aceite e erros esperados | Origem | Prioridade | Dependências/impactos |
| --- | --- | --- | --- | --- | --- |
| `RF-INS-001` | Aluno autenticado DEVE solicitar inscrição em turma elegível. | O pedido não fica persistido como inscrição `SOLICITADA`: termina diretamente em inscrição `CONFIRMADA`, entrada na lista de espera, candidatura seletiva ou rejeição sem participação. Com conta ativa, idade compatível, no máximo uma outra modalidade ativa ou em seleção e sem duplicidade de modalidade, o resultado é criado uma única vez. Conflito de horário gera aviso e confirmação, mas não bloqueia. | `SE007`, `RN001`, `RN008`, `RN012` | Crítica | Definido em 14/08/2026 |
| `RF-INS-002` | Ao faltar vaga, o sistema DEVE inserir o aluno elegível na lista de espera, por ordem de chegada. | Solicitações concorrentes produzem uma ordem única e auditável. O aluno ocupa no máximo uma lista de espera por modalidade e vê a própria posição numérica exata, sem nomes nem dados das outras pessoas. Após sair, vê o estado encerrado, a última posição ocupada e o instante da saída, sem novo cálculo de posição ativa. Ao retornar, entra no final; para trocar de turma da mesma modalidade, sai da fila antiga e entra no final da nova. | `SE008`, `RN009` | Crítica | Definido em 14/08/2026 |
| `RF-INS-003` | Aluno DEVE cancelar a própria participação a qualquer momento. | O aluno pode cancelar uma inscrição confirmada, sua entrada na lista de espera ou sua candidatura ao processo seletivo. O cancelamento altera o estado e preserva o histórico; uma inscrição confirmada libera a vaga uma única vez. Para voltar, o aluno faz nova solicitação e, quando houver fila, entra no final. Participação inexistente, já encerrada ou pertencente a outra pessoa não é alterada. | `SE009` | Alta | Definido em 14/08/2026; retenção na seção 12.3 |
| `RF-INS-004` | Ao surgir vaga, o sistema DEVE oferecê-la ao primeiro aluno elegível da fila. | Somente uma oferta ativa ocupa a vaga. A oferta gera notificação interna e inicia 48 horas de disponibilidade, incluindo fins de semana e feriados. Indisponibilidade oficial do SIDESP pausa o prazo; problema particular de internet não pausa. O aluno confirma ou recusa somente em `Minhas ofertas`; apagar a notificação não cancela a oferta. Confirmação efetiva a inscrição; recusa ou expiração passa a vaga ao próximo sem duplicidade. | `RN010`, `RN011`, `SE034` | Crítica | Canal interno na primeira versão; WhatsApp depende de fornecedor futuro |
| `RF-INS-005` | Aluno autenticado DEVE consultar suas turmas, horários e informações públicas do professor. | Do professor são exibidos somente nome, modalidade, fotografia opcional e breve apresentação opcional. CPF, telefone, e-mail e endereço nunca são exibidos. Somente inscrições do próprio aluno aparecem; alterações de horário são refletidas. | `SE012` | Alta | Fuso inicial `America/Sao_Paulo` conforme `RNF-COM-001` |
| `RF-INS-006` | Administrador autorizado DEVE gerenciar a lista de espera sem alterar manualmente sua ordem. | O administrador pode consultar e executar ações operacionais permitidas, mas não pode mover um aluno para frente ou para trás nem restaurar posição anterior. Toda saída e reentrada coloca o aluno no final da fila e preserva o histórico. | `SE024` | Alta | Definido em 14/08/2026 |
| `RF-INS-007` | Turma configurada com processo seletivo DEVE encaminhar solicitações a um painel Kanban. | Professor com vínculo vigente e designação de avaliador ou administrador com permissão marca critérios `ATENDEU`/`NÃO_ATENDEU` e toma a decisão humana final. Aprovar com obrigatório não atendido exige justificativa. Sem capacidade, permanece `EM_ANÁLISE` até surgir vaga ou haver exceção do administrador total. Primeira decisão concorrente confirmada prevalece; correção final é exclusiva do administrador total. | `SE025`, `RN018` | Alta | Definido em 17/08/2026 |
| `RF-INS-008` | Somente administrador total PODE criar ou cancelar inscrição em exceção operacional justificada. | A exceção pode tratar idade, limite de modalidades ou capacidade da turma. Exige justificativa e registra regra excepcionada, autor, data e resultado. Não pode alterar a ordem da lista de espera nem dispensar ou manipular o processo seletivo. | `RN023` | Alta | Auditoria obrigatória; não exige segunda aprovação segundo a decisão atual |

### 9.3 Frequência, aulas e justificativas

| ID | Ator e requisito | Critério de aceite e erros esperados | Origem | Prioridade | Dependências/impactos |
| --- | --- | --- | --- | --- | --- |
| `RF-FRQ-001` | Aluno autenticado DEVE consultar presença, faltas e histórico das próprias aulas. | Dados correspondem às chamadas válidas; aulas canceladas não geram falta; correções e justificativas aceitas atualizam a contagem. Nenhuma informação de outro aluno é retornada; período sem aulas apresenta estado vazio. | `SE010`, `RN017` | Alta | Retenção inicial definida na seção 12.3 |
| `RF-FRQ-002` | Professor DEVE visualizar somente turmas às quais está vinculado. | O acesso começa na data inicial e termina na data final do vínculo. Professor substituto perde o acesso ao fim da substituição. Listagem e acesso direto recusam turma sem vínculo vigente. | `SE014`, `RN013` | Crítica | Definido em 14/08/2026 |
| `RF-FRQ-003` | Professor vinculado DEVE registrar presença/falta dos alunos de uma aula até 24 horas após seu encerramento. | Cada aluno elegível recebe somente `PRESENTE` ou `AUSENTE`. Justificativa aceita permanece vinculada à ausência, mas a retira da contagem. Aula cancelada não possui chamada. O salvamento no servidor é atômico e idempotente. Sem internet, o navegador mantém rascunho marcado como “não sincronizado” e tenta enviá-lo quando a conexão voltar. Depois de salva ou encerrado o prazo, o professor não pode alterá-la. | `SE015`, `RN013`, `RN019` | Crítica | Definido em 14/08/2026 |
| `RF-FRQ-004` | Professor DEVE registrar conteúdo da aula antes de salvar a chamada e PODE incluir observação operacional. | O conteúdo é obrigatório e aceita até 1.000 caracteres; a observação aceita até 500. Diagnósticos, detalhes de saúde ou informações pessoais desnecessárias são proibidos nesse campo. Chamada e conteúdo ficam vinculados. | `SE016`, `RN014` | Alta | Definido em 14/08/2026 |
| `RF-FRQ-005` | Professor DEVE consultar dados mínimos, frequência e ficha de saúde dos alunos das próprias turmas. | Cada aluno possui no máximo uma ficha atual. Alterações geram versão anterior protegida, acessível somente a administrador autorizado e Heitor Leite; o log comum registra autor, instante e campos alterados, sem copiar valores médicos. Em emergência, professor consulta dados mínimos e não altera a ficha. Todo acesso é restrito e auditado. | `SE018`, `RN013` | Crítica | Retenção na seção 12.3; procedimento presencial pertence à operação do polo |
| `RF-FRQ-006` | Administrador total ou parcial com permissão de correção DEVE poder corrigir uma chamada salva por até 30 dias após a aula, somente mediante justificativa. | Estado anterior e novo, autor, data e justificativa são registrados sem possibilidade de alteração; professor não consegue corrigir nem excluir chamada salva. | `RN019` | Crítica | Definido em 14/08/2026 |
| `RF-JUS-001` | Aluno DEVE poder enviar justificativa para uma ou várias faltas próprias em até 7 dias corridos de cada aula. | A justificativa começa `EM_ANALISE`. Antes da primeira decisão, o aluno pode cancelá-la após aviso de que as faltas voltarão a contar e poderão cancelar a inscrição. Uma solicitação pode reunir ausências do mesmo aluno e motivo, inclusive de modalidades diferentes. Descrição é obrigatória e são opcionais de 0 a 3 arquivos seguros. | `SE011`, `RN003`, `RN004` | Crítica | Definido em 14/08/2026; retenção na seção 12.3 |
| `RF-JUS-002` | Administrador autorizado DEVE analisar e aceitar ou recusar justificativa em até 5 dias corridos. | Estados são `EM_ANALISE`, `ACEITA`, `RECUSADA`, `EM_RECURSO`, `ACEITA_EM_RECURSO`, `RECUSADA_FINAL` e `CANCELADA`. Recusa exige motivo. O aluno pode recorrer uma vez em até 3 dias; outro administrador decide o recurso. Cada justificativa guarda no máximo duas decisões. | `SE026`, `RN024` | Alta | Definido em 14/08/2026 |
| `RF-JUS-003` | O sistema DEVE notificar o aluno sobre a decisão inicial e a decisão final do recurso. | A notificação interna informa o resultado e, na recusa inicial, o prazo de 3 dias corridos para recurso. Cada decisão gera no máximo uma notificação por evento. | `RN025` | Alta | Canal interno e conteúdo inicial definidos; regras de visibilidade em `RF-COM-005` |

### 9.4 Administração de cadastros

| ID | Ator e requisito | Critério de aceite e erros esperados | Origem | Prioridade | Dependências/impactos |
| --- | --- | --- | --- | --- | --- |
| `RF-ADM-001` | Administrador autorizado DEVE cadastrar, consultar, alterar e inativar polos. | O polo possui nome, endereço, bairro, cidade, CEP, telefone público, horário de funcionamento e informação de acessibilidade. Código/identificador é único; polo referenciado não é excluído fisicamente; novo uso de polo inativo é bloqueado. | `SE019`, `RN015` | Alta | Coordenadas dependem da solução futura de mapas |
| `RF-ADM-002` | Administrador autorizado DEVE cadastrar, consultar, alterar e inativar modalidades, incluindo idade e limite mensal de faltas. | Mudança da faixa etária vale apenas para novas solicitações de inscrição. Mudança do limite de faltas começa no mês seguinte e não reescreve o histórico. Modalidade referenciada não é excluída. | `SE020`, `RN002`, `RN008`, `RN015` | Crítica | Definido em 14/08/2026 |
| `RF-ADM-003` | Administrador autorizado DEVE cadastrar, inativar e reativar professores e gerenciar vínculos com turmas. | O sistema localiza ou cria a pessoa única pelo CPF e adiciona o perfil de professor sem duplicar outros perfis. Nome, CPF, nascimento, e-mail e telefone são obrigatórios; foto, apresentação e formação são opcionais. Reativação preserva conta e histórico. Um professor pode atuar em várias turmas e uma turma pode ter vários professores. | `SE021`, `RN013`, `RN015`, `RN016` | Alta | Definido em 14/08/2026 |
| `RF-ADM-004` | Administrador autorizado DEVE criar e gerenciar turmas, aulas, horários, capacidade, polo, modalidade e processo seletivo. | Suspensão preserva inscrições e fila, bloqueia novas inscrições e não gera aulas ou faltas; não amplia automaticamente o encerramento. Ao reativar, administrador decide período e ocorrências e notifica os alunos. Reagendamento preserva a mesma aula e registra valores anterior/novo, motivo, autor e instante; fica `REAGENDADA` até passar a `REALIZADA`. Local pode ser polo cadastrado ou temporário sem alterar o polo permanente. | `SE022`, `RN012`, `RN015`, `RN018`, `RN020` | Crítica | Definido em 14/08/2026 |
| `RF-ADM-005` | Administrador autorizado DEVE consultar e gerenciar alunos dentro de suas permissões. | Busca e alteração respeitam minimização; ações críticas são auditadas; inativação não elimina histórico; acesso em massa exige permissão. Somente administrador total executa pedidos de inativação ou exclusão e fornece resposta inicial em até 15 dias corridos. O aluno pode retirar o pedido enquanto nenhuma ação tiver sido executada. Heitor Leite analisa casos com dados sensíveis; dados sujeitos à retenção permanecem protegidos, com motivo e data prevista de descarte informados. O prazo definitivo será validado institucionalmente antes da implantação real. | `SE023`, `RN015` | Alta | Definido para o projeto acadêmico; implantação real exige validação institucional |
| `RF-ADM-006` | Administrador autorizado DEVE criar, editar, agendar, publicar e inativar notícias. | Administrador total ou parcial com permissão de notícias pode publicar diretamente, sem segunda aprovação na primeira versão. Publicação futura aparece automaticamente no instante configurado; notícia inativa ou futura não aparece ao público; autoria e todas as mudanças ficam registradas. | `SE030`, `RN021` | Média | Definido em 14/08/2026 |
| `RF-ADM-007` | Administrador total autorizado DEVE cadastrar administradores e atribuir nível total ou parcial. | O sistema reutiliza a pessoa única pelo CPF e adiciona perfil, conta ou permissões necessárias. Nome, CPF, e-mail confirmado, telefone, nível e permissões são obrigatórios; MFA por e-mail é exigido. O último administrador total não pode ser inativado e ninguém remove o próprio acesso total. Revisão ocorre a cada 90 dias; inativação revoga sessões sem apagar outros perfis. | `SE031`, `RN016`, `RN022` | Crítica | Definido em 14/08/2026 |

#### 9.4.1 Matriz inicial de permissões administrativas

| Área | Administrador total | Administrador parcial |
| --- | --- | --- |
| Polos e modalidades | Acesso completo | Somente com permissão concedida |
| Professores e vínculos | Acesso completo | Somente com permissão concedida |
| Turmas | Acesso completo | Somente com permissão concedida |
| Alunos | Acesso completo | Somente com permissão concedida |
| Inscrições e lista de espera | Acesso completo, sem alterar a ordem da fila | Somente com permissão concedida, sem alterar a ordem da fila |
| Processo seletivo | Acesso completo, respeitando estados e critérios | Somente com permissão concedida |
| Justificativas | Acesso completo | Somente com permissão concedida |
| Correção de chamada | Acesso completo com justificativa | Somente com permissão concedida e justificativa |
| Notícias | Acesso completo | Somente com permissão concedida |
| Relatórios e exportações | Acesso completo conforme proteção dos dados | Somente com permissão específica para visualizar e/ou exportar |
| Ficha de saúde | Acesso completo e auditado | Somente com permissão específica e auditada |
| Notificações | Acesso completo | Somente com permissão concedida |
| Criar administradores e alterar permissões | Permitido | Proibido |
| Executar exceção de inscrição | Permitido com justificativa | Proibido |

### 9.5 Comunicação, relatórios e análises

| ID | Ator e requisito | Critério de aceite e erros esperados | Origem | Prioridade | Dependências/impactos |
| --- | --- | --- | --- | --- | --- |
| `RF-COM-001` | Professor vinculado DEVE cancelar uma ocorrência específica de aula ou mudar o local dessa ocorrência. | O sistema atualiza a ocorrência, mantém a programação permanente da turma e cria aviso interno para cada aluno. Quando o aluno for menor, também envia e-mail ao responsável confirmado. Antes do início da aula, o professor pode desfazer a alteração; depois, somente administrador autorizado corrige, com motivo e auditoria. Toda alteração ou reversão gera nova notificação. Alterações permanentes de horário, polo ou período ficam restritas ao administrador. | `SE017`, `RN020` | Alta | SIDESP e e-mail na primeira versão; WhatsApp depende de fornecedor futuro |
| `RF-COM-002` | O sistema DEVE avisar o aluno quando faltar uma ausência não justificada para atingir o limite mensal da modalidade e também o responsável quando o aluno for menor. | Quando a contagem chegar a `limite - 1`, o sistema gera uma única notificação interna para o aluno e, no caso de menor, envia e-mail ao responsável confirmado. Em uma versão posterior, quando o WhatsApp estiver ativo, o aviso também será enviado ao número obrigatório do responsável. A regra funciona para qualquer limite configurado na modalidade. | `SE032`, `RN006`, `RN007` | Crítica | SIDESP e e-mail na primeira versão; WhatsApp depende de fornecedor futuro |
| `RF-COM-003` | O sistema DEVE cancelar a inscrição quando as faltas não justificadas ultrapassarem o limite mensal da modalidade. | A contagem reinicia a cada mês; aulas canceladas não contam; correções e justificativas aceitas recalculam o total. Se a falta que causaria o cancelamento possuir justificativa ou recurso em análise, o cancelamento fica suspenso até a decisão final. Ao cancelar, o sistema cria imediatamente a notificação interna. Reprocessamento não duplica o cancelamento. | `SE033`, `RN002`, `RN005` | Crítica | Definido em 14/08/2026 |
| `RF-COM-004` | Em uma versão posterior, o sistema DEVE manter histórico das tentativas de notificação via WhatsApp. | Cada tentativa contém evento, destinatário referenciado, modelo da mensagem, horário, resultado e identificador do provedor, sem segredo ou conteúdo sensível desnecessário. | `SE035` | Alta | Fora da primeira versão; fornecedor, modelos e acesso ao histórico serão definidos depois |
| `RF-COM-005` | O sistema DEVE manter uma central interna de notificações para alunos e demais perfis aplicáveis. | A central mostra estado “lida/não lida” para inscrição e cancelamento, lista e oferta de vaga, processo seletivo, faltas, justificativas, alterações de aula e avisos importantes de conta/segurança. A notificação fica disponível por até 1 ano, mas o usuário pode apagá-la antes. Apagar uma notificação de oferta não cancela a oferta, que permanece em `Minhas ofertas` até resposta ou expiração. Somente o registro técnico mínimo de envio pode permanecer pelo prazo de auditoria. | Decisão da equipe em 14/08/2026 | Alta | Definido em 14/08/2026 |
| `RF-COM-006` | O sistema DEVE enviar os e-mails transacionais previstos e acompanhar falhas. | A primeira tentativa é imediata; em falha temporária, novas tentativas ocorrem após 5 e 30 minutos, totalizando no máximo 3. Se todas falharem ou o endereço for inválido, cria pendência administrativa. Falha de e-mail ao responsável não desfaz a operação principal nem a notificação interna do aluno. Reprocessamento não gera envios ilimitados. | `RN007` | Alta | Definido em 14/08/2026 |
| `RF-REL-001` | Em uma versão posterior, administrador autorizado DEVE gerar os relatórios que forem definidos pela Secretaria. | Cada relatório aprovado deverá informar campos, filtros, fórmula, período e permissões. Os mesmos dados e filtros deverão produzir o mesmo resultado; filtros sem resultado retornarão relatório vazio. | `SE027` | Alta | Fora da primeira versão; não implementar antes da definição da Secretaria |
| `RF-REL-002` | Em uma versão posterior, administrador autorizado DEVE exportar em Excel e PDF os relatórios que a Secretaria aprovar. | O arquivo corresponde à prévia e aos filtros aprovados, usa somente dados autorizados, evita fórmula executável em planilha e não apresenta arquivo incompleto como sucesso. | `SE028` | Alta | Fora da primeira versão; campos, filtros e limites dependerão da Secretaria; arquivos gerados durarão no máximo 24 horas |
| `RF-REL-003` | Em uma versão posterior, administrador autorizado DEVE visualizar mapa de calor com filtros e indicadores aprovados. | A visualização reunirá dados por polo ou bairro, nunca mostrará endereços individuais e informará filtro/período. Resultados com menos de três pessoas serão ocultados ou reunidos a uma área maior. | `SE029` e objetivo da visão | Média | Fora da primeira versão; depende de fornecedor e indicadores da Secretaria |

## 10. Regras de negócio

As descrições normativas vêm do Documento de Visão. Os critérios abaixo as tornam verificáveis, mas não resolvem os conflitos listados na seção 18.

| ID | Regra | Verificação mínima | Status |
| --- | --- | --- | --- |
| `RN-001` | Cada aluno pode ocupar no máximo duas modalidades diferentes ao mesmo tempo. | Contam as inscrições ativas e as candidaturas ainda abertas em processo seletivo. Duas turmas da mesma modalidade não são permitidas. A lista de espera não ocupa uma vaga no limite, mas uma nova inscrição direta pode exigir a saída de outra fila, com aviso e confirmação do aluno. Conflito de horário gera alerta claro, sem bloqueio automático. | Definido em 14/08/2026 |
| `RN-002` | Cada modalidade define seu próprio limite mensal de faltas não justificadas. | O administrador autorizado informa o limite ao criar a modalidade. Alteração começa no primeiro dia do mês seguinte. A contagem reinicia mensalmente; aulas canceladas não contam; correções e justificativas aceitas recalculam o total. | Definido em 14/08/2026 |
| `RN-003` | Toda falta pode receber justificativa em até 7 dias corridos após a aula. | Uma justificativa pode reunir várias ausências do mesmo aluno e mesmo motivo/período, inclusive em modalidades diferentes, desde que cada falta esteja no próprio prazo e em no máximo uma justificativa ativa. Aceite retira as faltas da contagem; recusa as mantém. O aluno pode recorrer uma vez em até 3 dias. | Definido em 14/08/2026 |
| `RN-004` | Toda justificativa exige descrição e pode possuir de 0 a 3 arquivos comprobatórios. | Arquivo é opcional. Quando anexado, deve ser PDF, JPG ou PNG, com até 10 MB, e aguarda em quarentena a verificação contra conteúdo malicioso antes de poder ser aberto. | Definido em 14/08/2026; retenção na seção 12.3 |
| `RN-005` | Ultrapassar o limite mensal de faltas não justificadas cancela automaticamente a inscrição. | Se o limite for três, o cancelamento ocorre ao chegar à quarta falta não justificada. Se a falta que ultrapassou o limite tiver justificativa em análise, o cancelamento fica suspenso até a decisão. Se a justificativa for aceita, a falta é desconsiderada; se for recusada, o cancelamento prossegue. A operação ocorre uma única vez, registra o motivo e libera a vaga conforme a ordem aprovada. | Definido em 14/08/2026 |
| `RN-006` | O sistema alerta quando faltar uma ausência não justificada para o aluno atingir o limite mensal da modalidade. | Ao alcançar `limite - 1`, um único alerta é criado para o aluno e, quando ele for menor, também para o responsável válido. | Definido em 14/08/2026 |
| `RN-007` | Para menor de 18 anos, comunicações importantes também vão ao responsável cadastrado. | Na primeira versão, inscrição/cancelamento/oferta, resultado do processo seletivo, alerta/cancelamento por faltas, decisão de justificativa e cancelamento ou mudança de local de aula são enviados ao e-mail confirmado do responsável. O aluno recebe a notificação dentro do SIDESP. O e-mail segue as três tentativas de `RF-COM-006`; sua falha não desfaz a operação principal. WhatsApp será um canal adicional futuro. | Definido em 14/08/2026 |
| `RN-008` | Modalidade possui idade mínima e máxima; aluno fora da faixa não se inscreve. | A idade é calculada no momento da solicitação. Mudança posterior da faixa etária vale somente para novas solicitações. Quem já está inscrito permanece até o encerramento da turma, inclusive se ultrapassar a idade máxima. | Definido em 14/08/2026 |
| `RN-009` | Sem vagas, aluno elegível entra na lista de espera por ordem de chegada. | Concorrência resulta em sequência única e persistida. O aluno vê a própria posição numérica exata, sem nomes nem dados de terceiros, e ocupa no máximo uma fila por modalidade. Após sair, vê estado encerrado, última posição e instante da saída, sem recálculo ativo. Se voltar, entra no final; para mudar de turma da mesma modalidade, sai da fila anterior e entra no final da nova. Nenhum administrador pode restaurar ou alterar sua posição. | Definido em 14/08/2026 |
| `RN-010` | Uma vaga é oferecida primeiro ao aluno da frente da fila daquela turma. | Nenhum aluno posterior recebe oferta enquanto o anterior estiver elegível e dentro do prazo. Confirmação, recusa e expiração são idempotentes e auditadas. | Definido em 17/08/2026 |
| `RN-011` | Oferta deve receber resposta durante 48 horas de disponibilidade; expirada, segue ao próximo. | O prazo começa com a oferta e a notificação interna, inclui fins de semana e feriados e é pausado apenas durante indisponibilidade oficial registrada do SIDESP. Falha particular de internet, e-mail ou futuro WhatsApp não suspende o prazo. | Definido em 17/08/2026 |
| `RN-012` | Turma possui capacidade máxima de inscrições confirmadas. | Solicitações podem ultrapassar a capacidade e o excedente elegível entra na fila. O sistema bloqueia redução abaixo do total já confirmado e não remove alunos automaticamente. Exceção justificada segue `RN-023`. | Definido em 14/08/2026 |
| `RN-013` | Professor só consulta e chama turmas vinculadas pelo administrador. | Um professor pode ter várias turmas e uma turma pode ter vários professores. O acesso começa e termina nas datas do vínculo; substituto perde o acesso ao final. O backend verifica turma e vigência em cada acesso. | Definido em 14/08/2026 |
| `RN-014` | Conteúdo da aula é obrigatório ao salvar chamada e aceita até 1.000 caracteres; observação operacional é opcional e aceita até 500. | Campo vazio ou acima do limite bloqueia o salvamento. Observação não deve conter diagnóstico, detalhes de saúde ou dados pessoais desnecessários. | Definido em 14/08/2026 |
| `RN-015` | Polos, modalidades, professores e turmas não são excluídos; são inativados. | Operação comum não remove fisicamente registro nem rompe referências históricas. | Proposto |
| `RN-016` | Uma pessoa é única pelo CPF e pode acumular perfis. | A mesma pessoa pode ser responsável, aluno, professor ou administrador sem duplicar dados e possui no máximo uma conta. O perfil de responsável não cria conta automaticamente. Restrição normalizada impede duplicidade inclusive em concorrência. | Definido em 14/08/2026 |
| `RN-017` | Usuário acessa apenas funcionalidades do perfil autorizado. | Backend nega acesso vertical e horizontal independentemente da interface. | Definido para a primeira versão em 14/08/2026 |
| `RN-018` | Turma pode exigir processo seletivo; solicitação depende de decisão humana via Kanban. | Professor precisa de vínculo vigente e designação como avaliador; administrador precisa da permissão correspondente. Critérios não decidem automaticamente, mas aprovação com obrigatório não atendido exige justificativa. Sem capacidade, a candidatura continua `EM_ANÁLISE`. Primeira decisão concorrente confirmada prevalece. Somente administrador total corrige decisão final, com motivo, auditoria e compensação quando necessário. | Definido em 17/08/2026 |
| `RN-019` | Professor registra a chamada até 24 horas após a aula e não altera nem exclui chamada salva. Administrador total ou parcial com permissão pode corrigir em até 30 dias, com justificativa. | Tentativa do professor após salvamento ou prazo falha; correção autorizada preserva antes/depois, autor, data e justificativa. | Definido em 14/08/2026 |
| `RN-020` | Professor vinculado pode cancelar uma ocorrência específica de aula ou mudar somente o local dessa ocorrência. | O sistema atualiza a ocorrência, mantém a programação permanente e avisa os alunos no SIDESP e os responsáveis de menores por e-mail. O professor pode desfazer antes do início; depois, somente administrador autorizado corrige, com motivo e auditoria. Toda mudança gera novo aviso. Alterações permanentes continuam restritas ao administrador. | Definido em 14/08/2026; WhatsApp fora da primeira versão |
| `RN-021` | Administrador total ou parcial com permissão de notícias pode publicar diretamente ou agendar uma notícia, sem segunda aprovação na primeira versão. | Antes do horário a notícia não é pública; no instante aprovado passa a ser exibida. A listagem ordena por publicação decrescente e, em empate, por criação decrescente, usando `America/Sao_Paulo`. Todas as mudanças são auditadas. | Definido em 14/08/2026 |
| `RN-022` | Existem dois níveis administrativos: total e parcial. | Administrador total possui todas as áreas da matriz e concede permissões individuais ao parcial. O parcial não acessa operação não concedida, mesmo por chamada direta à API. Somente o total cria administradores, altera permissões e executa exceção de inscrição. | Definido em 14/08/2026 |
| `RN-023` | Somente administrador total pode criar ou cancelar inscrição fora das regras de idade, limite de modalidades ou capacidade, por necessidade operacional justificada. | A exceção exige justificativa e auditoria da regra ignorada. Não pode mudar a ordem da lista de espera nem dispensar, aprovar ou reorganizar o processo seletivo. | Definido em 14/08/2026 |
| `RN-024` | Administrador autorizado analisa justificativa em até 5 dias corridos; professor não acessa a função. | A recusa exige motivo. O aluno pode recorrer uma vez em até 3 dias corridos e outro administrador autorizado profere a decisão final. Todas as decisões são registradas. | Definido em 14/08/2026 |
| `RN-025` | Aluno é notificado sobre a decisão inicial e sobre a decisão final do recurso. | A recusa inicial informa o prazo de recurso; cada decisão cria uma notificação interna rastreável. | Definido em 14/08/2026 |
| `RN-026` | Aluno menor deve cadastrar um responsável legal vinculado à própria conta do aluno. O responsável não possui conta própria. | Nome e contato do responsável, incluindo WhatsApp obrigatório, são informados no cadastro. O vínculo deve ser confirmado por código enviado ao e-mail ou WhatsApp do responsável; sem confirmação, a conta do menor não é criada. Ao completar 18 anos, o vínculo deixa de ficar ativo e os dados do responsável deixam de aparecer e de ser usados na conta; o histórico segue a retenção da seção 12.3. | Definido em 14/08/2026 |
| `RN-027` | Todo cadastro exige e-mail confirmado. Administradores usam um segundo código por e-mail em todos os logins. | Senha possui no mínimo 8 caracteres, pelo menos uma letra maiúscula e pelo menos um número; letras minúsculas e símbolos são permitidos, mas opcionais. O sistema aceita pelo menos 64 caracteres. Senha comum gera aviso de fraqueza, mas não é bloqueada. A espera começa na 3ª falha consecutiva: 30 segundos, depois 1, 2, 5 e no máximo 15 minutos. O código administrativo vale 10 minutos e aceita no máximo 5 tentativas. A recuperação usa link de uso único válido por 30 minutos. | Definido em 14/08/2026 |
| `RN-028` | Perda de acesso ao e-mail exige atendimento manual por administrador total. | O administrador confere CPF e documento oficial com foto, altera o e-mail, revoga as sessões existentes e exige recuperação de senha pelo novo endereço confirmado. A equipe não guarda cópia do documento sem necessidade aprovada. | Definido em 14/08/2026 |
| `RN-029` | Cadastro de aluno exige os campos de `RF-IDN-001`; dados de saúde são opcionais, exceto contato de emergência. | Responsável de menor informa nome, CPF, vínculo, e-mail e WhatsApp e pode estar ligado a vários alunos, sem possuir conta própria. Telefone, bairro, cidade, emergência e saúde podem ser atualizados pelo aluno. E-mail exige nova confirmação; nome, CPF e nascimento exigem correção administrativa com documento; contato do responsável exige nova confirmação. | Definido em 14/08/2026 |
| `RN-030` | Dados públicos de polo e professor são limitados ao necessário para conhecer a oferta esportiva. | Polo exibe os campos de `RF-ADM-001`. Professor exibe nome, modalidade e, se informadas, foto e breve apresentação; contato e identificadores pessoais não são públicos. | Definido em 14/08/2026 |
| `RN-031` | Em emergência, professor consulta os dados mínimos de saúde permitidos e segue o procedimento presencial do polo. | Professor não altera a ficha e não usa o diário da aula para registrar diagnóstico ou detalhes de saúde. Acesso à ficha fica auditado. | Definido em 14/08/2026 |
| `RN-032` | Cadastro de professor exige nome, CPF, nascimento, e-mail e telefone; foto, apresentação e formação são opcionais. | Reativação reutiliza a mesma conta e mantém histórico. Campos públicos seguem `RN-030`. | Definido em 14/08/2026 |
| `RN-033` | Permissões administrativas são revisadas a cada 90 dias por Heitor Leite com o administrador total. | Inativação revoga imediatamente as sessões e impede novo login, sem apagar a identificação histórica do administrador. | Definido em 14/08/2026 |
| `RN-034` | Administrador exige nome, CPF, e-mail confirmado, telefone, nível, permissões e MFA por e-mail. | O último administrador total não pode ser inativado; ninguém remove o próprio acesso total. Reativação reutiliza conta e histórico. | Definido em 14/08/2026 |

## 11. Requisitos não funcionais

O Documento de Visão não apresenta uma lista explícita e mensurável de RNFs. Os itens abaixo são **propostos a partir das restrições, dos dados tratados e do Guia Mestre**. Valores não conhecidos permanecem pendentes em vez de serem inventados.

| ID | Requisito e critério de aceite | Prioridade | Status |
| --- | --- | --- | --- |
| `RNF-SEG-001` | Todo acesso não público DEVE exigir autenticação, e toda operação DEVE aplicar autorização no backend por papel, objeto e vínculo. Testes devem cobrir acesso horizontal e vertical. | Crítica | Proposto |
| `RNF-SEG-002` | Credenciais e dados pessoais em trânsito DEVEM usar HTTPS; produção NÃO DEVE aceitar HTTP para fluxos sensíveis. | Crítica | Proposto |
| `RNF-SEG-003` | Senhas DEVEM ser armazenadas apenas com algoritmo de hash de senha forte e parâmetros atuais; senha, token e segredo NÃO DEVEM aparecer em código, logs ou respostas. | Crítica | Proposto |
| `RNF-SEG-010` | MySQL, arquivos e cópias de segurança DEVEM permanecer criptografados enquanto armazenados. O método e o gerenciamento das chaves DEVEM ser alinhados com a Prefeitura e a Embrass antes da implantação; chaves não podem ficar junto dos dados protegidos nem no código-fonte. | Crítica | Requisito definido; implementação depende do alinhamento de infraestrutura |
| `RNF-SEG-009` | A autenticação DEVE exigir e-mail confirmado. Administradores DEVEM usar código enviado por e-mail além da senha em todos os logins; o código vale 10 minutos e aceita no máximo 5 tentativas. Reenvio exige intervalo de 60 segundos, aceita no máximo 5 envios por hora e invalida o código anterior. Recuperação DEVE ocorrer por link de uso único enviado ao e-mail confirmado e válido por 30 minutos. | Crítica | Definido em 14/08/2026 |
| `RNF-SEG-004` | Sessão de administrador DEVE encerrar após 15 minutos sem atividade ou 8 horas totais; demais sessões, após 30 minutos sem atividade ou 24 horas totais. Recuperação de senha revoga todas as sessões. Alteração normal de senha mantém somente a sessão atual e revoga as demais. Logout encerra a sessão utilizada. Não haverá “manter conectado” na primeira versão. O frontend NÃO DEVE guardar credenciais como texto em armazenamento do navegador. | Crítica | Parâmetros iniciais definidos em 14/08/2026; mecanismo técnico na Arquitetura e Segurança |
| `RNF-SEG-005` | Login, recuperação, cadastro e integrações DEVEM ter limites contra abuso. No login, a 3ª falha consecutiva exige 30 segundos de espera; as seguintes exigem 1, 2, 5 e depois no máximo 15 minutos. Login, cadastro e recuperação também aceitam no máximo 10 requisições por minuto conforme `RNF-SEG-008`. | Alta | Valores iniciais definidos em 14/08/2026 |
| `RNF-SEG-006` | Uploads DEVEM validar tipo real, extensão, tamanho, nome e conteúdo e permanecer fora de caminho público ou executável. Justificativa aceita de 1 a 3 arquivos PDF, JPG ou PNG, cada um com até 10 MB. Antes da abertura, DEVEM passar por verificação contra conteúdo malicioso; se ela estiver indisponível, permanecem em quarentena. | Crítica | Comportamento definido; ferramenta será escolhida na implementação de segurança |
| `RNF-SEG-007` | Ações administrativas, autenticação, chamada/correção, justificativa, inscrição excepcional, exportação e mudanças de permissão DEVEM gerar auditoria com ator, ação, alvo, instante e resultado, sem dado sensível excessivo. | Crítica | Proposto; registros mantidos por 5 anos conforme seção 12.3 |
| `RNF-SEG-008` | As APIs DEVEM limitar requisições: operações autenticadas, 100 por minuto por usuário; operações públicas, 60 por minuto por endereço IP; login, cadastro e recuperação, 10 por minuto por IP e identificador informado; upload, 10 por minuto por usuário. Ao exceder, DEVEM responder HTTP 429 sem processar a operação e registrar o evento. Limites mais restritos podem ser aplicados a operações críticas mediante registro da decisão. | Crítica | Valores iniciais definidos em 14/08/2026 |
| `RNF-PRI-001` | O sistema DEVE coletar apenas dados necessários, classificar CPF, contato, saúde, menores, frequência e documentos, e registrar finalidade, hipótese legal, acesso, compartilhamento, retenção e descarte antes da produção. | Crítica | Retenção inicial na seção 12.3; finalidade, hipótese legal e validação institucional ainda pendentes |
| `RNF-PRI-002` | Dados de saúde e comprovantes DEVEM ser restritos aos papéis aprovados; a interface e os relatórios NÃO DEVEM expô-los por padrão. | Crítica | Papéis da ficha de saúde definidos em `RF-FRQ-005`; validação institucional necessária antes do uso real |
| `RNF-PRI-003` | Em versão posterior, mapas de calor e relatórios agregados DEVEM usar polo ou bairro, sem endereço individual. Resultados com menos de três pessoas DEVEM ser ocultados ou reunidos a uma área maior. | Alta | Fora da primeira versão; limiar definido em 3 pessoas e indicadores serão aprovados pela Secretaria |
| `RNF-DSP-001` | Com até 100 usuários simultâneos, pelo menos 95 de cada 100 respostas de consultas e telas comuns DEVEM ocorrer em até 2 segundos. Cadastros e alterações comuns DEVEM terminar em até 3 segundos, medidos no servidor e sem contar a qualidade da internet do usuário. | Alta | Meta inicial definida; relatórios ficam fora da primeira versão |
| `RNF-CAP-001` | A primeira versão DEVE suportar pelo menos 5.000 alunos cadastrados, 200 turmas ativas e 100 usuários simultâneos sem violar `RNF-DSP-001`. | Alta | Volumes iniciais definidos; crescimento será revisto antes da implantação real |
| `RNF-DIS-001` | O serviço DEVE alcançar 99,5% de disponibilidade mensal. São permitidas até 4 horas mensais de manutenção programada, avisada com 48 horas de antecedência e excluída do cálculo. Cópias de segurança DEVEM ocorrer diariamente, com perda máxima de 24 horas e restauração em até 8 horas; backup só conta após restauração testada. | Alta | Metas definidas; operação será detalhada com Prefeitura e Embrass |
| `RNF-RES-001` | Falhas de WhatsApp, mapas ou exportação NÃO DEVEM corromper a operação principal. Retentativas DEVEM ser limitadas e idempotentes, com estado visível para suporte. | Alta | Proposto |
| `RNF-CON-001` | Durante a chamada, uma queda de internet DEVE manter no navegador um rascunho mínimo marcado como “não sincronizado”. Quando a conexão voltar, o sistema DEVE tentar enviá-lo sem duplicar registros. A chamada só pode aparecer como salva após confirmação do servidor. O rascunho local DEVE ser removido depois da sincronização ou do descarte confirmado. | Crítica | Comportamento definido; forma técnica de armazenamento será detalhada na Arquitetura e Segurança |
| `RNF-ACE-001` | Interfaces web DEVERIAM atender WCAG 2.2 nível AA nos fluxos críticos, incluindo teclado, foco, contraste, rótulos e mensagens de erro. | Alta | Proposto para validação |
| `RNF-UX-001` | A interface DEVE ser responsiva em computador e celular e fornecer retorno claro de sucesso, vazio, validação, indisponibilidade e autorização negada. A primeira versão suportará as duas versões mais recentes de Chrome, Edge, Firefox e Safari. | Alta | Matriz inicial definida; testes por navegador são responsabilidade de QA |
| `RNF-OBS-001` | Serviços DEVEM produzir logs estruturados, métricas e identificador de correlação, sem senha, código, documento ou dado de saúde desnecessário. Logs operacionais ficam por 90 dias; auditoria de segurança, por 5 anos. O funcionamento é verificado a cada minuto e falha crítica gera alerta para Heitor Leite em até 5 minutos. | Alta | Valores iniciais definidos em 14/08/2026 |
| `RNF-MAN-001` | Contratos da API DEVEM ser versionados e documentados em OpenAPI; regras críticas DEVEM possuir testes automatizados e rastreabilidade para `RF`/`RN`. | Alta | Proposto |
| `RNF-COM-001` | Datas e horários DEVEM usar formato inequívoco e preservar instante/fuso; a apresentação será em português do Brasil e usará `America/Sao_Paulo`. | Média | Definido em 14/08/2026 |
| `RNF-EXP-001` | Exportações NÃO DEVEM permitir injeção de fórmula em planilhas, expor campos não autorizados nem permanecer acessíveis por mais de 24 horas. | Alta | Prazo inicial definido; campos e limites dependem dos relatórios aprovados |
| `RNF-CUS-001` | A implantação DEVE usar a infraestrutura fornecida pela Prefeitura em parceria com a Embrass, dentro dos limites técnicos e administrativos formalizados. Custos adicionais e serviços externos ainda DEVEM ser aprovados antes da contratação. | Alta | Origem da infraestrutura definida; limites e responsabilidades serão confirmados antes da implantação |
| `RNF-POR-001` | Dados e arquivos DEVEM ser exportáveis em formato documentado e fornecedor deve possuir estratégia de saída, evitando dependência irreversível. | Média | Proposto |

## 12. Dados e arquivos

### 12.1 Inventário inicial

| Categoria/entidade | Exemplos | Titular | Classificação inicial | Acesso mínimo proposto | Decisões pendentes |
| --- | --- | --- | --- | --- | --- |
| Identidade e contato | nome, CPF, nascimento, e-mail, telefone, bairro, cidade e contato de emergência | Aluno, responsável, professor, administrador | Pessoal/confidencial | Próprio titular e administração autorizada | Campos iniciais definidos; finalidade, hipótese legal e retenção na seção 12.3 |
| Credenciais | hash de senha, sessão, códigos de MFA e tokens de recuperação | Usuários | Restrita | Serviço de identidade; suporte não vê senha nem código | Validade dos códigos, sessão e expiração |
| Menor e responsável | nome, CPF, e-mail, WhatsApp, vínculo e confirmação do contato | Aluno menor e responsável | Pessoal/restrita | Conta do aluno, administração autorizada e serviços de notificação | Atualização e validação institucional para uso real; retenção na seção 12.3 |
| Saúde/emergência | alergias, restrições físicas, medicamentos, deficiências/adaptações, observações médicas e tipo sanguíneo opcional | Aluno | Pessoal sensível/restrita | Administrador total, administrador parcial com permissão compatível e professor de turma vinculada | Campos são opcionais; contato de emergência é obrigatório; procedimento do polo e validação institucional para uso real; retenção na seção 12.3 |
| Polo/modalidade/turma | endereço público, horários, capacidade, regras | Secretaria | Pública ou interna por campo | Público para oferta; administração para gestão | Campos internos versus públicos |
| Inscrição/lista/processo | estados, posição, decisões e justificativas | Aluno | Pessoal/confidencial | Próprio aluno, professor no necessário, administração autorizada | Visibilidade da posição; retenção na seção 12.3 |
| Aula/frequência | presença, falta, conteúdo, observações, correções | Aluno/professor | Pessoal/confidencial | Aluno próprio, professor vinculado e administração | Período de correção; retenção na seção 12.3 |
| Comprovante de falta | documento anexado e decisão | Aluno | Potencialmente sensível/restrita | Aluno próprio e administrador aprovador | Tipos, tamanho, malware e hipótese legal; retenção na seção 12.3 |
| Notificação | canal, destinatário, modelo, resultado e estado de leitura | Usuários | Pessoal/confidencial | Sistema, suporte mínimo e usuário quando aplicável | Usuário pode apagar o conteúdo antes do prazo; registro técnico mínimo segue a seção 12.3; WhatsApp é futuro |
| Notícia | título, conteúdo, autoria e agenda | Secretaria/autor | Pública após publicação; interna antes | Público após publicação; administração antes | Aprovação editorial e retenção |
| Auditoria | ator, alvo, ação, instante, resultado | Usuários/Secretaria | Restrita | Segurança, suporte autorizado e auditoria | Integridade e revisão; retenção na seção 12.3 |
| Relatório/exportação | combinações dos dados acima | Conforme conteúdo | Herda maior classificação dos campos | Papel explicitamente autorizado | Campos dependem da Secretaria; agregação mínima de 3 pessoas e prazo na seção 12.3 |

### 12.2 Requisitos para arquivos

- Cada justificativa aceita de 1 a 3 arquivos PDF, JPG ou PNG, cada um com até 10 MB, enviados em até 7 dias corridos após a aula.
- O nome fornecido pelo usuário não será usado como caminho físico e não poderá causar sobrescrita.
- Arquivos serão validados pelo conteúdo, armazenados fora da área pública e entregues somente após autorização por objeto. Antes da abertura, passam por verificação contra conteúdo malicioso; se a verificação estiver indisponível, permanecem em quarentena.
- Comprovantes não serão copiados para ambientes de desenvolvimento/teste.
- Exportações usarão dados sintéticos em testes e neutralizarão células iniciadas por caracteres de fórmula.

### 12.3 Política inicial de retenção e descarte

Esta é uma política acadêmica inicial aprovada pela equipe. Antes de uma implantação real com dados da Prefeitura, a Secretaria e as áreas institucionais competentes deverão confirmar os prazos e as formas de descarte.

| Categoria | Prazo inicial | O que acontece ao final |
| --- | --- | --- |
| Conta e perfil | Durante a conta ativa e por 5 anos após a inativação | Excluir ou anonimizar o que não precisar ser preservado por obrigação institucional |
| Inscrições e frequência | 5 anos após o encerramento do vínculo ou registro | Excluir ou anonimizar, preservando apenas estatísticas que não identifiquem a pessoa |
| Ficha e informações de saúde | Durante o vínculo ativo e por 1 ano após seu encerramento | Excluir com segurança os dados e arquivos associados |
| Dados do responsável legal | Até o aluno completar 18 anos e por mais 1 ano | Desativar aos 18 anos; ao final do prazo, excluir ou anonimizar o histórico que não precise ser preservado |
| Comprovantes de justificativa | 1 ano após a decisão final | Excluir o arquivo; manter somente o registro mínimo da decisão quando necessário |
| Notificações e tentativas de envio | Até 1 ano após o envio | O usuário pode apagar o conteúdo da própria central a qualquer momento. No máximo ao final de 1 ano, excluir o conteúdo restante; preservar somente o registro técnico mínimo quando necessário para auditoria |
| Registros de auditoria | 5 anos após o evento | Excluir com segurança, desde que não exista investigação ou obrigação de preservação em andamento |
| Arquivos de relatório e exportação | 24 horas após a geração | Excluir automaticamente o arquivo gerado |
| Cópias de segurança | 30 dias após a criação | Expirar e eliminar automaticamente conforme a rotação das cópias |

O usuário poderá corrigir os campos de contato permitidos e solicitar inativação ou exclusão pelo SIDESP. O administrador total executará o pedido e Heitor Leite analisará os casos com dados sensíveis. Informações ainda abrangidas pelos prazos desta seção serão desativadas e terão o descarte programado. Em uma implantação real, a Secretaria deverá indicar o responsável institucional e confirmar as exceções legais.
- A política inicial de retenção, descarte e backup está definida nesta seção. A criptografia dos dados armazenados é obrigatória por `RNF-SEG-010`; método e chaves serão alinhados com Prefeitura e Embrass. A validação institucional permanece necessária antes do uso real.

## 13. Interfaces e integrações

| Interface | Finalidade | Dados principais | Comportamento em falha | Situação |
| --- | --- | --- | --- | --- |
| Frontend Angular ↔ API SIDESP | Todos os fluxos autenticados e públicos | Dados de tela e comandos autorizados | Erro seguro, código de acompanhamento e nenhuma confirmação falsa | Confirmado quanto às tecnologias; contrato OpenAPI futuro |
| API Java/Spring Boot ↔ MySQL | Consultar e alterar os dados estruturados do SIDESP | Cadastros, inscrições, turmas, frequência, permissões e históricos | A operação falha sem confirmar resultado parcial; o frontend nunca acessa o banco diretamente | MySQL 8.x confirmado; hospedagem será fornecida pela Prefeitura/Embrass; versão exata e ferramenta de alteração do banco serão homologadas antes do desenvolvimento |
| Infraestrutura Prefeitura/Embrass | Hospedar frontend, API, MySQL, arquivos, backups e monitoramento | Aplicação e dados conforme acesso técnico autorizado | Responsabilidades, ambientes, capacidade, segurança, suporte e recuperação devem ser formalizados antes da implantação | Fonte de infraestrutura confirmada; detalhes operacionais pendentes de alinhamento |
| Provedor de WhatsApp | Canal adicional para alertas, avisos, decisões e oferta de vaga | Telefone, modelo e parâmetros mínimos | Registrar falha e manter a notificação interna obrigatória; a operação principal não é desfeita | Canal opcional no produto; fornecedor e modelos pendentes |
| E-mail/serviço de identidade | Confirmação, recuperação de acesso e código administrativo | Endereço e token opaco de curta duração | Resposta antienumeração; suporte conforme fluxo aprovado | Integração configurável; fornecedor será escolhido antes da implantação |
| Mapas/geocodificação | Exibir polos e apoiar visualizações geográficas em versão posterior | Endereço/coordenadas de polo; agregados autorizados | A lista pública continua disponível sem o mapa | Fora da primeira versão; fornecedor futuro |
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
| `ERR-007` | WhatsApp indisponível | Operação principal e notificação interna são preservadas; tentativa externa é marcada como falha sem retirar o aviso do SIDESP |
| `ERR-008` | Conexão cai durante chamada | Manter rascunho marcado como não sincronizado, reenviar sem duplicidade e só mostrar sucesso após confirmação do servidor |
| `ERR-009` | Administrador reduz capacidade abaixo das inscrições | Bloquear ou exigir plano explícito; nunca cancelar alunos silenciosamente |
| `ERR-010` | A falta que ultrapassaria o limite possui justificativa em análise | Suspender o cancelamento automático até a decisão. Se a justificativa for aceita, retirar a falta da contagem; se for recusada, concluir o cancelamento uma única vez. |
| `ERR-011` | Oferta de vaga expira enquanto aluno confirma | Processamento transacional produz um único vencedor e resposta clara ao aluno |
| `ERR-012` | Exportação excede limite | Rejeitar ou processar assincronamente segundo limite aprovado; não degradar todo o serviço |
| `ERR-013` | Provedor de mapas falha | Exibir lista/estado degradado e manter demais funções disponíveis |
| `ERR-014` | Professor tenta alterar chamada salva | Negar; orientar fluxo administrativo de correção sem apagar histórico |
| `ERR-015` | Conta/polo/modalidade/turma inativa é referenciada | Preservar consultas históricas e impedir novos vínculos quando a regra exigir |

## 15. Critérios de aceite do produto

A baseline de requisitos será aceita quando:

- todos os requisitos críticos tiverem ator, entrada, saída, erro, permissão e aceite definidos ou estiverem marcados como pendência bloqueadora;
- o responsável de negócio resolver as pendências restantes de lista de espera, exceção administrativa e processo seletivo;
- a matriz de papéis e permissões for aprovada e testável;
- os dados pessoais/sensíveis tiverem finalidade, acesso, retenção e descarte definidos pelo responsável interno por privacidade; uma eventual operação real deverá receber validação institucional do órgão cliente;
- os requisitos não funcionais críticos receberem valores mensuráveis ou justificativa formal;
- cada requisito funcional estiver ligado a pelo menos um caso de uso e a um cenário de teste;
- diagramas e protótipos não contradisserem o comportamento aprovado;
- Product Owner, responsável de negócio/Scrum Master, responsável técnico, QA, segurança e privacidade do projeto registrarem aprovação nas suas competências.

Para uma funcionalidade individual ser aceita, o caminho principal, estados vazios, validações, falta de autorização, concorrência/repetição relevante, auditoria e efeitos de privacidade devem ser demonstrados com dados sintéticos.

## 16. Riscos e lacunas

| ID | Risco/lacuna | Impacto | Tratamento inicial | Responsável |
| --- | --- | --- | --- | --- |
| `RIS-001` | Implementação pode aplicar incorretamente o limite variável de faltas | Cancelamento ou notificação incorretos | Testar `limite - 1`, ultrapassagem, justificativa aceita/recusada/em análise, aula cancelada e correção de chamada | Negócio/PO/QA |
| `RIS-002` | Dados de saúde, menores e comprovantes possuem política acadêmica inicial, mas ainda não têm validação institucional para uso real | Dano ao titular e uso inadequado | Aplicar acesso restrito e retenção da seção 12.3; exigir validação do órgão cliente antes de dados reais | Privacidade/Negócio |
| `RIS-003` | Permissões administrativas podem permanecer indevidas após mudança de função | Escalada de privilégio ou bloqueio operacional | Aplicar matriz da seção 9.4.1, revisar a cada 90 dias e revogar sessões na inativação | Negócio/Segurança |
| `RIS-004` | Exceções de idade, modalidades ou capacidade podem ser usadas em excesso | Inconsistência e abuso administrativo | Restringir a administrador total, exigir justificativa, auditoria e monitoramento | Negócio/Segurança |
| `RIS-005` | WhatsApp ainda não possui fornecedor | Mensagens externas não entregues e custo desconhecido | Manter a integração fora da primeira versão, usar notificações internas obrigatórias e só ativar WhatsApp após avaliação de fornecedor e modelos | Negócio/Técnico |
| `RIS-006` | Internet insuficiente nos polos | Chamada perdida ou duplicada | Manter rascunho local sinalizado, sincronizar sem duplicidade e só confirmar após resposta do servidor | Técnico/Negócio |
| `RIS-007` | Os volumes e metas iniciais podem ser menores que a demanda real da Prefeitura | Lentidão ou indisponibilidade após crescimento | Testar os valores de `RNF-DSP-001` e `RNF-CAP-001` e revisar a capacidade antes da implantação real | Técnico/Negócio/QA |
| `RIS-008` | Campos, fórmulas, indicadores e fornecedor de mapas ainda não foram definidos | Relatórios divergentes ou inadequados | Manter relatórios e mapas fora da primeira versão; só implementar após aprovação, com agregação mínima de 3 pessoas | Negócio/Privacidade |
| `RIS-009` | Recuperação “por CPF ou e-mail” pode ser interpretada como validação fraca | Tomada de conta | Adotar token em canal verificado e resposta antienumeração | Segurança/Técnico |
| `RIS-010` | Protótipo usa CSV e não representa requisitos de produção | Decisões técnicas inadequadas por herança acidental | Tratar protótipo apenas como referência conceitual | Tech Lead |
| `RIS-011` | Detalhes técnicos e responsabilidades da infraestrutura Prefeitura/Embrass ainda não foram formalizados no projeto | Ambiente incompatível, atraso ou falha de suporte | Confirmar capacidade, acessos, ambientes, backups, monitoramento, segurança e suporte antes da implantação | Prefeitura/Embrass/Técnico |
| `RIS-012` | Uma migração futura pode ser iniciada sem inventário ou validação | Perda ou duplicidade de registros | Primeira versão começa vazia; qualquer migração futura será atividade separada, com fontes, qualidade, autorização e conferência próprias | Negócio/Dados |
| `RIS-013` | Comprovante malicioso ou não verificado pode alcançar um administrador | Comprometimento do dispositivo ou exposição de dados | Validar tipo e conteúdo, manter em quarentena e impedir abertura até a verificação | Segurança/Técnico |

## 17. Decisões já registradas

| ID | Decisão | Fonte | Estado |
| --- | --- | --- | --- |
| `DEC-001` | A documentação de requisitos representa o produto completo, não apenas o backend. | Orientação Tech Lead | Aceita para este documento |
| `DEC-002` | O backend alvo será desenvolvido em Java/Spring Boot. | Orientação do Tech Lead | Diretriz técnica; detalhamento pertence à Arquitetura/ADR |
| `DEC-003` | O README será elaborado posteriormente, quando houver código relevante. | Orientação do Tech Lead | Aceita para a sequência de trabalho |
| `DEC-004` | O protótipo Java básico não define o escopo nem a arquitetura do produto. | Orientação do Tech Lead | Aceita |
| `DEC-005` | O frontend alvo será desenvolvido em Angular/TypeScript. | Orientação do Tech Lead | Diretriz técnica; detalhamento pertence à Arquitetura/ADR |
| `DEC-006` | O nome oficial do produto é **Sistema Integrado de Desenvolvimento Esportivo Público**, mantendo a sigla SIDESP. | Decisão da equipe em 13/08/2026 | Aceita; deve ser usada em toda a documentação |
| `DEC-007` | O banco relacional será MySQL 8.x, preferencialmente em uma versão LTS homologada antes do desenvolvimento. | Decisão da equipe em 14/08/2026 | Aceita; versão exata pendente |
| `DEC-008` | O grupo criará uma API própria em Java/Spring Boot. O frontend acessará essa API, e somente o backend acessará o MySQL. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-009` | Os protótipos existentes no Figma são referência visual para o frontend Angular, sem substituir requisitos e regras aprovados. | Link fornecido pela equipe em 14/08/2026 | Aceita |
| `DEC-010` | Kauãn Raphael responde por negócio e Scrum Master; Livia Andrade por Product Owner; Heitor Leite por técnica, segurança e privacidade interna; Micael Phillipini por QA. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-011` | A oferta de vaga terá 48 horas de disponibilidade a partir do registro da oferta e da notificação interna; indisponibilidade oficial registrada do SIDESP pausa o prazo. | Decisão da equipe em 14/08/2026, complementada em 17/08/2026 | Aceita |
| `DEC-012` | Notificações internas são obrigatórias; WhatsApp é um canal adicional, ativado somente após escolha de fornecedor. Quando estiver ativo para um número cadastrado, o usuário não poderá desabilitá-lo individualmente. Para menor, o número do responsável é obrigatório. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-013` | Exceções de idade, limite de modalidades e capacidade são exclusivas do administrador total e exigem justificativa. Ordem da fila e processo seletivo não podem ser alterados por exceção. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-014` | A ficha de saúde terá os campos e acessos descritos em `RF-FRQ-005`; tipo sanguíneo é opcional. | Decisão da equipe em 14/08/2026 | Aceita; retenção inicial na seção 12.3 |
| `DEC-015` | O responsável do menor confirma o vínculo por código enviado ao próprio e-mail ou WhatsApp. O número de WhatsApp do responsável é obrigatório; sem confirmação, a conta do menor não é criada. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-016` | Todo cadastro exige e-mail confirmado; recuperação ocorre por link nesse e-mail; administrador informa código enviado por e-mail em todos os logins. | Decisão da equipe em 14/08/2026 | Aceita; tempos e limites definidos em `DEC-024` |
| `DEC-017` | Administrador total possui todas as áreas e concede permissões individuais ao administrador parcial conforme a matriz da seção 9.4.1. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-018` | O processo seletivo usa os cinco estados de `RF-INS-007`; critérios variam por modalidade e faixa etária e não existe prazo máximo geral. | Decisão da equipe em 14/08/2026 | Aceita; complementada por `DEC-020` quanto à ausência de anexos na primeira versão |
| `DEC-019` | A senha permite letras minúsculas, maiúsculas, números e símbolos; exige ao menos uma letra maiúscula e um número. Símbolos são opcionais. | Decisão da equipe em 14/08/2026 | Aceita; tempos e limites definidos em `DEC-024` |
| `DEC-020` | A primeira versão do processo seletivo não receberá anexos. A avaliação registrará critérios, resultado e observação em texto. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-021` | Campos, filtros, fórmulas e permissões dos relatórios serão definidos futuramente pela Secretaria; nenhum relatório específico deve ser implementado antes dessa aprovação. | Decisão da equipe em 14/08/2026 | Aceita como adiamento; funcionalidade fora da primeira versão |
| `DEC-022` | O mapa de calor agrupará dados por polo ou bairro, não mostrará endereço individual e ocultará ou reunirá resultados com menos de 3 pessoas. | Decisão da equipe em 14/08/2026 | Aceita para versão posterior; indicadores e fornecedor serão definidos pela Secretaria |
| `DEC-023` | A política acadêmica inicial de retenção e descarte é a da seção 12.3. | Decisão da equipe em 14/08/2026 | Aceita para o projeto; requer validação institucional antes do uso real |
| `DEC-024` | A espera do login começa na 3ª falha consecutiva e progride por 30 segundos, 1, 2, 5 e no máximo 15 minutos. O código administrativo vale 10 minutos e aceita 5 tentativas; o link de recuperação vale 30 minutos e é de uso único. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-025` | Relatórios e exportações específicos ficam fora da primeira versão e só entram no desenvolvimento após a Secretaria definir campos, filtros, fórmulas e permissões. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-026` | O usuário pode corrigir contatos e solicitar inativação ou exclusão pelo SIDESP. Administrador total executa o pedido; Heitor Leite analisa casos com dados sensíveis; retenções ainda vigentes geram descarte programado. | Decisão da equipe em 14/08/2026 | Aceita para o projeto; implantação real exige responsável institucional |
| `DEC-027` | A primeira versão usará as metas de desempenho, capacidade, disponibilidade, recuperação, navegadores e arquivos descritas nos requisitos não funcionais da seção 11. | Decisão da equipe em 14/08/2026 | Aceita; revisar antes da implantação real |
| `DEC-028` | A chamada terá rascunho parcialmente offline no navegador, aviso de não sincronizado e envio sem duplicidade quando a conexão voltar; somente a confirmação do servidor conclui o salvamento. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-029` | A integração real com WhatsApp fica fora da primeira versão. O canal interno atenderá os fluxos obrigatórios até que fornecedor, custo e modelos sejam aprovados. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-030` | Administrador total e administrador parcial com permissão de notícias podem publicar diretamente, sem segunda aprovação na primeira versão; todas as ações são auditadas. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-031` | Professor pode atuar em várias turmas, uma turma pode ter vários professores e substituições temporárias usam data inicial e final; somente administrador autorizado gerencia os vínculos. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-032` | O aluno ocupa no máximo uma lista de espera por modalidade. Saída e reentrada ou troca de turma colocam o aluno no final da nova fila, sem restauração manual da posição. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-033` | Presença por QR Code fica fora da primeira versão e poderá ser reavaliada como melhoria futura. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-034` | A primeira versão começará com banco vazio e não migrará planilhas, sistemas anteriores nem dados do protótipo Java. Qualquer migração futura será uma atividade separada. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-035` | A central interna exibirá os eventos definidos em `RF-COM-005` como lidos ou não lidos por até 1 ano. O usuário poderá apagar o conteúdo a qualquer momento; somente o registro técnico mínimo poderá seguir o prazo de auditoria. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-036` | Justificativa pode ser enviada em 7 dias corridos, deve ser analisada em 5 dias corridos e admite um recurso em 3 dias corridos, decidido por outro administrador autorizado. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-037` | Inscrição ativa pode ser cancelada a qualquer momento; retorno exige nova solicitação. Chamada pode ser registrada em 24 horas e corrigida por administrador autorizado em até 30 dias, sempre com justificativa. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-038` | Na perda do e-mail, somente administrador total troca o endereço após conferir CPF e documento oficial com foto; sessões são revogadas e a senha deve ser recuperada pelo novo e-mail confirmado. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-039` | O cadastro e a edição do perfil seguem os campos e controles de `RF-IDN-001/004`; saúde é opcional, exceto contato de emergência. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-040` | Os dados públicos de polos e professores são os definidos em `RN-030`; contatos e identificadores pessoais do professor não são exibidos. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-041` | Mudança de idade vale para novas solicitações; limite de faltas muda no mês seguinte; excesso de solicitações à capacidade entra na fila; horários recorrentes têm dias, horário, início e fim e suas alterações notificam os alunos. | Decisão da equipe em 14/08/2026 | Aceita; complementada por `DEC-044` |
| `DEC-042` | Diário da aula usa limites de 1.000 caracteres para conteúdo e 500 para observação. Em emergência, professor consulta dados mínimos e segue o procedimento do polo, sem alterar a ficha. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-043` | Sessões e limites de requisições seguem os valores de `RNF-SEG-004/005/008`. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-044` | O sistema bloqueia redução de capacidade abaixo das inscrições confirmadas e não remove alunos automaticamente. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-045` | Cadastro, reativação e vigência de vínculo de professor seguem `RF-ADM-003`, `RF-FRQ-002` e `RN-032`; conta e histórico são preservados. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-046` | Heitor Leite e o administrador total revisam acessos administrativos a cada 90 dias; inativação revoga sessões imediatamente e preserva o histórico. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-047` | Cada justificativa aceita de 1 a 3 arquivos de até 10 MB; arquivos permanecem em quarentena até a verificação contra conteúdo malicioso. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-048` | Prefeitura e Embrass fornecerão hospedagem, banco e infraestrutura. Mapas ficam fora da primeira versão, e-mail será configurável e indicadores de melhoria serão medidos após piloto. | Decisão da equipe em 14/08/2026 | Aceita; detalhes operacionais da infraestrutura serão confirmados antes da implantação |
| `DEC-049` | Contas administrativas usam os campos e proteções de `RF-ADM-007` e `RN-034`; o último administrador total e o próprio acesso total não podem ser removidos. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-050` | Notícias são ordenadas pela publicação mais recente e, em empate, pela criação mais recente, no fuso `America/Sao_Paulo`. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-051` | Logs operacionais duram 90 dias, auditoria dura 5 anos, funcionamento é verificado a cada minuto e falha crítica alerta Heitor Leite em até 5 minutos. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-052` | MySQL, arquivos e backups devem usar criptografia enquanto armazenados; método e chaves serão alinhados com Prefeitura e Embrass. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-053` | São permitidas até 4 horas mensais de manutenção programada, avisada com 48 horas de antecedência e não contada na disponibilidade de 99,5%. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-054` | O aluno vê a própria posição numérica exata na fila, sem nomes nem dados das demais pessoas. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-055` | O aluno pode cancelar inscrição confirmada, entrada na lista de espera ou candidatura ao processo seletivo; eventual retorno exige nova solicitação e nova posição. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-056` | Na primeira versão, o aluno não recorre de `APROVADO` ou `REPROVADO`; somente administrador total corrige erro administrativo com motivo e auditoria, sem manipular ordem ou critérios. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-057` | Professor vinculado pode cancelar ou mudar o local de uma ocorrência específica; alteração permanente de horário, polo ou período continua administrativa. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-058` | Na primeira versão, comunicações importantes do menor chegam ao aluno dentro do SIDESP e ao e-mail confirmado do responsável; WhatsApp será adicional no futuro. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-059` | Após sair da fila, o aluno vê estado encerrado, última posição e instante da saída, sem recálculo de posição ativa. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-060` | Correção seletiva anterior à matrícula é direta; depois dela, usa compensação. Aprovação tardia exige vaga ou exceção de capacidade separada. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-061` | E-mail transacional terá no máximo três tentativas: imediata, após 5 minutos e após 30 minutos. Falha final cria pendência e não desfaz a operação principal. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-062` | Professor desfaz alteração de ocorrência antes do início da aula; depois, somente administrador autorizado corrige, com motivo, auditoria e novo aviso. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-063` | Pedido de privacidade pode ser retirado antes de qualquer execução e recebe resposta inicial em até 15 dias corridos; prazo definitivo exige validação institucional. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-064` | Responsável legal mantém CPF no cadastro, não possui conta própria e pode estar vinculado a vários alunos. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-065` | Justificativa exige descrição, mas o comprovante é opcional; podem ser anexados de 0 a 3 arquivos. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-066` | Frequência possui somente `PRESENTE` e `AUSENTE`; justificativa aceita desconsidera a ausência, e aula cancelada não tem chamada. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-067` | Local de uma ocorrência pode ser polo cadastrado ou local temporário com nome, endereço e complemento, sem mudar o polo permanente. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-068` | Turma usa `PLANEJADA`, `ATIVA`, `SUSPENSA`, `ENCERRADA` e `INATIVA`; suspensão preserva vínculos, bloqueia novas inscrições e não gera aulas ou faltas. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-069` | Conta usa `PENDENTE_CONFIRMACAO`, `ATIVO` e `INATIVO`; espera progressiva de login não cria estado bloqueado. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-070` | Uma justificativa pode abranger várias ausências do mesmo aluno e motivo/período, inclusive de modalidades diferentes, respeitando o prazo individual. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-071` | Ficha de saúde mantém versão atual e histórico protegido; log comum registra somente autor, instante e campos alterados. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-072` | Suspensão não amplia automaticamente a turma; o administrador define novo período e ocorrências ao reativar. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-073` | Reagendamento preserva a aula e registra valores anterior/novo, motivo, autor e instante; passa de `REAGENDADA` para `REALIZADA`. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-074` | Uma pessoa é única pelo CPF, pode acumular perfis e possui conta opcional; ser responsável não cria conta automaticamente. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-075` | Mudança confirmada no contato do responsável compartilhado vale para todos os vínculos e gera aviso aos alunos associados. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-076` | Processo seletivo usa critérios textuais versionados por modalidade e idade, avaliados como `ATENDEU` ou `NÃO ATENDEU`, sem pontuação automática. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-077` | Pedido de inscrição não persiste `SOLICITADA`; produz confirmação, fila, candidatura ou rejeição. | Decisão da equipe em 14/08/2026 | Aceita |
| `DEC-078` | Justificativa usa sete estados e pode ser cancelada pelo aluno somente antes da primeira decisão, após aviso dos efeitos. | Decisão da equipe em 14/08/2026 | Aceita |

## 18. Questões pendentes e respondidas

### 18.1 Bloqueadoras para aprovar a baseline

Não há questão de escopo ou regra da primeira versão aguardando decisão da equipe. Relatórios, WhatsApp, mapas interativos, mapa de calor, migração legada e QR Code foram formalmente adiados. A implantação real ainda depende das validações institucionais e dos alinhamentos de infraestrutura indicados neste documento.

### 18.2 Importantes, não bloqueadoras do primeiro refinamento

As questões numeradas foram respondidas. O documento está em `0.2.0 — Pronto para revisão`; as situações ainda mencionadas como futuras pertencem a versões posteriores, à implantação ou à aprovação formal, e não devem ser tratadas como autorização para desenvolver funcionalidade fora da primeira versão.

### 18.3 Questões respondidas em 14/08/2026

| ID | Decisão registrada | Pendência relacionada que permanece |
| --- | --- | --- |
| `Q-001` | O alerta ocorre em `limite - 1`; todas as faltas podem ser justificadas; o cancelamento ocorre somente quando as faltas não justificadas ultrapassam o limite. | Prazos foram definidos em `RF-JUS-001/002`. |
| `Q-002` | O limite é de duas modalidades diferentes. Contam inscrições ativas e processos seletivos em andamento; lista de espera não conta. Duas turmas da mesma modalidade não são permitidas. Conflito de horário gera aviso, mas não bloqueio. | Reentrada e troca de fila foram resolvidas em `Q-021`. |
| `Q-004` | A idade é verificada no momento da inscrição. O aluno que ultrapassar a idade máxima depois pode permanecer até o encerramento da turma. | Nenhuma para esta regra. |
| `Q-005` | A contagem reinicia mensalmente; aula cancelada não gera falta; correção recalcula o total; justificativa aceita retira a falta. Se a falta que causaria cancelamento estiver em análise, o cancelamento fica suspenso até a decisão. | Análise e recurso foram definidos em `RF-JUS-002` e `RN-024`. |
| `Q-003` | A oferta possui 48 horas de disponibilidade, iniciadas com o registro da oferta e da notificação interna. Indisponibilidade oficial do SIDESP pausa o prazo; falhas particulares de internet, e-mail ou WhatsApp não pausam. | WhatsApp ficou fora da primeira versão em `Q-009`; o canal interno é suficiente para o fluxo inicial. |
| `Q-006` | Somente administrador total pode excepcionar idade, limite de modalidades ou capacidade, sempre com justificativa. Não pode alterar fila nem processo seletivo. | Nenhuma para o alcance da exceção; auditoria continua obrigatória. |
| `Q-007` | A ficha de saúde contém alergias, restrições físicas, medicamentos, deficiências/adaptações, observações médicas e tipo sanguíneo opcional. Esses campos são opcionais; contato de emergência é obrigatório. O professor acessa dados mínimos em emergência e segue o procedimento do polo. | A retenção inicial está na seção 12.3; ainda é necessária validação institucional antes do uso real. |
| `Q-008` | O responsável não possui conta. Seus dados são obrigatórios na conta do aluno menor; o vínculo é confirmado por código enviado ao e-mail ou WhatsApp do responsável. Ao completar 18 anos, os dados deixam de ficar ativos e visíveis. Mudança do contato enquanto o aluno é menor exige nova confirmação. | Retenção inicial está na seção 12.3; WhatsApp fica para versão futura, com confirmação por e-mail na primeira versão. |
| `Q-009`/`Q-017` | Notificações internas são obrigatórias. A integração real com WhatsApp fica fora da primeira versão e só será ativada após escolha de fornecedor. Quando estiver ativa, não poderá ser desabilitada individualmente para número cadastrado. O número do responsável continua obrigatório para aluno menor. | Fornecedor, custo e modelos serão tratados no backlog da versão futura. |
| `Q-008`/`Q-017` | O responsável confirma o vínculo pelo próprio e-mail na primeira versão. Seu número de WhatsApp também é obrigatório no cadastro do menor, mas esse canal só será ativado depois da escolha do fornecedor. Sem confirmação do e-mail, a conta não é criada. | Mudança de contato exige nova confirmação; retenção após a maioridade está na seção 12.3. |
| `Q-011` | A matriz da seção 9.4.1 define as permissões. Somente administrador total cria administradores, concede ou revoga permissões e executa exceções de inscrição. | A revisão ocorre a cada 90 dias conforme `RN-033`. |
| `Q-012` | O Kanban usa `INSCRITO`, `EM_ANÁLISE`, `APROVADO`, `REPROVADO` e `CANCELADO`; critérios variam por modalidade e faixa etária; não há prazo máximo geral. Na primeira versão não haverá anexos nem recurso do aluno. Antes da matrícula, administrador total corrige erro diretamente; depois, executa compensação. Aprovação tardia exige vaga ou exceção de capacidade separada. | Critérios específicos serão configurados por modalidade e faixa etária. |
| `Q-010` | Cadastro exige e-mail confirmado; senha tem no mínimo 8 caracteres, uma letra maiúscula e um número. A espera começa na 3ª falha: 30 segundos, depois 1, 2, 5 e no máximo 15 minutos. O código administrativo vale 10 minutos e 5 tentativas; o link de recuperação vale 30 minutos e é de uso único. | Sessões e limites da API foram definidos em `RNF-SEG-004/005/008`. |
| `Q-013` | A equipe decidiu que relatórios e exportações específicos ficam fora da primeira versão. Todo o conteúdo será definido futuramente pela Secretaria. | Não implementar antes da aprovação de campos, filtros, fórmulas e permissões. |
| `Q-014` | O mapa de calor usará polo ou bairro, não mostrará endereços individuais e exigirá grupo mínimo de 3 pessoas; grupos menores serão ocultados ou reunidos. | Ficou fora da primeira versão; indicadores e fornecedor serão definidos junto aos relatórios futuros. |
| `Q-015` | A equipe aprovou os prazos da seção 12.3. O usuário poderá corrigir contatos e solicitar inativação ou exclusão pelo SIDESP; administrador total executará o pedido e Heitor Leite analisará casos com dados sensíveis. Prazos ainda vigentes geram descarte programado. | Implantação real requer validação institucional; migração foi resolvida em `Q-023`. |
| `Q-016` | A primeira versão suportará 5.000 alunos, 200 turmas ativas e 100 usuários simultâneos; consultas comuns terão p95 de 2 segundos e alterações, 3 segundos. A disponibilidade será 99,5%; cópia diária, perda máxima de 24 horas e recuperação em 8 horas. Serão testadas as duas versões mais recentes dos navegadores aprovados. Justificativas aceitarão até 3 arquivos de 10 MB cada. | Limites de exportação serão definidos quando os relatórios entrarem no escopo; capacidade será revista antes do uso real. |
| `Q-018` | A chamada terá rascunho parcialmente offline, indicação clara de “não sincronizado” e reenvio sem duplicidade. Somente a confirmação do servidor considera a chamada salva. | A forma técnica de armazenamento será detalhada na Arquitetura e Segurança. |
| `Q-019` | Administrador total e administrador parcial com permissão de notícias podem publicar diretamente, sem segunda aprovação na primeira versão. Criação, edição, agendamento, publicação e inativação são auditados. | Nenhuma para o fluxo inicial. |
| `Q-020` | Um professor pode atuar em várias turmas e uma turma pode ter vários professores. Substituições temporárias têm início e fim e somente administrador autorizado gerencia os vínculos. Cadastro e reativação seguem `RF-ADM-003`. | Nenhuma para o fluxo inicial. |
| `Q-021` | O aluno ocupa no máximo uma fila por modalidade e vê a própria posição numérica exata, sem dados de terceiros. Ao sair, vê o estado encerrado, a última posição e o instante da saída, sem recálculo ativo. Ao retornar, entra no final; ao trocar de turma da mesma modalidade, sai da fila anterior e entra no final da nova. | Nenhuma para ordem, visibilidade, histórico e reentrada. |
| `Q-022` | Presença por QR Code fica fora da primeira versão. | Pode ser reavaliada como melhoria futura mediante novo refinamento de requisitos e segurança. |
| `Q-023` | A primeira versão começará com banco vazio, sem importar planilhas, sistemas anteriores ou dados do protótipo Java. | Qualquer migração futura será uma atividade separada, com inventário, autorização e conferência próprios. |
| `Q-024` | A central interna mostrará inscrição/cancelamento, fila/oferta, processo seletivo, faltas, justificativas, mudanças de aula e avisos de conta/segurança. As mensagens terão estado lida/não lida e prazo máximo de 1 ano. | O usuário pode apagar o conteúdo a qualquer momento; apenas o registro técnico mínimo pode permanecer para auditoria. |

### 18.4 Decisões complementares dos fluxos de atividades

| ID | Decisão registrada | Situação |
| --- | --- | --- |
| `DEC-079` | No processo seletivo, cada critério informa se é obrigatório ou opcional, mas nenhum resultado decide automaticamente. Professor ou administrador responsável registra a decisão humana final. | Aceita em 14/08/2026; complementada pelas decisões `DEC-087` a `DEC-090` |
| `DEC-080` | No fluxo de chamada offline, o rascunho local dura no máximo até o limite de 24 horas após a aula, não contém dados de saúde e é apagado depois da sincronização ou expiração. Conflito com uma versão salva no servidor nunca é sobrescrito automaticamente. | Aceita em 14/08/2026 |
| `DEC-081` | Uma justificativa que reúne várias ausências recebe uma única decisão para o conjunto. Ausências que precisem de resultados diferentes devem ser enviadas em justificativas separadas. | Aceita em 14/08/2026 |
| `DEC-082` | Heitor Leite coordena incidentes pelo grupo; Prefeitura e Embrass apoiam contenção e recuperação da infraestrutura; Kauãn Raphael e Livia Andrade participam das decisões de impacto e comunicação. Comunicação regulatória dependerá das autoridades municipais designadas antes da implantação real. | Aceita em 14/08/2026 |
| `DEC-083` | Recuperação de senha revoga todas as sessões. Alteração normal de senha mantém a sessão atual e revoga as demais. | Aceita em 14/08/2026 |
| `DEC-084` | Reenvio de código MFA exige intervalo mínimo de 60 segundos, aceita no máximo 5 envios por hora e invalida o código anterior. | Aceita em 14/08/2026 |
| `DEC-085` | Oferta de vaga só pode ser confirmada ou recusada dentro do SIDESP autenticado; mensagens apenas direcionam para `Minhas ofertas`. | Aceita em 14/08/2026 |
| `DEC-086` | Apagar a notificação não cancela nem oculta a oferta ativa da área `Minhas ofertas`. | Aceita em 14/08/2026 |
| `DEC-087` | Professor só avalia processo seletivo quando possui vínculo vigente com a turma e designação explícita; administrador precisa da permissão correspondente. | Aceita em 17/08/2026 |
| `DEC-088` | Aprovação com critério obrigatório não atendido exige justificativa específica e auditável. | Aceita em 17/08/2026 |
| `DEC-089` | Em decisões seletivas simultâneas, a primeira confirmada prevalece; a segunda não sobrescreve e a correção final fica com administrador total. | Aceita em 17/08/2026 |
| `DEC-090` | Sem capacidade, a candidatura permanece `EM_ANÁLISE` até surgir vaga ou administrador total executar exceção de capacidade justificada. | Aceita em 17/08/2026 |
| `DEC-091` | Indisponibilidade oficial registrada do SIDESP pausa o prazo da oferta; problema particular de conexão não pausa. | Aceita em 17/08/2026 |
| `DEC-092` | O Flyway será usado para versionar e aplicar, em ordem, as alterações controladas do banco de dados. | Aceita em 17/08/2026 |
| `DEC-093` | Na primeira versão, as sessões serão persistidas no MySQL; armazenamento dedicado, como Redis, só será avaliado se a necessidade técnica for demonstrada. | Aceita em 17/08/2026 |
| `DEC-094` | Os identificadores serão UUID na aplicação e na API, armazenados como `BINARY(16)` no MySQL. | Aceita em 17/08/2026 |
| `DEC-095` | CPF e e-mail serão criptografados e terão hashes normalizados separados para pesquisa; informações de saúde serão criptografadas por campo. Método e chaves serão alinhados com Prefeitura e Embrass. | Aceita em 17/08/2026 |
| `DEC-096` | Depois do prazo de retenção, dados pessoais e arquivos sem necessidade de preservação serão eliminados ou anonimizados; estatísticas só permanecem quando a anonimização for irreversível. | Aceita em 17/08/2026 |
| `DEC-097` | O e-mail confirmado usado para entrar no SIDESP é exclusivo de uma conta. E-mails usados somente como contato de responsável podem aparecer em vários vínculos e não concedem acesso às contas relacionadas. | Aceita em 17/08/2026 |
| `DEC-098` | Estados serão armazenados como códigos de texto validados, sem usar o `ENUM` nativo do MySQL, facilitando a evolução por migrações Flyway. | Aceita em 17/08/2026 |
| `DEC-099` | Registros de auditoria serão somente de inclusão; a aplicação comum não poderá alterá-los ou apagá-los. Após cinco anos, somente rotina administrativa separada e auditada poderá eliminá-los quando não houver obrigação de preservação. | Aceita em 17/08/2026 |
| `DEC-100` | O MySQL usará `utf8mb4`; pesquisas gerais não diferenciarão maiúsculas, minúsculas e acentos, enquanto códigos, tokens e hashes terão comparação exata. | Aceita em 17/08/2026 |
| `DEC-101` | Sessões e tokens consumidos ou expirados serão removidos após 30 dias; controles detalhados de tentativas e limites após 90 dias; eventos resumidos de segurança permanecem na auditoria por cinco anos. | Aceita em 17/08/2026 |
| `DEC-102` | Se um CPF já estiver cadastrado, o sistema não cria outra pessoa: direciona conta ativa à recuperação, permite reativação segura da inativa ou vincula uma nova conta à pessoa que existia apenas como responsável. | Aceita em 17/08/2026 |
| `DEC-103` | CPF, e-mails, telefones, contatos de emergência e saúde terão criptografia por campo. Nome e nascimento permanecerão pesquisáveis, protegidos pela criptografia do banco, controles de acesso e ausência em logs. | Aceita em 17/08/2026 |
| `DEC-104` | Na disputa pela última vaga, a transação bloqueia temporariamente a turma ou reserva, recalcula a capacidade e aceita somente a primeira confirmação concluída; as demais seguem o fluxo de espera ou análise. | Aceita em 17/08/2026 |
| `DEC-105` | Históricos usam inativação e `ON DELETE RESTRICT`; pedidos de exclusão não apagam registros relacionados durante a retenção, e o descarte ou anonimização posterior ocorre por rotina controlada. | Aceita em 17/08/2026 |
| `DEC-106` | O primeiro administrador total será criado sem senha fixa em seed, por procedimento com e-mail confirmado, link único, definição de senha, MFA e auditoria; Heitor executa no ambiente acadêmico e a implantação real será alinhada com Prefeitura/Embrass. | Aceita em 17/08/2026 |
| `DEC-107` | O backend será um monólito modular, organizado por domínios e com portas/adaptadores, permanecendo uma única base de código e unidade de release. | Aceita em 17/08/2026 |
| `DEC-108` | Angular e Spring Boot se comunicarão por API REST/JSON documentada em OpenAPI, inicialmente sob o prefixo `/api/v1`; quebra de compatibilidade exige nova versão. | Aceita em 17/08/2026 |
| `DEC-109` | Notificações e tarefas automáticas usarão outbox transacional no MySQL e worker do mesmo produto; RabbitMQ, Kafka ou outro broker só entram se medições futuras justificarem. | Aceita em 17/08/2026 |
| `DEC-110` | Angular será publicado em `/` e a API em `/api/v1` na mesma origem lógica; se Prefeitura/Embrass exigir origens separadas, será usada lista exata de origens autorizadas. | Aceita em 17/08/2026 |
| `DEC-111` | Uma única imagem Spring Boot será gerada; API e worker executarão separadamente em produção, podendo operar juntos no ambiente de desenvolvimento/testes. | Aceita em 17/08/2026; ajustada por `DEC-115` |
| `DEC-112` | O rascunho offline da chamada usará IndexedDB criptografado, com dados mínimos, sem saúde/contatos/credenciais, e será apagado após sincronização, logout ou 24 horas; conflito não sobrescreve o servidor. | Aceita em 17/08/2026 |
| `DEC-113` | Comprovantes usarão storage privado compatível com S3 e ClamAV; Prefeitura/Embrass poderão homologar ferramentas equivalentes sem alterar quarentena, autorização, retenção ou auditoria. | Aceita em 17/08/2026 |
| `DEC-114` | Segredos usarão cofre ou mecanismo seguro da infraestrutura; logs serão estruturados em JSON e métricas/traces seguirão OpenTelemetry, com ferramentas físicas escolhidas junto de Prefeitura/Embrass. | Aceita em 17/08/2026 |
| `DEC-115` | O SIDESP terá apenas dois ambientes permanentes: desenvolvimento/testes e produção. O mesmo artefato validado será promovido após coordenação de Kauãn, validações de Heitor e Micael, aprovação de Livia e, no uso real, participação de Prefeitura/Embrass. | Aceita em 17/08/2026 |
| `DEC-116` | Java LTS, Spring Boot e Angular terão versões estáveis/compatíveis fixadas no início; pacote `com.github.heitorleite.sidesp`, backend `sidesp-backend` e frontend `sidesp-web`. | Aceita em 17/08/2026 |

## 19. Rastreabilidade

Os IDs abaixo correspondem ao catálogo oficial definido em `CASOS_DE_USO.md`. Qualquer inclusão, remoção ou renumeração deve ser atualizada simultaneamente nos dois documentos.

| Módulo | Requisitos | Regras | Casos de uso oficiais | Testes esperados | Código alvo |
| --- | --- | --- | --- | --- | --- |
| Público/notícias/mapas | `RF-PUB-*`, `RF-ADM-006` | `RN-021` | `UC-PUB-01` Consultar notícias; `UC-PUB-02` Consultar detalhe de notícia; `UC-PUB-03` Consultar polos e modalidades; `UC-ADM-11` Gerenciar notícias; `UC-AUT-05` Publicar notícia agendada | `CT-PUB-*`, `CT-ADM-NOT-*` | Pendente |
| Identidade/perfil | `RF-IDN-*`, `RF-ADM-005/007` | `RN-016/017/022` | `UC-IDN-01` Cadastrar aluno; `UC-IDN-02` Autenticar usuário; `UC-IDN-03` Recuperar acesso; `UC-IDN-04` Atualizar perfil; `UC-IDN-05` Alterar senha; `UC-IDN-06` Solicitar inativação ou exclusão; `UC-ADM-12` Gerenciar administradores; `UC-ADM-14` Atender pedido de privacidade | `CT-IDN-*`, `CT-AUT-*` | Pendente |
| Inscrição/fila/seleção | `RF-INS-*` | `RN-001`, `RN-008` a `RN-012`, `RN-018`, `RN-023` | `UC-INS-01` Consultar ofertas esportivas; `UC-INS-02` Solicitar inscrição; `UC-INS-03` Entrar na lista de espera; `UC-INS-04` Cancelar participação; `UC-INS-05` Consultar posição na espera; `UC-INS-06` Consultar turmas e aulas; `UC-INS-07` Confirmar oferta de vaga; `UC-ADM-07` Gerenciar inscrições e lista de espera; `UC-ADM-08` Gerenciar processo seletivo; `UC-ADM-13` Executar exceção de inscrição; `UC-AUT-01` Ofertar vaga da lista de espera | `CT-INS-*`, incluindo concorrência/idempotência | Pendente |
| Chamada/frequência | `RF-FRQ-*` | `RN-002`, `RN-005/006`, `RN-013/014/019` | `UC-FRQ-01` Consultar frequência; `UC-PRF-01` Consultar turmas vinculadas; `UC-PRF-02` Realizar chamada; `UC-PRF-03` Registrar diário da aula; `UC-PRF-04` Consultar alunos da turma; `UC-ADM-10` Corrigir chamada | `CT-FRQ-*`, autorização por turma | Pendente |
| Justificativa | `RF-JUS-*` | `RN-003/004/024/025` | `UC-JUS-01` Enviar justificativa; `UC-JUS-02` Anexar comprovante; `UC-JUS-03` Recorrer de decisão; `UC-ADM-09` Analisar justificativa; `UC-AUT-04` Notificar decisão de justificativa | `CT-JUS-*`, upload/privacidade | Pendente |
| Cadastros operacionais | `RF-ADM-001` a `RF-ADM-005` | `RN-008`, `RN-012/013/015/016/018` | `UC-ADM-01` Gerenciar polos; `UC-ADM-02` Gerenciar modalidades; `UC-ADM-03` Gerenciar professores; `UC-ADM-04` Gerenciar vínculos de professor; `UC-ADM-05` Gerenciar turmas; `UC-ADM-06` Gerenciar alunos | `CT-ADM-*`, inativação/histórico | Pendente |
| Comunicação | `RF-COM-*` | `RN-005` a `RN-007`, `RN-010/011/020/025` | `UC-COM-01` Cancelar ou mudar local de aula; `UC-COM-04` Gerenciar notificações internas; `UC-COM-05` Enviar e-mail transacional; `UC-AUT-01` Ofertar vaga; `UC-AUT-02` Alertar limite de faltas; `UC-AUT-03` Cancelar por excesso de faltas; `UC-AUT-04` Notificar decisão. `UC-COM-02/03` ficam para o WhatsApp futuro. | `CT-COM-*`, falha/replay/fallback | Pendente |
| Relatórios/análises | `RF-REL-*` | Regras de dados aplicáveis | `UC-REL-01` Gerar relatório; `UC-REL-02` Exportar relatório; `UC-REL-03` Visualizar mapa de calor | `CT-REL-*`, autorização/agregação/formula injection | Pendente |

Quando a implementação começar, cada PR deverá citar os IDs afetados e os testes deverão usar os padrões acima ou registrar mapeamento equivalente. O estado de requisito só muda de `Proposto` para `Parcial` ou `Atual` após evidência em código e teste aceito.

## 20. Checklist para aprovação deste documento

- [ ] Responsável de negócio e responsáveis técnicos confirmaram seus papéis.
- [ ] Prioridades foram ratificadas pelo Product Owner.
- [x] Questões bloqueadoras foram decididas ou formalmente adiadas para versão posterior.
- [x] Regras e requisitos passaram por conferência interna de coerência.
- [x] Matriz de permissões foi referenciada na seção 9.4.1.
- [ ] Inventário de dados, finalidade, base legal, retenção e descarte foram aprovados.
- [x] Critérios mensuráveis de desempenho, capacidade, disponibilidade e recuperação foram definidos.
- [ ] Casos de uso com IDs oficiais foram revisados contra as mudanças da versão `0.2.0`.
- [ ] QA validou se os critérios são testáveis.
- [ ] Segurança e privacidade registraram revisão.
- [ ] Responsável de negócio e Product Owner aprovaram a baseline.

## 21. Histórico de versões

| Versão | Data | Autor | Alterações | Situação |
| --- | --- | --- | --- | --- |
| `0.1.0` | 12/08/2026 | Heitor Leite | Primeira consolidação do Documento de Visão no padrão do Guia Mestre; inclusão de critérios de aceite, dados, erros, riscos, pendências e rastreabilidade | Rascunho |
| `0.2.0` | 17/08/2026 | Heitor Leite | Incorporação das decisões da equipe sobre regras, segurança, privacidade, notificações, capacidade, fluxos críticos, seleção humana com concorrência/capacidade, pausa oficial de oferta, primeira versão e infraestrutura Prefeitura/Embrass | Pronto para revisão |

