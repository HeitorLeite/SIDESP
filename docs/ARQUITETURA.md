# Arquitetura de Software — SIDESP

> **SIDESP — Sistema Integrado de Desenvolvimento Esportivo Público**

## Identificação do documento

| Campo | Valor |
| --- | --- |
| Documento | Arquitetura de Software |
| Projeto | SIDESP — Sistema Integrado de Desenvolvimento Esportivo Público |
| Versão | `0.2.0` |
| Data | 17/08/2026 |
| Status | **Pronto para revisão — ainda não aprovado para implementação** |
| Classificação | Uso interno |
| Responsável técnico / Segurança / Privacidade interna | Heitor Leite |
| Responsável de negócio / Scrum Master | Kauãn Raphael |
| Product Owner | Livia Andrade |
| QA | Micael Phillipini |
| Revisores necessários | Produto, Backend, Frontend, Dados, Segurança, Privacidade e Operações |
| Documentos relacionados | `LEVANTAMENTO_DE_REQUISITOS.md`, `CASOS_DE_USO.md`, `CLASSES_OU_COMPONENTES.md`, `ATIVIDADES.md`, `../database/BANCO_DE_DADOS.md` e `SEGURANCA.md` |

## Aprovações

| Papel | Responsável | Situação | Data |
| --- | --- | --- | --- |
| Responsável de negócio / Scrum Master | Kauãn Raphael | Pendente | — |
| Product Owner | Livia Andrade | Pendente | — |
| Liderança técnica | Heitor Leite | Pendente | — |
| Backend | Heitor Leite | Pendente | — |
| Segurança e privacidade interna | Heitor Leite | Pendente | — |
| QA | Micael Phillipini | Pendente | — |
| Operações/Infraestrutura para implantação | Prefeitura/Embrass | Alinhamento futuro | — |

## 1. Objetivo e escopo

Este documento explica a estrutura técnica planejada do SIDESP, suas fronteiras de responsabilidade, fontes da verdade, integrações, fluxos de dados e decisões que devem ser preservadas durante o desenvolvimento.

A arquitetura descreve o produto completo, inclusive o frontend em Angular/TypeScript, a API e o backend em Java/Spring Boot, os dados em MySQL e os serviços externos. O código sob responsabilidade desta frente será o backend. A Prefeitura e a Embrass fornecerão hospedagem, banco e infraestrutura; a topologia e as responsabilidades operacionais serão homologadas antes da implantação.

O documento deve permitir responder:

- onde autenticação e autorização são aplicadas;
- quais componentes podem acessar cada dado;
- onde permanecem estado, arquivos e segredos;
- como requisições repetidas evitam efeitos duplicados;
- como notificações, jobs e exportações funcionam sem corromper o fluxo principal;
- como o serviço se comporta quando banco, internet ou fornecedor falham;
- como implantar, observar, recuperar e evoluir a solução com segurança.

## 2. Estado das decisões arquiteturais

| Decisão | Direção inicial | Status |
| --- | --- | --- |
| Estilo do backend | Monólito modular, orientado a domínios, com portas e adaptadores | Confirmado em 17/08/2026 |
| Linguagem/backend | Java LTS e Spring Boot estável/compatível, com versões exatas fixadas no início | Confirmado em 17/08/2026 |
| Interface principal | API HTTP REST/JSON em `/api/v1`, documentada por OpenAPI | Confirmado em 17/08/2026 |
| Frontend | Angular/TypeScript estável e com suporte ativo, com versão exata fixada no início | Confirmado pela equipe |
| Autenticação web | Sessão opaca no servidor, persistida no MySQL, e cookie seguro | Confirmado pelas decisões de identidade e dados |
| Autorização | RBAC com validação adicional por objeto, vínculo e vigência no backend | Base confirmada; detalhamento por endpoint será testado e documentado |
| Banco | MySQL `8.x`, preferencialmente versão LTS homologada | Confirmado no modelo de dados |
| Migrações | Flyway, scripts versionados e imutáveis após aplicação | Confirmado em 17/08/2026 |
| Arquivos | Storage privado compatível com S3 e ClamAV; banco guarda metadados e autorização | Baseline confirmada; equivalente pode ser homologado |
| Assíncrono inicial | Outbox transacional e worker no mesmo produto, sem broker externo | Confirmado em 17/08/2026 |
| Broker de mensagens | Não faz parte da baseline inicial | Adiar até necessidade comprovada |
| Integrações | Portas internas e adaptadores por fornecedor | Confirmado com o estilo arquitetural |
| Origem pública | Angular em `/` e API em `/api/v1` no mesmo endereço; origens separadas somente se a infraestrutura exigir | Confirmado em 17/08/2026 |
| Implantação | Uma imagem Spring Boot; API e worker separados em produção e combináveis em desenvolvimento/testes | Confirmado logicamente; infraestrutura Prefeitura/Embrass |
| Ambientes permanentes | `desenvolvimento/testes` e `produção` | Confirmado em 17/08/2026 |
| Rascunho offline | IndexedDB criptografado, dados mínimos e expiração máxima de 24 horas | Confirmado em 17/08/2026 |
| Segredos e observabilidade | Cofre da infraestrutura; logs JSON e padrão OpenTelemetry | Confirmado logicamente; ferramentas serão alinhadas com Prefeitura/Embrass |
| Versões e nomes | Versões LTS/estáveis fixadas; pacote `com.github.heitorleite.sidesp`, artefatos `sidesp-backend` e `sidesp-web` | Confirmado em 17/08/2026 |
| Multi-tenancy | Instância de uma única Secretaria | Confirmado pelo escopo atual; revisar se o produto mudar |

Versões exatas, fornecedores externos e detalhes físicos de produção precisam de homologação antes da implantação, mesmo quando a direção lógica já estiver aprovada.

### 2.1 Glossário técnico

| Termo | Significado no documento |
| --- | --- |
| Monólito modular | Uma única aplicação implantável, organizada internamente em módulos com responsabilidades separadas. |
| Porta e adaptador | Separação entre a regra do sistema e a tecnologia usada para HTTP, banco, arquivos ou fornecedor externo. |
| API REST | Forma padronizada de o frontend conversar com o backend por HTTP e recursos identificados por URLs. |
| OpenAPI | Arquivo que documenta operações, entradas, respostas e erros da API. |
| Sessão opaca | Identificador sem dados do usuário, guardado em cookie; o estado real da sessão fica no servidor. |
| Cookie `HttpOnly` | Cookie que o JavaScript do navegador não consegue ler, reduzindo o risco de roubo por script. |
| Outbox | Tabela gravada junto da operação principal para que uma notificação ou tarefa posterior não seja perdida. |
| Worker | Processo que executa tarefas em segundo plano, como notificações e expirações. |
| Job | Tarefa automática executada em determinado momento ou intervalo. |
| Idempotência | Garantia de que repetir a mesma solicitação não duplica o efeito. |
| Cache | Cópia temporária usada para acelerar leituras; nunca substitui a fonte oficial dos dados. |
| Broker de mensagens | Serviço especializado em filas/eventos; não será usado inicialmente sem necessidade comprovada. |
| Storage privado | Armazenamento de arquivos sem acesso público direto, liberado somente após autorização. |
| IndexedDB | Banco local do navegador usado aqui apenas para o rascunho temporário e criptografado da chamada. |
| S3 compatível | Padrão de acesso a armazenamento de arquivos que permite trocar a solução física por outra equivalente. |
| ClamAV | Ferramenta de código aberto que verifica arquivos em busca de conteúdo malicioso conhecido. |
| Cofre de segredos | Serviço protegido para senhas técnicas, chaves e credenciais da aplicação. |
| OpenTelemetry | Padrão aberto para produzir métricas, rastreamentos e informações técnicas sem prender o sistema a uma ferramenta visual específica. |
| ADR | Registro curto de uma decisão arquitetural, suas alternativas e consequências. |
| RPO | Perda máxima de dados aceita após falha; inicialmente 24 horas. |
| RTO | Tempo máximo planejado para restaurar o serviço; inicialmente 8 horas. |

## 3. Contexto e objetivos arquiteturais

O SIDESP centraliza operações atualmente manuais e dispersas: cadastros, oferta esportiva, inscrições, lista de espera, processos seletivos, chamadas, frequência, justificativas, comunicação e informações gerenciais.

Os principais objetivos arquiteturais são:

1. **integridade transacional:** vagas, fila, frequência e decisões não podem divergir sob concorrência ou repetição;
2. **segurança por padrão:** nenhum identificador fornecido pelo cliente concede acesso por si só;
3. **rastreabilidade:** transições críticas preservam autor, instante, motivo e estado;
4. **isolamento de fornecedores:** e-mail e, em versões futuras, WhatsApp, mapas e geradores de exportação não entram nas regras centrais do sistema;
5. **evolução incremental:** começar com uma unidade implantável simples, mantendo módulos substituíveis;
6. **operação degradada:** falha externa não corrompe nem bloqueia funções independentes;
7. **privacidade:** dados de menores, frequência, comprovantes e exportações recebem acesso e retenção específicos;
8. **portabilidade:** dados, arquivos e contratos permanecem documentados para evitar dependência irreversível.

## 4. Requisitos que dirigem a arquitetura

| Direcionador | Consequência arquitetural |
| --- | --- |
| Autorização por papel, objeto e vínculo | Identidade derivada da sessão; políticas executadas no backend e repetidas em jobs/consumidores |
| Última vaga e fila concorrente | Banco relacional, transações, constraints, bloqueio/controle de versão e idempotência |
| Chamada atômica e correção auditável | Agregado transacional, histórico imutável e concorrência otimista |
| Notificação por evento | Outbox na mesma transação do negócio e worker com retentativa limitada |
| Upload potencialmente sensível | Quarentena, varredura, storage privado e autorização por objeto |
| Relatórios e mapas futuros | Consultas autorizadas, definições versionadas, agregação e supressão de grupos pequenos quando entrarem no escopo |
| Conectividade instável nos polos | Rascunho local de chamada por até 24 horas, marcado como não sincronizado; somente confirmação do servidor conclui o salvamento |
| Segurança e privacidade | Sessão revogável, MFA administrativo, cofre, minimização, auditoria e descarte |
| Metas iniciais de desempenho/volume | Validar 5.000 alunos, 200 turmas, 100 usuários simultâneos, p95 de 2 segundos em consultas e 3 segundos em alterações antes da implantação |
| Orçamento e contratação pública | Componentes substituíveis, custos mensuráveis e ausência de dependência não justificada |

Rastreabilidade principal: `RNF-SEG-*`, `RNF-PRI-*`, `RNF-DSP-001`, `RNF-CAP-001`, `RNF-DIS-001`, `RNF-RES-001`, `RNF-CON-001`, `RNF-OBS-001`, `RNF-MAN-001`, `RNF-EXP-001` e `RNF-POR-001`.

## 5. Visão de contexto

```mermaid
flowchart LR
    publico["Usuário público"]
    aluno["Aluno"]
    professor["Professor"]
    admin["Administrador/Gestor"]
    suporte["Suporte e operação autorizados"]
    sidesp["SIDESP"]
    whatsapp["Provedor de WhatsApp\nexpansão futura"]
    email["Serviço de e-mail/recuperação"]
    mapas["Provedor de mapas\nexpansão futura"]
    storage["Armazenamento privado de arquivos"]
    observabilidade["Plataforma de logs, métricas e alertas"]

    publico -->|"notícias, polos e modalidades"| sidesp
    aluno -->|"conta, inscrição, frequência e justificativa"| sidesp
    professor -->|"turmas, chamada e avisos"| sidesp
    admin -->|"cadastros, decisões e permissões"| sidesp
    suporte -->|"operação controlada e auditada"| sidesp
    sidesp -->|"mensagens mínimas e templates aprovados"| whatsapp
    sidesp -->|"token opaco/aviso mínimo"| email
    sidesp -->|"polos públicos ou agregados"| mapas
    sidesp -->|"comprovantes e futuras exportações"| storage
    sidesp -->|"telemetria minimizada"| observabilidade
```

### 5.1 Atores e confiança

- Navegadores, dispositivos, redes dos polos, callbacks e respostas de fornecedores são externos e não confiáveis.
- O frontend melhora a experiência, mas não é fronteira de autorização.
- Professores e administradores são usuários autenticados, não componentes confiáveis por padrão.
- O backend é o ponto de aplicação dos contratos, autorização, regras, transações e auditoria.
- Banco, storage, cofre e observabilidade são serviços internos protegidos, cada um com identidade e privilégio próprios.

## 6. Visão de contêineres

```mermaid
flowchart TB
    subgraph internet["Internet / dispositivos não confiáveis"]
        navegador["Frontend web no navegador"]
    end

    subgraph borda["Borda pública"]
        dns["DNS/CDN para conteúdo estático — opcional"]
        edge["Proxy reverso / balanceador / WAF"]
    end

    subgraph aplicacao["Rede da aplicação"]
        api["Backend SIDESP\nJava + Spring Boot\nmonólito modular"]
        worker["Worker/jobs SIDESP\nmesmo código e módulos\nprocesso ou perfil separado"]
    end

    subgraph dados["Rede de dados privada"]
        db[("MySQL 8.x\ndados e sessões")]
        files[("Storage privado\ncomprovantes/exportações")]
        vault["Cofre de segredos"]
    end

    subgraph operacao["Operação protegida"]
        telemetry["Logs, métricas, traces e alertas"]
        backup["Backups criptografados"]
    end

    subgraph terceiros["Fornecedores externos"]
        msg["WhatsApp\nfuturo"]
        mail["E-mail/identidade"]
        map["Mapas\nfuturo"]
    end

    navegador --> dns
    navegador -->|"HTTPS"| edge
    edge -->|"HTTPS/rede privada"| api
    api --> db
    api --> files
    api --> vault
    worker --> db
    worker --> files
    worker --> vault
    worker --> msg
    worker --> mail
    api --> map
    api --> telemetry
    worker --> telemetry
    db --> backup
    files --> backup
```

### 6.1 Responsabilidades dos contêineres

| Contêiner | Responsabilidade | Não deve fazer |
| --- | --- | --- |
| Frontend web | Apresentação, acessibilidade, navegação, validação de experiência e consumo do contrato | Autorizar, guardar segredo, decidir regra ou persistir credencial em storage JavaScript |
| Borda | TLS, roteamento, limites de conexão/tamanho, headers e proteção complementar | Substituir autorização do backend ou expor endpoints de gestão |
| Backend HTTP | Contrato, sessão, autorização, casos de uso síncronos, transações e resposta | Chamar fornecedor diretamente dentro da transação quando o efeito puder ser assíncrono |
| Worker/jobs | Outbox, retentativas, expirações, cálculos, publicação agendada e geração de arquivos | Criar regra paralela diferente da usada pela API |
| MySQL | Fonte da verdade transacional, sessões, constraints, metadados e auditoria | Ser acessível pelo frontend ou por usuário funcional via SQL |
| Storage privado | Conteúdo de comprovantes e exportações | Autorizar sozinho ou usar nome do usuário como caminho |
| Cache futuro | Acelerar somente leituras medidas e aprovadas; sessões ficam inicialmente no MySQL | Virar fonte da verdade de vaga, inscrição, frequência ou permissão |
| Cofre | Segredos e chaves por ambiente/workload | Entregar segredo ao navegador ou registrar valor em log |
| Observabilidade | Telemetria, investigação e alerta | Receber senha, token, comprovante ou dado sensível desnecessário |

## 7. Estilo arquitetural do backend

### 7.1 Monólito modular

O backend começa como uma única base de código, uma única imagem e uma única unidade de release. Em produção, a API e o worker executam como processos ou containers separados a partir da mesma imagem; em desenvolvimento/testes, podem funcionar juntos. Internamente, cada domínio possui fronteira, pacote, modelo e interfaces explícitos.

Essa escolha reduz custo operacional e transações distribuídas no início, sem aceitar um “monólito sem fronteiras”. Um módulo não acessa diretamente tabelas ou classes internas de outro módulo. A colaboração ocorre por serviço de aplicação publicado, porta de consulta ou evento interno.

Microsserviços somente serão considerados quando houver evidência, por exemplo:

- necessidade de escala ou disponibilidade realmente distinta;
- ciclo de implantação independente com equipe responsável;
- isolamento regulatório ou de risco;
- volume assíncrono que não possa ser atendido pelo worker/outbox;
- medição mostrando que a separação compensa rede, consistência e operação adicionais.

### 7.2 Portas e adaptadores

```mermaid
flowchart LR
    http["Adaptador HTTP\ncontrollers + DTOs"]
    job["Adaptador de jobs/outbox"]
    app["Aplicação\ncasos de uso + transações"]
    domain["Domínio\nentidades + políticas + invariantes"]
    repo["Portas de repositório"]
    ext["Portas externas"]
    persistence["Adaptador de persistência\nJPA/SQL"]
    providers["Adaptadores de fornecedor\nWhatsApp, e-mail, mapas"]
    files["Adaptador de arquivos"]

    http --> app
    job --> app
    app --> domain
    app --> repo
    app --> ext
    repo --> persistence
    ext --> providers
    ext --> files
```

Regras de dependência:

- domínio não depende de Spring, JPA, HTTP, SDK de fornecedor ou storage;
- controller não implementa regra de negócio nem acessa repositório diretamente;
- serviço de aplicação coordena autorização contextual, transação, idempotência e eventos;
- adaptadores dependem das portas definidas pela aplicação/domínio;
- entidade JPA não é serializada diretamente na API;
- DTOs usam allowlist e não recebem dono, papel, auditoria ou estado controlado pelo servidor;
- jobs invocam os mesmos casos de uso e políticas do fluxo HTTP.

## 8. Módulos do produto

```mermaid
flowchart LR
    public["Conteúdo público\nnotícias e oferta"]
    identity["Identidade e acesso"]
    sports["Estrutura esportiva"]
    enrollment["Inscrições e seleção"]
    attendance["Frequência e justificativas"]
    communication["Comunicação"]
    reporting["Relatórios e análises\nexpansão futura"]
    files["Arquivos"]
    audit["Auditoria"]

    public --> sports
    public --> identity
    enrollment --> identity
    enrollment --> sports
    attendance --> identity
    attendance --> sports
    attendance --> enrollment
    communication -.->|"consome eventos"| enrollment
    communication -.->|"consome eventos"| attendance
    reporting -.->|"consultas autorizadas"| sports
    reporting -.->|"consultas autorizadas"| enrollment
    reporting -.->|"consultas autorizadas"| attendance
    attendance --> files
    reporting --> files
    identity -.-> audit
    sports -.-> audit
    enrollment -.-> audit
    attendance -.-> audit
    communication -.-> audit
    reporting -.-> audit
```

| Módulo | Responsabilidades | Dados que controla |
| --- | --- | --- |
| `public-content` | Notícias publicadas, detalhes públicos, consulta pública de polos/modalidades | Notícias e versões; projeções públicas |
| `identity-access` | Cadastro, credencial, sessão, recuperação, papéis, permissões e responsáveis | Usuários, perfis, credenciais, sessões, vínculos e RBAC |
| `sports-catalog` | Polos, modalidades, regras, turmas, agendas, aulas e vínculos de professor | Estrutura esportiva e vigências |
| `enrollment` | Elegibilidade, inscrição, fila, oferta, seleção, exceção e histórico | Inscrições, espera, ofertas e candidaturas |
| `attendance` | Chamada, diário, frequência, correção, justificativa e apuração de faltas | Frequência, decisões e metadados do comprovante |
| `communication` | Eventos, destinatários, templates, tentativas, callbacks e fallback | Notificações, outbox e entrega |
| `reporting` | Expansão futura para indicadores, filtros, agregação, mapas, resultados e exportações | Definições e resultados derivados, somente após aprovação da Secretaria |
| `files` | Quarentena, varredura, armazenamento, download autorizado e descarte | Metadados técnicos e acesso ao objeto privado |
| `audit` | Registro imutável/minimizado de ações críticas | Eventos de auditoria e correlação |

### 8.1 Regras entre módulos

- Um módulo é proprietário das alterações em suas tabelas.
- Consulta cruzada simples usa porta de leitura; alteração cruzada usa caso de uso publicado ou evento.
- Comunicação não consulta listas arbitrárias recebidas do frontend; resolve destinatários a partir do evento e dos módulos proprietários.
- Reporting não altera domínio operacional e não acessa credenciais, sessões ou conteúdo de comprovante.
- Files não decide sozinho se um usuário pode baixar; o módulo proprietário fornece a política do objeto.
- Shared kernel deve conter apenas tipos realmente transversais, como `Identifier`, `Clock`, `ActorContext`, resultado e código de erro. Entidades de negócio não entram nele.

### 8.2 Estrutura de pacotes proposta

```text
com.github.heitorleite.sidesp
├── identity
│   ├── api
│   ├── application
│   ├── domain
│   └── infrastructure
├── sports
├── enrollment
├── attendance
├── communication
├── reporting
├── files
├── audit
└── shared
    ├── application
    ├── domain
    └── infrastructure
```

Cada módulo repete a separação `api/application/domain/infrastructure`. O pacote-base acadêmico será `com.github.heitorleite.sidesp`, o artefato do backend será `sidesp-backend` e a aplicação Angular será `sidesp-web`. Qualquer mudança futura de namespace oficial exige decisão registrada e migração planejada.

## 9. Frontend e código público no navegador

O frontend será uma aplicação Angular/TypeScript responsiva e acessível, baseada também nos [protótipos existentes no Figma](https://www.figma.com/design/41evldxEaPMcPxX15Z5CLd/Untitled?node-id=0-1&m=dev&t=7mEnpP0Kx9jTrD0K-1). A forma física de hospedagem será alinhada com Prefeitura e Embrass; o frontend sempre consumirá a API própria do SIDESP e nunca acessará o MySQL diretamente.

Responsabilidades:

- renderizar telas e estados de sucesso, vazio, validação, indisponibilidade e acesso negado;
- executar validação de experiência, sem substituir validação do servidor;
- enviar cookie de sessão somente pelo mecanismo do navegador;
- enviar proteção CSRF conforme contrato;
- preservar `correlationId` recebido para suporte sem expor detalhes internos;
- aplicar acessibilidade WCAG 2.2 AA nos fluxos críticos, conforme requisito proposto.

Tudo enviado ao navegador é público, inclusive:

- JavaScript, HTML, CSS, source maps publicados e assets;
- URL/base pública da API;
- flags de interface e configurações serializadas;
- chaves explicitamente públicas de mapa, somente se o fornecedor permitir e com restrição por origem/API.

Nunca são enviados ao frontend:

- senha de banco, segredo de sessão, chave de API privada ou segredo de webhook;
- credencial de storage, chave de assinatura ou connection string;
- decisão de autorização baseada apenas na interface;
- caminho físico, bucket interno ou metadado restrito de arquivo;
- hash de senha/token ou detalhe administrativo desnecessário.

O frontend não persiste session ID, access token ou refresh token em `localStorage`, `sessionStorage`, IndexedDB, Cache API ou URL.

### 9.1 Rascunho parcialmente offline da chamada

O rascunho de chamada é a única exceção funcional de persistência local autenticada na primeira versão:

- usa IndexedDB, nunca `localStorage`, e mantém o conteúdo criptografado com os recursos seguros do navegador;
- guarda somente identificadores da aula e dos alunos, nomes mínimos para a chamada, `PRESENTE`/`AUSENTE`, versão, horários e chave de idempotência;
- não guarda CPF, contato, ficha de saúde, justificativa, comprovante, sessão ou token;
- fica claramente marcado como **não sincronizado** e não altera o estado oficial até o servidor confirmar;
- é apagado depois da sincronização, do logout ou ao completar 24 horas;
- uma versão já salva no servidor nunca é sobrescrita automaticamente; conflito exige recarga e, quando cabível, correção administrativa conforme a regra aprovada.

## 10. APIs e contratos

### 10.1 Estilo do contrato

- API HTTP REST/JSON sob o prefixo versionado `/api/v1`.
- OpenAPI versionado é a fonte do contrato HTTP e deve permanecer sincronizado com rotas e DTOs.
- Compatibilidade deve ser preservada dentro da versão; mudança incompatível exige nova versão ou janela formal de depreciação.
- Datas usam ISO 8601; instantes preservam offset/UTC; apresentação usa `America/Sao_Paulo` após ratificação.
- Identificadores externos são UUIDs opacos e nunca funcionam como autorização.
- Paginação, filtro e ordenação usam allowlists e limites aprovados.
- Respostas não serializam entidades de persistência diretamente.

### 10.2 Status e erros

| Situação | Resposta esperada |
| --- | --- |
| Sucesso de leitura | `200` |
| Criação concluída | `201` com referência segura |
| Processamento assíncrono aceito | `202` com recurso de acompanhamento |
| Operação idempotente sem corpo | `204` ou resposta original documentada |
| Entrada inválida | `400`/`422`, conforme convenção única aprovada |
| Não autenticado | `401` sem revelar dado |
| Não autorizado | `403`; `404` pode ser usado para não revelar existência conforme política |
| Conflito/versão/estado | `409` |
| Limite excedido | `429` com `Retry-After` quando aplicável |
| Falha interna | `500` com código seguro e correlação |
| Dependência indisponível | `502`/`503` somente quando o fluxo depender dela |

Erros seguem Problem Details (`application/problem+json`) ou formato equivalente aprovado, contendo código estável, mensagem segura, status, caminho lógico e `correlationId`. Stack trace, SQL, caminho interno e segredo não chegam ao cliente.

### 10.3 Cabeçalhos e controles

- `Idempotency-Key` ou identificador de comando em inscrição, cancelamento, confirmação, chamada, decisão, exportação e envio manual.
- `If-Match`/versão equivalente pode proteger alterações concorrentes de recursos administrativos.
- CSRF obrigatório para operação mutável autenticada por cookie.
- CORS por allowlist exata de origem, método e header em cada ambiente.
- Limites de corpo, arquivo, filtros, paginação e resposta aplicados na borda e no backend.
- Correlation ID é aceito somente em formato limitado ou gerado novamente pelo servidor.

## 11. Identidade, autenticação e autorização

### 11.1 Autenticação

A baseline definida é sessão opaca no servidor:

1. login recebe CPF ou e-mail e senha por HTTPS;
2. o backend normaliza o identificador e responde de forma antienumeração;
3. a credencial é verificada por hash de senha aprovado;
4. sessão nova é criada/rotacionada no servidor;
5. o navegador recebe cookie `Secure`, `HttpOnly`, `SameSite` aprovado, sem `Domain` e preferencialmente `__Host-`;
6. logout, recuperação, mudança de senha, revogação e incidente invalidam a sessão no servidor.

MFA por código de e-mail é obrigatório em todo login administrativo. Recuperação usa link no e-mail confirmado; recuperação de senha revoga todas as sessões. OAuth/OIDC ou JWT somente entram futuramente mediante ADR e modelo de ameaças.

### 11.2 Ponto real de autorização

A autorização é aplicada no backend em duas etapas complementares:

1. **serviço de aplicação/política:** valida papel e permissão para executar o caso de uso;
2. **regra contextual/repositório:** limita o objeto pelo aluno autenticado, vínculo vigente do professor, turma, estado e escopo administrativo.

Exemplos:

- aluno consulta inscrição por `sessao.usuarioId`, não por `alunoId` aceito livremente;
- professor registra chamada somente se o vínculo com a turma estiver vigente na data aplicável;
- administrador parcial precisa da permissão específica e não pode elevar a si próprio;
- visualizar relatório não concede automaticamente exportar;
- acessar justificativa não concede acesso ao comprovante;
- job, worker e webhook usam identidade técnica e as mesmas invariantes do negócio.

Spring Security pode aplicar autenticação, CSRF, sessão e autorização de entrada. A decisão final por objeto permanece no caso de uso/domínio e é testada contra BOLA/IDOR.

## 12. Processamento síncrono

Operações interativas simples são síncronas quando podem concluir dentro da meta aprovada sem depender de fornecedor externo.

```mermaid
sequenceDiagram
    autonumber
    actor U as Usuário
    participant F as Frontend
    participant C as Controller
    participant A as Serviço de aplicação
    participant D as Domínio/política
    participant P as Persistência
    participant DB as MySQL

    U->>F: Executa ação
    F->>C: HTTPS + sessão + CSRF + idempotência
    C->>C: Valida contrato e identidade
    C->>A: Comando com ActorContext
    A->>A: Autoriza ação e objeto
    A->>P: Busca estado necessário
    P->>DB: Consulta parametrizada
    DB-->>P: Estado/versionamento
    P-->>A: Entidades/visões
    A->>D: Aplica invariantes
    D-->>A: Novo estado ou erro de domínio
    A->>P: Persiste estado + histórico + evento/outbox
    P->>DB: Transação atômica
    DB-->>P: Commit confirmado
    P-->>A: Referência/versão
    A-->>C: Resultado seguro
    C-->>F: Status + DTO + correlationId
    F-->>U: Sucesso somente após confirmação
```

Se o commit não for confirmado, o frontend não exibe sucesso. Em estado desconhecido por perda de conexão, a operação é consultada/repetida pela mesma chave idempotente.

## 13. Processamento assíncrono, outbox e jobs

### 13.1 Outbox transacional

Notificação, publicação externa e outros efeitos não essenciais ao commit principal usam outbox:

```mermaid
sequenceDiagram
    autonumber
    participant A as Caso de uso
    participant DB as MySQL
    participant W as Worker de outbox
    participant N as Módulo de comunicação
    participant X as Provedor externo

    A->>DB: Estado de negócio + evento + outbox
    DB-->>A: Commit único
    A-->>A: Fluxo principal concluído
    loop Busca controlada
        W->>DB: Reserva itens pendentes com lease/bloqueio
        DB-->>W: Evento mínimo
        W->>N: Tratar evento idempotente
        N->>DB: Criar notificação/destinatários/tentativa
        N->>X: Enviar com timeout e chave idempotente
        alt Aceito/entregue
            X-->>N: ID e estado do provedor
            N->>DB: Atualizar tentativa e processar outbox
        else Falha temporária
            X-->>N: Timeout/erro recuperável
            N->>DB: Agendar retry com backoff e limite
        else Falha definitiva
            X-->>N: Erro não recuperável
            N->>DB: Marcar falha final e gerar alerta/fallback
        end
    end
```

Garantias:

- estado de negócio e outbox são gravados na mesma transação;
- entrega interna é pelo menos uma vez, portanto todo consumidor é idempotente;
- worker reserva itens por lease/bloqueio e recupera trabalho abandonado;
- retry usa backoff, jitter, limite e classificação de erro;
- dead-letter lógico/falha final fica visível e possui reprocessamento administrativo auditado;
- evento contém apenas dados mínimos e referências, nunca comprovante, token ou segredo;
- aceitação pelo provedor e entrega ao destinatário são estados distintos.

### 13.2 Jobs previstos

| Job | Gatilho | Regra de segurança/confiabilidade |
| --- | --- | --- |
| Publicar notícia agendada | Horário aprovado | Condição atômica por estado/instante; idempotente |
| Expirar oferta de vaga | `expira_em` | Lock/versão; passa ao próximo uma única vez |
| Apurar faltas | Chamada/correção e competência | Regra versionada; não cancela com pendência indefinida |
| Reprocessar outbox | Intervalo/fila | Lease, limite e telemetria |
| Atualizar entrega por callback — futuro | Webhook assinado | Só entra com fornecedor externo; identificador único, timestamp e proteção contra repetição |
| Gerar exportação — futuro | Solicitação autorizada | Revalida permissão; arquivo parcial não fica disponível |
| Descartar arquivos e futuras exportações | Expiração/retenção | Relatório de descarte e reconciliação com storage |
| Revogar/limpar sessão e token | Expiração/revogação | Inutilização imediata e limpeza posterior |

### 13.3 Mensageria

Um broker externo não é necessário na primeira baseline. O outbox no banco e worker atendem o estágio atual com menos componentes. Se volume, isolamento ou disponibilidade exigirem broker, o evento publicado continuará vindo do outbox; a adoção exige ADR, contrato de schema, particionamento, retenção, dead-letter, autenticação e custo.

## 14. Persistência e estado

### 14.1 Fontes da verdade

| Informação | Fonte da verdade | Observação |
| --- | --- | --- |
| Usuário, papel e permissão | MySQL do módulo de identidade | Cache nunca autoriza após revogação sem estratégia explícita |
| Polo, modalidade, turma e aula | MySQL do módulo esportivo | Alteração preserva vigência/histórico |
| Inscrição, fila e oferta | MySQL do módulo de inscrição | Constraints/transações resolvem concorrência |
| Chamada e frequência | MySQL do módulo de frequência | Uma chamada por aula; correções não apagam histórico |
| Justificativa/decisão | MySQL; conteúdo no storage privado | Metadado e estado autorizam o objeto |
| Evento e entrega | MySQL/outbox | O provedor externo não é fonte do evento de negócio |
| Arquivo | Metadado no MySQL e conteúdo no storage | Estado `DISPONIVEL/APROVADO` só após confirmação e hash |
| Resultado de relatório | Dados operacionais + versão de indicador/filtro | Exportação é derivada e temporária |
| Sessão | MySQL na primeira versão | Sessão revogável e compartilhada; Redis só entra se medições futuras justificarem |
| Contrato HTTP | OpenAPI versionado | Implementação e testes devem detectar divergência |

### 14.2 Banco e transações

- MySQL 8.x é o banco relacional definido em `../database/BANCO_DE_DADOS.md`; a versão LTS exata será homologada antes do desenvolvimento.
- Flyway é a ferramenta definida para migrações; a aplicação não executa DDL com sua conta comum.
- Transação cobre o agregado e os registros de histórico/outbox diretamente relacionados.
- Isolamento, lock e índice são escolhidos por caso; não se usa transação longa durante chamada externa.
- Consultas são parametrizadas; filtro/ordenação dinâmica usa allowlist.
- JPA pode atender persistência comum; SQL explícito é permitido em consulta crítica/analítica mediante encapsulamento, teste e revisão.
- O pool de conexões possui limite menor que a capacidade segura do banco e métricas de espera/uso.

### 14.3 Arquivos

```mermaid
sequenceDiagram
    autonumber
    actor U as Aluno
    participant API as Backend
    participant DB as MySQL
    participant S as Storage privado
    participant AV as Scanner

    U->>API: Upload autorizado e limitado
    API->>API: Valida tamanho, nome e tipo inicial
    API->>S: Grava com chave aleatória em quarentena
    S-->>API: Confirma objeto + metadado
    API->>DB: Registra hash, tamanho e estado EM_QUARENTENA
    API->>AV: Solicita varredura por referência privada
    alt Arquivo aprovado
        AV-->>API: Tipo real válido e sem ameaça detectada
        API->>S: Move/promove para área privada aprovada
        API->>DB: Estado APROVADO
    else Arquivo rejeitado ou falha
        AV-->>API: Rejeição/indeterminado
        API->>DB: Estado REJEITADO ou pendente seguro
        API->>S: Descarta conforme política
    end
```

Cada justificativa aceita de zero a três arquivos PDF, JPG ou PNG de até 10 MB cada. Quarentena, verificação contra conteúdo malicioso, storage privado e retenção de um ano após a decisão final estão definidos; a tecnologia de storage e o scanner serão homologados com a infraestrutura.

A baseline usa storage privado compatível com S3 e ClamAV para a verificação inicial. Se Prefeitura/Embrass oferecerem soluções equivalentes, somente os adaptadores de storage e scanner mudam; quarentena, autorização, limites, estados, retenção e auditoria permanecem iguais.

## 15. Cache e invalidação

Cache não faz parte da fonte da verdade e será adicionado apenas com objetivo e medição.

Baseline proposta:

- notícias publicadas, polos e modalidades podem usar cache HTTP (`ETag`, `Last-Modified`, `Cache-Control`) e CDN com TTL curto;
- dados autenticados, posição na fila, permissões e comprovantes não são cacheados no navegador como conteúdo offline; somente o rascunho mínimo de chamada segue a exceção controlada da seção 9.1;
- cache de servidor, se necessário, usa chave que inclui versão/escopo de autorização e TTL limitado;
- mudança ou inativação publica invalidação; expiração é fallback, não única garantia para revogação crítica;
- permissão, sessão e oferta de vaga não dependem de valor stale;
- falha do cache deve degradar para a fonte da verdade, sem derrubar o banco por avalanche; aplicar limites e proteção contra stampede.

Redis ou equivalente não será necessário para as sessões da primeira versão, que ficarão no MySQL. Só poderá ser adicionado futuramente para cache ou limites distribuídos após medições, ADR e definição do comportamento em falha.

## 16. Integrações externas

### 16.1 Padrão comum

Cada integração possui uma porta interna e um adaptador. O domínio conhece resultados normalizados, não SDKs ou códigos específicos do fornecedor.

Controles mínimos:

- fornecedor, contrato, país, suboperadores, retenção, SLA, custo e portabilidade aprovados;
- credencial própria por ambiente no cofre;
- allowlist de destino e proteção contra SSRF;
- TLS, timeout curto, retry apenas em falha segura, circuit breaker e limite de concorrência;
- request/response validados e limitados;
- idempotência e correlação sem conteúdo pessoal em log;
- dashboard/alerta e modo degradado documentado;
- troca de fornecedor implementada por novo adaptador, preservando porta e domínio.

### 16.2 WhatsApp/mensagens — expansão futura

- chamado por worker, fora da transação do fluxo principal;
- envia template e parâmetros mínimos, nunca comprovante, CPF completo, senha ou token;
- webhook público dedicado valida assinatura, timestamp e identificador único antes de alterar tentativa;
- callback repetido é ignorado idempotentemente;
- “enviado ao provedor” não é “entregue”;
- falha final aciona fallback aprovado e fica visível ao suporte;
- a integração real fica fora da primeira versão; notificações internas e e-mail atendem os fluxos atuais até a futura aprovação de fornecedor, modelos, custo e regras institucionais.

### 16.3 E-mail/recuperação

- mensagem de recuperação contém token opaco de uso único e curta duração;
- resposta inicial não revela existência de conta;
- link usa HTTPS, não registra token e invalida sessões quando a política determinar;
- é uma integração necessária na primeira versão para confirmação, recuperação, MFA administrativo e comunicação do responsável; provedor e domínio/remetente serão homologados antes da implantação.

### 16.4 Mapas/geocodificação — expansão futura

- somente endereço/coordenada pública de polo ou agregados aprovados atravessam a fronteira;
- posição/endereço de aluno ou responsável nunca é enviado;
- falha mantém lista textual/tabular;
- chave pública no frontend somente se classificada assim pelo fornecedor e restrita por origem/API.

### 16.5 Excel/PDF — expansão futura

- gerador recebe modelo de saída autorizado, não entidade completa;
- planilha neutraliza formula injection;
- PDF não incorpora script, anexo ativo ou recurso remoto desnecessário;
- geração ocorre em processo limitado, com timeout, memória e tamanho máximos;
- arquivo parcial é descartado e nunca publicado como sucesso.

## 17. Fronteiras de confiança e fluxos de dados

```mermaid
flowchart LR
    subgraph Z0["Zona 0 — não confiável"]
        browser["Navegador"]
        webhook["Webhook externo"]
        provider["Resposta de fornecedor"]
        upload["Arquivo enviado"]
    end
    subgraph Z1["Zona 1 — borda"]
        edge["Proxy/WAF/TLS"]
    end
    subgraph Z2["Zona 2 — aplicação"]
        api["API SIDESP"]
        worker["Workers/jobs"]
    end
    subgraph Z3["Zona 3 — dados restritos"]
        db[("Banco")]
        storage[("Storage privado")]
        secret["Cofre"]
    end
    subgraph Z4["Zona 4 — operação"]
        obs["Observabilidade"]
        admin["Acesso operacional nominal"]
    end

    browser -->|"HTTPS, cookie, CSRF, JSON"| edge
    webhook -->|"HTTPS, assinatura, timestamp"| edge
    upload -->|"multipart limitado"| edge
    edge -->|"request validada na aplicação"| api
    api -->|"SQL parametrizado, identidade da aplicação"| db
    api -->|"objeto por chave opaca"| storage
    worker -->|"egress allowlist, payload mínimo"| provider
    api --> secret
    worker --> secret
    api -->|"telemetria minimizada"| obs
    worker -->|"telemetria minimizada"| obs
    admin -->|"MFA, acesso temporário e auditado"| obs
    admin -.->|"excepcional"| db
```

| Fronteira | Dados permitidos | Controles principais |
| --- | --- | --- |
| Navegador → borda/backend | Campos do DTO, cookie, CSRF, arquivo permitido | HTTPS, limites, validação, CORS, rate limit, autenticação |
| Backend → banco | Dados de domínio necessários | Rede privada, identidade de workload, menor privilégio, SQL parametrizado |
| Backend → storage | Conteúdo/arquivo e chave opaca | Quarentena, criptografia, bucket privado, hash, IAM |
| Worker → fornecedor | Template e parâmetros mínimos | Cofre, egress allowlist, timeout, idempotência, contrato |
| Fornecedor → webhook | ID de evento e estado documentados | Assinatura, timestamp, replay protection, schema |
| Aplicação → observabilidade | Evento técnico minimizado | Redação/mascaramento, acesso, retenção, integridade |
| Operação → ambiente | Comando aprovado e evidência | Identidade nominal, MFA, bastion/canal protegido, auditoria |

## 18. Segurança arquitetural

`SEGURANCA.md` é a baseline normativa detalhada. A arquitetura deve preservar:

- TLS em trânsito e redes privadas para banco, storage e management;
- sessão opaca revogável, proteção CSRF e cookies seguros;
- MFA por código de e-mail em todo login administrativo e reautenticação para ações críticas conforme `SEGURANCA.md`;
- autorização no backend por ação, objeto, vínculo, vigência e campo;
- rate limit na borda e aplicação, com proteção específica por fluxo;
- validação allowlist, DTOs, encoding, SQL parametrizado e prevenção de mass assignment;
- storage privado, quarentena, scanner e download reautorizado;
- segregação de contas da aplicação, migração, backup, analytics e suporte;
- segredo em cofre, injetado em runtime, distinto por ambiente;
- logs/auditoria sem senha, token, cookie, comprovante ou dado excessivo;
- CI com testes, SAST, SCA, secret scan, imagem/IaC scan, SBOM e artefato reproduzível;
- procedimento de rotação/revogação de segredo e sessão comprometidos.

O produto não fixa um fornecedor de cofre ou monitoramento. Segredos usam o cofre ou mecanismo equivalente oferecido pela infraestrutura, com criptografia, acesso por ambiente e rotação. Logs técnicos usam JSON estruturado, e métricas/traces seguem OpenTelemetry; Prefeitura/Embrass homologarão as ferramentas de coleta, visualização e alerta.

### 18.1 Revogação

| Evento | Ação arquitetural |
| --- | --- |
| Logout | Revogar sessão no armazenamento central e expirar cookie |
| Troca/recuperação de senha | Rotacionar credencial e revogar sessões conforme política |
| Remoção de papel/inativação | Invalidar autorização e sessões afetadas; não aguardar cache longo |
| Segredo exposto | Revogar/rotacionar no cofre, atualizar workloads, investigar logs e redeploy se necessário |
| Chave de fornecedor comprometida | Revogar no fornecedor, trocar no cofre e validar callbacks/uso anormal |
| Incidente com dado | Conter acessos, preservar evidência e acionar Heitor Leite, Prefeitura/Embrass e os responsáveis institucionais que forem designados antes da implantação |

## 19. Privacidade e residência de dados

- O backend coleta somente os campos aprovados para a finalidade documentada.
- A ficha de saúde contém somente os campos aprovados, usa criptografia por campo e tem acesso restrito ao administrador total, administrador parcial autorizado e professor da turma do aluno.
- Comprovante é potencialmente sensível e fica separado do acesso comum a aluno/frequência.
- Relatórios e mapas, quando entrarem em versão futura, aplicarão minimização, versão do indicador e grupo mínimo de três pessoas.
- Fornecedor recebe somente dados mínimos; transferência internacional e suboperadores exigem avaliação.
- País/região de banco, storage, backup, logs e fornecedores deve ser registrado antes da contratação/produção.
- Produção não é copiada para desenvolvimento; dados sintéticos são o padrão.
- Retenção e descarte seguem os prazos acadêmicos definidos em `../database/BANCO_DE_DADOS.md`; a Prefeitura deverá validá-los antes do uso real.
- Analytics de frontend, se adotado, não recebe CPF, telefone, e-mail, identificador desnecessário, frequência ou conteúdo sensível.

## 20. Observabilidade

### 20.1 Sinais

| Sinal | Conteúdo mínimo |
| --- | --- |
| Logs | Timestamp, nível, ambiente, serviço/módulo, código do evento, correlação e resultado minimizado |
| Métricas | Taxa, erro, duração, saturação, conexões, filas/outbox, jobs, storage e dependências |
| Traces | Caminho entre borda, API, banco e adaptadores sem payload pessoal |
| Auditoria | Ator, ação, alvo, instante, resultado, motivo/antes-depois quando necessário |
| Health | Liveness do processo e readiness das dependências essenciais |

### 20.2 Health checks

- Liveness verifica se o processo executa, sem consultar fornecedor externo.
- Readiness verifica capacidade de receber tráfego e dependências essenciais, principalmente banco e armazenamento de sessão.
- WhatsApp, e-mail e mapas não retiram a API inteira de readiness quando há modo degradado.
- Detalhes de Actuator/management ficam em porta/rede protegida; endpoint público retorna apenas estado mínimo.

### 20.3 Indicadores iniciais

- p50/p95/p99 de endpoints por caso de uso;
- taxa de `2xx`, `4xx`, `401`, `403`, `409`, `429` e `5xx`;
- uso/espera do pool de banco e duração de transações;
- itens de outbox pendentes, idade do item mais antigo e falhas finais;
- tempo e falha de jobs, exportações e scanner;
- taxa de entrega, timeout, callback inválido e circuit breaker por fornecedor;
- quantidade/tamanho de uploads e rejeições;
- autenticação falha, recuperação, revogação e ações administrativas críticas;
- sucesso, duração e idade do último backup/restore testado.

Logs operacionais permanecem por 90 dias e auditoria por cinco anos. O funcionamento é verificado a cada minuto e falha crítica deve alertar Heitor Leite em até cinco minutos. A plataforma e os canais operacionais serão alinhados com Prefeitura e Embrass.

## 21. Disponibilidade, resiliência e desempenho

### 21.1 Baseline de resiliência

- Instâncias HTTP não mantêm estado crítico em disco/memória local.
- Sessão, idempotência, outbox e locks necessários permanecem em serviço central aprovado.
- Timeouts existem em todas as chamadas externas e consultas críticas.
- Retry só ocorre quando seguro e idempotente; não é infinito.
- Circuit breaker limita falha de fornecedor e bulkhead/limite de concorrência protege recursos.
- Banco usa transações curtas, índices revisados e pool limitado.
- Worker processa em lotes limitados e permite backpressure.
- Storage e banco são reconciliados quando uma operação parcial ocorrer.

### 21.2 Comportamento degradado

| Falha | Comportamento esperado |
| --- | --- |
| WhatsApp indisponível | Operação principal permanece confirmada; tentativa fica pendente/falha e fallback aprovado é acionado |
| Mapas indisponíveis | Exibir lista textual de polos; demais módulos continuam |
| Gerador PDF/Excel falha | Solicitação marca falha; nenhum arquivo parcial disponível |
| Scanner indisponível | Arquivo permanece em quarentena e não pode ser baixado como aprovado |
| Storage indisponível | Upload/exportação falha ou permanece pendente sem sucesso falso; cadastros independentes continuam quando seguro |
| Banco indisponível | Operações dependentes falham rapidamente com resposta segura; não confirmar mutação |
| Sessão/cache indisponível | Negar com segurança ou ficar indisponível; não aceitar sessão não verificável |
| Observabilidade indisponível | Aplicação não vaza dados nem bloqueia transação comum, mas gera alerta local/operacional e limita operação de risco conforme política |
| Conexão cai durante chamada | Manter rascunho mínimo não sincronizado por até 24 horas, reenviar com idempotência e só exibir sucesso após confirmação do servidor |

### 21.3 Escalabilidade

- Backend pode escalar horizontalmente atrás do balanceador após centralizar sessão/estado.
- Workers escalam por reserva exclusiva de itens e limite global por fornecedor.
- Banco escala primeiro por índices, consultas, pool e capacidade; réplica de leitura somente com consistência e autorização definidas.
- Arquivos não transitam por memória sem limite; upload/download usam streaming controlado quando aprovado.
- CDN/cache atende conteúdo público, não dados sensíveis.
- Particionamento, réplica analítica e broker somente entram após volume e teste demonstrarem necessidade.

As metas iniciais são p95 de 2 segundos para consultas, 3 segundos para alterações, 5.000 alunos, 200 turmas, 100 usuários simultâneos e 99,5% de disponibilidade mensal. Elas orientam os testes iniciais e serão revistas com dados reais antes da implantação.

## 22. Backup, recuperação e continuidade

- Banco e arquivos possuem backup criptografado e segregado segundo RPO aprovado.
- Restore é testado periodicamente em ambiente isolado; backup não testado não conta como recuperação.
- Metadado e objeto precisam ser reconciliáveis no ponto restaurado.
- Segredos/chaves necessários ao restore têm recuperação separada e protegida.
- Após restore, sessões, tokens, ofertas, links e arquivos expirados são revalidados para não reativar acesso antigo.
- Procedimento mede RTO, integridade, contagens e funcionamento de identidade, inscrição, frequência, outbox, auditoria e storage.
- Correções comuns de release usam rollback/roll-forward de aplicação; restauração de dados só ocorre após avaliar perda e reconciliação.

A política acadêmica inicial usa backup diário, RPO de 24 horas, RTO de 8 horas e retenção de 30 dias. Prefeitura e Embrass fornecerão a infraestrutura; região, horários, responsáveis operacionais e evidências de restauração serão formalizados antes da implantação.

## 23. Implantação por ambiente

O SIDESP terá somente dois ambientes permanentes: `desenvolvimento/testes` e `produção`. Execuções temporárias do pipeline não são consideradas um terceiro ambiente e nunca recebem dados ou segredos de produção.

```mermaid
flowchart TB
    git["Repositório e PR"]
    ci["CI\nbuild, testes, scans, SBOM"]
    registry["Registro de artefato/imagem"]

    subgraph devtest["Desenvolvimento / testes"]
        devapp["Angular + API + worker"]
        devdb[("MySQL com dados sintéticos")]
        devstorage[("Storage de testes")]
    end

    subgraph prod["Produção"]
        prodedge["Borda HTTPS"]
        prodapi["API não root"]
        prodworker["Worker não root"]
        proddb[("Banco privado")]
        prodstorage[("Storage privado")]
        prodobs["Observabilidade"]
    end

    git --> ci
    ci --> registry
    registry --> devapp
    registry -->|"aprovações e mesmo digest"| prodapi
    registry -->|"mesma imagem"| prodworker
    devapp --> devdb
    devapp --> devstorage
    prodedge --> prodapi
    prodapi --> proddb
    prodapi --> prodstorage
    prodapi --> prodobs
    prodworker --> proddb
    prodworker --> prodstorage
    prodworker --> prodobs
```

| Ambiente | Finalidade | Dados e segredos | Promoção |
| --- | --- | --- | --- |
| Desenvolvimento/testes | Desenvolvimento, integração, testes automáticos, aceite funcional e desempenho controlado | Somente dados sintéticos e segredos exclusivos do ambiente; sem acesso a produção | Gera e valida o candidato de release |
| Produção | Serviço real | Dados e segredos reais, rede privada e acesso nominal | Mesmo artefato validado, após aprovações e gates |

Antes da produção acadêmica, Kauãn Raphael coordena a liberação, Heitor Leite valida arquitetura, segurança e operação, Micael Phillipini valida os testes e Livia Andrade aprova o produto. Na implantação real, Prefeitura/Embrass também aprovam e executam as ações sob sua responsabilidade.

### 23.1 Topologia de produção proposta

- frontend Angular estático e API são publicados sob a mesma origem lógica: `/` para o frontend e `/api/v1` para o backend;
- proxy/balanceador termina TLS e encaminha somente as rotas necessárias; se a infraestrutura exigir origens separadas, CORS usa lista exata e credenciais somente para a origem homologada;
- backend e worker em containers mínimos, usuário não root e filesystem somente leitura quando viável;
- banco, storage, cofre e management em rede privada;
- egress limitado aos fornecedores aprovados;
- infraestrutura como código versionada e revisada;
- Prefeitura e Embrass fornecerão a infraestrutura; região, orquestrador, capacidade, custos adicionais e responsabilidades operacionais ainda serão formalizados.

## 24. CI/CD e cadeia de suprimentos

Pipeline mínimo:

1. validar formatação, compilação e testes unitários;
2. executar testes de arquitetura/módulos e integração;
3. validar OpenAPI e migrações;
4. executar SAST, SCA/licenças e secret scanning;
5. analisar imagem, IaC e configurações;
6. gerar SBOM, checksum e evidência do artefato;
7. publicar artefato imutável em registro aprovado;
8. implantar em desenvolvimento/testes e executar smoke, contrato, integração e aceite;
9. exigir aprovações e gates antes de produção;
10. promover o mesmo digest, registrar deploy e observar pós-implantação.

Migrações Flyway aplicadas não são editadas. A correção para frente é preferencial; retorno para a imagem anterior só ocorre quando ela for compatível com o schema já migrado. Qualquer restauração de dados segue o procedimento de continuidade e considera RPO, reconciliação e auditoria.

Dependência, Action, imagem, plugin, SDK, MCP ou ferramenta de IA também integra a cadeia de suprimentos e deve ter origem, versão, licença e risco avaliados.

## 25. Estratégia arquitetural de testes

| Camada | Testes esperados |
| --- | --- |
| Domínio | Invariantes, estados, elegibilidade, faltas, oferta e agregação |
| Aplicação | Autorização contextual, idempotência, transação, compensação e auditoria |
| Módulos | Regras de dependência e proibição de acesso interno indevido, preferencialmente automatizadas |
| Persistência | Constraints, concorrência, locks, queries, migração e rollback/roll-forward |
| API | OpenAPI, DTOs, erros, paginação, limites, CSRF, CORS e mass assignment |
| Segurança | Autenticação, BOLA/IDOR, acesso vertical, rate limit, upload, webhook e exportação |
| Integração | Contrato com adaptadores simulados e sandbox aprovado, timeout/retry/circuit breaker |
| Assíncrono | Outbox, redelivery, lease, falha final, replay e reprocessamento |
| Operação | Health, métricas, logs minimizados, backup/restore e modo degradado |
| Desempenho | Concorrência da última vaga, chamada em volume, relatórios e filas com carga aprovada |

Uma estratégia de testes própria ainda deve ser documentada; esta seção define apenas requisitos arquiteturais mínimos.

## 26. Decisões e ADRs

As decisões abaixo formam o registro arquitetural atual. As já aceitas pela equipe ainda podem receber um arquivo ADR próprio antes da implementação, para preservar alternativas e consequências.

| ADR | Decisão | Estado |
| --- | --- | --- |
| `ADR-001` | Adotar monólito modular em vez de microsserviços na baseline | Aceito em 17/08/2026 |
| `ADR-002` | Adotar REST/JSON + OpenAPI em `/api/v1` como contrato principal | Aceito em 17/08/2026 |
| `ADR-003` | Usar sessão opaca no servidor, persistida inicialmente no MySQL, em vez de token no navegador | Aceito pelas decisões de identidade e dados |
| `ADR-004` | Usar MySQL 8.x e Flyway | Aceito em 17/08/2026 |
| `ADR-005` | Usar outbox transacional e worker antes de broker externo | Aceito em 17/08/2026 |
| `ADR-006` | Armazenar comprovantes e futuras exportações em storage privado compatível com S3, com ClamAV ou equivalente | Aceito em 17/08/2026; ferramenta equivalente pode ser homologada |
| `ADR-007` | Aplicar portas/adaptadores e módulos por domínio | Aceito em 17/08/2026 |
| `ADR-008` | Manter arquitetura para uma única Secretaria no escopo atual | Aceito pelo escopo |
| `ADR-009` | Usar Angular/TypeScript em `/` e API em `/api/v1` sob a mesma origem lógica | Aceito; origem separada somente se Prefeitura/Embrass exigir |
| `ADR-010` | Manter sessões no MySQL; usar cofre da infraestrutura e OpenTelemetry com logs JSON | Aceito logicamente; ferramentas físicas serão homologadas |
| `ADR-011` | Usar ClamAV ou scanner equivalente; homologar fornecedor de e-mail; WhatsApp e mapas ficam para versões futuras | Baseline aceita; e-mail depende da implantação |
| `ADR-012` | Manter rascunho de chamada criptografado no IndexedDB por até 24 horas | Aceito em 17/08/2026 |
| `ADR-013` | Gerar uma imagem Spring Boot e executar API e worker separadamente em produção | Aceito em 17/08/2026 |
| `ADR-014` | Manter somente os ambientes permanentes de desenvolvimento/testes e produção | Aceito em 17/08/2026 |
| `ADR-015` | Fixar versões LTS/estáveis e usar os nomes acadêmicos de pacote e artefatos aprovados | Aceito em 17/08/2026 |

ADRs substituídos permanecem no histórico com referência à decisão sucessora; não devem ser apagados.

## 27. Limites conhecidos, riscos e dívida técnica

| ID | Limite/risco | Impacto | Tratamento |
| --- | --- | --- | --- |
| `ARQ-RIS-001` | Implementação pode divergir das regras aprovadas de faltas e justificativas | Cancelamento ou alerta incorreto | Testes rastreados para requisitos, regras e atividades antes do job produtivo |
| `ARQ-RIS-002` | Dispositivo compartilhado ou script malicioso pode tentar acessar o rascunho offline | Exposição ou alteração da chamada | IndexedDB criptografado, dados mínimos, CSP, limpeza em logout/sincronização/24 horas e confirmação obrigatória do servidor |
| `ARQ-RIS-003` | Matriz aprovada pode não ser aplicada em todos os endpoints e jobs | Escalada ou bloqueio operacional | Testes de acesso vertical, horizontal, vínculo e vigência para API e tarefas automáticas |
| `ARQ-RIS-004` | Ferramentas físicas de cofre e observabilidade dependem da infraestrutura | Segredos ou incidentes mal administrados | Exigir os controles lógicos aprovados e homologar ferramentas com Prefeitura/Embrass antes do ambiente real |
| `ARQ-RIS-005` | WhatsApp/fallback não contratado | Avisos não entregues e dependência | Manter porta/adaptador e bloquear integração real |
| `ARQ-RIS-006` | S3/ClamAV podem não estar disponíveis na infraestrutura | Exposição de dado ou malware | Aceitar equivalente homologado sem remover quarentena, varredura, autorização e auditoria |
| `ARQ-RIS-013` | Não existe ambiente separado de homologação | Defeito pode chegar à produção se o aceite for incompleto | Executar integração, aceite, segurança e desempenho em desenvolvimento/testes; exigir aprovações registradas e promover exatamente o artefato validado |
| `ARQ-RIS-007` | Fórmulas e limiar analítico ausentes | Relatório divergente/reidentificação | Aprovar dicionário de indicadores e privacidade |
| `ARQ-RIS-008` | Metas acadêmicas podem não representar a demanda real | Infraestrutura sub ou superdimensionada | Testar 5.000 alunos, 200 turmas e 100 usuários e revisar com a Prefeitura antes da implantação |
| `ARQ-RIS-009` | Topologia e região da infraestrutura Prefeitura/Embrass não formalizadas | Residência e operação incertas | Registrar responsabilidades, região, capacidade e recuperação antes da produção |
| `ARQ-RIS-010` | Monólito pode perder fronteiras internas | Acoplamento e evolução lenta | Testes arquiteturais, ownership de módulos e revisão |
| `ARQ-RIS-011` | Outbox no banco cresce sem controle | Latência e custo | Índice, lotes, retenção, métricas e plano de broker se necessário |
| `ARQ-RIS-012` | Relatórios no banco operacional podem competir com transações | Lentidão | Limites; consultas otimizadas; réplica/analítico após evidência |

## 28. Procedimento seguro de evolução

Toda alteração arquitetural relevante deve:

1. identificar requisitos, casos de uso, dados e ameaças afetados;
2. registrar decisão em ADR com opções, consequências, segurança, custo e rollback;
3. atualizar OpenAPI, modelo de dados, segurança e diagramas antes ou junto da mudança;
4. preservar compatibilidade ou publicar plano de versionamento/depreciação;
5. usar migração expand/contract para banco e manter caminho recuperável;
6. implementar atrás de configuração/feature flag segura quando houver risco de rollout;
7. testar contrato, autorização, concorrência, falha externa, observabilidade e recuperação;
8. promover o mesmo artefato por ambientes e observar métricas após implantação;
9. possuir rollback de aplicação ou roll-forward documentado; nunca improvisar alteração manual em produção;
10. remover código/configuração antiga somente após confirmar migração e ausência de consumidores.

Mudanças que exigem ADR/revisão reforçada incluem banco, autenticação, sessão, frontend, fornecedor, região, mensageria, cache, storage, criptografia, multi-tenancy, exposição de API e divisão em serviços.

## 29. Rastreabilidade consolidada

| Área arquitetural | Requisitos/casos | Documentos de apoio |
| --- | --- | --- |
| Contexto e módulos | `RF-PUB-*`, `RF-IDN-*`, `RF-INS-*`, `RF-FRQ-*`, `RF-JUS-*`, `RF-ADM-*`, `RF-COM-*`, `RF-REL-*` | Casos de uso e classes/componentes |
| Identidade e autorização | `RF-IDN-002/003/004`, `RF-ADM-007`, `RN-016/017/022`, `RNF-SEG-001/003/004/005` | Segurança, fluxos 1/2/9 de atividades |
| Concorrência e transação | `RF-INS-001` a `RF-INS-004`, `RF-FRQ-003/006`, `RN-009` a `RN-012`, `RN-019`, `RNF-RES-001` | Modelo de dados, fluxos 3 a 7 |
| Arquivos | `RF-JUS-001`, `RF-REL-002`, `RN-004`, `RNF-SEG-006`, `RNF-PRI-002`, `RNF-EXP-001` | Segurança e modelo de dados |
| Comunicação/assíncrono | `RF-COM-001`, `RF-COM-002`, `RF-COM-003`, `RF-COM-004`, `RF-INS-004`, `RF-JUS-003`, `RN-020/025`, `RNF-RES-001` | Fluxo 8 e classes de comunicação |
| Relatórios/análises futuras | `RF-REL-001`, `RF-REL-002`, `RF-REL-003`, `RNF-PRI-003` | Modelo de dados e fluxo 10; fora da primeira versão |
| Operação e recuperação | `RNF-DIS-001`, `RNF-OBS-001`, `RNF-POR-001` | Segurança, modelo de dados e fluxo 11 |
| Conectividade | `RNF-CON-001`, `DEC-028`, `DEC-080` | Atividades, segurança e `ADR-012` |

## 30. Validações externas antes da implantação real

Não existem decisões arquiteturais acadêmicas em aberto. Os pontos abaixo não impedem a revisão do documento pela equipe, mas precisam ser confirmados antes da implantação com dados reais.

| Item | Validação necessária | Impacto |
| --- | --- | --- |
| Infraestrutura | Prefeitura/Embrass devem confirmar região, rede, capacidade, containers, DNS, certificados, backup e responsáveis operacionais | Topologia física e continuidade |
| Ferramentas | Homologar storage S3 ou equivalente, ClamAV ou scanner equivalente, cofre e plataforma compatível com OpenTelemetry | Arquivos, segredos e observabilidade |
| E-mail | Escolher provedor, domínio/remetente, limites e operação de falha | Confirmação, recuperação, MFA e comunicação de responsáveis |
| Versões | Fixar no início do desenvolvimento as versões exatas de Java LTS, Spring Boot, Angular e dependências | Build reproduzível e suporte |
| Capacidade | Executar testes com as metas acadêmicas e revisar a demanda real com o cliente | Dimensionamento e custo |

## 31. Critérios de aprovação

- [ ] Objetivos, contexto e módulos correspondem ao produto aprovado.
- [ ] Monólito modular e regras de dependência foram aceitos pela equipe.
- [ ] Frontend, backend, banco, storage, sessão e integrações possuem responsáveis para o desenvolvimento e responsáveis institucionais antes da implantação.
- [ ] Autenticação, MFA, sessão, CSRF e matriz de autorização foram aprovados.
- [ ] Contrato OpenAPI, erros, idempotência, paginação e limites foram definidos.
- [ ] Outbox, workers, jobs, retries e falha final foram revisados.
- [ ] Modelo de dados, transações, migrações e ownership de tabelas estão coerentes.
- [ ] Upload e e-mail possuem ferramentas homologadas; exportações, WhatsApp e mapas permanecem fora da primeira versão.
- [ ] Logs, métricas, traces, auditoria, alertas e runbooks foram planejados.
- [ ] SLO, capacidade, RPO, RTO, backup e restore foram aprovados/testáveis.
- [ ] Os dois ambientes, rede, cofre, CI/CD, artefatos e acesso operacional foram definidos e validados com Prefeitura/Embrass.
- [ ] Privacidade, residência, retenção e transferências foram revisadas.
- [ ] Riscos, dívidas e ADRs possuem responsável e prazo.
- [ ] Diagramas refletem o estado **planejado**, sem serem apresentados como implantação já existente.

## 32. Histórico de versões

| Versão | Data | Alteração | Autor |
| --- | --- | --- | --- |
| `0.1.0` | 13/08/2026 | Arquitetura inicial: contexto, monólito modular, camadas, módulos, APIs, outbox, dados, integrações, implantação, segurança e operação | Heitor Leite |
| `0.2.0` | 17/08/2026 | Decisões resolvidas: Angular, monólito modular, REST/OpenAPI, MySQL/Flyway, sessões, outbox, origem pública, offline, arquivos, observabilidade, dois ambientes, CI/CD e versões; documento pronto para revisão | Heitor Leite |
