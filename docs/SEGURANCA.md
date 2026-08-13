# Segurança — SIDESP

> **SIDESP — Sistema Integrado de Desenvolvimento Esportivo Público**  
> Política e baseline de segurança do produto completo, aplicável ao frontend, à API Java/Spring Boot, ao banco de dados, aos arquivos, às integrações, à infraestrutura e à operação.

## Identificação do documento

| Campo | Valor |
| --- | --- |
| Projeto | SIDESP — Sistema Integrado de Desenvolvimento Esportivo Público |
| Órgão demandante | Secretaria de Esportes de Guaratinguetá |
| Documentos relacionados | `LEVANTAMENTO_DE_REQUISITOS.md` `0.1.0`; `CASOS_DE_USO.md` `0.1.0` |
| Responsável de negócio | Secretaria de Esportes — representante nominal pendente |
| Responsável técnico | Heitor Leite — Tech Lead |
| Responsável por segurança | **Pendente de designação** |
| Controlador dos dados | **Pendente de confirmação formal pela Prefeitura/Secretaria** |
| Encarregado/Privacidade | **Pendente de identificação e contato** |
| Versão | `0.1.0` |
| Data de elaboração e pesquisa | 12/08/2026 |
| Classificação | Interna |
| Status | Rascunho — não aprovado |
| Frequência de revisão | A cada 6 meses e sempre que ocorrer um gatilho da seção 37 |
| Próxima revisão | Após definição de arquitetura, controlador, encarregado, matriz de permissões e fornecedores |

## Aprovações

| Papel | Responsável | Situação | Data |
| --- | --- | --- | --- |
| Responsável de negócio | Pendente | Não aprovado | — |
| Responsável técnico | Heitor Leite | Pendente de revisão | — |
| Segurança | Pendente | Não avaliado | — |
| Privacidade/Encarregado | Pendente | Não avaliado | — |
| Operações/Infraestrutura | Pendente | Não avaliado | — |
| QA | Micael Phillipini | Pendente de revisão dos testes | — |

## 1. Finalidade e escopo

Este documento define os controles mínimos obrigatórios para proteger a confidencialidade, a integridade, a disponibilidade, a autenticidade, a rastreabilidade e a privacidade no SIDESP.

Aplica-se a:

- frontend web e recursos entregues ao navegador;
- API Java/Spring Boot e processos em segundo plano;
- banco de dados, cache, filas e armazenamento de arquivos, quando adotados;
- cadastros, inscrições, chamadas, justificativas, relatórios e mapas;
- autenticação, autorização, sessões e recuperação de conta;
- integrações com WhatsApp, mapas, e-mail e futuros fornecedores;
- estações e contas administrativas utilizadas para desenvolver ou operar o produto;
- repositórios, dependências, pipeline, artefatos, containers e infraestrutura;
- logs, auditoria, backup, continuidade e resposta a incidentes;
- pessoas, fornecedores, IAs e agentes automatizados que tenham acesso ao projeto.

Este documento não substitui análise jurídica, decisão do controlador, avaliação do encarregado, modelo de ameaças detalhado, teste de invasão autorizado, política corporativa ou resposta a incidentes própria. Quando houver conflito, prevalece a hierarquia da seção 2.

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

### 2.3 Identificadores

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
| Controlador dos dados | Definir finalidades, hipóteses legais, retenção, compartilhamentos, direitos e decisão regulatória sobre incidentes. |
| Encarregado/Privacidade | Orientar LGPD, titulares, menores, saúde, fornecedores, transferência internacional e necessidade de RIPD. |
| Responsável por segurança | Manter baseline, modelo de ameaças, exceções, vulnerabilidades, testes e incidentes. |
| Tech Lead | Transformar controles em arquitetura, padrões de código, revisões e evidências técnicas. |
| Desenvolvimento backend | Aplicar autorização, validações, proteção de dados, auditoria, integrações e testes no servidor. |
| Desenvolvimento frontend | Proteger sessão, saída, navegação, dependências, headers e ausência de segredo no bundle. |
| QA | Manter matriz de testes de segurança e bloquear aceite quando critério obrigatório falhar. |
| Operações/DevOps | Isolar ambientes, proteger pipeline/cofre, monitorar, fazer backup, restore, patching e rollback. |
| Administrador funcional | Operar apenas permissões concedidas; justificar ações críticas; nunca compartilhar conta. |
| Fornecedor | Cumprir contrato, menor privilégio, segurança, incidentes, retenção, suboperadores e saída. |
| IA/agente | Seguir `AGENTS.md`, não receber segredo/dado real, não aprovar risco e submeter produção a revisão humana. |

### 4.1 Controles de governança

| ID | Controle obrigatório | Evidência esperada | Status |
| --- | --- | --- | --- |
| `SEG-GOV-011` | Responsável por segurança, controlador e encarregado DEVEM ser formalmente identificados antes de dados reais. | Registro de nome, contato e aprovação. | Pendente; bloqueador |
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
| `SEG-DAD-001` | Todo campo DEVE possuir classificação, finalidade, proprietário, acesso e retenção antes de produção. | Dicionário de dados aprovado. | Pendente; bloqueador de produção |
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

Para aplicação web, a opção preferencial é sessão opaca mantida no servidor, identificada por cookie seguro. OAuth/OIDC ou JWT somente poderão ser adotados após decisão de arquitetura e modelo de ameaças; JWT assinado não deve ser tratado como sigiloso.

| ID | Controle obrigatório | Critério/evidência | Status |
| --- | --- | --- | --- |
| `SEG-IDN-001` | Login DEVE aceitar CPF ou e-mail sem revelar qual identificador existe. | Respostas e tempos não permitem enumeração prática. | Proposto |
| `SEG-IDN-002` | CPF NÃO DEVE ser usado como senha, token, fator de recuperação ou prova suficiente de identidade. | Testes de recuperação e cadastro. | Proposto |
| `SEG-IDN-003` | Senha DEVE ser armazenada somente com algoritmo de hash de senha aprovado, salt único e parâmetros calibrados. Preferir Argon2id; alternativa exige ADR. | Configuração, teste e revisão periódica. | Proposto |
| `SEG-IDN-004` | Senha de fator único DEVE ter no mínimo 15 caracteres; com MFA, no mínimo 8; o sistema DEVE aceitar pelo menos 64 e não impor composição arbitrária. | Testes de limite e política. | Proposto, sujeito à política oficial |
| `SEG-IDN-005` | Senhas comuns/comprometidas DEVEM ser bloqueadas sem enviar a senha completa a serviço externo. | Lista/serviço aprovado e teste. | Proposto |
| `SEG-IDN-006` | Colagem e gerenciadores de senha DEVEM ser permitidos; troca periódica NÃO DEVE ser exigida sem evidência de comprometimento. | Teste de interface e política. | Proposto |
| `SEG-IDN-007` | Administradores DEVEM usar MFA. Preferir WebAuthn/passkeys; TOTP pode ser fallback aprovado. SMS/WhatsApp não deve ser único fator administrativo. | Testes de ativação, recuperação e bypass. | Proposto; solução pendente |
| `SEG-IDN-008` | Ações críticas DEVEM exigir autenticação recente ou step-up: mudança de permissão, exceção de inscrição, exportação sensível, troca de MFA/contato e acesso administrativo de alto impacto. | Política e teste de reautenticação. | Proposto |
| `SEG-IDN-009` | Recuperação DEVE usar token aleatório, opaco, de uso único, curta duração e entregue por canal previamente verificado. | Token armazenado protegido, expiração e replay testados. | Proposto |
| `SEG-IDN-010` | Recuperação DEVE invalidar token após uso e revogar sessões quando o risco exigir. Pergunta de segurança é PROIBIDA. | Testes de reutilização e sessão. | Proposto |
| `SEG-IDN-011` | Conta inativa, desligada ou com papel removido DEVE perder acesso e sessões relevantes imediatamente ou dentro do SLA aprovado. | Teste de revogação e auditoria. | Proposto |
| `SEG-IDN-012` | Contas de serviço DEVEM ser distintas de pessoas, sem login interativo e com credenciais curtas/dinâmicas quando viável. | Inventário e configuração. | Proposto |

### 9.2 Limites iniciais contra abuso

Os valores abaixo são uma baseline **proposta**, a ser ajustada por teste de carga, risco e realidade de redes compartilhadas. IP nunca será o único sinal.

| Fluxo | Limite inicial proposto | Resposta e observação |
| --- | --- | --- |
| Login | 5 falhas por conta e 20 por IP em 15 minutos | Atraso progressivo, `429` quando aplicável, alerta por anomalia; evitar bloqueio permanente explorável para DoS |
| Recuperação | 3 solicitações por conta e 10 por IP por hora | Resposta sempre genérica; no máximo um token ativo por propósito |
| Cadastro | 5 tentativas por IP por hora, com proteção adicional adaptativa | Considerar redes compartilhadas dos polos; não coletar biometria/CAPTCHA invasivo sem avaliação |
| MFA/OTP | 5 tentativas por desafio | Invalidar desafio e exigir novo fluxo; impedir replay |

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

### 10.1 Tempos iniciais propostos

| Perfil | Inatividade | Duração absoluta | Reautenticação de ação crítica |
| --- | --- | --- | --- |
| Administrador | 15 minutos | 8 horas | Autenticação nos últimos 10 minutos e MFA válido |
| Professor | 30 minutos | 12 horas | 15 minutos para ação crítica, se houver |
| Aluno | 30 minutos | 24 horas | 15 minutos para troca de contato/senha |

Os valores dependem de aprovação de segurança/negócio e devem considerar uso em dispositivos compartilhados. “Lembrar-me” não poderá criar sessão ilimitada.

## 11. Autorização e segregação de funções

| ID | Controle obrigatório | Critério/evidência | Status |
| --- | --- | --- | --- |
| `SEG-AUTZ-001` | Toda operação não pública DEVE validar autorização no backend; guard, rota ou botão do frontend não é controle suficiente. | Testes por endpoint/serviço. | Proposto |
| `SEG-AUTZ-002` | A matriz de papéis e permissões DEVE definir aluno, professor, administrador parcial, administrador total, gestor, suporte, segurança e serviço. | Matriz aprovada e versionada. | Pendente; bloqueador |
| `SEG-AUTZ-003` | Autorização DEVE verificar ação, objeto e contexto: aluno próprio; professor vinculado à turma e vigência; administrador com permissão explícita. | Testes BOLA/IDOR e de vínculo. | Proposto |
| `SEG-AUTZ-004` | Identificador fornecido pelo cliente NÃO DEVE substituir a identidade/vínculo derivado da sessão. | Revisão de código e teste horizontal. | Proposto |
| `SEG-AUTZ-005` | Autoelevação e concessão acima do poder do concedente são PROIBIDAS. Mudança de permissão DEVE ser auditada e revogar sessão quando necessário. | Testes verticais e auditoria. | Proposto |
| `SEG-AUTZ-006` | Professor NÃO DEVE analisar justificativa nem corrigir chamada salva. | Testes de `UC-ADM-09/10`. | Proposto |
| `SEG-AUTZ-007` | Dados de saúde e comprovantes DEVEM possuir permissão separada dos demais dados do aluno. | Teste de campo/objeto e logs de acesso. | Pendente; bloqueador do fluxo sensível |
| `SEG-AUTZ-008` | Exportação, mapa de calor, ações em massa, exceção de inscrição e gestão de administradores DEVEM ter permissões próprias. | Matriz e testes. | Proposto |
| `SEG-AUTZ-009` | Jobs, filas, webhooks e consumidores DEVEM autenticar o serviço e aplicar o mesmo limite de negócio dos endpoints. | Testes de serviço e configuração. | Proposto |
| `SEG-AUTZ-010` | Acesso privilegiado DEVE ser revisado pelo menos trimestralmente e após mudança de função, desligamento ou incidente. | Relatório de revisão e revogação. | Proposto |

## 12. Segurança da API

### 12.1 Contrato e superfície

| ID | Controle obrigatório | Critério/evidência | Status |
| --- | --- | --- | --- |
| `SEG-API-001` | Toda rota DEVE constar do OpenAPI versionado ou de registro explícito de exceção; endpoints antigos DEVEM ser desativados ou protegidos. | Diferença contrato/rotas e inventário. | Proposto |
| `SEG-API-002` | API DEVE aceitar somente métodos, `Content-Type`, campos e tamanhos previstos; campos desconhecidos críticos DEVEM ser rejeitados. | Testes negativos de contrato. | Proposto |
| `SEG-API-003` | DTOs de entrada/saída DEVEM usar allowlist e impedir mass assignment de papel, dono, estado, auditoria e campos internos. | Revisão e teste de propriedade. | Proposto |
| `SEG-API-004` | Paginação, filtros, ordenação, profundidade e tamanho de resposta DEVEM possuir limites máximos. | Testes de consumo de recurso. | Proposto; valores pendentes |
| `SEG-API-005` | Erros DEVEM usar formato consistente, código seguro e correlation ID; stack trace, SQL, caminho interno e segredo NÃO DEVEM chegar ao cliente. | Testes de erro em produção. | Proposto |
| `SEG-API-006` | Requisições DEVEM ter rate limiting por usuário, credencial, IP, endpoint e risco, com `429` e `Retry-After` quando aplicável. | Teste de limite e observabilidade. | Proposto; limites finais pendentes |
| `SEG-API-007` | Fluxos sensíveis DEVEM ter proteção de negócio além do limite genérico: login, cadastro, recuperação, inscrição, fila, upload, exportação e mensagens. | Testes de automação/abuso. | Proposto |
| `SEG-API-008` | Operações repetíveis com efeito crítico DEVEM ser idempotentes e transacionais. | Testes concorrentes e de replay. | Proposto |
| `SEG-API-009` | CORS DEVE listar origens, métodos e headers exatos por ambiente; `*` com credenciais é PROIBIDO. | Testes de preflight/origem hostil. | Proposto |
| `SEG-API-010` | Documentação pública NÃO DEVE revelar segredo, URL interna, modelo sensível ou endpoint administrativo desnecessário. | Revisão do OpenAPI publicado. | Proposto |
| `SEG-API-011` | Console, Swagger interativo, debug, profiler e endpoint de administração DEVEM estar desabilitados publicamente em produção. | Varredura de superfície. | Proposto |
| `SEG-API-012` | Requisições de saída DEVEM usar allowlist de destino, DNS/redirect controlado e bloqueio a redes internas/metadados para prevenir SSRF. | Testes SSRF e configuração de egress. | Proposto |

### 12.2 Limites operacionais iniciais propostos

| Categoria | Limite inicial | Observação |
| --- | --- | --- |
| API pública de leitura | 60 requisições/minuto/IP | Cache e CDN podem ser usados sem ignorar origem confiável |
| API autenticada comum | 120 requisições/minuto/usuário e limite complementar por IP | Ajustar por carga e perfil |
| Inscrição/confirmação/cancelamento | 10 operações/minuto/usuário | Usar idempotency key/identificador de comando |
| Upload | 10 tentativas/10 minutos/usuário | Somar limites de quantidade e tamanho |
| Exportação | 5 solicitações/hora/gestor | Volume e concorrência também limitados |
| Envio manual de aviso | 10 envios/hora/professor, por turma e risco | Prevenir spam; emergências exigem fluxo aprovado |

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

Até que negócio, segurança e privacidade aprovem tipos, tamanho e retenção, o fluxo de upload permanece bloqueado para produção.

| ID | Controle obrigatório | Critério/evidência | Status |
| --- | --- | --- | --- |
| `SEG-ARQ-001` | Tipos, extensões, MIME real, assinaturas, quantidade e tamanho DEVEM usar allowlist. | Política de upload e testes. | Pendente; bloqueador |
| `SEG-ARQ-002` | Nome original NÃO DEVE definir o nome físico; usar identificador aleatório e preservar nome apenas como metadado sanitizado se necessário. | Inspeção do storage. | Proposto |
| `SEG-ARQ-003` | Arquivo DEVE ser armazenado fora da raiz pública, em storage privado, sem permissão de execução e com acesso por objeto. | Teste de URL direta e configuração. | Proposto |
| `SEG-ARQ-004` | Upload DEVE passar por validação de conteúdo e varredura antimalware/quarentena antes de ficar disponível ao administrador. | Pipeline de estado `recebido → quarentena → aprovado/rejeitado`. | Proposto; ferramenta pendente |
| `SEG-ARQ-005` | Arquivo comprimido, macro, conteúdo ativo e formato complexo DEVEM ser recusados por padrão, salvo necessidade e controle formal. | Testes com macro/zip bomb. | Proposto |
| `SEG-ARQ-006` | Download DEVE revalidar papel, titular/objeto e finalidade; URL assinada, se usada, deve ser curta e não reutilizável além do necessário. | Testes de IDOR e expiração. | Proposto |
| `SEG-ARQ-007` | Professor NÃO DEVE acessar comprovante de justificativa. | Teste de autorização. | Proposto |
| `SEG-ARQ-008` | Arquivo rejeitado, temporário ou expirado DEVE ser descartado com rotina verificável; backups respeitam a retenção aprovada. | Job, relatório de descarte e restore. | Pendente: retenção |
| `SEG-ARQ-009` | Respostas NÃO DEVEM expor caminho interno, nome físico, bucket, credencial ou stack trace. | Testes de erro/download. | Proposto |

Baseline inicial proposta: PDF, JPEG e PNG; máximo de 10 MiB por arquivo e um arquivo por justificativa. Esses valores **não estão aprovados** e devem ser validados antes da implementação do fluxo.

### 14.2 Exportações

| ID | Controle obrigatório | Critério/evidência | Status |
| --- | --- | --- | --- |
| `SEG-EXP-001` | Permissão para visualizar NÃO concede automaticamente permissão para exportar. | Matriz e testes. | Proposto |
| `SEG-EXP-002` | Campos DEVEM ser selecionados no servidor por relatório e papel, sem aceitar lista arbitrária do cliente. | Teste de campo adicional. | Proposto |
| `SEG-EXP-003` | Exportação DEVE registrar ator, tipo, filtros, volume, classificação, instante e resultado, sem copiar o conteúdo para o log. | Evento de auditoria. | Proposto |
| `SEG-EXP-004` | Arquivo DEVE ser temporário, protegido, possuir expiração e ser removido após retenção aprovada. | Teste de expiração/descarte. | Pendente: prazo |
| `SEG-EXP-005` | Relatório/mapa DEVE suprimir ou agrupar grupos abaixo do limiar de reidentificação aprovado. | Teste de filtros sucessivos. | Pendente; bloqueador analítico |
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
| `SEG-BE-001` | Java e Spring Boot DEVEM usar versões suportadas, fixadas e atualizadas; a escolha será registrada em arquitetura/ADR. | Matriz de suporte, SCA e build. | Pendente: versões |
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
| `SEG-DB-006` | Dados restritos em repouso DEVEM usar criptografia de volume/serviço e, quando o modelo de ameaças exigir, criptografia por campo com chaves separadas. | Arquitetura, configuração e teste de recuperação. | Proposto; decisão pendente |
| `SEG-DB-007` | Migração DEVE ser versionada, revisada, testada em cópia sintética/mascarada e possuir rollback ou estratégia de avanço seguro. | Pipeline e evidência. | Proposto |
| `SEG-DB-008` | Seed e fixture DEVEM ser sintéticos; CPF, telefone, saúde e comprovante reais são PROIBIDOS. | Scan e revisão. | Proposto |
| `SEG-DB-009` | Cache NÃO DEVE misturar usuários, papéis ou classificações; chaves devem incluir escopo seguro e expirar. | Testes de cache crossing. | Proposto |
| `SEG-DB-010` | Acesso administrativo ao banco DEVE ser nominal, temporário quando viável, MFA/identidade forte e auditado. | Logs e revisão de acesso. | Proposto |

## 18. Criptografia, transporte e certificados

| ID | Controle obrigatório | Critério/evidência | Status |
| --- | --- | --- | --- |
| `SEG-CRP-001` | Produção e homologação com dado/confidencial DEVEM usar HTTPS/TLS em todo fluxo; HTTP só em loopback local sem dado real. | Teste TLS e redirecionamento. | Proposto |
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
| `SEG-SEG-004` | Cofre oficial (“GitNode” no Guia ou substituto formalmente aprovado) DEVE ser identificado antes de build/deploy com segredo real. | ADR/política e configuração do pipeline. | Pendente; bloqueador de integração/produção |
| `SEG-SEG-005` | Segredo DEVE ser injetado em runtime/deploy por identidade de workload, nunca embutido em imagem, camada, cache ou artefato. | Inspeção de imagem/artefato. | Proposto |
| `SEG-SEG-006` | Desenvolvimento, homologação e produção DEVEM ter segredos distintos; PR/preview não recebe segredo de produção. | Configuração de ambiente. | Proposto |
| `SEG-SEG-007` | Todo segredo DEVE possuir proprietário, escopo, ambiente, criação, revisão/expiração, rotação, revogação e trilha. | Inventário. | Proposto |
| `SEG-SEG-008` | Logs de CI e aplicação DEVEM mascarar valores; segredo não deve aparecer em argumento de comando. | Teste de pipeline. | Proposto |

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

O SIDESP tratará dados pessoais e poderá tratar dados de saúde, comprovantes e dados de crianças/adolescentes. A [LGPD](https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2018/lei/l13709compilado.htm) exige medidas técnicas e administrativas, finalidade, necessidade, transparência, segurança, prevenção e responsabilização. A hipótese legal **não será presumida neste documento**; deverá ser definida pelo controlador e revisada pelo encarregado.

| ID | Controle obrigatório | Critério/evidência | Status |
| --- | --- | --- | --- |
| `SEG-PRI-001` | Antes de dado real, DEVEM ser definidos controlador, operador(es), encarregado, titulares, finalidades, hipóteses legais, compartilhamentos, países, retenção e direitos. | `PRIVACIDADE_E_DADOS.md` aprovado. | Pendente; bloqueador |
| `SEG-PRI-002` | Coleta DEVE limitar-se ao estritamente necessário; campo “útil no futuro” sem finalidade é PROIBIDO. | Revisão de formulário e dicionário. | Proposto |
| `SEG-PRI-003` | Dados de saúde somente PODEM ser coletados após justificar necessidade, base, profissionais/papéis autorizados, emergência, retenção e risco. | Decisão de privacidade e teste de acesso. | Pendente; bloqueador |
| `SEG-PRI-004` | Tratamento de crianças/adolescentes DEVE considerar melhor interesse, transparência apropriada e verificação do responsável conforme decisão jurídica. | Fluxo e aviso aprovados. | Pendente; bloqueador |
| `SEG-PRI-005` | Vínculo do responsável legal DEVE ser comprovado, atualizado, revogável e protegido contra associação indevida. | Processo e testes. | Pendente |
| `SEG-PRI-006` | Avisos e telas DEVEM explicar finalidades, uso compartilhado, duração, controlador, contato e direitos de modo claro. | Aviso de privacidade aprovado. | Pendente |
| `SEG-PRI-007` | Solicitações de titular DEVEM possuir canal, autenticação proporcional, prazo, registro e resposta sem revelar terceiros. | Procedimento e testes. | Pendente |
| `SEG-PRI-008` | Retenção e descarte DEVEM ser definidos por categoria; backup e log também respeitam a política. | Tabela de retenção e jobs. | Pendente; bloqueador de produção |
| `SEG-PRI-009` | Relatório e mapa DEVEM evitar reidentificação e uso discriminatório; filtros de gênero, idade, satisfação e região exigem finalidade e limiar. | Avaliação e testes. | Pendente |
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
| `SEG-WA-001` | Fornecedor oficial, contrato, templates, base/consentimento, opt-out, fallback e custo DEVEM ser aprovados antes da integração. | Decisão formal. | Pendente; bloqueador |
| `SEG-WA-002` | Mensagem DEVE conter somente o mínimo; comprovante, dado de saúde, senha, token, CPF completo e detalhe desnecessário são PROIBIDOS. | Revisão de templates. | Proposto |
| `SEG-WA-003` | Número de telefone e vínculo com aluno/responsável DEVEM ser verificados e atualizáveis. | Fluxo e teste. | Pendente |
| `SEG-WA-004` | Webhook DEVE validar assinatura com comparação segura, timestamp, tolerância curta e proteção de replay. | Testes de assinatura/replay. | Proposto |
| `SEG-WA-005` | Evento e tentativa DEVEM ter IDs idempotentes; callback repetido não duplica mudança. | Testes. | Proposto |
| `SEG-WA-006` | Segredo de webhook/API fica no cofre e deve ser rotacionável sem indisponibilidade prolongada. | Runbook e teste de rotação. | Proposto |
| `SEG-WA-007` | Histórico DEVE minimizar/mascarar telefone e conteúdo conforme papel; retenção pendente. | Teste de `UC-COM-03`. | Pendente |
| `SEG-WA-008` | Falha de entrega DEVE ficar visível e acionar fallback aprovado; “enviado ao provedor” não significa “entregue”. | Estados e alertas. | Proposto |

### 21.3 Mapas e geocodificação

| ID | Controle obrigatório | Critério/evidência | Status |
| --- | --- | --- | --- |
| `SEG-MAP-001` | Serviço de mapas NÃO DEVE receber posição/endereço de aluno ou responsável. | Inspeção de tráfego. | Proposto |
| `SEG-MAP-002` | Apenas polo público e agregados acima do limiar aprovado podem ser enviados/exibidos. | Teste de payload/filtro. | Pendente: limiar |
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
| `SEG-LOG-005` | Logs DEVEM ter controle de acesso, integridade, criptografia, retenção e descarte definidos. | Política e configuração. | Pendente: retenção |
| `SEG-LOG-006` | Relógios DEVEM ser sincronizados; eventos entre serviços devem ser correlacionáveis. | Configuração e teste. | Proposto |
| `SEG-LOG-007` | Acesso e exportação de logs DEVEM ser auditados e limitados; log não é banco alternativo. | Revisão de acesso. | Proposto |
| `SEG-LOG-008` | Alertas DEVEM possuir responsável, severidade, canal, horário de cobertura e runbook. | Catálogo de alertas. | Pendente |

### 22.3 Alertas mínimos

- pico de falha de login, recuperação ou MFA;
- enumeração/varredura de IDs e aumento de `401`, `403` ou `429`;
- concessão de administrador, autoelevação tentada ou exceção de inscrição;
- exportação anormal por volume, horário ou frequência;
- acesso anormal a comprovante/dado sensível;
- assinatura inválida/replay de webhook;
- malware ou taxa elevada de upload rejeitado;
- falha persistente de WhatsApp/mapas e fila acumulada;
- secret scanning, SAST, SCA ou container scan crítico;
- aumento de `5xx`, latência, consumo, falha de banco, espaço ou backup;
- mudança de configuração/infraestrutura fora do pipeline.

## 23. Disponibilidade, resiliência, backup e recuperação

| ID | Controle obrigatório | Critério/evidência | Status |
| --- | --- | --- | --- |
| `SEG-RES-001` | SLO, RPO e RTO DEVEM ser aprovados antes da infraestrutura final e produção. | Documento de arquitetura/continuidade. | Pendente; bloqueador de produção |
| `SEG-RES-002` | Operações críticas DEVEM ser transacionais/idempotentes e distinguir `pendente`, `confirmada`, `falha` e `desconhecida`. | Testes de interrupção/replay. | Proposto |
| `SEG-RES-003` | Timeout, retry e circuit breaker DEVEM evitar cascata; retry não pode ser infinito. | Configuração e testes. | Proposto |
| `SEG-RES-004` | Banco e arquivos necessários DEVEM ter backup criptografado, segregado e protegido contra alteração; escopo/frequência seguem RPO. | Job, inventário e evidência. | Proposto; valores pendentes |
| `SEG-RES-005` | Restore DEVE ser testado periodicamente em ambiente isolado e medir RTO; backup não testado não conta como recuperação. | Ata de exercício. | Proposto |
| `SEG-RES-006` | Chaves/segredos necessários ao restore DEVEM possuir recuperação segura e separada. | Procedimento testado. | Proposto |
| `SEG-RES-007` | Dados restaurados DEVEM manter autorização, auditoria e política de retenção; teste não usa cópia real sem aprovação. | Checklist de restore. | Proposto |
| `SEG-RES-008` | Estratégia de conectividade instável na chamada DEVE impedir perda silenciosa e confirmação falsa. | Decisão e teste offline/reenvio. | Pendente; bloqueador do fluxo |
| `SEG-RES-009` | Operação degradada de WhatsApp/mapas DEVE estar documentada; falha de fornecedor não bloqueia fluxos independentes. | Runbook e teste. | Proposto |

## 24. Infraestrutura, containers e ambientes

| ID | Controle obrigatório | Critério/evidência | Status |
| --- | --- | --- | --- |
| `SEG-INF-001` | Desenvolvimento, teste, homologação e produção DEVEM ser isolados por contas/projetos, rede, dados e segredos. | Diagrama e configuração. | Proposto |
| `SEG-INF-002` | Produção só DEVE ser acessível por identidade nominal, MFA e canal administrativo protegido; acesso direto deve ser excepcional, temporário e auditado. | IAM e logs. | Proposto |
| `SEG-INF-003` | Banco, storage privado e portas de management NÃO DEVEM estar públicos. | Scanner externo e regras de rede. | Proposto |
| `SEG-INF-004` | Firewall/security group DEVE negar por padrão e permitir apenas origem, destino, porta e protocolo necessários. | Revisão de regra. | Proposto |
| `SEG-INF-005` | Container DEVE usar imagem mínima, versão/digest fixado, usuário não root, capabilities removidas e filesystem read-only quando viável. | Scan e manifesto. | Proposto |
| `SEG-INF-006` | Imagem NÃO DEVE conter shell/ferramenta desnecessária, segredo, dado, código-fonte não requerido ou cache sensível. | Inspeção de imagem. | Proposto |
| `SEG-INF-007` | IaC DEVE ser versionada, revisada, escaneada e aplicada pelo pipeline; estado de IaC é restrito e protegido. | PR, scan e backend de estado. | Proposto |
| `SEG-INF-008` | Patching, inventário e SLA de atualização DEVEM cobrir SO, runtime, banco, imagem e serviços. | Relatório e tickets. | Proposto |
| `SEG-INF-009` | Quotas de CPU, memória, storage, conexões e custo DEVEM limitar exaustão e abuso. | Configuração e alertas. | Proposto; valores pendentes |
| `SEG-INF-010` | Ambiente efêmero DEVE usar dado sintético, segredo próprio e destruição segura; PR externo não recebe segredo. | Pipeline e teste. | Proposto |
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

### 25.1 SLA inicial de vulnerabilidades

| Severidade | Ação inicial | Prazo máximo proposto para corrigir/mitigar |
| --- | --- | --- |
| Crítica explorada/exposta | Conter imediatamente; pode suspender serviço/release | 24 horas |
| Crítica não explorada | Bloquear release e priorizar | 72 horas |
| Alta | Plano imediato e bloqueio quando alcançável/explorável | 15 dias |
| Média | Tratar no ciclo priorizado | 60 dias |
| Baixa | Registrar e planejar | 120 dias |

Os prazos dependem de política oficial. Exceção requer risco formal; indisponibilidade de patch exige mitigação compensatória e monitoramento.

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
| `SEG-VUL-001` | Canal privado para relato de vulnerabilidade DEVE existir antes da publicação do produto. | Contato/processo divulgado. | Pendente |
| `SEG-VUL-002` | Achado DEVE ser triado por impacto real, alcance, exploração e dado afetado, sem reduzir risco só pelo score automático. | Ticket confidencial. | Proposto |
| `SEG-VUL-003` | Evidência com dado/segredo DEVE ter acesso restrito e nunca ir a issue pública. | Processo. | Proposto |
| `SEG-VUL-004` | Correção DEVE incluir teste de regressão e busca por variantes. | PR/testes. | Proposto |
| `SEG-VUL-005` | Vulnerabilidade explorada ou com dado pessoal afetado DEVE ser avaliada como incidente. | Registro de decisão. | Proposto |
| `SEG-VUL-006` | Exceção de vulnerabilidade DEVE expirar e ser reavaliada; “sem patch” não é aceite permanente. | Registro de exceção. | Proposto |

## 30. Resposta a incidentes

Um `RESPOSTA_A_INCIDENTES.md` e um runbook operacional deverão detalhar contatos e procedimentos. Até lá, aplica-se o processo mínimo abaixo.

### 30.1 Severidade inicial

| Nível | Exemplo no SIDESP | Resposta inicial proposta |
| --- | --- | --- |
| `SEV-1 Crítica` | credencial/chave de produção exposta; acesso não autorizado a saúde/comprovantes; alteração ampla; indisponibilidade crítica; exploração ativa | Mobilização imediata, contenção prioritária e comando de incidente |
| `SEV-2 Alta` | acesso indevido limitado, perda relevante de integridade, exportação anormal, malware contido | Resposta em até 1 hora e análise de impacto |
| `SEV-3 Média` | tentativa recorrente bloqueada, vulnerabilidade sem exploração conhecida, falha parcial de fornecedor | Resposta no mesmo dia útil |
| `SEV-4 Baixa` | evento sem impacto confirmado, melhoria de controle | Triagem no fluxo normal |

Os tempos dependem de aprovação e cobertura operacional.

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

### 30.4 Contatos pendentes

| Função | Nome | Canal primário | Alternativo |
| --- | --- | --- | --- |
| Comando de incidente | Pendente | Pendente | Pendente |
| Segurança | Pendente | Pendente | Pendente |
| Operações | Pendente | Pendente | Pendente |
| Controlador/Negócio | Pendente | Pendente | Pendente |
| Encarregado/Privacidade | Pendente | Pendente | Pendente |
| Jurídico/Comunicação | Pendente | Pendente | Pendente |

Ausência desses contatos é bloqueadora de produção.

## 31. Acesso de suporte e administração

| ID | Controle obrigatório | Critério/evidência | Status |
| --- | --- | --- | --- |
| `SEG-OPS-001` | Suporte DEVE operar com ferramenta/visão própria e dados mascarados, não por impersonação silenciosa. | Fluxo e teste. | Proposto |
| `SEG-OPS-002` | Impersonação, se indispensável, exige permissão separada, motivo, tempo limitado, indicação visual e auditoria reforçada. | Aprovação e teste. | Pendente |
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

### Gate 2 — Antes da primeira integração/homologação

- [ ] OpenAPI e contrato de erros/limites;
- [ ] autorização por objeto e testes negativos;
- [ ] fornecedor, contrato, webhook e transferência avaliados;
- [ ] cofre, TLS, rotação e ambientes isolados;
- [ ] logs, alertas e auditoria crítica;
- [ ] migrações, backups iniciais e rollback;
- [ ] SAST, SCA, secret scan e SBOM ativos.

### Gate 3 — Antes de produção

- [ ] ASVS 5.0 nível 2 verificado e controles reforçados selecionados;
- [ ] pentest/revisão independente proporcional ao risco;
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

## 36. Pendências bloqueadoras

| ID | Pendência | Bloqueia |
| --- | --- | --- |
| `PSEG-001` | Designar segurança, controlador, encarregado e contatos de incidente | Uso de dados reais e produção |
| `PSEG-002` | Aprovar matriz granular de papéis, incluindo saúde, comprovante, exportação e exceção | Implementação de autorização administrativa/sensível |
| `PSEG-003` | Decidir arquitetura de autenticação, MFA, sessão, recuperação e tempos | Implementação de identidade |
| `PSEG-004` | Definir cofre oficial e fluxo de segredos | Primeira integração com credencial real e deploy |
| `PSEG-005` | Definir dados de saúde, finalidade, base, acesso, retenção e RIPD | Coleta/exibição de saúde |
| `PSEG-006` | Definir tratamento de menores e comprovação do responsável | Cadastro/comunicação de menor em produção |
| `PSEG-007` | Aprovar tipos, limite, varredura, storage e retenção de comprovante | Upload em produção |
| `PSEG-008` | Selecionar e avaliar fornecedor de WhatsApp, contrato, webhook, templates e fallback | Integração real |
| `PSEG-009` | Definir fórmulas, campos, limiar de agregação, retenção e limites de exportação | Relatórios/mapa/exportações em produção |
| `PSEG-010` | Aprovar SLO, volumes, RPO, RTO, backup e restore | Arquitetura final e produção |
| `PSEG-011` | Decidir operação com internet instável | Chamada nos polos |
| `PSEG-012` | Criar modelo de ameaças, estratégia de testes, contrato de API, banco, runbook e incidentes | Gate correspondente |

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
| `0.1.0` | 12/08/2026 | Equipe SIDESP, com consolidação documental assistida por IA | Primeira baseline de segurança cobrindo governança, dados, identidade, sessão, autorização, API, arquivos, frontend, backend, banco, segredos, privacidade, fornecedores, logs, resiliência, infraestrutura, supply chain, CI/CD, testes, vulnerabilidades, incidentes e exceções | Rascunho |
