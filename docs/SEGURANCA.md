# Segurança — SIDESP

> **SIDESP — Sistema Integrado de Desenvolvimento Esportivo Público**  
> Política e baseline de segurança do produto completo, aplicável ao frontend, à API Java/Spring Boot, ao banco de dados, aos arquivos, às integrações, à infraestrutura e à operação.

## Identificação do documento

| Campo | Valor |
| --- | --- |
| Projeto | SIDESP — Sistema Integrado de Desenvolvimento Esportivo Público |
| Órgão demandante | Secretaria de Esportes de Guaratinguetá |
| Documentos relacionados | `LEVANTAMENTO_DE_REQUISITOS.md`, `CASOS_DE_USO.md`, `CLASSES_OU_COMPONENTES.md`, `ATIVIDADES.md`, `ARQUITETURA.md` e `../database/BANCO_DE_DADOS.md`, todos em `0.2.0` |
| Responsável de negócio / Scrum Master | Kauãn Raphael |
| Product Owner | Livia Andrade |
| Responsável técnico / Segurança / Privacidade interna | Heitor Leite |
| QA | Micael Phillipini |
| Responsáveis institucionais | Prefeitura deverá indicar controlador, encarregado, jurídico e contatos antes da implantação real |
| Versão | `0.2.0` |
| Data de elaboração e pesquisa | 17/08/2026 |
| Classificação | Interna |
| Status | **Pronto para revisão — ainda não aprovado para implementação** |
| Frequência de revisão | A cada 6 meses e sempre que ocorrer um gatilho da seção 37 |
| Próxima revisão | Até 17/02/2027, ou antes se ocorrer um gatilho da seção 37 |

## Aprovações

| Papel | Responsável | Situação | Data |
| --- | --- | --- | --- |
| Responsável de negócio / Scrum Master | Kauãn Raphael | Pendente de revisão | — |
| Product Owner | Livia Andrade | Pendente de revisão | — |
| Responsável técnico | Heitor Leite | Pendente de revisão | — |
| Segurança e privacidade interna | Heitor Leite | Pendente de revisão | — |
| Operações/Infraestrutura | Prefeitura/Embrass | Alinhamento futuro | — |
| QA | Micael Phillipini | Pendente de revisão dos testes | — |

## 1. Finalidade e escopo

Este documento define os controles mínimos obrigatórios para proteger a confidencialidade, a integridade, a disponibilidade, a autenticidade, a rastreabilidade e a privacidade no SIDESP.

Aplica-se a:

- frontend web e recursos entregues ao navegador;
- API Java/Spring Boot e processos em segundo plano;
- MySQL, sessões, outbox, worker e armazenamento privado de arquivos;
- cadastros, inscrições, chamadas, justificativas e notificações da primeira versão;
- autenticação, autorização, sessões e recuperação de conta;
- integração de e-mail; WhatsApp, mapas, relatórios e exportações são expansões futuras;
- estações e contas administrativas utilizadas para desenvolver ou operar o produto;
- repositórios, dependências, pipeline, artefatos, containers e infraestrutura;
- logs, auditoria, backup, continuidade e resposta a incidentes;
- pessoas, fornecedores, IAs e agentes automatizados que tenham acesso ao projeto.

Este documento não substitui análise jurídica, decisão institucional da Prefeitura, modelo de ameaças detalhado, teste de invasão autorizado, política corporativa ou resposta a incidentes própria. O grupo acadêmico não possui encarregado oficial da Prefeitura; Heitor Leite responde internamente por segurança e privacidade durante o projeto.

## 2. Hierarquia normativa e convenções

### 2.1 Hierarquia

1. legislação e regulamentação aplicáveis;
2. contratos, normas e políticas oficiais da Prefeitura/Secretaria;
3. decisões formalmente aprovadas de segurança e privacidade;
4. este `SEGURANCA.md`, o `AGENTS.md` da raiz e o futuro `PRIVACIDADE_E_DADOS.md`;
5. requisitos, casos de uso, arquitetura, banco e contratos de API;
6. conveniência de implementação.

Pessoa desenvolvedora, fornecedor ou IA **NÃO PODE** aprovar exceção, definir hipótese legal nem aceitar risco em nome da organização.

### 2.2 Termos

- **DEVE / OBRIGATÓRIO:** controle que não pode ser ignorado sem exceção formal.
- **NÃO DEVE / PROIBIDO:** prática vedada.
- **DEVERIA / RECOMENDADO:** controle esperado; ausência exige justificativa e revisão.
- **PODE:** alternativa permitida após avaliação.
- **Proposto:** controle previsto, ainda sem evidência de implementação.
- **Pendente:** controle que depende de responsável, valor, fornecedor ou decisão externa.
- **Bloqueador:** condição que impede desenvolvimento do fluxo afetado, integração ou produção.

### 2.3 Glossário técnico

| Termo | Significado no projeto |
| --- | --- |
| API | Meio pelo qual o frontend Angular conversa com o backend Spring Boot. |
| Autenticação | Verificação de quem está tentando entrar no sistema. |
| Autorização | Verificação do que a pessoa autenticada pode consultar ou fazer. |
| MFA | Segunda verificação além da senha. No SIDESP, o administrador informa um código enviado ao e-mail em todos os logins. |
| Sessão opaca | Código aleatório que identifica uma sessão mantida no servidor, sem carregar dados do usuário no próprio código. |
| Cookie seguro | Pequeno dado enviado pelo navegador com proteções que dificultam leitura por scripts e envio indevido. |
| CSRF | Ataque que tenta usar uma sessão já aberta para executar uma ação sem a intenção do usuário. |
| XSS | Injeção de conteúdo malicioso que pode executar no navegador de outra pessoa. |
| Rate limit | Limite de requisições em certo período para evitar abuso ou sobrecarga. |
| Hash de senha | Transformação de mão única usada para verificar a senha sem armazená-la em texto legível. |
| Salt | Valor aleatório e exclusivo somado à senha antes do hash para dificultar ataques com listas prontas. |
| Step-up | Nova confirmação de identidade exigida imediatamente antes de uma ação de maior risco. |
| Menor privilégio | Conceder a cada conta somente os acessos necessários para sua função. |
| SAST | Análise automática do código-fonte para encontrar possíveis falhas de segurança. |
| SCA | Análise das bibliotecas usadas pelo projeto e das vulnerabilidades conhecidas nelas. |
| SBOM | Lista das bibliotecas e componentes incluídos em uma versão do sistema. |
| CSP | Regra do navegador que restringe as origens de scripts e outros conteúdos da página. |
| HSTS | Regra que obriga o navegador a usar HTTPS após sua ativação segura. |
| IndexedDB | Banco local do navegador usado somente para o rascunho temporário e protegido da chamada offline. |
| Storage S3 compatível | Armazenamento privado de arquivos que segue a interface do serviço S3. |
| ClamAV | Ferramenta de verificação de arquivos contra conteúdo malicioso; pode ser substituída por equivalente homologado. |
| Outbox | Registros de eventos salvos junto da operação principal para que um worker os processe depois sem perder a tarefa. |
| Worker | Parte do backend que executa tarefas em segundo plano, como o envio de notificações. |
| OpenTelemetry | Padrão para gerar métricas e rastros de execução sem prender o projeto a uma ferramenta específica. |

### 2.4 Identificadores

Os controles usam IDs estáveis no formato `SEG-<DOMÍNIO>-<NÚMERO>`. Código, testes, PRs, auditorias e exceções deverão citá-los quando aplicável.

## 3. Objetivos e princípios

| ID | Objetivo/Princípio | Aplicação no SIDESP |
| --- | --- | --- |
| `SEG-GOV-001` | Negar por padrão | Acesso só é permitido por regra explícita no backend. |
| `SEG-GOV-002` | Menor privilégio | Usuário, serviço, pipeline e banco recebem apenas poderes necessários. |
| `SEG-GOV-003` | Defesa em profundidade | Interface, API, serviço, banco, rede, logs e operação possuem controles complementares. |
| `SEG-GOV-004` | Privacy by design/default | Coletar, mostrar, compartilhar e reter somente o mínimo aprovado. |
| `SEG-GOV-005` | Segregação de funções | Conceder permissão, aprovar exceção, operar produção e revisar evidência não devem depender de uma única pessoa quando o risco for alto. |
| `SEG-GOV-006` | Rastreabilidade | Ações críticas têm autor/processo, alvo, instante, resultado e correlação. |
| `SEG-GOV-007` | Configuração segura por padrão | Ambiente novo inicia fechado, sem dados reais, debug público ou acesso administrativo amplo. |
| `SEG-GOV-008` | Falhar com segurança | Erro não concede acesso, não confirma operação não persistida e não expõe detalhe interno. |
| `SEG-GOV-009` | Segurança verificável | Todo controle crítico possui teste, evidência ou monitoramento correspondente. |
| `SEG-GOV-010` | Reversibilidade | Segredos, acessos, deploys, fornecedores e dados devem possuir revogação, rollback ou estratégia de saída. |

## 4. Governança e responsabilidades

| Papel | Responsabilidades mínimas |
| --- | --- |
| Responsável de negócio | Aprovar escopo, riscos residuais, disponibilidade, permissões e impacto operacional; não aprova matéria jurídica sozinho. |
| Responsável de negócio / Scrum Master — Kauãn Raphael | Coordenar o trabalho, avaliar impacto operacional e apoiar a comunicação de incidentes; não aprova matéria jurídica sozinho. |
| Product Owner — Livia Andrade | Aprovar prioridades, regras de negócio e impactos do produto; apoiar decisões e comunicações que afetem os usuários. |
| Responsável técnico, segurança e privacidade interna — Heitor Leite | Manter esta baseline, arquitetura de segurança, acessos, vulnerabilidades, exceções técnicas, testes e incidentes do projeto. |
| Desenvolvimento backend | Aplicar autorização, validações, proteção de dados, auditoria, integrações e testes no servidor. |
| Desenvolvimento frontend | Proteger sessão, saída, navegação, dependências, headers e ausência de segredo no bundle. |
| QA — Micael Phillipini | Manter matriz de testes de segurança e bloquear aceite quando critério obrigatório falhar. |
| Prefeitura/Embrass | Fornecer e operar a infraestrutura homologada, incluindo hospedagem, banco, armazenamento, backup, cofre, monitoramento e suporte, conforme responsabilidades que ainda serão formalizadas. |
| Controlador, encarregado e jurídico institucionais | Definir hipóteses legais, direitos, comunicações regulatórias e demais decisões oficiais antes de eventual uso real. Esses papéis não pertencem ao grupo acadêmico. |
| Administrador funcional | Operar apenas permissões concedidas; justificar ações críticas; nunca compartilhar conta. |
| Fornecedor | Cumprir contrato, menor privilégio, segurança, incidentes, retenção, suboperadores e saída. |
| IA/agente | Seguir `AGENTS.md`, não receber segredo/dado real, não aprovar risco e submeter produção a revisão humana. |

### 4.1 Controles de governança

| ID | Controle obrigatório | Evidência esperada | Status |
| --- | --- | --- | --- |
| `SEG-GOV-011` | Heitor Leite é o responsável interno por segurança e privacidade do projeto. Controlador, encarregado, jurídico e contatos institucionais DEVEM ser indicados pela Prefeitura antes do uso de dados reais. | Registro dos responsáveis internos e, antes da implantação real, dos responsáveis institucionais. | Parcialmente definido; bloqueador apenas da implantação real |
| `SEG-GOV-012` | Mudança de arquitetura, identidade, dado sensível, fornecedor ou exposição de rede DEVE passar por revisão de segurança. | Checklist/ADR e aprovação. | Proposto |
| `SEG-GOV-013` | A equipe DEVE manter inventário de ativos, dados, dependências, contas e fornecedores. | Inventário versionado com proprietário. | Proposto |
| `SEG-GOV-014` | Risco residual e exceção DEVEM ter proprietário, prazo e aprovação competente. | Registro conforme seção 34. | Proposto |
| `SEG-GOV-015` | Acesso administrativo e de produção DEVE ser nominal; conta compartilhada é PROIBIDA. | Inventário de identidade e revisão de acesso. | Proposto |

## 5. Baseline de verificação

O SIDESP DEVE adotar como baseline:

- [OWASP ASVS 5.0.0](https://owasp.org/www-project-application-security-verification-standard/) **nível 2** para a aplicação inteira;
- controles adicionais equivalentes ao nível 3 para administração, permissões, dados de saúde, menores, comprovantes, exportações e segredos, selecionados no modelo de ameaças;
- [OWASP API Security Top 10 — edição 2023](https://owasp.org/API-Security/editions/2023/en/0x10-api-security-risks/) como catálogo mínimo de riscos de API;
- [NIST SP 800-63-4](https://pages.nist.gov/800-63-4/sp800-63.html) para decisões de identidade, autenticação e sessão;
- [NIST SSDF 1.1](https://csrc.nist.gov/pubs/sp/800/218/final) para desenvolvimento seguro;
- LGPD e regulamentações vigentes da ANPD, conforme responsabilidade do controlador e do encarregado.

A versão de cada referência DEVE ser registrada na estratégia de testes. Atualização de versão exige análise de impacto; não se presume conformidade apenas por citar o padrão.

## 6. Classificação da informação

| Classe | Exemplos no SIDESP | Controles mínimos |
| --- | --- | --- |
| Pública | Notícias publicadas; nome/endereço público de polos; modalidades; documentação aprovada | Integridade, revisão editorial, publicação controlada, proteção contra alteração indevida |
| Interna | Código não sensível; arquitetura sem detalhes exploráveis; procedimentos; backlog; dados sintéticos | Autenticação, repositório privado ou política aprovada, compartilhamento interno, revisão |
| Confidencial | CPF, nome, contato, nascimento, inscrição, frequência, lista de espera, vínculo de professor, relatórios internos, logs identificáveis | Menor privilégio, TLS, criptografia em repouso conforme arquitetura, auditoria, mascaramento, retenção e descarte |
| Restrita | Senha/hash, sessão, token, segredo, chave, comprovante, possível dado de saúde, dados de menor em contexto sensível, backup, exportação sensível, trilha de acesso privilegiado | Acesso estrito e segregado, criptografia, cofre, monitoramento, aprovação, proibição em ambientes não produtivos, descarte verificável |

### 6.1 Regras de classificação

| ID | Controle obrigatório | Evidência esperada | Status |
| --- | --- | --- | --- |
| `SEG-DAD-001` | Todo campo DEVE possuir classificação, finalidade, responsável, acesso e retenção antes de produção. O inventário inicial está no levantamento de requisitos e no modelo de dados. | Dicionário de dados aprovado e validação institucional. | Definido para o projeto; validação institucional pendente |
| `SEG-DAD-002` | Um conjunto ou arquivo herda a classe mais alta de seu conteúdo. | Classificação em catálogo e storage. | Proposto |
| `SEG-DAD-003` | Dado pseudonimizado continua pessoal enquanto houver reidentificação razoável. | Método e avaliação documentados. | Proposto |
| `SEG-DAD-004` | “Anonimizado” só PODE ser declarado após método, teste de reidentificação e aprovação. | Relatório de anonimização. | Proposto |
| `SEG-DAD-005` | Produção NÃO DEVE ser copiada para desenvolvimento/teste sem processo formal de minimização e mascaramento. | Aprovação e evidência de mascaramento. | Proposto |

## 7. Ativos e fronteiras críticas

| Ativo/fronteira | Riscos principais | Proteção mínima |
| --- | --- | --- |
| Conta e sessão | tomada de conta, enumeração, fixação, roubo, replay | hash forte, MFA administrativo, cookie seguro, rotação, revogação, rate limiting |
| API SIDESP | BOLA/IDOR, acesso vertical, mass assignment, injeção, abuso | autorização por objeto/ação, DTOs, validação, limites, auditoria |
| Dados de aluno/menor | exposição, uso excessivo, alteração indevida | minimização, segregação, trilha, retenção, direitos do titular |
| Dados de saúde/comprovantes | exposição de dado sensível, malware | acesso separado, storage privado, validação/varredura, criptografia, descarte |
| Chamadas e justificativas | fraude, correção sem rastro, cancelamento incorreto | imutabilidade lógica, justificativa, versão, eventos idempotentes |
| Lista de espera e inscrição | corrida, fraude de posição, abuso administrativo | transação, ordem auditável, autorização, idempotência, exceção formal |
| Relatórios e exportações | extração em massa, reidentificação, formula injection | autorização granular, agregação, limites, arquivo temporário, auditoria |
| WhatsApp/webhooks | vazamento, falsificação, replay, dependência | contrato, segredo em cofre, assinatura, timestamp, idempotência, fallback |
| Banco/backup | acesso público, perda, ransomware, cópia indevida | rede privada, menor privilégio, criptografia, backup segregado e restore testado |
| Pipeline/artefato | segredo, dependência maliciosa, alteração de build | proteção de branch, scans, SBOM, identidade de workload, provenance |
| Administração/operação | privilégio excessivo, conta compartilhada, abuso interno | identidade nominal, MFA, segregação, revisão periódica, auditoria |

## 8. Ameaças e casos de abuso prioritários

O futuro `MODELO_DE_AMEACAS.md` deverá aplicar metodologia formal, como STRIDE, à arquitetura aprovada. Até lá, os seguintes cenários são obrigatórios no desenho e nos testes:

| ID | Caso de abuso | Controles relacionados |
| --- | --- | --- |
| `ABU-001` | Aluno altera identificador para ler inscrição, frequência ou comprovante de terceiro. | `SEG-AUTZ-*`, `SEG-API-*` |
| `ABU-002` | Professor acessa turma/aluno sem vínculo vigente. | `SEG-AUTZ-003/004`, auditoria |
| `ABU-003` | Administrador parcial chama endpoint de administrador total. | `SEG-AUTZ-002/005`, MFA, teste vertical |
| `ABU-004` | Atacante cria contas, tenta senhas ou solicita recuperação em massa. | `SEG-IDN-*`, `SEG-API-006/007` |
| `ABU-005` | Requisição repetida ocupa duas vagas, duplica chamada ou envia mensagens múltiplas. | idempotência, transação, chave de operação |
| `ABU-006` | Arquivo comprobatório contém malware, path traversal ou conteúdo disfarçado. | `SEG-ARQ-*` |
| `ABU-007` | Exportação contém dados não autorizados ou fórmula maliciosa. | `SEG-EXP-*` |
| `ABU-008` | Filtro de mapa de calor reidentifica grupo pequeno. | agregação, limiar, privacidade |
| `ABU-009` | Webhook falso ou repetido muda estado de notificação. | assinatura, timestamp, nonce/idempotência |
| `ABU-010` | Segredo aparece em Git, bundle, imagem, log ou prompt de IA. | `SEG-SEG-*`, scans e incidente |
| `ABU-011` | Exceção administrativa contorna limite/idade/capacidade sem controle. | permissão restrita, dupla aprovação a decidir, justificativa e alerta |
| `ABU-012` | Correção de chamada remove falta e altera cancelamento sem trilha. | versão antes/depois, compensação e auditoria |
| `ABU-013` | Falha do WhatsApp ou internet causa confirmação falsa ou perda de chamada. | estado explícito, retry controlado, idempotência, operação degradada |
| `ABU-014` | Dependência ou pipeline comprometido injeta código no artefato. | SCA, assinatura/provenance, revisão, SBOM, isolamento de CI |

## 9. Identidade, autenticação e recuperação

### 9.1 Arquitetura obrigatória

O SIDESP usará sessão opaca mantida no servidor e persistida no MySQL, identificada no navegador por cookie seguro. Redis ou outro armazenamento dedicado só será avaliado se medições futuras demonstrarem necessidade. OAuth/OIDC ou JWT exigiriam nova decisão de arquitetura e revisão deste documento.

| ID | Controle obrigatório | Critério/evidência | Status |
| --- | --- | --- | --- |
| `SEG-IDN-001` | Login DEVE aceitar CPF ou e-mail sem revelar qual identificador existe. | Respostas e tempos não permitem enumeração prática. | Proposto |
| `SEG-IDN-002` | CPF NÃO DEVE ser usado como senha, token, fator de recuperação ou prova suficiente de identidade. | Testes de recuperação e cadastro. | Proposto |
| `SEG-IDN-003` | Senha DEVE ser armazenada somente com Argon2id, salt único por senha e parâmetros calibrados no ambiente do projeto. Bcrypt só poderá ser usado como alternativa mediante ADR que justifique a necessidade e defina parâmetros seguros. | Configuração, teste de desempenho e revisão periódica. | Definido |
| `SEG-IDN-004` | Senha DEVE possuir de 8 a 64 caracteres, pelo menos uma letra maiúscula e um número. Letras minúsculas e símbolos são permitidos, mas não obrigatórios. | Testes de tamanho, composição e caracteres permitidos. | Definido |
| `SEG-IDN-005` | Senha comum ou fraca DEVE gerar aviso claro, sem impedir o cadastro somente por esse motivo. A senha completa NÃO DEVE ser enviada a serviço externo. | Lista ou verificação local aprovada e teste do aviso. | Definido |
| `SEG-IDN-006` | Colagem e gerenciadores de senha DEVEM ser permitidos; troca periódica NÃO DEVE ser exigida sem evidência de comprometimento. | Teste de interface e política. | Proposto |
| `SEG-IDN-007` | Administradores DEVEM usar MFA por código enviado ao e-mail confirmado em todos os logins. O código vale 10 minutos e admite 5 tentativas; o reenvio espera 60 segundos, permite no máximo 5 envios por hora e invalida o código anterior. | Testes de expiração, tentativas, reenvio, invalidação, replay e bypass. | Definido |
| `SEG-IDN-008` | Criar ou inativar administrador, conceder ou remover permissão, executar exceção administrativa de inscrição, trocar manualmente o e-mail perdido e executar exclusão com dado sensível DEVEM exigir novamente a senha do administrador e um novo código MFA por e-mail. A confirmação vale por 10 minutos somente na sessão atual. | Testes de senha incorreta, MFA, expiração, troca de sessão e auditoria. | Definido |
| `SEG-IDN-009` | Recuperação DEVE usar link aleatório, opaco, válido por 30 minutos, de uso único e enviado exclusivamente ao e-mail confirmado. A resposta não pode revelar se a conta existe. | Token armazenado protegido, expiração, resposta genérica e replay testados. | Definido |
| `SEG-IDN-010` | Recuperação concluída DEVE invalidar o link e revogar todas as sessões. Alteração normal de senha mantém apenas a sessão atual; logout revoga somente a sessão usada. Pergunta de segurança é PROIBIDA. | Testes de reutilização, revogação e manutenção apenas da sessão permitida. | Definido |
| `SEG-IDN-011` | Conta inativa, desligada ou com papel removido DEVE perder acesso e sessões relevantes imediatamente ou dentro do SLA aprovado. | Teste de revogação e auditoria. | Proposto |
| `SEG-IDN-012` | Contas de serviço DEVEM ser distintas de pessoas, sem login interativo e com credenciais curtas/dinâmicas quando viável. | Inventário e configuração. | Proposto |

### 9.2 Limites iniciais contra abuso

Os limites combinam proteção da API com espera progressiva no login. Eles poderão ser reduzidos se testes ou incidentes mostrarem risco, mas aumentos exigem análise de capacidade e segurança. IP nunca será o único sinal para identificar abuso.

| Fluxo | Limite inicial definido | Resposta e observação |
| --- | --- | --- |
| Login | 10 requisições por minuto, combinando IP e identificador | A partir da 3ª senha inválida, aplicar esperas de 30 segundos, 1, 2 e 5 minutos, crescendo até o máximo de 15 minutos; não bloquear permanentemente a conta |
| Recuperação | 10 requisições por minuto, combinando IP e identificador | Resposta sempre genérica; novo link invalida o anterior para a mesma finalidade |
| Cadastro | 10 requisições por minuto, combinando IP e identificador disponível | Considerar redes compartilhadas dos polos e não usar biometria ou CAPTCHA invasivo sem avaliação |
| MFA por e-mail | 5 tentativas por código; 5 envios por hora e intervalo de 60 segundos | Código válido por 10 minutos; reenvio invalida o anterior e replay é impedido |

## 10. Sessão, cookies, CSRF e logout

| ID | Controle obrigatório | Critério/evidência | Status |
| --- | --- | --- | --- |
| `SEG-SES-001` | Identificador de sessão DEVE ser aleatório, imprevisível, opaco e sem dado pessoal. | Entropia/biblioteca aprovada e inspeção do cookie. | Proposto |
| `SEG-SES-002` | Cookie de sessão DEVE usar `Secure`, `HttpOnly`, `SameSite=Lax` ou `Strict`, `Path=/`, sem `Domain`, preferencialmente prefixo `__Host-`. | Teste automatizado de headers. | Proposto |
| `SEG-SES-003` | JWT, access token, refresh token e session ID NÃO DEVEM ser persistidos em `localStorage`, `sessionStorage`, IndexedDB, Cache API, URL ou cookie acessível a JavaScript. | Varredura do frontend e teste runtime. | Proposto |
| `SEG-SES-004` | Sessão DEVE rotacionar após login, recuperação, troca de senha, MFA e elevação de privilégio. | Teste de fixação/rotação. | Proposto |
| `SEG-SES-005` | Logout DEVE revogar a sessão no servidor e remover o cookie. | Reuso após logout falha. | Proposto |
| `SEG-SES-006` | Mudança de papel, desativação, recuperação e incidente DEVEM permitir revogação das sessões afetadas. | Teste e função operacional. | Proposto |
| `SEG-SES-007` | Operações mutáveis autenticadas por cookie DEVEM possuir proteção CSRF. `SameSite` sozinho não é suficiente. | Token/padrão aprovado e testes CSRF. | Proposto |
| `SEG-SES-008` | A aplicação NÃO DEVE aceitar credencial ou sessão por query string, fragmento ou corpo de log. | Testes e varredura. | Proposto |
| `SEG-SES-009` | Cada conta administrativa PODE manter somente uma sessão ativa; novo login encerra a sessão anterior daquela conta. Contas administrativas diferentes PODEM estar conectadas simultaneamente. Alunos e professores podem manter até 3 sessões; a quarta encerra a mais antiga. | Testes com contas distintas, novo login e limite por conta. | Definido |

### 10.1 Tempos definidos

| Perfil | Inatividade | Duração absoluta | Reautenticação de ação crítica |
| --- | --- | --- | --- |
| Administrador | 15 minutos | 8 horas | Nova senha e novo MFA; confirmação válida por 10 minutos |
| Professor e aluno | 30 minutos | 24 horas | Senha atual para alteração normal de senha ou e-mail |

Não haverá opção “manter conectado” na primeira versão. O rascunho offline da chamada no IndexedDB não é uma sessão nem uma credencial e segue as regras específicas da seção 15.

## 11. Autorização e segregação de funções

| ID | Controle obrigatório | Critério/evidência | Status |
| --- | --- | --- | --- |
| `SEG-AUTZ-001` | Toda operação não pública DEVE validar autorização no backend; guard, rota ou botão do frontend não é controle suficiente. | Testes por endpoint/serviço. | Proposto |
| `SEG-AUTZ-002` | A matriz DEVE definir aluno, professor, administrador parcial e administrador total. Permissões técnicas de serviço e operação DEVEM ser separadas das contas funcionais. | Matriz aprovada no levantamento de requisitos e testes por papel. | Definido |
| `SEG-AUTZ-003` | Autorização DEVE verificar ação, objeto e contexto: aluno próprio; professor vinculado à turma e vigência; administrador com permissão explícita. | Testes BOLA/IDOR e de vínculo. | Proposto |
| `SEG-AUTZ-004` | Identificador fornecido pelo cliente NÃO DEVE substituir a identidade/vínculo derivado da sessão. | Revisão de código e teste horizontal. | Proposto |
| `SEG-AUTZ-005` | Autoelevação e concessão acima do poder do concedente são PROIBIDAS. Mudança de permissão DEVE ser auditada e revogar sessão quando necessário. | Testes verticais e auditoria. | Proposto |
| `SEG-AUTZ-006` | Professor NÃO DEVE analisar justificativa nem corrigir chamada salva. | Testes de `UC-ADM-09/10`. | Proposto |
| `SEG-AUTZ-007` | Dados de saúde e comprovantes DEVEM possuir permissões separadas. Administrador total, administrador parcial com permissão compatível e professor da turma consultam a ficha de saúde; comprovantes ficam com o próprio aluno e o administrador autorizado, nunca com o professor. | Testes de campo, vínculo, objeto e logs de acesso. | Definido para o projeto; validação institucional pendente |
| `SEG-AUTZ-008` | Ações em massa, exceção de inscrição e gestão de administradores DEVEM ter permissões próprias. Relatórios, exportações e mapa de calor serão definidos apenas quando entrarem no escopo de versão futura. | Matriz e testes do escopo implementado. | Definido para a primeira versão |
| `SEG-AUTZ-009` | Jobs, filas, webhooks e consumidores DEVEM autenticar o serviço e aplicar o mesmo limite de negócio dos endpoints. | Testes de serviço e configuração. | Proposto |
| `SEG-AUTZ-010` | Acesso privilegiado DEVE ser revisado pelo menos trimestralmente e após mudança de função, desligamento ou incidente. | Relatório de revisão e revogação. | Proposto |

## 12. Segurança da API

### 12.1 Contrato e superfície

| ID | Controle obrigatório | Critério/evidência | Status |
| --- | --- | --- | --- |
| `SEG-API-001` | Toda rota DEVE constar do OpenAPI versionado ou de registro explícito de exceção; endpoints antigos DEVEM ser desativados ou protegidos. | Diferença contrato/rotas e inventário. | Proposto |
| `SEG-API-002` | API DEVE aceitar somente métodos, `Content-Type`, campos e tamanhos previstos; campos desconhecidos críticos DEVEM ser rejeitados. | Testes negativos de contrato. | Proposto |
| `SEG-API-003` | DTOs de entrada/saída DEVEM usar allowlist e impedir mass assignment de papel, dono, estado, auditoria e campos internos. | Revisão e teste de propriedade. | Proposto |
| `SEG-API-004` | Listagens DEVEM retornar 20 registros por padrão e no máximo 100 por página. Corpo JSON aceita até 1 MB, sem contar uploads; pesquisa textual aceita até 200 caracteres e operação em lote, até 100 itens. Filtros, ordenação, profundidade e tamanho da resposta também DEVEM ser limitados pelo contrato. | Testes de borda, paginação e consumo de recurso. | Definido |
| `SEG-API-005` | Erros DEVEM usar formato consistente, código seguro e correlation ID; stack trace, SQL, caminho interno e segredo NÃO DEVEM chegar ao cliente. | Testes de erro em produção. | Proposto |
| `SEG-API-006` | Requisições DEVEM ter rate limiting por usuário, credencial, IP, endpoint e risco, com `429` e `Retry-After` quando aplicável. | Teste dos valores da seção 12.2 e observabilidade. | Definido inicialmente |
| `SEG-API-007` | Fluxos sensíveis DEVEM ter proteção de negócio além do limite genérico: login, cadastro, recuperação, inscrição, fila, upload, exportação e mensagens. | Testes de automação/abuso. | Proposto |
| `SEG-API-008` | Operações repetíveis com efeito crítico DEVEM ser idempotentes e transacionais. | Testes concorrentes e de replay. | Proposto |
| `SEG-API-009` | CORS DEVE listar origens, métodos e headers exatos por ambiente; `*` com credenciais é PROIBIDO. | Testes de preflight/origem hostil. | Proposto |
| `SEG-API-010` | Documentação pública NÃO DEVE revelar segredo, URL interna, modelo sensível ou endpoint administrativo desnecessário. | Revisão do OpenAPI publicado. | Proposto |
| `SEG-API-011` | Console, Swagger interativo, debug, profiler e endpoint de administração DEVEM estar desabilitados publicamente em produção. | Varredura de superfície. | Proposto |
| `SEG-API-012` | Requisições de saída DEVEM usar allowlist de destino, DNS/redirect controlado e bloqueio a redes internas/metadados para prevenir SSRF. | Testes SSRF e configuração de egress. | Proposto |

### 12.2 Limites operacionais definidos inicialmente

| Categoria | Limite inicial | Observação |
| --- | --- | --- |
| API pública de leitura | 60 requisições/minuto/IP | Cache e CDN podem ser usados sem ignorar origem confiável |
| API autenticada comum | 100 requisições/minuto/usuário e limite complementar por IP | Ajustar somente com medição e análise de abuso |
| Login, cadastro e recuperação | 10 requisições/minuto/IP e identificador | Somar a espera progressiva e respostas sem enumeração |
| Upload | 10 requisições/minuto/usuário | Somar limites de quantidade, tipo e tamanho dos arquivos |
| Operações críticas repetíveis | Limite específico no contrato, nunca superior ao limite autenticado comum | Usar chave de idempotência ou identificador de comando |
| Exportação e envio em massa | Fora da primeira versão | Definir volume e concorrência antes da implementação futura |

Limites devem ser configuráveis por ambiente, monitorados e validados por teste de carga. Aumento exige análise de capacidade e abuso.

## 13. Validação, injeção e tratamento de saída

| ID | Controle obrigatório | Critério/evidência | Status |
| --- | --- | --- | --- |
| `SEG-VAL-001` | Toda entrada externa — parâmetro, header, cookie, claim, arquivo, webhook, banco e resposta externa — DEVE ser tratada como não confiável. | Padrão de código e testes. | Proposto |
| `SEG-VAL-002` | Validação DEVE usar allowlist, tipo forte, intervalo, formato, tamanho, cardinalidade e regra de negócio. | Testes de borda/entrada inválida. | Proposto |
| `SEG-VAL-003` | Consultas DEVEM ser parametrizadas; concatenação de SQL/JPQL com entrada é PROIBIDA. | SAST, revisão e testes de injeção. | Proposto |
| `SEG-VAL-004` | Saída HTML/atributo/URL/JavaScript DEVE ser codificada conforme contexto; HTML livre só com sanitizador aprovado. | Testes XSS e revisão. | Proposto |
| `SEG-VAL-005` | Command execution com entrada do usuário é PROIBIDA; quando inevitável, usar API segura e allowlist rígida. | Revisão e teste. | Proposto |
| `SEG-VAL-006` | XML externo DEVE ter DTD/entidades externas desabilitadas; desserialização polimórfica insegura é PROIBIDA. | Configuração e testes XXE/deserialização. | Proposto |
| `SEG-VAL-007` | Nomes/caminhos fornecidos pelo usuário NÃO DEVEM controlar caminho físico. Path traversal e sobrescrita devem ser impedidos. | Testes `../`, nomes especiais e colisão. | Proposto |
| `SEG-VAL-008` | Planilhas DEVEM neutralizar células iniciadas por `=`, `+`, `-`, `@`, tab ou retorno quando originadas de dado não confiável. | Teste de formula injection. | Proposto |

## 14. Arquivos comprobatórios e exportações

### 14.1 Upload de comprovantes

O comprovante é opcional. Uma justificativa pode possuir de 0 a 3 arquivos PDF, JPG ou PNG, cada um com até 10 MB. O arquivo permanece em quarentena até a verificação contra conteúdo malicioso e nunca fica disponível por caminho público.

| ID | Controle obrigatório | Critério/evidência | Status |
| --- | --- | --- | --- |
| `SEG-ARQ-001` | Tipos, extensões, MIME real e assinatura DEVEM corresponder a PDF, JPG ou PNG. Cada justificativa aceita até 3 arquivos de no máximo 10 MB cada. | Testes de tipo verdadeiro, quantidade, tamanho e arquivo disfarçado. | Definido |
| `SEG-ARQ-002` | Nome original NÃO DEVE definir o nome físico; usar identificador aleatório e preservar nome apenas como metadado sanitizado se necessário. | Inspeção do storage. | Proposto |
| `SEG-ARQ-003` | Arquivo DEVE ser armazenado fora da raiz pública, em storage privado, sem permissão de execução e com acesso por objeto. | Teste de URL direta e configuração. | Proposto |
| `SEG-ARQ-004` | Upload DEVE passar por validação e ClamAV antes de ficar disponível. Prefeitura/Embrass poderão homologar ferramenta equivalente sem remover quarentena, autorização ou auditoria. Indisponibilidade da ferramenta mantém o arquivo em quarentena. | Pipeline de estado `recebido → quarentena → aprovado/rejeitado`. | Definido |
| `SEG-ARQ-005` | Arquivo comprimido, macro, conteúdo ativo e formato complexo DEVEM ser recusados por padrão, salvo necessidade e controle formal. | Testes com macro/zip bomb. | Proposto |
| `SEG-ARQ-006` | Download DEVE revalidar papel, titular/objeto e finalidade; URL assinada, se usada, deve ser curta e não reutilizável além do necessário. | Testes de IDOR e expiração. | Proposto |
| `SEG-ARQ-007` | Professor NÃO DEVE acessar comprovante de justificativa. | Teste de autorização. | Proposto |
| `SEG-ARQ-008` | Arquivo rejeitado, temporário ou expirado DEVE ser descartado com rotina verificável; comprovantes seguem a retenção inicial de 1 ano após a decisão definitiva, salvo investigação ou obrigação institucional. Backups respeitam a mesma política. | Job, relatório de descarte e restore. | Definido para o projeto; validação institucional pendente |
| `SEG-ARQ-009` | Respostas NÃO DEVEM expor caminho interno, nome físico, bucket, credencial ou stack trace. | Testes de erro/download. | Proposto |

Os arquivos serão mantidos em storage privado compatível com S3. A infraestrutura poderá fornecer tecnologia equivalente, desde que conserve as mesmas garantias de acesso privado, quarentena, criptografia, retenção e auditoria.

### 14.2 Exportações

| ID | Controle obrigatório | Critério/evidência | Status |
| --- | --- | --- | --- |
| `SEG-EXP-001` | Permissão para visualizar NÃO concede automaticamente permissão para exportar. | Matriz e testes. | Proposto |
| `SEG-EXP-002` | Campos DEVEM ser selecionados no servidor por relatório e papel, sem aceitar lista arbitrária do cliente. | Teste de campo adicional. | Proposto |
| `SEG-EXP-003` | Exportação DEVE registrar ator, tipo, filtros, volume, classificação, instante e resultado, sem copiar o conteúdo para o log. | Evento de auditoria. | Proposto |
| `SEG-EXP-004` | Quando a exportação futura for implementada, o arquivo DEVE ser temporário, protegido e removido automaticamente em até 24 horas. | Teste de expiração/descarte. | Definido para versão futura |
| `SEG-EXP-005` | Relatório/mapa futuro DEVE suprimir ou agrupar grupos com menos de 3 pessoas e impedir filtros sucessivos que facilitem reidentificação. | Teste de filtros sucessivos. | Definido para versão futura; campos ainda dependem da Secretaria |
| `SEG-EXP-006` | Excel/CSV DEVE impedir formula injection; PDF DEVE escapar conteúdo e não incorporar anexo/script ativo. | Testes de arquivo malicioso. | Proposto |
| `SEG-EXP-007` | Exportações de grande volume DEVEM usar fila, limite de concorrência, timeout e notificação segura, sem manter sessão aberta indefinidamente. | Teste de carga e falha. | Proposto; volumes pendentes |

## 15. Frontend e segurança do navegador

| ID | Controle obrigatório | Critério/evidência | Status |
| --- | --- | --- | --- |
| `SEG-FE-001` | Tudo entregue ao navegador DEVE ser tratado como público. Segredo, senha, connection string, client secret ou chave privada no bundle é PROIBIDO. | Secret scan no build e inspeção de assets/source maps. | Proposto |
| `SEG-FE-002` | Arquivos Angular `environment*.ts` e equivalentes PODEM conter somente configuração pública. | Revisão do bundle. | Proposto |
| `SEG-FE-003` | CSP DEVE ser restritiva; `unsafe-eval` é PROIBIDO em produção e script inline exige nonce/hash quando inevitável. | Header e teste de violação. | Proposto |
| `SEG-FE-004` | `frame-ancestors 'none'` ou política aprovada DEVE impedir clickjacking; aplicar `X-Content-Type-Options: nosniff`. | Teste de headers. | Proposto |
| `SEG-FE-005` | Aplicar `Referrer-Policy` restritiva e `Permissions-Policy` desabilitando recursos não usados. | Teste de headers. | Proposto |
| `SEG-FE-006` | Conteúdo externo e do usuário DEVE usar binding seguro e codificação; uso de `innerHTML`/bypass de sanitização exige revisão específica. | SAST e revisão. | Proposto |
| `SEG-FE-007` | Dependência/script de terceiro DEVE ser minimizado, versionado e avaliado; SRI deve ser usado quando aplicável. | Inventário e headers/HTML. | Proposto |
| `SEG-FE-008` | Source map de produção NÃO DEVE ser público quando expuser código/detalhe interno; upload a observabilidade deve ser autenticado e privado. | Varredura do deploy. | Proposto |
| `SEG-FE-009` | Analytics NÃO DEVE receber CPF, saúde, documento, token, URL sensível ou conteúdo de formulário. | Inspeção de rede e configuração. | Proposto |
| `SEG-FE-010` | Estado de autorização negada DEVE ser tratado sem depender de ocultação de UI; resposta `401/403` não pode vazar recurso. | Testes E2E e API. | Proposto |
| `SEG-FE-011` | O rascunho offline da chamada DEVE usar IndexedDB criptografado e guardar somente identificadores mínimos, nomes necessários, presença, versão, horário e chave de idempotência. Saúde, contatos e credenciais são PROIBIDOS. | Inspeção do navegador, teste offline e teste de descarte. | Definido |
| `SEG-FE-012` | O rascunho offline DEVE ser apagado após sincronização, logout ou 24 horas. Conflito com uma chamada já salva no servidor NÃO DEVE sobrescrever a versão do servidor automaticamente. | Testes de sincronização, expiração, logout e conflito. | Definido |

### 15.1 Headers mínimos propostos para produção

- `Strict-Transport-Security` após validação completa de HTTPS e subdomínios;
- `Content-Security-Policy` definida por ambiente;
- `X-Content-Type-Options: nosniff`;
- `Referrer-Policy: no-referrer` ou valor formalmente aprovado;
- `Permissions-Policy` negando câmera, microfone, geolocalização e outros recursos não usados;
- política de framing via `frame-ancestors`; `X-Frame-Options: DENY` como compatibilidade quando aplicável;
- `Cache-Control: no-store` para respostas com dado restrito, autenticação e download sensível.

## 16. Backend Java/Spring Boot

| ID | Controle obrigatório | Critério/evidência | Status |
| --- | --- | --- | --- |
| `SEG-BE-001` | Java e Spring Boot DEVEM usar versões LTS/estáveis suportadas, fixadas na preparação do projeto e mantidas atualizadas. | Matriz de suporte, SCA e build reproduzível. | Critério definido; números serão fixados no início do código |
| `SEG-BE-002` | Spring Security DEVE ser a camada padrão de autenticação/autorização; desabilitar controle só é permitido em teste isolado. | Configuração e testes. | Proposto |
| `SEG-BE-003` | Autorização DEVE existir também na camada de serviço/domínio para regras por objeto, não somente no mapeamento da rota. | Revisão e testes unitários/integrados. | Proposto |
| `SEG-BE-004` | Bean Validation e validações de negócio DEVEM ocorrer na fronteira e antes de efeito persistente. | Testes negativos. | Proposto |
| `SEG-BE-005` | Configuração de produção NÃO DEVE incluir stack trace, página de erro detalhada, DevTools, console de banco ou profile de desenvolvimento. | Teste do artefato/profile. | Proposto |
| `SEG-BE-006` | Actuator/management DEVE expor apenas endpoints necessários, sem detalhe sensível, em rede/porta protegida e com autenticação quando aplicável. | Varredura e configuração. | Proposto |
| `SEG-BE-007` | Serialização DEVE evitar tipos polimórficos inseguros, campos internos e ciclos que exponham entidade completa; API usa DTOs. | Testes de resposta e revisão. | Proposto |
| `SEG-BE-008` | Exceções DEVEM ser traduzidas centralmente em erro seguro; segredo e dado pessoal NÃO DEVEM ser interpolados em mensagem/log. | Testes de falha. | Proposto |
| `SEG-BE-009` | Tarefas automáticas DEVEM usar lock/idempotência quando múltiplas instâncias puderem processar o mesmo evento. | Testes concorrentes. | Proposto |
| `SEG-BE-010` | Datas DEVEM preservar instante e fuso; regras temporais críticas usam relógio injetável/testável. | Testes de fuso, expiração e horário de verão. | Proposto |
| `SEG-BE-011` | Requisição de saída DEVE definir timeout de conexão/leitura, limite de resposta, retry com backoff/jitter e circuit breaker conforme risco. | Testes de falha externa. | Proposto |

## 17. Banco de dados, persistência e cache

| ID | Controle obrigatório | Critério/evidência | Status |
| --- | --- | --- | --- |
| `SEG-DB-001` | Banco NÃO DEVE ser exposto publicamente; acesso apenas por rede/identidade autorizada. | Regra de rede e teste externo. | Proposto |
| `SEG-DB-002` | Aplicação NÃO DEVE usar conta `root`, `sa`, `postgres` ou equivalente. Contas de runtime, migração, backup e administração DEVEM ser separadas. | Grants e inventário. | Proposto |
| `SEG-DB-003` | Conta da aplicação DEVE possuir menor privilégio e não alterar schema em produção. | Revisão de grants. | Proposto |
| `SEG-DB-004` | Constraints DEVEM reforçar unicidade, chaves, capacidade e estados quando possível; autorização continua obrigatória no serviço. | Migrações e testes concorrentes. | Proposto |
| `SEG-DB-005` | Consulta dinâmica DEVE ser parametrizada; nome de coluna/ordenação usa allowlist. | SAST e revisão. | Proposto |
| `SEG-DB-006` | MySQL, arquivos e backups DEVEM usar criptografia enquanto armazenados. CPF, e-mails, telefones, contatos de emergência e saúde usam criptografia por campo; CPF e e-mail também possuem hash normalizado separado para pesquisa. Nome e nascimento permanecem pesquisáveis, protegidos pelo banco, pelo acesso e pela ausência em logs. | Arquitetura, configuração e teste de recuperação. | Definido; método e chaves serão alinhados com Prefeitura/Embrass |
| `SEG-DB-007` | Migração Flyway DEVE ser versionada no padrão `V001__descricao.sql`, revisada, testada em dados sintéticos/mascarados e possuir rollback ou estratégia de avanço seguro. | Pipeline e evidência. | Definido |
| `SEG-DB-008` | Seed e fixture DEVEM ser sintéticos; CPF, telefone, saúde e comprovante reais são PROIBIDOS. | Scan e revisão. | Proposto |
| `SEG-DB-009` | Cache NÃO DEVE misturar usuários, papéis ou classificações; chaves devem incluir escopo seguro e expirar. | Testes de cache crossing. | Proposto |
| `SEG-DB-010` | Acesso administrativo ao banco DEVE ser nominal, temporário quando viável, MFA/identidade forte e auditado. | Logs e revisão de acesso. | Proposto |
| `SEG-DB-011` | Sessões e tokens expirados ou consumidos DEVEM ser removidos após 30 dias; controles detalhados de tentativas e limites, após 90 dias; eventos resumidos permanecem na auditoria por 5 anos. | Rotinas de descarte e testes. | Definido |

## 18. Criptografia, transporte e certificados

| ID | Controle obrigatório | Critério/evidência | Status |
| --- | --- | --- | --- |
| `SEG-CRP-001` | Produção DEVE usar HTTPS/TLS em todo fluxo; HTTP só é permitido em desenvolvimento local, sem dado real e apenas em loopback. | Teste TLS e redirecionamento. | Proposto |
| `SEG-CRP-002` | TLS mínimo 1.2; preferir 1.3; certificado válido e hostname verificado. Desabilitar validação é PROIBIDO. | Scanner TLS. | Proposto |
| `SEG-CRP-003` | HSTS DEVE ser habilitado após confirmar que todos os recursos e subdomínios necessários usam HTTPS. | Header e plano de ativação. | Proposto |
| `SEG-CRP-004` | Segredo ou dado sensível em URL é PROIBIDO. | Testes e logs. | Proposto |
| `SEG-CRP-005` | Algoritmo/chave próprios ou criptografia caseira são PROIBIDOS; usar bibliotecas e serviços aprovados. | Revisão de arquitetura/código. | Proposto |
| `SEG-CRP-006` | Chaves DEVEM ter proprietário, finalidade, ambiente, versão, rotação, revogação e backup/recuperação quando necessário. | Inventário de chaves. | Proposto |
| `SEG-CRP-007` | Webhook e comunicação serviço a serviço de alto risco DEVEM usar assinatura, identidade de workload ou mTLS conforme modelo de ameaças. | Testes de falsificação/replay. | Proposto |

## 19. Segredos e configurações

| ID | Controle obrigatório | Critério/evidência | Status |
| --- | --- | --- | --- |
| `SEG-SEG-001` | Senha, token, chave, connection string e certificado privado DEVEM permanecer no backend/cofre aprovado. | Inventário e scan. | Proposto |
| `SEG-SEG-002` | `.env` real, arquivo de credencial e segredo em código, commit, issue, chat, e-mail, prompt de IA ou documentação são PROIBIDOS. | `.gitignore`, secret scanning e treinamento. | Proposto |
| `SEG-SEG-003` | `.env.example` PODE conter apenas nomes, descrição e valores fictícios. | Revisão. | Proposto |
| `SEG-SEG-004` | Segredos reais DEVEM usar o cofre ou mecanismo seguro fornecido e homologado pela infraestrutura da Prefeitura/Embrass. | Registro do mecanismo escolhido e configuração do pipeline. | Solução definida; ferramenta física pendente de infraestrutura |
| `SEG-SEG-005` | Segredo DEVE ser injetado em runtime/deploy por identidade de workload, nunca embutido em imagem, camada, cache ou artefato. | Inspeção de imagem/artefato. | Proposto |
| `SEG-SEG-006` | Desenvolvimento/testes e produção DEVEM ter segredos distintos; PR ou execução de teste não recebe segredo de produção. | Configuração de ambiente. | Proposto |
| `SEG-SEG-007` | Todo segredo DEVE possuir proprietário, escopo, ambiente, criação, revisão/expiração, rotação, revogação e trilha. | Inventário. | Proposto |
| `SEG-SEG-008` | Logs de CI e aplicação DEVEM mascarar valores; segredo não deve aparecer em argumento de comando. | Teste de pipeline. | Proposto |
| `SEG-SEG-009` | Segredo DEVE ser trocado imediatamente após exposição, suspeita de vazamento ou retirada de acesso. Credenciais e tokens de integração serão revistos e trocados pelo menos a cada 90 dias; chaves de criptografia serão revistas anualmente e trocadas quando necessário. Prazo menor do fornecedor ou da Prefeitura prevalece. | Inventário com datas, alertas de vencimento e teste do procedimento de rotação. | Definido |

### 19.1 Segredo exposto

Ao detectar exposição, a equipe DEVE:

1. interromper o uso sem reproduzir o valor;
2. comunicar pelo canal de segurança;
3. revogar/rotacionar imediatamente;
4. preservar evidências e revisar escopo/logs;
5. verificar abuso;
6. remover do estado atual e, quando aprovado, do histórico;
7. registrar e tratar como incidente.

Apagar o valor do arquivo atual não encerra o incidente.

## 20. Privacidade, LGPD e dados de menores

O SIDESP tratará dados pessoais, dados de saúde, comprovantes e dados de crianças/adolescentes. A [LGPD](https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2018/lei/l13709compilado.htm) exige medidas técnicas e administrativas, finalidade, necessidade, transparência, segurança, prevenção e responsabilização. A equipe definiu controles internos e uma retenção acadêmica inicial; hipótese legal, avisos oficiais e validação da retenção caberão aos responsáveis institucionais antes de eventual uso real.

| ID | Controle obrigatório | Critério/evidência | Status |
| --- | --- | --- | --- |
| `SEG-PRI-001` | Antes de dado real, a Prefeitura DEVE indicar controlador, operador(es), encarregado e contatos, além de validar finalidades, hipóteses legais, compartilhamentos, retenção e direitos. | `PRIVACIDADE_E_DADOS.md` ou decisão institucional equivalente aprovada. | Pendente; bloqueador da implantação real |
| `SEG-PRI-002` | Coleta DEVE limitar-se ao estritamente necessário; campo “útil no futuro” sem finalidade é PROIBIDO. | Revisão de formulário e dicionário. | Proposto |
| `SEG-PRI-003` | A ficha de saúde limita-se a alergias, restrições físicas, medicamentos, deficiências/adaptações, observações médicas, tipo sanguíneo opcional e contato de emergência. O acesso segue `SEG-AUTZ-007`, e a retenção termina 1 ano após o vínculo. | Decisão de privacidade, testes de acesso e descarte. | Definido para o projeto; hipótese legal e validação institucional pendentes |
| `SEG-PRI-004` | Tratamento de crianças/adolescentes DEVE considerar o melhor interesse, usar linguagem clara e validar o responsável antes de criar a conta do menor. | Fluxo, telas, comunicação e testes. | Definido para o projeto; validação institucional pendente |
| `SEG-PRI-005` | O responsável não possui conta própria. O vínculo exige nome, CPF, e-mail e WhatsApp, confirmação do e-mail e proteção contra associação indevida. Um responsável pode se vincular a mais de um aluno. Aos 18 anos, o vínculo é desativado e segue a retenção definida. | Processo, confirmação, unicidade e testes de maioridade. | Definido |
| `SEG-PRI-006` | Avisos e telas DEVEM explicar finalidades, compartilhamentos, duração, controlador, contato e direitos em linguagem clara. | Aviso de privacidade aprovado. | Conteúdo mínimo definido; texto institucional pendente antes do uso real |
| `SEG-PRI-007` | Solicitações de inativação ou exclusão DEVEM ser tratadas por administrador total, com resposta inicial em até 15 dias corridos, autenticação proporcional e registro sem revelar terceiros. Heitor Leite analisa os casos com dados sensíveis. | Procedimento, auditoria e testes. | Definido para o projeto; prazo definitivo depende de validação institucional |
| `SEG-PRI-008` | Retenção e descarte DEVEM seguir a política inicial do levantamento de requisitos: perfil e registros principais por 5 anos; saúde e responsável por 1 ano após o marco aplicável; comprovantes por 1 ano; notificações por até 1 ano; auditoria por 5 anos; exportações por 24 horas; backups por 30 dias. | Tabela de retenção, rotinas e testes de descarte. | Definido para o projeto; validação institucional pendente |
| `SEG-PRI-009` | Relatório e mapa futuros DEVEM evitar reidentificação e uso discriminatório. Grupos com menos de 3 pessoas não podem ser exibidos; filtros sucessivos também devem ser avaliados. | Avaliação e testes. | Definido para versão futura; campos dependem da Secretaria |
| `SEG-PRI-010` | Dados reais NÃO DEVEM ser enviados a IA externa, ferramenta, plugin ou suporte sem fornecedor, finalidade, transferência, retenção e acesso aprovados. | Política `AGENTS.md` e registro de fornecedor. | Proposto |
| `SEG-PRI-011` | Necessidade de RIPD DEVE ser avaliada antes de saúde, menores, análise em escala ou alto risco. | Decisão do controlador/encarregado. | Pendente |
| `SEG-PRI-012` | Alteração de finalidade ou novo compartilhamento exige revisão antes da implementação. | Registro de mudança e aprovação. | Proposto |

## 21. Integrações e fornecedores

### 21.1 Regras gerais

| ID | Controle obrigatório | Critério/evidência | Status |
| --- | --- | --- | --- |
| `SEG-INT-001` | Fornecedor DEVE ser avaliado quanto a segurança, privacidade, país, suboperadores, retenção, incidente, SLA, custo, portabilidade e encerramento. | Questionário/contrato aprovado. | Pendente |
| `SEG-INT-002` | Integração DEVE enviar apenas campos necessários e usar credencial específica por ambiente/serviço. | Mapeamento de payload e cofre. | Proposto |
| `SEG-INT-003` | Resposta de fornecedor é não confiável e DEVE ser validada por tipo, tamanho, estado e vínculo com solicitação. | Testes de resposta malformada. | Proposto |
| `SEG-INT-004` | Falha externa NÃO DEVE corromper o estado principal; usar timeout, circuit breaker, retry limitado e idempotência. | Testes de caos/falha. | Proposto |
| `SEG-INT-005` | Mudança de fornecedor/contrato exige revisão de segurança, privacidade e arquitetura. | ADR e aprovação. | Proposto |

### 21.2 WhatsApp

| ID | Controle obrigatório | Critério/evidência | Status |
| --- | --- | --- | --- |
| `SEG-WA-001` | WhatsApp está fora da primeira versão. Antes de sua futura ativação, fornecedor oficial, contrato, templates, base legal, funcionamento obrigatório para quem cadastrar o canal, fallback e custo DEVEM ser aprovados. | Decisão formal. | Pendente apenas para versão futura |
| `SEG-WA-002` | Mensagem DEVE conter somente o mínimo; comprovante, dado de saúde, senha, token, CPF completo e detalhe desnecessário são PROIBIDOS. | Revisão de templates. | Proposto |
| `SEG-WA-003` | O número de WhatsApp do responsável é obrigatório no cadastro do menor e deverá ser verificado antes de receber mensagens pelo canal futuro. Mudança do número exige nova verificação. | Fluxo e teste. | Cadastro definido; verificação do canal depende do fornecedor futuro |
| `SEG-WA-004` | Webhook DEVE validar assinatura com comparação segura, timestamp, tolerância curta e proteção de replay. | Testes de assinatura/replay. | Proposto |
| `SEG-WA-005` | Evento e tentativa DEVEM ter IDs idempotentes; callback repetido não duplica mudança. | Testes. | Proposto |
| `SEG-WA-006` | Segredo de webhook/API fica no cofre e deve ser rotacionável sem indisponibilidade prolongada. | Runbook e teste de rotação. | Proposto |
| `SEG-WA-007` | Histórico DEVE minimizar/mascarar telefone e conteúdo conforme papel e ser descartado em até 1 ano, preservando somente o registro técnico mínimo quando necessário. | Teste e rotina de descarte. | Definido para versão futura |
| `SEG-WA-008` | Falha de entrega DEVE ficar visível e acionar fallback aprovado; “enviado ao provedor” não significa “entregue”. | Estados e alertas. | Proposto |

### 21.3 Mapas e geocodificação

| ID | Controle obrigatório | Critério/evidência | Status |
| --- | --- | --- | --- |
| `SEG-MAP-001` | Serviço de mapas NÃO DEVE receber posição/endereço de aluno ou responsável. | Inspeção de tráfego. | Proposto |
| `SEG-MAP-002` | Apenas polo público e agregados com pelo menos 3 pessoas podem ser enviados ou exibidos; filtros sucessivos não podem permitir reidentificação. | Teste de payload/filtro. | Definido para versão futura |
| `SEG-MAP-003` | Chave pública restrita por origem/API pode aparecer no cliente apenas se o fornecedor a definir como pública; segredo permanece no backend. | Configuração do fornecedor e bundle scan. | Proposto |
| `SEG-MAP-004` | Falha do mapa DEVE preservar alternativa textual/tabular. | Teste de indisponibilidade. | Proposto |

## 22. Logs, auditoria, erros e observabilidade

### 22.1 Eventos obrigatórios

| Categoria | Eventos mínimos |
| --- | --- |
| Identidade | login/logout, falha, bloqueio, recuperação, MFA, senha, sessão revogada |
| Autorização | negação relevante, concessão/revogação de permissão, tentativa de autoelevação |
| Administração | criação/inativação, mudança de regra, vínculo, notícia, processo seletivo, exceção de inscrição |
| Frequência | chamada salva, correção antes/depois, justificativa acessada/decidida |
| Dados | exportação, acesso a comprovante/saúde, ação em massa, descarte |
| Integração | mensagem, tentativa, callback, assinatura inválida, timeout, TLS/falha do fornecedor |
| Segurança/operação | rate limit, segredo detectado, scan falho, deploy, rollback, backup/restore, mudança de configuração |

### 22.2 Controles

| ID | Controle obrigatório | Critério/evidência | Status |
| --- | --- | --- | --- |
| `SEG-LOG-001` | Log DEVE ser estruturado e conter timestamp sincronizado, nível, serviço, ambiente, correlation ID e identidade técnica/usuário quando necessário. | Esquema e amostras sintéticas. | Proposto |
| `SEG-LOG-002` | Senha, token, cookie, segredo, código MFA/recuperação, conteúdo integral de comprovante e saúde desnecessária NÃO DEVEM ser registrados. | Teste de canário e scan de logs. | Proposto |
| `SEG-LOG-003` | Cliente NÃO DEVE receber stack trace, SQL, segredo ou caminho interno; detalhe fica em ambiente protegido. | Testes de erro. | Proposto |
| `SEG-LOG-004` | Auditoria crítica DEVE ser resistente a alteração/exclusão pela aplicação comum e registrar antes/depois quando aplicável. | Permissões/storage e teste. | Proposto |
| `SEG-LOG-005` | Logs DEVEM ter controle de acesso, integridade, criptografia e descarte. Logs operacionais duram 90 dias e registros de auditoria, 5 anos, salvo preservação por investigação. | Política, configuração e rotina de descarte. | Definido |
| `SEG-LOG-006` | Relógios DEVEM ser sincronizados; eventos entre serviços devem ser correlacionáveis. | Configuração e teste. | Proposto |
| `SEG-LOG-007` | Acesso e exportação de logs DEVEM ser auditados e limitados; log não é banco alternativo. | Revisão de acesso. | Proposto |
| `SEG-LOG-008` | Alertas DEVEM possuir severidade, canal e orientação de resposta. O funcionamento será verificado a cada minuto e falha crítica deverá alertar Heitor Leite em até 5 minutos. | Catálogo de alertas e teste de entrega. | Definido inicialmente; ferramenta depende da infraestrutura |

### 22.3 Alertas mínimos

- pico de falha de login, recuperação ou MFA;
- enumeração/varredura de IDs e aumento de `401`, `403` ou `429`;
- concessão de administrador, autoelevação tentada ou exceção de inscrição;
- exportação anormal por volume, horário ou frequência;
- acesso anormal a comprovante/dado sensível;
- assinatura inválida/replay de webhook;
- malware ou taxa elevada de upload rejeitado;
- falha persistente de e-mail, outbox ou worker; WhatsApp e mapas serão acrescentados quando entrarem no escopo;
- secret scanning, SAST, SCA ou container scan crítico;
- aumento de `5xx`, latência, consumo, falha de banco, espaço ou backup;
- mudança de configuração/infraestrutura fora do pipeline.

## 23. Disponibilidade, resiliência, backup e recuperação

| ID | Controle obrigatório | Critério/evidência | Status |
| --- | --- | --- | --- |
| `SEG-RES-001` | O serviço DEVE buscar 99,5% de disponibilidade mensal. São permitidas até 4 horas mensais de manutenção programada, avisada com 48 horas de antecedência e excluída do cálculo. O RPO é 24 horas e o RTO, 8 horas. | Documento de arquitetura, monitoramento e exercício de recuperação. | Definido; homologação operacional com Prefeitura/Embrass pendente |
| `SEG-RES-002` | Operações críticas DEVEM ser transacionais/idempotentes e distinguir `pendente`, `confirmada`, `falha` e `desconhecida`. | Testes de interrupção/replay. | Proposto |
| `SEG-RES-003` | Timeout, retry e circuit breaker DEVEM evitar cascata; retry não pode ser infinito. | Configuração e testes. | Proposto |
| `SEG-RES-004` | Banco e arquivos necessários DEVEM ter backup diário criptografado, segregado e protegido contra alteração, com retenção de 30 dias. | Job, inventário e evidência. | Definido |
| `SEG-RES-005` | Restore DEVE ser testado periodicamente em ambiente isolado e medir RTO; backup não testado não conta como recuperação. | Ata de exercício. | Proposto |
| `SEG-RES-006` | Chaves/segredos necessários ao restore DEVEM possuir recuperação segura e separada. | Procedimento testado. | Proposto |
| `SEG-RES-007` | Dados restaurados DEVEM manter autorização, auditoria e política de retenção; teste não usa cópia real sem aprovação. | Checklist de restore. | Proposto |
| `SEG-RES-008` | A chamada offline DEVE usar o rascunho protegido das seções 10 e 15, mostrar o estado “não sincronizado”, reenviar com idempotência e nunca sobrescrever automaticamente uma versão já salva no servidor. | Teste offline, reenvio, expiração e conflito. | Definido |
| `SEG-RES-009` | Falha de e-mail não desfaz a operação principal nem substitui a notificação interna obrigatória. Futuros WhatsApp e mapas também deverão preservar fluxos independentes. | Runbook e teste. | Definido para a primeira versão |

## 24. Infraestrutura, containers e ambientes

| ID | Controle obrigatório | Critério/evidência | Status |
| --- | --- | --- | --- |
| `SEG-INF-001` | O SIDESP terá dois ambientes permanentes: desenvolvimento/testes e produção. Eles DEVEM ter dados, acessos, redes e segredos separados; desenvolvimento/testes usa somente dados sintéticos ou formalmente mascarados. | Diagrama e configuração. | Definido |
| `SEG-INF-002` | Produção só DEVE ser acessível por identidade nominal, MFA e canal administrativo protegido; acesso direto deve ser excepcional, temporário e auditado. | IAM e logs. | Proposto |
| `SEG-INF-003` | Banco, storage privado e portas de management NÃO DEVEM estar públicos. | Scanner externo e regras de rede. | Proposto |
| `SEG-INF-004` | Firewall/security group DEVE negar por padrão e permitir apenas origem, destino, porta e protocolo necessários. | Revisão de regra. | Proposto |
| `SEG-INF-005` | Container DEVE usar imagem mínima, versão/digest fixado, usuário não root, capabilities removidas e filesystem read-only quando viável. | Scan e manifesto. | Proposto |
| `SEG-INF-006` | Imagem NÃO DEVE conter shell/ferramenta desnecessária, segredo, dado, código-fonte não requerido ou cache sensível. | Inspeção de imagem. | Proposto |
| `SEG-INF-007` | IaC DEVE ser versionada, revisada, escaneada e aplicada pelo pipeline; estado de IaC é restrito e protegido. | PR, scan e backend de estado. | Proposto |
| `SEG-INF-008` | Patching, inventário e SLA de atualização DEVEM cobrir SO, runtime, banco, imagem e serviços. | Relatório e tickets. | Proposto |
| `SEG-INF-009` | Quotas de CPU, memória, storage, conexões e custo DEVEM limitar exaustão e abuso. | Configuração e alertas. | Proposto; valores pendentes |
| `SEG-INF-010` | Execução temporária de teste ou PR DEVE usar dado sintético, segredo próprio e destruição segura; nunca recebe segredo de produção. | Pipeline e teste. | Proposto |
| `SEG-INF-011` | Console/cloud root ou proprietário não deve ser usado rotineiramente; conta de emergência fica protegida, monitorada e testada. | IAM e procedimento break-glass. | Proposto |

## 25. Dependências e cadeia de suprimentos

| ID | Controle obrigatório | Critério/evidência | Status |
| --- | --- | --- | --- |
| `SEG-SUP-001` | Dependência nova DEVE ter necessidade, origem, mantenedor, licença, saúde e vulnerabilidades avaliadas. | Registro no PR. | Proposto |
| `SEG-SUP-002` | Versões DEVEM ser fixadas/geridas de forma reproduzível; atualização automática só entra após build, teste e revisão. | Arquivo de build/lock e pipeline. | Proposto |
| `SEG-SUP-003` | SCA e licença DEVEM rodar no PR e periodicamente; vulnerabilidade crítica/alta explorável bloqueia release salvo exceção formal. | Relatório e gate. | Proposto |
| `SEG-SUP-004` | SBOM DEVEM ser gerada para artefatos de release em formato padrão aprovado. | SBOM anexada ao release. | Proposto |
| `SEG-SUP-005` | Artefato/imagem DEVE possuir checksum e, quando viável, assinatura/provenance verificável. | Evidência do release. | Proposto |
| `SEG-SUP-006` | Plugins, Actions, imagens, MCP servers e ferramentas de IA também são dependências e DEVEM ser fixados e avaliados. | Inventário. | Proposto |
| `SEG-SUP-007` | Repositório/pacote com nome semelhante, origem nova ou install script exige revisão reforçada contra typosquatting. | Checklist do PR. | Proposto |
| `SEG-SUP-008` | Dependência sem manutenção, vulnerável ou desnecessária DEVE ser removida/substituída com prazo. | Plano de correção. | Proposto |

### 25.1 Prazos iniciais para vulnerabilidades

| Severidade | Ação inicial | Prazo máximo para corrigir ou mitigar |
| --- | --- | --- |
| Crítica explorada/exposta | Conter imediatamente; pode suspender serviço/release | 24 horas |
| Crítica não explorada | Bloquear release e priorizar | 72 horas |
| Alta | Plano imediato e bloqueio quando alcançável/explorável | 15 dias |
| Média | Tratar no ciclo priorizado | 60 dias |
| Baixa | Registrar e planejar | 120 dias |

Estes são os prazos iniciais do projeto. Exceção exige risco formal, responsável e vencimento; indisponibilidade de correção exige proteção compensatória e monitoramento.

## 26. Desenvolvimento seguro e revisão de código

| ID | Controle obrigatório | Critério/evidência | Status |
| --- | --- | --- | --- |
| `SEG-DEV-001` | Branch principal DEVE ser protegida; merge exige PR e ao menos uma revisão humana independente. Mudança crítica exige revisão do responsável técnico/segurança. | Regra do repositório. | Proposto |
| `SEG-DEV-002` | Autor não deve ser único aprovador de mudança de autenticação, autorização, criptografia, upload, exportação, permissão ou pipeline. | Histórico de revisão. | Proposto |
| `SEG-DEV-003` | Código DEVE passar por formatação, testes, SAST, SCA, secret scan e build antes do merge. | Checks obrigatórios. | Proposto |
| `SEG-DEV-004` | Exemplo, teste e documentação DEVEM usar dados sintéticos e segredos fictícios. | Scan/revisão. | Proposto |
| `SEG-DEV-005` | TODO de segurança não pode ser escondido; vira issue com risco, responsável e prazo e pode bloquear release. | Issue vinculada. | Proposto |
| `SEG-DEV-006` | IA PODE auxiliar código/documentação, mas saída exige revisão, teste e atribuição humana; dados/segredos proibidos não entram no prompt. | `AGENTS.md` e PR. | Proposto |
| `SEG-DEV-007` | Mudança de controle crítico DEVE atualizar documentos e testes no mesmo PR. | Diff e rastreabilidade. | Proposto |
| `SEG-DEV-008` | Teste ofensivo só pode ocorrer com autorização, escopo, janela e ambiente aprovados. Produção não é alvo implícito. | Registro de autorização. | Proposto |
| `SEG-DEV-009` | Antes da produção, Micael Phillipini, Heitor Leite e pelo menos uma pessoa que não desenvolveu o trecho analisado DEVEM revisar os controles e as evidências de segurança. Teste de invasão formal só ocorre com autorização e quando exigido pela Prefeitura/Embrass. | Registro dos revisores, checklist e resultados. | Definido |

## 27. CI/CD e gates

### 27.1 Pipeline mínimo

1. validação de formatação e lint;
2. testes unitários;
3. testes de integração e contrato;
4. SAST;
5. SCA e licença;
6. secret scanning, inclusive histórico/diff quando aplicável;
7. build reproduzível;
8. scan de container e IaC;
9. geração de SBOM;
10. testes de segurança automatizados e DAST em ambiente autorizado;
11. assinatura/provenance e armazenamento imutável do artefato;
12. promoção do mesmo artefato entre ambientes;
13. aprovação para produção;
14. migração, smoke test, observabilidade e rollback verificado.

| ID | Controle obrigatório | Critério/evidência | Status |
| --- | --- | --- | --- |
| `SEG-CICD-001` | Deploy manual que ignore cofre, testes, aprovação ou artefato promovido é PROIBIDO. | Permissões/pipeline. | Proposto |
| `SEG-CICD-002` | Pipeline DEVE usar identidade própria e menor privilégio; credencial pessoal de desenvolvedor é PROIBIDA em deploy. | IAM. | Proposto |
| `SEG-CICD-003` | PR não confiável NÃO DEVE executar com segredo nem permissão de escrita no ambiente. | Configuração e teste. | Proposto |
| `SEG-CICD-004` | Artefato aprovado NÃO DEVE ser recompilado para produção; configuração/segredo entram por ambiente. | Hash/provenance. | Proposto |
| `SEG-CICD-005` | Falha de gate crítico bloqueia merge/release; bypass exige exceção registrada e visível. | Regras do pipeline. | Proposto |
| `SEG-CICD-006` | Rollback DEVE considerar código, configuração e migração; retorno inseguro ou incompatível é proibido. | Teste e runbook. | Proposto |

## 28. Testes mínimos de segurança

### 28.1 Identidade e sessão

- login válido/inválido, usuário inativo e enumeração;
- política/hash de senha e senha comprometida;
- rate limiting por conta, IP e sinais combinados;
- MFA administrativo, replay, recuperação e bypass;
- token de recuperação expirado, reutilizado e concorrente;
- fixação, rotação, expiração por inatividade/absoluta, logout e revogação;
- CSRF em cada operação mutável baseada em cookie;
- ausência de token em storage, URL, log, analytics e bundle;
- step-up para ações críticas.

### 28.2 Autorização

- aluno A acessando dados/arquivo/inscrição do aluno B;
- professor acessando turma/aluno não vinculado e vínculo expirado;
- professor analisando justificativa ou corrigindo chamada;
- administrador parcial chamando caso total;
- autoelevação e concessão acima do poder;
- exportação, mapa, ação em massa e exceção sem permissão;
- endpoint oculto, job, fila, storage e webhook;
- campo sensível e mass assignment, não apenas rota.

### 28.3 API, entrada e arquivos

- SQL/JPQL injection, XSS, command injection, SSRF, XXE e desserialização;
- campos desconhecidos, tipos, tamanhos, profundidade, paginação e filtros excessivos;
- IDOR/BOLA/BFLA, concorrência e idempotência;
- upload com extensão falsa, MIME inconsistente, malware, arquivo grande, macro, zip bomb e traversal;
- download direto, URL expirada e professor acessando comprovante;
- CSV/XLSX formula injection e PDF com conteúdo ativo;
- webhook com assinatura inválida, timestamp antigo e replay;
- timeout, retry, resposta externa gigante/malformada.

### 28.4 Frontend e operação

- CSP, framing, nosniff, referrer, permissions policy e HSTS;
- CORS com origem hostil e credenciais;
- source maps, console/debug, actuator e Swagger expostos;
- segredo/dado real em bundle, imagem, artefato, log e SBOM;
- dependência vulnerável/licença, imagem root e IaC pública;
- backup/restore, perda de fornecedor, falha de internet e rollback;
- alertas, retenção, relógio, auditoria imutável e resposta a incidente.

### 28.5 Evidência e aceite

| Severidade do achado | Gate |
| --- | --- |
| Crítica | Bloqueia merge/release/produção até correção ou exceção formal do nível competente |
| Alta | Bloqueia produção quando explorável/alcançável; caso contrário exige plano e aprovação |
| Média | Exige registro, proprietário e prazo antes do release |
| Baixa | Registra no backlog e revisa na próxima avaliação |

QA manterá matriz `controle SEG-* × requisito RF/RNF × caso UC × teste CT × evidência`.

## 29. Gestão de vulnerabilidades

| ID | Controle obrigatório | Critério/evidência | Status |
| --- | --- | --- | --- |
| `SEG-VUL-001` | Vulnerabilidade DEVE ser relatada pelo recurso privado de segurança do GitHub ou diretamente a Heitor Leite pelo canal interno da equipe. Issue pública NÃO DEVE conter vulnerabilidade ainda não corrigida nem evidência sensível. Antes da implantação real, a Prefeitura indicará o contato institucional. | Recurso privado habilitado, orientação aos colaboradores e contato institucional antes da produção. | Definido para o projeto; contato institucional pendente |
| `SEG-VUL-002` | Achado DEVE ser triado por impacto real, alcance, exploração e dado afetado, sem reduzir risco só pelo score automático. | Ticket confidencial. | Proposto |
| `SEG-VUL-003` | Evidência com dado/segredo DEVE ter acesso restrito e nunca ir a issue pública. | Processo. | Proposto |
| `SEG-VUL-004` | Correção DEVE incluir teste de regressão e busca por variantes. | PR/testes. | Proposto |
| `SEG-VUL-005` | Vulnerabilidade explorada ou com dado pessoal afetado DEVE ser avaliada como incidente. | Registro de decisão. | Proposto |
| `SEG-VUL-006` | Exceção de vulnerabilidade DEVE expirar e ser reavaliada; “sem patch” não é aceite permanente. | Registro de exceção. | Proposto |

## 30. Resposta a incidentes

Um `RESPOSTA_A_INCIDENTES.md` e um runbook operacional deverão detalhar contatos e procedimentos. Até lá, aplica-se o processo mínimo abaixo.

### 30.1 Severidade inicial

| Nível | Exemplo no SIDESP | Resposta inicial definida |
| --- | --- | --- |
| `SEV-1 Crítica` | credencial/chave de produção exposta; acesso não autorizado a saúde/comprovantes; alteração ampla; indisponibilidade crítica; exploração ativa | Mobilização imediata, contenção prioritária e comando de incidente |
| `SEV-2 Alta` | acesso indevido limitado, perda relevante de integridade, exportação anormal, malware contido | Resposta em até 1 hora e análise de impacto |
| `SEV-3 Média` | tentativa recorrente bloqueada, vulnerabilidade sem exploração conhecida, falha parcial de fornecedor | Resposta no mesmo dia útil |
| `SEV-4 Baixa` | evento sem impacto confirmado, melhoria de controle | Triagem no fluxo normal |

Os tempos acima são a baseline inicial do projeto. A cobertura real e os canais de acionamento serão formalizados com Prefeitura/Embrass antes da produção.

### 30.2 Fluxo obrigatório

1. detectar e registrar horário/fonte sem apagar evidência;
2. acionar canal e responsável de incidente;
3. classificar severidade e dados/sistemas afetados;
4. preservar logs, artefatos, snapshots e cadeia de custódia;
5. conter com menor impacto: revogar sessão/segredo, bloquear rota/conta, isolar workload;
6. investigar causa, alcance, período, titulares e fornecedores;
7. erradicar e corrigir, incluindo variantes;
8. recuperar por artefato confiável, validar integridade e monitorar recorrência;
9. controlador, encarregado e jurídico avaliam comunicação regulatória/titulares;
10. documentar lições, atualizar riscos, testes, arquitetura e este documento.

### 30.3 Incidente com dados pessoais

Incidente pode envolver acesso, alteração, perda, destruição ou indisponibilidade. Quando houver risco ou dano relevante, a decisão de comunicação pertence ao controlador com apoio do encarregado/jurídico. Conforme a [orientação vigente da ANPD](https://www.gov.br/anpd/pt-br/canais_atendimento/agente-de-tratamento/comunicado-de-incidente-de-seguranca-cis), a comunicação aplicável à ANPD e aos titulares deve observar o prazo de **três dias úteis**, ressalvada legislação específica. Informação incompleta pode exigir comunicação preliminar e complementar nos termos regulamentares.

IA, pessoa desenvolvedora ou fornecedor NÃO PODE decidir sozinho que a comunicação não é necessária.

### 30.4 Responsáveis e contatos

| Função | Nome | Canal primário | Alternativo |
| --- | --- | --- | --- |
| Comando técnico e segurança | Heitor Leite | Canal interno da equipe | Canal ainda será registrado antes da produção |
| Impacto operacional e coordenação | Kauãn Raphael | Canal interno da equipe | Canal ainda será registrado antes da produção |
| Produto e comunicação com usuários | Livia Andrade | Canal interno da equipe | Canal ainda será registrado antes da produção |
| QA e preservação das evidências de teste | Micael Phillipini | Canal interno da equipe | Canal ainda será registrado antes da produção |
| Operações de infraestrutura | Prefeitura/Embrass | Pendente de formalização | Pendente de formalização |
| Controlador, encarregado, jurídico e comunicação oficial | A indicar pela Prefeitura | Pendente de formalização | Pendente de formalização |

Os responsáveis internos permitem organizar o projeto acadêmico. A ausência dos nomes e canais institucionais continua bloqueadora somente da implantação com dados reais.

## 31. Acesso de suporte e administração

| ID | Controle obrigatório | Critério/evidência | Status |
| --- | --- | --- | --- |
| `SEG-OPS-001` | Suporte DEVE operar com ferramenta/visão própria e dados mascarados, não por impersonação silenciosa. | Fluxo e teste. | Proposto |
| `SEG-OPS-002` | Impersonação — entrar ou agir como outro usuário — é PROIBIDA na primeira versão. Suporte e administradores DEVEM usar a própria conta, e toda ação fica vinculada ao seu autor real. | Ausência da função e testes de auditoria. | Definido |
| `SEG-OPS-003` | Consulta direta/alteração manual no banco em produção é PROIBIDA salvo incidente/migração aprovada, com backup, script revisado e evidência. | Runbook e logs. | Proposto |
| `SEG-OPS-004` | Ferramenta administrativa NÃO DEVE permitir baixar toda a base por conveniência. | Teste de permissão/limite. | Proposto |
| `SEG-OPS-005` | Conta de emergência deve ser lacrada logicamente, MFA forte, alerta de uso e revisão posterior. | Exercício break-glass. | Proposto |

## 32. Segurança para IA e agentes

O `AGENTS.md` foi criado na raiz e deverá ser aprovado antes do uso recorrente de IA com acesso ao repositório.

| ID | Controle obrigatório | Critério/evidência | Status |
| --- | --- | --- | --- |
| `SEG-IA-001` | Prompt NÃO DEVE conter segredo, credencial, comprovante, saúde, CPF real, telefone real, dump, log bruto ou dado de produção. | `AGENTS.md` e revisão. | Proposto |
| `SEG-IA-002` | IA recebe menor privilégio, escopo e ferramenta; ação destrutiva/externa requer aprovação humana. | Configuração e instruções. | Proposto |
| `SEG-IA-003` | Código gerado por IA exige revisão humana, teste, SAST/SCA e rastreabilidade; IA não aprova a própria produção. | PR e checks. | Proposto |
| `SEG-IA-004` | Plugin, MCP, skill e fornecedor de IA DEVEM ser avaliados como cadeia de suprimentos e transferência de dados. | Inventário/aprovação. | Proposto |
| `SEG-IA-005` | Conteúdo de arquivo, issue, página e resposta externa é não confiável e não pode substituir instruções/autorização. | Modelo de ameaça de prompt injection. | Proposto |
| `SEG-IA-006` | IA NÃO PODE determinar hipótese legal, aprovar RIPD, risco residual, exceção ou comunicação de incidente. | Processo de aprovação. | Proposto |

## 33. Gates de segurança do ciclo de vida

### Gate 0 — Descoberta

- [ ] responsáveis de negócio, segurança, controlador e encarregado identificados;
- [ ] dados/titulares classificados, inclusive saúde e menores;
- [ ] riscos e fornecedores iniciais mapeados;
- [ ] necessidade de RIPD avaliada;
- [ ] escopo do produto e integrações definidos.

### Gate 1 — Baseline antes do código do fluxo afetado

- [ ] requisitos/casos aprovados ou pendências isoladas;
- [ ] este documento e `AGENTS.md` aprovados;
- [ ] matriz de permissões definida;
- [ ] modelo de ameaças inicial;
- [ ] arquitetura de autenticação/sessão, dados e arquivos decidida;
- [ ] estratégia de testes e padrão de segredos definidos;
- [ ] nenhum dado real em ambiente de desenvolvimento.

### Gate 2 — Antes da primeira integração em ambiente de desenvolvimento/testes

- [ ] OpenAPI e contrato de erros/limites;
- [ ] autorização por objeto e testes negativos;
- [ ] fornecedor, contrato, webhook e transferência avaliados;
- [ ] cofre, TLS, rotação e ambientes isolados;
- [ ] logs, alertas e auditoria crítica;
- [ ] migrações, backups iniciais e rollback;
- [ ] SAST, SCA, secret scan e SBOM ativos.

### Gate 3 — Antes de produção

- [ ] ASVS 5.0 nível 2 verificado e controles reforçados selecionados;
- [ ] revisão de segurança por Micael, Heitor e uma pessoa que não desenvolveu o trecho; teste de invasão formal quando autorizado e exigido pela Prefeitura/Embrass;
- [ ] nenhuma vulnerabilidade crítica e alta sem tratamento/aprovação;
- [ ] privacidade, retenção, direitos e RIPD decididos;
- [ ] restore e rollback testados; RPO/RTO aprovados;
- [ ] runbook, incidentes, contatos e comunicação testados;
- [ ] acessos revisados, MFA administrativo e cofre operantes;
- [ ] fornecedores e operação degradada aprovados;
- [ ] aceite formal de negócio, segurança, privacidade e operações.

### Gate 4 — Operação

- [ ] alertas e vulnerabilidades triados;
- [ ] acessos administrativos revistos trimestralmente;
- [ ] restore, incidente e rotação exercitados;
- [ ] dependências, SBOM e patches atualizados;
- [ ] documentos e modelo de ameaças revisados após mudanças/incidentes;
- [ ] fornecedores e transferências reavaliados periodicamente.

## 34. Processo de exceção e aceite de risco

Exceção é temporária e não transforma prática insegura em padrão.

### 34.1 Conteúdo obrigatório

| Campo | Conteúdo |
| --- | --- |
| ID e controle | Ex.: `EXC-2026-001`, `SEG-SES-007` |
| Escopo | Serviço, ambiente, rota, dados e usuários afetados |
| Justificativa | Motivo técnico/operacional verificável |
| Risco | ameaça, probabilidade, impacto e dado afetado |
| Alternativas | opções consideradas e por que não são viáveis agora |
| Compensações | controles temporários, monitoramento e limitação |
| Proprietário | pessoa responsável por corrigir/acompanhar |
| Aprovações | negócio, segurança, privacidade/jurídico conforme impacto |
| Prazo | início e expiração obrigatória |
| Plano de saída | tarefa, marco e evidência de encerramento |

### 34.2 Regras

- Pessoa desenvolvedora ou IA não aprova a própria exceção.
- Exceção que afete dado pessoal/sensível exige privacidade/controlador.
- Exceção crítica não pode ser renovada indefinidamente.
- Expiração sem renovação formal bloqueia o fluxo/release afetado.
- Aceite de risco não pode contrariar lei, regulamento ou contrato.

## 35. Matriz de rastreabilidade

### 35.1 Requisitos não funcionais de segurança

| Requisito do levantamento | Controles de detalhamento neste documento |
| --- | --- |
| `RNF-SEG-001` — autenticação e autorização no backend | `SEG-IDN-*`, `SEG-AUTZ-*`, `SEG-BE-002/003` |
| `RNF-SEG-002` — HTTPS para credenciais e dados | `SEG-CRP-001/002/003/004`, `SEG-SES-002` |
| `RNF-SEG-003` — hash de senha e ausência de segredo | `SEG-IDN-003/004/005/006`, `SEG-SEG-*`, `SEG-LOG-002` |
| `RNF-SEG-004` — ciclo de vida da sessão | `SEG-SES-*`, `SEG-IDN-011` |
| `RNF-SEG-005` — limites contra abuso | `SEG-IDN-001/009`, seção 9.2, `SEG-API-006/007` e seção 12.2 |
| `RNF-SEG-006` — upload seguro | `SEG-ARQ-*`, `SEG-VAL-007`, `SEG-API-004/007` |
| `RNF-SEG-007` — auditoria de ações críticas | `SEG-LOG-*`, seção 22.1, `SEG-EXP-003` |
| `RNF-SEG-008` — limitação de requisições e resposta HTTP 429 | `SEG-API-006/007`, seção 12.2, registro de rate limit da seção 22.1 e alertas da seção 22.3 |
| `RNF-PRI-001/002/003` — dados, saúde e agregação | `SEG-DAD-*`, `SEG-PRI-*`, `SEG-EXP-005`, `SEG-MAP-*` |
| `RNF-OBS-001` — logs e observabilidade | `SEG-LOG-*`, seção 22.3 |
| `RNF-EXP-001` — exportação protegida | `SEG-EXP-*`, `SEG-VAL-008` |

### 35.2 Módulos e casos de uso

| Área do SIDESP | Requisitos/casos principais | Controles principais |
| --- | --- | --- |
| Cadastro, login e recuperação | `RF-IDN-*`; `UC-IDN-*` | `SEG-IDN-*`, `SEG-SES-*`, `SEG-API-006/007`, `SEG-LOG-*` |
| Perfil e dados pessoais | `RF-IDN-004`; `UC-IDN-04/05` | `SEG-AUTZ-*`, `SEG-PRI-*`, `SEG-DAD-*` |
| Inscrição e lista de espera | `RF-INS-*`; `UC-INS-*`, `UC-AUT-01` | `SEG-AUTZ-*`, `SEG-API-008`, `SEG-RES-002`, auditoria |
| Chamada e frequência | `RF-FRQ-*`; `UC-PRF-*`, `UC-FRQ-01`, `UC-ADM-10` | `SEG-AUTZ-003/006`, `SEG-BE-003/009/010`, `SEG-LOG-*`, `SEG-RES-008` |
| Justificativa/comprovante | `RF-JUS-*`; `UC-JUS-*`, `UC-ADM-09`, `UC-AUT-04` | `SEG-ARQ-*`, `SEG-AUTZ-006/007`, `SEG-PRI-*` |
| Administração | `RF-ADM-*`; `UC-ADM-*` | `SEG-IDN-007/008`, `SEG-AUTZ-002/005/008/010`, `SEG-OPS-*` |
| Notícias | `RF-PUB-*`, `RF-ADM-006`; `UC-PUB-*`, `UC-ADM-11`, `UC-AUT-05` | `SEG-FE-*`, `SEG-VAL-004`, auditoria editorial |
| WhatsApp | `RF-COM-*`; `UC-COM-*`, `UC-AUT-01/02/04` | `SEG-WA-*`, `SEG-INT-*`, `SEG-SEG-*` |
| Relatórios/mapa | `RF-REL-*`; `UC-REL-*` | `SEG-EXP-*`, `SEG-PRI-009`, `SEG-MAP-*`, autorização granular |
| Infraestrutura e operação | RNFs de disponibilidade/observabilidade | `SEG-INF-*`, `SEG-CICD-*`, `SEG-RES-*`, `SEG-VUL-*` |

## 36. Pendências de segurança

As decisões internas necessárias para revisar esta versão estão encerradas. Permanecem somente dependências da implantação real e de módulos que estão fora da primeira versão.

| ID | Pendência | Bloqueia |
| --- | --- | --- |
| `PSEG-010` | Prefeitura/Embrass devem formalizar infraestrutura, cofre, ferramentas físicas, responsabilidades, canais e contatos institucionais. | Implantação real e uso de dados reais |
| `PSEG-011` | Responsáveis institucionais devem validar hipótese legal, avisos, RIPD quando aplicável e política de retenção. | Implantação real e uso de dados reais |
| `PSEG-012` | WhatsApp, mapas, relatórios e exportações exigem decisões e avaliações específicas quando entrarem em versão futura. | Somente os módulos futuros correspondentes |

## 37. Revisão e gatilhos

Este documento será revisado no mínimo a cada seis meses e também após:

- incidente, vulnerabilidade crítica ou segredo exposto;
- mudança relevante na LGPD, ANPD, política ou contrato;
- entrada de saúde, menor, novo dado sensível ou novo uso analítico;
- mudança de login, sessão, MFA, autorização ou recuperação;
- nova integração, fornecedor, país, cloud, IA, plugin ou subprocessador;
- upload, exportação, webhook, fila, cache ou exposição de rede;
- mudança de banco, infraestrutura, pipeline, container ou cofre;
- alteração relevante nas referências OWASP/NIST;
- falha de restore, rollback, pentest ou controle crítico.

Toda revisão deve registrar autor, data, fontes, controles alterados, riscos e aprovações.

## 38. Checklist de aprovação

- [ ] Identificação, responsáveis e contatos estão completos.
- [ ] Controlador e encarregado aprovaram dados, menores, saúde e retenção.
- [ ] Matriz de permissões e segregação foi aprovada.
- [ ] Arquitetura de identidade, sessão, segredos e criptografia foi decidida.
- [ ] Modelo de ameaças cobriu todos os ativos e casos de abuso.
- [ ] Controles críticos possuem testes e responsáveis.
- [ ] Limites de API, arquivos, exportação e sessão foram medidos/aprovados.
- [ ] Fornecedores foram avaliados e possuem plano de saída/incidente.
- [ ] Pipeline e ambientes aplicam os gates previstos.
- [ ] Backup/restore, rollback e resposta a incidente foram exercitados.
- [ ] Exceções estão registradas, aprovadas e dentro do prazo.
- [ ] Negócio, segurança, privacidade, técnico, QA e operações aprovaram sua competência.

## 39. Referências

Referências verificadas em 12/08/2026:

- [Lei nº 13.709/2018 — LGPD, texto oficial](https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2018/lei/l13709compilado.htm)
- [ANPD — Comunicação de Incidente de Segurança](https://www.gov.br/anpd/pt-br/canais_atendimento/agente-de-tratamento/comunicado-de-incidente-de-seguranca-cis)
- [OWASP ASVS 5.0.0](https://owasp.org/www-project-application-security-verification-standard/)
- [OWASP API Security Top 10 — 2023](https://owasp.org/API-Security/editions/2023/en/0x10-api-security-risks/)
- [OWASP File Upload Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/File_Upload_Cheat_Sheet.html)
- [OWASP Session Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html)
- [NIST SP 800-63-4 — Digital Identity Guidelines](https://pages.nist.gov/800-63-4/sp800-63.html)
- [NIST SP 800-218 — Secure Software Development Framework 1.1](https://csrc.nist.gov/pubs/sp/800/218/final)

## 40. Histórico de versões

| Versão | Data | Autor | Alterações | Situação |
| --- | --- | --- | --- | --- |
| `0.1.0` | 12/08/2026 | Heitor Leite | Primeira baseline de segurança cobrindo governança, dados, identidade, sessão, autorização, API, arquivos, frontend, backend, banco, segredos, privacidade, fornecedores, logs, resiliência, infraestrutura, supply chain, CI/CD, testes, vulnerabilidades, incidentes e exceções | Rascunho |
| `0.2.0` | 17/08/2026 | Heitor Leite | Alinhamento com requisitos e arquitetura aprovados: responsáveis, Angular/Spring Boot/MySQL, identidade, MFA, sessões, API, uploads, dados, retenção, observabilidade, backup, dois ambientes, vulnerabilidades, incidentes, rotação de segredos e revisão antes da produção | Pronto para revisão |
