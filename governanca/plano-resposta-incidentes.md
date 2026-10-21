# Plano de Resposta a Incidentes

> **Modelo institucional reutilizável.** Este documento operacionaliza a **Política de Resposta a Incidentes**,
> detalhando como mobilizar e coordenar a resposta (inclusive fora do horário comercial), como classificar e
> escalonar um incidente, como executar a resposta segundo o DAIR, como conduzir em paralelo a trilha de
> privacidade exigida pela LGPD/RCIS, e como encerrar a resposta com rastreabilidade. Ele **não contém dados de
> nenhuma organização específica** e **não reproduz os cenários fictícios de treinamento** deste repositório:
> para aprender a preencher os instrumentos, use os exemplos em [`instrumentos/exemplos/`](../instrumentos/exemplos/)
> e o cenário em [`cenario/`](../cenario/). Os procedimentos técnicos detalhados de cada atividade de resposta
> estão no [Playbook DAIR](../playbook/01-procedimento-operacional.md).

## 0. Dados de controle do documento

| Campo | Valor |
|---|---|
| Organização | `[preencher]` |
| Proprietário do documento (owner) | `[preencher]` |
| Aprovador | `[preencher]` |
| Versão | `[preencher]` |
| Data de aprovação | `[preencher após aprovação formal]` |
| Vigência / próxima revisão | `[preencher]` |
| Fundamento | Política de Resposta a Incidentes (ver [`governanca/politica-resposta-incidentes.md`](politica-resposta-incidentes.md)) |

Este Plano só produz efeitos após aprovação formal e preenchimento dos campos editáveis indicados ao longo do
texto, sobretudo a lista de contatos da seção 1, que **nunca deve conter nomes ou dados reais nesta cópia de
modelo**.

## 1. Preparação e ativação

### 1.1 Critérios de ativação

A resposta deve ser ativada diante de qualquer um dos seguintes gatilhos, sem aguardar confirmação total:

- suspeita técnica identificada por monitoramento, alerta de segurança ou análise interna;
- relato humano, de colaborador, cliente, titular de dados ou terceiro;
- notificação de operador ou fornecedor informando incidente envolvendo dados tratados em nome da organização;
- notificação externa (pesquisador de segurança, imprensa, autoridade).

A ativação não depende da conclusão da investigação técnica ou da análise jurídica: a preservação de
evidências e a contenção emergencial têm início imediato, em paralelo à triagem (ver seção 3, *Prepare* e
*Detect*).

### 1.2 Canais de acionamento

| Canal | Uso | Disponibilidade |
|---|---|---|
| `[preencher: ex.: linha de emergência de segurança]` | Canal primário para suspeitas e confirmações | `[preencher: ex.: 24x7]` |
| `[preencher: ex.: canal interno de mensagens]` | Canal alternativo quando o canal primário estiver indisponível | `[preencher]` |
| `[preencher: ex.: e-mail corporativo de segurança]` | Registro formal e rastreável do acionamento | `[preencher]` |

A indisponibilidade do canal corporativo primário (ex.: falha de e-mail, de VPN ou do sistema de chamados) não
suspende a obrigação de reportar: a organização deve manter ao menos um canal alternativo que não dependa da
mesma infraestrutura potencialmente afetada pelo incidente (ex.: linha telefônica, aplicativo de mensagens
pessoal combinado previamente, canal de um fornecedor de telecomunicações distinto).

### 1.3 Lista de contatos por papel

| Papel | Titular | Contato primário | Contato alternativo | Substituto |
|---|---|---|---|---|
| Líder de Resposta a Incidentes | `[preencher]` | `[preencher]` | `[preencher]` | `[preencher]` |
| Segurança da Informação | `[preencher]` | `[preencher]` | `[preencher]` | `[preencher]` |
| TI | `[preencher]` | `[preencher]` | `[preencher]` | `[preencher]` |
| Encarregado (DPO) | `[preencher]` | `[preencher]` | `[preencher]` | `[preencher]` |
| Jurídico | `[preencher]` | `[preencher]` | `[preencher]` | `[preencher]` |
| Comunicação | `[preencher]` | `[preencher]` | `[preencher]` | `[preencher]` |
| Continuidade de Negócio | `[preencher]` | `[preencher]` | `[preencher]` | `[preencher]` |
| Direção / Diretoria designada | `[preencher]` | `[preencher]` | `[preencher]` | `[preencher]` |

Manter esta tabela atualizada é condição de uso deste Plano: uma lista desatualizada equivale, na prática, à
ausência de canal de acionamento. `[preencher: periodicidade de revisão da lista, ex.: trimestral]`.

### 1.4 Mobilização, liderança, sala de crise e distribuição de tarefas

- Ao identificar um gatilho de ativação (seção 1.1), quem o identificar aciona o Líder de Resposta a Incidentes
  (ou substituto) pelo canal primário.
- O Líder de Resposta a Incidentes declara a abertura do incidente (Política, seção 6), abre o registro central
  no Instrumento 1 e convoca os papéis necessários conforme a classificação inicial (seção 2).
- `[preencher: definir se a organização usa sala de crise física, virtual ou ambas, e a ferramenta de
  colaboração designada]`.
- Tarefas são distribuídas pelo Líder de Resposta, registradas no Instrumento 1 (ou no Instrumento 3, para o
  acompanhamento técnico operacional), com responsável e prazo.
- Em incidentes de longa duração, a passagem de turno (*handover*) deve registrar: estado atual, decisões
  pendentes, evidências em custódia e próximos passos, nunca apenas "em andamento".

### 1.5 Requisitos de acesso a registros, evidências e ferramentas

A organização deve garantir, antes da ativação, que os papéis mobilizáveis tenham acesso tempestivo a:

- registros de log e monitoramento relevantes, com retenção suficiente para a janela típica de investigação;
- os Instrumentos 1 a 5 deste repositório (ou equivalentes adotados pela organização);
- ferramentas de contenção (revogação de credenciais, isolamento de rede, desativação de contas/tokens);
- canais de comunicação interna, contratual, regulatória e com titulares.
`[preencher: lista de sistemas e o papel responsável por conceder acesso de emergência a cada um]`.

## 2. Classificação e escalonamento

### 2.1 Critérios operacionais de severidade/impacto

A classificação técnica inicial considera, no mínimo: tipo de violação (confidencialidade, integridade,
disponibilidade, autenticidade), criticidade dos sistemas/dados afetados, extensão estimada e reversibilidade.
`[preencher: se a organização adotar uma matriz de severidade com níveis (ex.: Baixa/Média/Alta/Crítica),
detalhar aqui os critérios e limiares de cada nível]`.

Uma eventual matriz de severidade técnica orienta a mobilização de recursos e a prioridade operacional: **ela
não decide, por si só, a obrigação de comunicação à ANPD**, que depende da avaliação fundamentada de risco aos
titulares (seção 4 e Instrumento 2), ainda que incidentes de maior severidade técnica tendam, na prática, a
exigir essa avaliação com mais urgência.

### 2.2 Autoridades de decisão e escalonamento

| Nível de severidade técnica | Autoridade de decisão | Participação obrigatória |
|---|---|---|
| `[preencher: ex.: Baixa]` | Líder de Resposta a Incidentes | Segurança, TI |
| `[preencher: ex.: Média]` | Líder de Resposta a Incidentes | + Encarregado (avaliação preliminar de dados pessoais) |
| `[preencher: ex.: Alta]` | Líder de Resposta a Incidentes + Direção (ciência) | + Jurídico, Comunicação |
| `[preencher: ex.: Crítica]` | Direção | + Continuidade de Negócio, Jurídico, Encarregado, Comunicação |

### 2.3 Critérios de participação de áreas específicas

- **Negócio:** sempre que o incidente afetar dados ou processos sob responsabilidade da área, ou clientes
  diretamente atendidos por ela.
- **Continuidade de Negócio:** sempre que houver indisponibilidade de sistema ou serviço crítico, ou risco de
  indisponibilidade decorrente da contenção.
- **Jurídico e Encarregado:** sempre que houver indício de envolvimento de dados pessoais, mesmo antes da
  confirmação completa, para que a trilha de privacidade (seção 4) comece em paralelo à investigação técnica.
- **Comunicação:** sempre que houver risco de repercussão externa, contratual ou regulatória.

### 2.4 Distinção entre classificação técnica e avaliação regulatória

A classificação técnica de severidade e a avaliação regulatória do risco aos titulares são registradas em
instrumentos distintos (Instrumento 1 e Instrumento 2, respectivamente) e podem evoluir de forma independente:
um incidente tecnicamente contido pode ainda exigir comunicação regulatória, e um incidente tecnicamente aberto
pode já ter avaliação regulatória concluída com risco não significativo. Nenhuma das duas decide a outra
automaticamente.

### 2.5 Matriz de autoridade de decisão

A tabela da seção 2.2 orienta o escalonamento por severidade; esta matriz complementa a Política (seção 6)
nomeando, decisão a decisão, quem decide, quem substitui e qual evidência a decisão deve deixar: o mesmo papel
que uma matriz RACI (ver Política, seção 5, e [`governanca/01-papeis-e-escalonamento.md`](01-papeis-e-escalonamento.md))
cumpre para a participação, mas aqui para a autoridade de decidir.

| Decisão | Autoridade | Delegado | Documentação exigida |
|---|---|---|---|
| Declarar o incidente | Líder de Resposta a Incidentes | Analista sênior de plantão | Registro de abertura no Instrumento 1, com data/hora e fundamento |
| Conter com impacto relevante ao negócio | Líder de Resposta + concordância do dono da área de negócio | Direção, se o Líder de Resposta estiver indisponível | Registro de decisão de contenção no Instrumento 1, com avaliação de impacto |
| Expandir o escopo da investigação | Líder de Resposta, com ciência da Direção | N/A | Registro de expansão de escopo no Instrumento 1, com o gatilho que a motivou |
| Sair do Loop de Ações de Resposta (encerramento técnico) | Líder de Resposta, com base em validação técnica | Direção, mediante aprovação registrada do Líder de Resposta | Decisão de encerramento assinada (seção 6.1) |
| Aceitar risco residual | Direção | N/A | Registro de aceite de risco, com responsável e prazo de acompanhamento |
| Autorizar o envio da comunicação à ANPD | Representante legalmente constituído do controlador | N/A | Aprovação registrada no Instrumento 5, Parte A/C, antes do peticionamento |
| Autorizar o envio da comunicação aos titulares | Representante autorizado do controlador | N/A | Aprovação registrada no Instrumento 5, Parte B/C, antes do envio |
| Declarar o encerramento do acompanhamento regulatório | Encarregado, com ciência do Jurídico | N/A | Registro de encerramento no Instrumento 5, Parte C |

Esta matriz é um ponto de partida; a organização deve adaptar autoridades e delegados aos seus próprios cargos e
substituir os nomes de papel pelos titulares reais na seção 1.3, nunca aqui, que permanece o modelo limpo.

## 3. Execução segundo o DAIR

A execução técnica segue o DAIR como abordagem dinâmica e iterativa (Política, seção 4), sem sequência rígida,
com reentradas no Loop de Ações de Resposta sempre que surgirem evidências novas, falhas de contenção,
persistência do agente de ameaça, mudança de escopo ou de prioridade. Os procedimentos técnicos detalhados de
cada atividade constam do [Playbook DAIR](../playbook/01-procedimento-operacional.md); esta seção define
objetivo, gatilhos, responsável, decisões, registros e evidências esperadas de cada waypoint, de forma aplicável
a incidentes de confidencialidade, integridade, disponibilidade e autenticidade, inclusive acidentais e sem
exfiltração de dados.

| Waypoint | Objetivo | Gatilhos típicos | Responsável | Decisões | Registro | Evidência esperada |
|---|---|---|---|---|---|---|
| Prepare | Estabelecer capacidade de resposta | Contínuo (não é reativo a um incidente específico) | Líder de Resposta / Segurança | Designação de papéis, inventário de capacidades | Instrumento 3 (checklist) | Lista de papéis, canais e ferramentas validada |
| Detect | Identificar evento com indícios de incidente | Alerta de monitoramento, relato humano, notificação de terceiro/operador | Segurança / TI / qualquer colaborador | Abrir registro; decidir se há indício suficiente para triagem | Instrumento 1 (evento) | Alerta bruto preservado, com data/hora e fonte |
| Verify and Triage | Confirmar natureza, extensão inicial e se há dados pessoais envolvidos | Evidência inicial do Detect | Líder de Resposta + Segurança + Encarregado | Declarar incidente; fixar marco de conhecimento; classificação inicial (seção 2) | Instrumento 1 (decisão), Instrumento 2 (abertura, se dados pessoais) | Evidência de confirmação, marco de conhecimento fundamentado |
| Scope (loop) | Delimitar o que é e o que não é afetado | Nova evidência, suspeita de escopo maior | Segurança / TI / Negócio | O que tratar como conhecido vs. desconhecido | Instrumento 1, Instrumento 3 | Lista de ativos, contas, dados e sistemas delimitados, com incertezas registradas |
| Contain (loop) | Interromper a progressão do incidente | Escopo delimitado ou contenção anterior malsucedida | Segurança / TI | Quais acessos/contas/sistemas conter; proporcionalidade frente à continuidade | Instrumento 1, Instrumento 3 | Evidência de bloqueio/revogação e de sua validação técnica |
| Eradicate (loop) | Remover a causa e o mecanismo de acesso | Contenção validada | Segurança / TI | Causa suficientemente identificada para erradicar, ou apenas mitigada | Instrumento 1, Instrumento 3 | Evidência de correção do mecanismo e de ausência de novo acesso pelo mesmo vetor |
| Recover (loop) | Restabelecer operação de forma validada | Erradicação concluída ou aceitável para retomada parcial | TI / Continuidade / Dono do serviço | Nível de restabelecimento (total/parcial/monitorado) | Instrumento 1, Instrumento 3 | Evidência de validação técnica e de monitoramento reforçado |
| Debrief | Consolidar lições e ações corretivas | Encerramento da resposta ativa (seção 6) | Líder de Resposta + papéis envolvidos | Ações corretivas, responsáveis e prazos | Instrumento 4 (relatório) | Relatório de encerramento com indicadores e pendências |

**Reentradas:** este Plano não impõe avançar de Scope a Recover uma única vez. Nova evidência relevante, falha
de contenção, indício de persistência, ampliação de escopo ou mudança de prioridade reabrem o loop a partir do
ponto necessário (frequentemente Scope), registrando a nova iteração separadamente no Instrumento 1, nunca
sobrescrevendo o registro da iteração anterior.

**Abrangência:** os waypoints e o loop se aplicam a incidentes de confidencialidade (ex.: exposição ou
exfiltração de dados), integridade (ex.: alteração indevida de registros), disponibilidade (ex.: indisponibilidade
de sistema ou serviço) e autenticidade (ex.: personificação ou assinatura indevida), qualquer que seja a causa:
**maliciosa** (ação deliberada de um agente de ameaça), **acidental** (erro humano, configuração indevida) ou
**ambiental** (falha de infraestrutura, energia ou de provedor de nuvem), e mesmo quando não há evidência de
exfiltração. A ausência de exfiltração confirmada, e a ausência de um agente de ameaça identificado, não
dispensam a avaliação de risco aos titulares (seção 4), que depende do risco e do dano potencial, não da causa.

## 4. Trilha transversal de privacidade

Esta trilha corre em paralelo à execução técnica (seção 3), desde a Verify and Triage, e não depende da conclusão
da investigação para começar.

1. **Identificação das operações e dados afetados:** o Encarregado, com apoio de Negócio e TI, identifica quais
   atividades de tratamento, categorias de dados e titulares podem estar envolvidos, mesmo que de forma
   preliminar e sujeita a revisão.
2. **Fundamentação do marco de conhecimento:** a data em que o controlador tomou conhecimento do incidente deve
   ser registrada com a evidência que a fundamenta (ex.: data/hora do alerta confirmado, do relato recebido, da
   notificação do operador); este marco inicia a contagem dos prazos legais (ver item 4).
3. **Avaliação versionada contra os critérios do RCIS:** a avaliação de risco aos titulares (art. 5º da
   Resolução CD/ANPD nº 15/2024) é registrada no Instrumento 2, com nova versão sempre que houver evidência
   relevante nova, nunca sobrescrevendo a versão anterior.
4. **Responsáveis pela elaboração, revisão e aprovação da avaliação:** conforme a seção 5 da Política,
   tipicamente Encarregado (elaboração), Jurídico (revisão) e representante autorizado do controlador
   (aprovação); `[preencher, se a organização definir papéis diferentes]`.
5. **Controle de prazos aplicáveis e suas exceções:** prazo de 3 dias úteis para comunicação à ANPD e aos
   titulares (RCIS, arts. 6º e 9º), contado do marco de conhecimento efetivo, ressalvado prazo específico
   previsto em legislação setorial; prazo de complementação de 20 dias úteis (art. 6º, §3º); dobra de prazos
   para agente de tratamento de pequeno porte formalmente enquadrado (art. 6º, §8º, e art. 9º, §6º). **A
   ampliação do escopo do incidente não reinicia automaticamente o prazo do mesmo incidente**: trata-se do
   mesmo processo de comunicação, complementado conforme o art. 6º, §3º; um novo prazo próprio somente se aplica
   a um incidente distinto, com seu próprio marco de conhecimento.
6. **Preparação, envio e comprovação das comunicações:** conforme o Instrumento 5 (Partes A, B e C); a
   preparação não equivale ao envio, e o envio não equivale à comprovação.
7. **Complementações e acompanhamento de requisições da ANPD:** registradas no Instrumento 5, Parte C, sem
   limite numérico de complementações, controladas pelo prazo do art. 6º, §3º.

## 5. Evidências e continuidade

- **Preservação proporcional:** a preservação de evidências é priorizada de forma proporcional ao risco e à
  disponibilidade de recursos; não se exige preservação exaustiva de tudo, mas a decisão sobre o que preservar
  e o que não preservar deve ser registrada e justificada.
- **Integridade, custódia e armazenamento restrito:** evidências relevantes devem ter sua integridade
  preservada (ex.: hash, cópia read-only) e cadeia de custódia registrada (quem coletou, quando, com qual
  ferramenta); o armazenamento é restrito a quem tenha necessidade de conhecimento.
- **Registro de limitações e lacunas:** qualquer lacuna (log indisponível, janela de retenção expirada,
  ferramenta sem cobertura) deve ser registrada como tal, nunca omitida ou presumida como irrelevante.
- **Coordenação com operadores e fornecedores:** quando a evidência estiver sob custódia de um operador ou
  fornecedor, a organização deve solicitar formalmente sua preservação e fornecimento, registrando a solicitação
  e a resposta.
- **Recuperação validada:** a recuperação operacional (Recover, seção 3) só é considerada concluída após
  validação técnica e definição de critérios de aceite e de monitoramento adicional pelo período definido pela
  organização. `[preencher: critérios de aceite padrão para recuperação, se a organização quiser padronizá-los]`.
- **Conflitos entre contenção, preservação e continuidade:** quando uma ação de contenção ou preservação
  colidir com a continuidade do negócio (ex.: isolar um sistema crítico em produção), a decisão (com a opção
  escolhida, o risco aceito e o responsável) deve ser registrada explicitamente, conforme a autoridade definida
  na Política, seção 6.

## 6. Encerramento e melhoria

### 6.1 Decisão de encerramento

O encerramento da resposta ativa (saída do loop Scope/Contain/Eradicate/Recover) é uma decisão explícita e
atribuível do Líder de Resposta a Incidentes (ou da Direção, conforme a severidade, seção 2.2), apoiada em
indicadores técnicos (ex.: ausência de novos acessos pelo vetor identificado, validação de recuperação) e de
negócio (ex.: operação restabelecida dentro dos parâmetros aceitáveis). A decisão deve registrar:

- indicadores que a sustentam;
- riscos residuais aceitos, com responsável e prazo de acompanhamento;
- pendências não resolvidas, com responsável e prazo.

### 6.2 Três encerramentos distintos

O encerramento técnico **não** implica, por si só, a conclusão das comunicações regulatórias/aos titulares nem o
encerramento do acompanhamento regulatório: são três estados distintos, registrados separadamente:

| Estado | O que significa | Onde é registrado |
|---|---|---|
| Encerramento técnico | Resposta ativa (Scope/Contain/Eradicate/Recover) concluída | Instrumento 1 |
| Conclusão das comunicações | Comunicação à ANPD e aos titulares preparadas, enviadas e comprovadas (quando devidas) | Instrumento 5, Parte C |
| Encerramento do acompanhamento regulatório | Processo perante a ANPD extinto, ou requisições adicionais respondidas e sem pendência | Instrumento 5, Parte C (registro "Outras providências") |

### 6.3 Reabertura

Um incidente com encerramento técnico pode ser reaberto quando surgirem evidências de persistência,
reincidência ou escopo não identificado anteriormente; a reabertura reaproveita o registro original
(Instrumento 1), com nova iteração do loop (seção 3), e não cria um incidente novo, salvo decisão fundamentada
em contrário.

### 6.4 Debrief e melhoria contínua

O waypoint Debrief persegue dois objetivos distintos, que não devem ser confundidos: **encerrar formalmente o
esforço de resposta** (avaliar e decidir sobre cada ativo temporário criado durante a resposta, entre eles
acessos, imagens forenses, contas e monitoramentos adicionais; consolidar a documentação dispersa em um registro
único e apurar as métricas do esforço) e **promover aprendizado organizacional** (revisão após a ação, relatório
final, apresentação às partes interessadas e plano de melhoria com ações, responsáveis e prazos). Todo incidente
com encerramento técnico gera um debrief (Instrumento 4), com:

- avaliação e decisão sobre a retenção, modificação ou descomissionamento de cada ativo temporário criado
  durante a resposta;
- ações corretivas identificadas, responsável e prazo para cada uma;
- prova de efetividade da ação corretiva, quando concluída (ex.: teste, evidência de configuração aplicada):
  **uma ação corretiva só é considerada concluída mediante essa prova, nunca apenas pela afirmação de que foi
  feita**;
- lições aprendidas incorporadas, quando aplicável, à revisão da Política, deste Plano e do Playbook.

---

*Este é um modelo institucional reutilizável, de caráter educacional e operacional. Não contém dados de nenhuma
organização específica, não reproduz os cenários fictícios de treinamento deste repositório, e não produz
efeitos até ser adaptado aos campos de configuração e formalmente aprovado pela autoridade competente da
organização adotante. Licenciado sob CC BY 4.0, por Felipe Nicácio.*
