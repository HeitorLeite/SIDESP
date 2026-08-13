# Padrão de Commits — SIDESP

> **SIDESP — Sistema Integrado de Desenvolvimento Esportivo Público**  
> Convenção para mensagens, organização, rastreabilidade e segurança dos commits do projeto.

## Identificação do documento

| Campo | Valor |
| --- | --- |
| Projeto | SIDESP — Sistema Integrado de Desenvolvimento Esportivo Público |
| Responsável pelo padrão | Heitor Leite — Tech Lead |
| Equipe | Diego Henrico, Heitor Leite, Kauã Raphael, Lívia Andrade, Luís Mario e Micael Phillipini |
| Versão | `0.1.0` |
| Data | 13/08/2026 |
| Classificação | Interna |
| Status | Rascunho — não aprovado |
| Próxima revisão | Ao definir o fluxo Git, a plataforma de hospedagem, a automação de releases ou a ferramenta de gestão de tarefas |

## Aprovações

| Papel | Responsável | Situação | Data |
| --- | --- | --- | --- |
| Tech Lead | Heitor Leite | Pendente de revisão | — |
| Product Owner | Lívia Andrade | Pendente de revisão da rastreabilidade | — |
| QA | Micael Phillipini | Pendente de revisão dos gates | — |
| Segurança | Pendente | Não avaliado | — |

## 1. Objetivo

Este documento define como a equipe deve registrar mudanças no histórico Git do SIDESP. O padrão busca garantir que cada commit seja:

- claro para quem revisa;
- pequeno e logicamente coeso;
- rastreável a requisito, regra, caso de uso, controle de segurança ou tarefa;
- seguro para permanecer permanentemente no histórico;
- utilizável em revisão, auditoria, geração de changelog e investigação de falhas;
- compreensível por integrantes de frontend, backend, QA, produto, segurança e operações.

## 2. Regras normativas

- **DEVE / OBRIGATÓRIO:** regra que não pode ser ignorada sem exceção aprovada.
- **NÃO DEVE / PROIBIDO:** prática vedada.
- **DEVERIA / RECOMENDADO:** prática esperada; ausência exige justificativa.
- **PODE:** alternativa permitida conforme o contexto.

## 3. Formato adotado

O SIDESP adota uma versão em português do padrão **Conventional Commits**:

```text
<tipo>[escopo opcional][! opcional]: <descrição curta>

[corpo opcional]

[rodapés opcionais]
```

Exemplo completo:

```text
feat(inscricao): adiciona entrada automática na lista de espera

Cria uma entrada ordenada quando a turma não possui vagas.
Impede posições duplicadas para o mesmo aluno e a mesma turma.

Refs: RF-INS-002, RN-009, UC-INS-03
Closes: #123
```

### 3.1 Componentes

| Componente | Obrigatório | Regra |
| --- | :---: | --- |
| Tipo | Sim | Identifica a natureza principal da mudança. |
| Escopo | Recomendado | Identifica o módulo afetado; deve usar a lista aprovada ou um escopo coerente. |
| `!` | Somente quando aplicável | Indica alteração incompatível. Também exige rodapé `BREAKING CHANGE`. |
| Descrição | Sim | Resume objetivamente o resultado da mudança. |
| Corpo | Quando necessário | Explica motivo, comportamento anterior, novo comportamento, decisões e limitações. |
| Rodapé | Quando aplicável | Registra requisitos, casos, issues, breaking changes, coautoria ou avisos. |

## 4. Idioma e estilo

| Elemento | Regra |
| --- | --- |
| Idioma | Português do Brasil. Termos técnicos consolidados, nomes de código e identificadores podem permanecer em inglês. |
| Tipo e escopo | Sempre em minúsculas, sem acento e sem espaço. |
| Descrição | Iniciar em minúscula, usar verbo no presente do indicativo e não terminar com ponto. |
| Título | Preferencialmente até 72 caracteres; máximo de 100. |
| Corpo | Quebrar linhas preferencialmente em até 100 caracteres. |
| Tom | Objetivo e impessoal; descrever o resultado, não a atividade do autor. |
| Codificação | UTF-8. |

### 4.1 Verbos recomendados

Use verbos que expressem o estado produzido pela mudança:

- `adiciona`, `implementa`, `permite`, `impede`, `corrige`;
- `remove`, `atualiza`, `documenta`, `valida`, `protege`;
- `refatora`, `otimiza`, `configura`, `integra`, `reverte`.

Evite descrições vagas como:

- `faz alterações`;
- `ajustes`;
- `corrige coisas`;
- `update`;
- `alterações finais`;
- `funcionando`;
- `commit do dia`.

## 5. Tipos permitidos

| Tipo | Quando usar | Exemplo |
| --- | --- | --- |
| `feat` | Nova funcionalidade ou comportamento observável do produto | `feat(inscricao): permite cancelar inscrição ativa` |
| `fix` | Correção de defeito funcional | `fix(frequencia): impede chamada duplicada para a mesma aula` |
| `security` | Correção ou fortalecimento de segurança/privacidade | `security(auth): revoga sessões após redefinição de senha` |
| `docs` | Somente documentação | `docs(requisitos): registra critérios da lista de espera` |
| `refactor` | Reorganização sem mudar comportamento externo | `refactor(aluno): separa validação de cadastro do serviço` |
| `test` | Criação ou alteração exclusiva de testes | `test(autorizacao): cobre acesso horizontal entre alunos` |
| `perf` | Melhoria mensurável de desempenho | `perf(relatorio): reduz consultas na apuração de frequência` |
| `build` | Build, empacotamento ou dependências | `build(backend): atualiza plugin do Spring Boot` |
| `ci` | Pipeline, automação de integração ou entrega | `ci(security): adiciona varredura de segredos no pull request` |
| `chore` | Manutenção sem alterar produto, teste ou build de forma relevante | `chore(repo): atualiza arquivos ignorados pelo Git` |
| `style` | Formatação sem mudar comportamento | `style(backend): aplica formatador nos controladores` |
| `revert` | Reversão de commit anterior | `revert: reverte integração com provedor de mensagens` |

### 5.1 Como escolher entre tipos parecidos

- Use `feat` quando o usuário ou sistema passa a conseguir fazer algo novo.
- Use `fix` quando o comportamento existente estava incorreto.
- Use `security` quando a intenção principal é reduzir risco, corrigir vulnerabilidade ou fortalecer controle.
- Use `refactor` quando comportamento, contrato e regra permanecem iguais.
- Use `build` para Maven, Gradle, npm, compilação, empacotamento e dependências.
- Use `ci` para workflows, checks, publicação e automações do repositório.
- Use `chore` somente quando nenhum tipo mais específico se aplicar.

Não use um tipo inventado sem atualizar e aprovar este documento.

## 6. Escopos recomendados

### 6.1 Domínios do produto

| Escopo | Conteúdo |
| --- | --- |
| `publico` | Notícias públicas, polos e modalidades públicas |
| `auth` | Autenticação, recuperação, senha, MFA e sessão |
| `aluno` | Cadastro e perfil de aluno/responsável |
| `professor` | Perfil e vínculos de professor |
| `inscricao` | Inscrições, cancelamentos e elegibilidade |
| `espera` | Lista de espera, oferta e confirmação de vaga |
| `turma` | Turmas, aulas, horários e capacidade |
| `frequencia` | Chamada, presença, falta e diário de aula |
| `justificativa` | Envio, comprovante e análise de justificativa |
| `selecao` | Processo seletivo e painel Kanban |
| `notificacao` | Notificações internas e histórico |
| `whatsapp` | Integração e webhooks do WhatsApp |
| `noticia` | Criação, agenda, publicação e inativação de notícias |
| `relatorio` | Relatórios, indicadores, exportações e mapa de calor |
| `admin` | Administração, permissões e ações excepcionais |

### 6.2 Áreas técnicas e documentais

| Escopo | Conteúdo |
| --- | --- |
| `api` | Contrato, versionamento, erros e componentes transversais da API |
| `backend` | Alteração transversal do backend não pertencente a um único domínio |
| `frontend` | Alteração transversal do frontend |
| `database` | Modelo, migrações, índices, constraints e seeds |
| `security` | Controles transversais de segurança e privacidade |
| `observability` | Logs, métricas, tracing e alertas |
| `infra` | Containers, cloud, redes e infraestrutura como código |
| `deps` | Dependências e cadeia de suprimentos |
| `repo` | Configuração geral do repositório |
| `docs` | Documentação transversal |
| `tests` | Infraestrutura ou utilitários compartilhados de teste |

### 6.3 Regras para escopos

- Escolha o menor escopo que represente a mudança principal.
- Não liste vários escopos separados por vírgula. Se a mudança for realmente transversal, use `api`, `backend`, `frontend`, `security`, `infra` ou omita o escopo.
- Evite escopos baseados em nome de pessoa, número de sprint, nome de branch ou arquivo isolado.
- Um novo escopo recorrente deve ser incluído neste documento.

## 7. Descrição curta

A descrição deve responder: **qual resultado este commit produz?**

Regras:

- use voz ativa;
- seja específico;
- não repita o tipo ou o escopo;
- não inclua issue no título quando ela puder ficar no rodapé;
- não use ponto final;
- não use emoji;
- não escreva apenas o nome do arquivo alterado.

Correto:

```text
fix(espera): preserva a ordem em solicitações concorrentes
```

Incorreto:

```text
fix: correções
```

## 8. Corpo do commit

O corpo é obrigatório quando o título não explica suficientemente:

- por que a mudança foi necessária;
- qual era o comportamento anterior;
- qual é o comportamento novo;
- quais decisões e alternativas foram consideradas;
- quais limitações permanecem;
- como foi feita migração ou compatibilidade;
- qual impacto existe em segurança, privacidade ou operação.

Modelo recomendado:

```text
<tipo>(<escopo>): <descrição curta>

Motivo da alteração e comportamento anterior.
Novo comportamento e decisões relevantes.
Limitações, migração ou impacto operacional, quando houver.

Refs: <IDs de documentação>
Closes: <issue>
```

O corpo deve explicar a decisão, não narrar cada linha modificada. O diff já demonstra a implementação.

## 9. Rodapés e rastreabilidade

### 9.1 Rodapés reconhecidos

| Rodapé | Uso |
| --- | --- |
| `Refs:` | Requisitos, regras, casos de uso, controles, ADRs ou tarefas relacionados |
| `Closes:` | Issue/tarefa resolvida pelo commit ou PR, conforme a plataforma |
| `Fixes:` | Defeito específico resolvido, quando a equipe optar por este termo |
| `BREAKING CHANGE:` | Descrição obrigatória da incompatibilidade e do plano de migração |
| `Co-authored-by:` | Coautoria no formato reconhecido pelo Git |
| `Relates-to:` | Relação sem encerramento automático da tarefa |

### 9.2 Identificadores do SIDESP

Quando a mudança implementar ou alterar comportamento documentado, o commit ou o PR DEVE citar os IDs aplicáveis:

- objetivos: `OBJ-01`;
- requisitos funcionais: `RF-INS-002`;
- requisitos não funcionais: `RNF-SEG-001`;
- regras de negócio: `RN-009`;
- casos de uso: `UC-INS-03`;
- controles de segurança: `SEG-AUTZ-003`;
- questões/pendências: `Q-003` ou `PSEG-008`;
- decisões de arquitetura: `ADR-001`;
- testes: `CT-INS-001`, quando definidos.

Exemplo:

```text
Refs: RF-INS-002, RN-009, UC-INS-03, SEG-API-008
Closes: #123
```

### 9.3 Onde colocar a rastreabilidade

- Commits simples podem registrar os IDs no rodapé.
- Quando vários commits formam uma única entrega, cada commit DEVERIA citar pelo menos a tarefa e o PR DEVE conter a rastreabilidade completa.
- Um commit de refatoração interna pode citar apenas a tarefa se não alterar requisito.
- A referência não substitui teste, documentação ou descrição adequada.

## 10. Alterações incompatíveis

Uma alteração é incompatível quando consumidores, ambientes ou dados existentes deixam de funcionar sem adaptação. Exemplos:

- remover ou renomear rota/campo da API;
- alterar sem compatibilidade o significado de um estado;
- tornar campo antes opcional em obrigatório;
- mudar regra de autenticação ou formato de sessão;
- alterar evento/webhook consumido externamente;
- executar migração que exige procedimento coordenado;
- remover variável de configuração necessária;
- mudar relatório/formato público contratado.

Formato obrigatório:

```text
feat(api)!: substitui o identificador de modalidade no contrato

O contrato passa a usar um identificador opaco no lugar do código numérico.
Clientes devem migrar para a versão v2 antes da remoção da v1.

BREAKING CHANGE: o campo modalidadeId deixa de aceitar números na API v2.
Plano de migração: manter v1 por 90 dias e publicar guia de adaptação.
Refs: ADR-004, RF-INS-001
```

Regras:

- o `!` no cabeçalho e o rodapé `BREAKING CHANGE:` são obrigatórios;
- o corpo deve declarar consumidores afetados, migração, rollback e prazo de compatibilidade;
- alteração incompatível exige revisão do Tech Lead e atualização dos contratos/documentos no mesmo PR;
- incompatibilidade não deve ser escondida sob `refactor`, `chore` ou `fix`.

## 11. Commits de segurança e privacidade

Use `security` quando a finalidade principal for corrigir ou fortalecer um controle, mesmo que o usuário não perceba uma nova funcionalidade.

Exemplos:

```text
security(autorizacao): impede professor de acessar turma não vinculada
security(upload): bloqueia arquivos com tipo real não permitido
security(deps): atualiza biblioteca afetada por vulnerabilidade crítica
security(session): rotaciona sessão após elevação de privilégio
```

### 11.1 Regras de divulgação

- O título e o corpo NÃO DEVEM ensinar como explorar uma falha ainda não corrigida ou revelar ambiente afetado.
- CVE público PODE ser citado quando isso não ampliar risco; achado privado deve usar referência restrita.
- Evidência, payload ofensivo, CPF, token, segredo, URL interna e log sensível NÃO DEVEM entrar na mensagem.
- Se a divulgação detalhada representar risco, use descrição segura no commit e mantenha detalhes na issue privada autorizada.
- Correção de segurança DEVE incluir teste de regressão e busca por variantes.
- Um segredo exposto DEVE ser revogado/rotacionado; apenas removê-lo em commit não resolve o incidente.

Exemplo seguro:

```text
security(api): reforça autorização por objeto nas inscrições

Centraliza a validação de propriedade na camada de serviço e adiciona
testes de acesso horizontal. Detalhes do achado permanecem no registro
restrito de segurança.

Refs: RNF-SEG-001, SEG-AUTZ-003
Relates-to: SEC-2026-004
```

## 12. Organização dos commits

### 12.1 Atomicidade

Cada commit DEVE representar uma unidade lógica revisável e, quando possível:

- compilar;
- manter testes existentes aprovados;
- conter os testes da mudança;
- atualizar a documentação diretamente afetada;
- não depender de um commit futuro para corrigir um estado inseguro.

### 12.2 Não misturar

Evite no mesmo commit:

- funcionalidade nova e refatoração ampla não necessária;
- correção de segurança e atualização massiva sem relação;
- formatação de todo o projeto e mudança funcional;
- migração de banco e alterações independentes de interface;
- vários requisitos sem relação;
- arquivos gerados e código-fonte, salvo quando o artefato for oficialmente versionado.

### 12.3 Quando dividir

Uma sequência coerente pode ser:

1. `refactor` preparatório sem alteração de comportamento;
2. `feat` ou `fix` com teste correspondente;
3. `docs` quando a documentação for uma entrega separada.

Entretanto, não divida artificialmente uma mudança de forma que commits intermediários quebrem o build, removam controle ou deixem vulnerabilidade explorável.

## 13. Commits temporários e histórico final

- Commits `WIP`, `temp`, `checkpoint`, `fixup!` e `squash!` PODEM ser usados apenas em branch pessoal enquanto a mudança está em andamento, se o fluxo da equipe permitir.
- Eles NÃO DEVEM permanecer na branch principal nem no histórico final da entrega.
- Antes do merge, o autor DEVE organizar o histórico de acordo com este documento.
- Reescrever histórico já compartilhado exige coordenação; é PROIBIDO forçar atualização da branch principal.
- Merge, squash ou rebase será decidido no fluxo Git do projeto. Até essa decisão, recomenda-se squash de commits temporários e preservação de commits lógicos úteis.

## 14. Reverts

Use `revert` e explique o motivo, o commit revertido e a condição para reaplicar.

```text
revert: reverte publicação agendada de notícias

Reverte o commit 1a2b3c4 porque o agendador pode publicar notícias
inativadas em uma condição de concorrência.

Refs: RF-ADM-006, UC-AUT-05
Relates-to: #245
```

Quando possível, use a mensagem gerada por `git revert` e complemente o corpo. Não apague o histórico da alteração com reset forçado na branch compartilhada.

## 15. Merges e integração

- A branch principal DEVE ser protegida.
- Mudanças DEVEM entrar por Pull Request, salvo procedimento emergencial formal.
- O PR DEVE ter checks aprovados e revisão humana independente.
- Autor não deve ser o único aprovador de autenticação, autorização, criptografia, upload, exportação, permissões, banco produtivo ou pipeline.
- Merge direto, bypass de gate ou alteração emergencial devem seguir o processo de exceção do `SEGURANCA.md`.
- O formato da mensagem de merge automática PODE ser definido pela plataforma; os commits e o título do PR devem continuar claros.

## 16. Assinatura e identidade

### 16.1 Identidade do autor

- Cada integrante DEVE usar conta individual e e-mail aprovado pela equipe.
- Conta ou credencial compartilhada é PROIBIDA.
- Nome do autor deve permitir identificar a pessoa responsável.
- Coautoria deve usar `Co-authored-by:` com consentimento dos participantes.

### 16.2 Assinatura criptográfica

- Commits e tags de release DEVERIAM ser assinados quando a plataforma e os equipamentos estiverem configurados.
- Assinatura será OBRIGATÓRIA para tags de release e commits feitos diretamente em procedimento emergencial após a equipe aprovar a tecnologia e o processo.
- Chaves de assinatura devem possuir proprietário, proteção, revogação e rotação.
- O repositório não deve rejeitar integrantes antes de existir um procedimento documentado de configuração e recuperação.

## 17. Dados e arquivos proibidos no Git

Nunca versionar, mesmo em branch temporária:

- senha, token, cookie, chave de API, client secret ou chave privada;
- `.env` real, arquivo de credencial ou connection string com senha;
- certificado privado, backup de cofre ou código MFA/recuperação;
- CPF, telefone, e-mail, endereço, saúde ou comprovante reais;
- banco, dump, backup ou planilha de produção;
- log bruto com dado pessoal, sessão, URL assinada ou segredo;
- arquivo enviado por aluno ou documento interno não autorizado;
- configuração privada de infraestrutura;
- artefato compilado, dependência baixada ou arquivo temporário sem decisão explícita de versionamento.

Permitido:

- dados sintéticos claramente fictícios;
- `.env.example` somente com nomes e valores falsos;
- chaves públicas destinadas a publicação;
- exemplos que não identifiquem pessoa real;
- fixtures mínimas criadas especificamente para testes.

Se um segredo ou dado real for commitado:

1. não reproduza o valor na mensagem seguinte;
2. comunique o responsável por segurança;
3. revogue ou rotacione o segredo;
4. preserve evidências e avalie o incidente;
5. remova do estado atual e, quando aprovado, do histórico;
6. verifique logs, forks, caches e artefatos;
7. registre teste/controle que evite recorrência.

## 18. Arquivos gerados, dependências e formatação

- Não versione pastas de build, IDE, cache, dependências baixadas, logs ou arquivos temporários.
- Arquivos gerados só devem entrar no Git quando forem fonte oficial necessária à execução, distribuição ou revisão.
- Migrações, OpenAPI, SBOM e lockfiles podem ser versionados conforme a arquitetura e o ecossistema.
- Atualização automática de dependência deve ter commit próprio ou grupo coerente, com testes e relatório de segurança/licença.
- Formatação ampla deve ficar em commit separado para não ocultar mudança funcional.

## 19. Exemplos válidos

### Funcionalidade

```text
feat(frequencia): registra conteúdo obrigatório com a chamada

Salva presença e diário de aula na mesma transação. A operação falha
integralmente quando o conteúdo não é informado.

Refs: RF-FRQ-003, RF-FRQ-004, RN-014, UC-PRF-02
Closes: #180
```

### Correção

```text
fix(espera): impede duas ofertas ativas para a mesma vaga

Aplica bloqueio transacional durante a seleção do primeiro aluno elegível
e torna o processamento idempotente.

Refs: RF-INS-004, RN-010, UC-AUT-01
```

### Documentação

```text
docs(security): documenta limites iniciais da API

Refs: RNF-SEG-005, SEG-API-006
```

### Teste

```text
test(autorizacao): cobre acesso de professor a turma não vinculada

Refs: RF-FRQ-002, RN-013, UC-PRF-01, SEG-AUTZ-003
```

### Banco

```text
feat(database): adiciona unicidade normalizada para CPF

A migração identifica colisões antes de criar a constraint e não registra
os valores encontrados nos logs.

Refs: RN-016, RF-IDN-001, SEG-DB-004
```

### Dependência de segurança

```text
security(deps): atualiza componente afetado por vulnerabilidade alta

Atualiza a versão suportada e mantém compatibilidade com o build atual.
O relatório detalhado permanece na tarefa restrita de segurança.

Relates-to: SEC-2026-008
```

### Pipeline

```text
ci(security): bloqueia pull request com segredo detectado

Refs: SEG-CICD-005, SEG-SEG-002
```

### Refatoração

```text
refactor(inscricao): centraliza validações de elegibilidade

Mantém contratos e regras existentes, reduzindo duplicidade entre criação
direta, lista de espera e processo seletivo.

Refs: RF-INS-001, RN-001, RN-008, RN-012
```

## 20. Exemplos inválidos

| Mensagem inválida | Problema | Versão melhor |
| --- | --- | --- |
| `update` | Sem tipo, escopo ou resultado | `docs(requisitos): atualiza regras de inscrição` |
| `fix: ajustes` | Descrição vaga | `fix(inscricao): rejeita terceira modalidade simultânea` |
| `feat aluno` | Formato inválido | `feat(aluno): permite atualizar dados de contato` |
| `FEAT(Auth): Adicionando Login.` | Caixa, tempo verbal e pontuação incorretos | `feat(auth): adiciona autenticação por e-mail` |
| `chore: nova funcionalidade de relatórios` | Tipo esconde funcionalidade | `feat(relatorio): adiciona exportação em PDF` |
| `refactor(api): altera resposta e remove campo` | Mudança externa não é refatoração | `feat(api)!: remove campo obsoleto da resposta` |
| `fix(security): senha=abc123` | Expõe segredo | Revogar o valor e usar `security(auth): remove credencial versionada` |
| `feat: tudo da sprint` | Mistura mudanças não relacionadas | Dividir em commits por unidade lógica |
| `fix: tentativa 2` | Não explica causa nem resultado | `fix(turma): impede capacidade menor que inscrições ativas` |
| `docs: final final agora vai` | Sem significado durável | `docs(casos-de-uso): detalha fluxo de confirmação de vaga` |

## 21. Checklist antes do commit

### Conteúdo

- [ ] O commit representa uma única mudança lógica.
- [ ] Tipo e escopo representam a intenção principal.
- [ ] A descrição explica o resultado e respeita o formato.
- [ ] O corpo explica motivo, impacto e limitações quando necessário.
- [ ] Requisitos, casos, controles e tarefa estão referenciados quando aplicável.
- [ ] Breaking change foi marcado e possui migração/rollback.

### Qualidade

- [ ] O código compila e os testes relevantes passam.
- [ ] Foram adicionados testes da mudança e de regressão quando necessário.
- [ ] Formatação, lint e análise estática foram executados.
- [ ] Documentação, contrato de API e migração foram atualizados quando afetados.
- [ ] O diff foi revisado e não contém arquivo acidental ou alteração não relacionada.

### Segurança e privacidade

- [ ] Não há segredo, credencial, `.env` real ou configuração privada.
- [ ] Não há dado pessoal/sensível real, comprovante, dump ou log bruto.
- [ ] Não há artefato, cache, dependência baixada ou arquivo temporário indevido.
- [ ] Autorização, validação, logs e erros foram avaliados quando o fluxo mudou.
- [ ] Secret scanning e análise de dependências não apontam problema bloqueador.
- [ ] O commit não divulga detalhes exploráveis de vulnerabilidade não corrigida.

## 22. Checklist antes do Pull Request e merge

- [ ] O histórico não contém `WIP`, `fixup!`, mensagens vagas ou commits quebrados.
- [ ] O título do PR segue o mesmo formato de commit.
- [ ] A descrição do PR consolida motivação, solução, testes, riscos e rastreabilidade.
- [ ] Checks obrigatórios estão aprovados.
- [ ] Houve revisão humana independente.
- [ ] Mudanças críticas receberam o aprovador exigido.
- [ ] O artefato não contém segredo, dados reais ou vulnerabilidade bloqueadora.
- [ ] Documentos afetados foram atualizados no mesmo PR.
- [ ] Plano de deploy, migração e rollback existe quando necessário.
- [ ] O método de merge preservará uma mensagem válida e útil.

## 23. Título e descrição do Pull Request

O título do PR DEVE seguir o formato do commit:

```text
feat(inscricao): adiciona lista de espera ordenada
```

A descrição DEVE conter, no mínimo:

```markdown
## Motivo

Problema ou necessidade atendida.

## Solução

Comportamento implementado e decisões relevantes.

## Como testar

Passos e evidências com dados sintéticos.

## Riscos e impactos

Segurança, privacidade, compatibilidade, banco, deploy e limitações.

## Rastreabilidade

Refs: RF-INS-002, RN-009, UC-INS-03
Closes: #123

## Checklist

- [ ] Testes
- [ ] Documentação
- [ ] Segurança e privacidade
- [ ] Migração/rollback quando aplicável
```

## 24. Exceções

Exceção a este padrão deve:

- ter motivo concreto;
- ser limitada a um escopo e período;
- indicar impacto no histórico, automação ou auditoria;
- ser aprovada pelo Tech Lead;
- incluir segurança quando puder afetar rastreabilidade, assinatura ou divulgação;
- possuir plano para retornar ao padrão.

Commits automáticos de ferramenta podem usar formato próprio somente quando a origem for confiável, a identidade do bot estiver protegida e o histórico continuar rastreável.

## 25. Critérios de aprovação deste documento

- [ ] Tipos e escopos foram validados pela equipe.
- [ ] Idioma, limite de título e política de corpo foram aprovados.
- [ ] Ferramenta de tarefas e formato de referência foram definidos.
- [ ] Método de merge e proteção da branch principal foram definidos.
- [ ] Política de assinatura foi aprovada e possui instruções de configuração.
- [ ] Pipeline valida formato, segredos e checks obrigatórios.
- [ ] Segurança revisou divulgação e dados proibidos.
- [ ] Tech Lead aprovou a versão `1.0`.

## 26. Histórico de versões

| Versão | Data | Autor | Alterações | Situação |
| --- | --- | --- | --- | --- |
| `0.1.0` | 13/08/2026 | Heitor Leite | Primeira convenção de commits, incluindo formato, tipos, escopos, rastreabilidade, breaking changes, segurança, assinatura, exemplos e checklists | Rascunho |

