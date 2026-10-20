# Política de Resposta a Incidentes

> **Modelo institucional reutilizável.** Este documento estabelece diretrizes, responsabilidades e autoridade
> para a resposta a incidentes de segurança da informação, incluindo os que envolvem dados pessoais. Ele **não
> contém dados de nenhuma organização específica** e **não constitui aprovação** — os campos de configuração
> abaixo devem ser preenchidos e o documento deve ser submetido à aprovação formal da organização antes de
> qualquer adoção. Esta Política define diretrizes, papéis e autoridade; os procedimentos de mobilização e
> execução estão no **Plano de Resposta a Incidentes**; os procedimentos técnicos detalhados estão no
> **Playbook DAIR**; os registros, avaliações e comunicações têm instrumentos próprios (ver [`README.md`](../README.md),
> seção "Hierarquia dos documentos").

## 0. Dados de controle do documento

| Campo | Valor |
|---|---|
| Organização | `[preencher: razão social ou nome da organização adotante]` |
| Proprietário do documento (owner) | `[preencher: cargo ou área responsável pela manutenção, ex.: Diretoria de Segurança da Informação]` |
| Aprovador | `[preencher: cargo com autoridade para aprovar, ex.: Diretoria Executiva / Comitê de Segurança]` |
| Versão | `[preencher: ex.: 1.0]` |
| Data de aprovação | `[preencher após aprovação formal — não inferir nem simular]` |
| Vigência | `[preencher: ex.: 24 meses a partir da aprovação, ou "até nova revisão"]` |
| Próxima revisão programada | `[preencher]` |
| Documentos relacionados | Plano de Resposta a Incidentes; Playbook DAIR; Instrumentos 1–5 (ver integração abaixo) |

Esta Política **não é autoaplicável**: ela só produz efeitos internos após adaptação aos campos acima e aprovação
pela autoridade competente da organização, registrada nesta tabela.

## 1. Objetivo e escopo

Esta Política estabelece as diretrizes, responsabilidades e autoridade para prevenir, detectar, responder e
encerrar incidentes de segurança da informação, incluindo incidentes de segurança envolvendo dados pessoais
(LGPD, art. 46 e seguintes), de forma proporcional, tempestiva e auditável.

Aplica-se a:

- empregados, estagiários e demais colaboradores da organização;
- terceiros, prestadores de serviço e fornecedores que tratem informações ou dados pessoais em nome da
  organização, ou que operem sistemas e serviços a ela conectados;
- sistemas, serviços, redes, aplicações e dados — próprios, hospedados por terceiros ou operados por terceiros —
  sob controle ou responsabilidade da organização;
- qualquer incidente identificado por meios internos, por notificação de terceiros, por operadores, por
  fornecedores, por titulares de dados ou por autoridades.

Esta Política não substitui normas setoriais específicas, cláusulas contratuais com maior exigência, nem
orientações da Autoridade Nacional de Proteção de Dados (ANPD) ou de outros reguladores aplicáveis — nos
conflitos, prevalece a norma ou o procedimento oficial vigente.

## 2. Definições

| Termo | Definição |
|---|---|
| Evento | Ocorrência observável em um sistema ou rede, sem indicação, por si só, de violação de segurança. |
| Suspeita de incidente | Evento ou conjunto de evidências que indica possível violação de segurança ainda não confirmada, exigindo verificação. |
| Incidente confirmado | Violação de segurança confirmada — da confidencialidade, integridade, disponibilidade ou autenticidade — de sistemas, serviços ou dados. |
| Incidente de segurança envolvendo dados pessoais | Incidente confirmado que afeta dados pessoais sob tratamento pela organização (como controladora) ou por ela operados (como operadora), nos termos do art. 46 da LGPD. |

Estas definições orientam a aplicação desta Política e do Plano; os critérios técnicos de classificação de
severidade e os critérios de avaliação do risco regulatório aos titulares estão detalhados, respectivamente, no
Plano de Resposta a Incidentes e no Instrumento 2 (Avaliação de Riscos aos Titulares e à ANPD).

## 3. Princípios de resposta

- **Proporcionalidade:** as medidas de contenção, investigação e comunicação devem ser proporcionais ao risco e
  ao impacto observados, evitando tanto a omissão quanto a reação desproporcional.
- **Preservação de evidências:** a coleta e a preservação de evidências relevantes têm prioridade sobre a
  conveniência operacional, sem condicionar a contenção emergencial à conclusão da investigação.
- **Continuidade:** a resposta busca restabelecer a operação de forma segura e validada, equilibrando
  continuidade de negócio com integridade da contenção e da investigação.
- **Proteção dos titulares:** o risco e o impacto aos titulares de dados pessoais são considerados desde a
  triagem inicial, de forma independente da classificação técnica de severidade.
- **Confidencialidade:** informações sobre o incidente são tratadas em bases de necessidade de conhecimento
  (*need-to-know*) enquanto a resposta está em curso, sem prejuízo das obrigações legais de comunicação.
  **Accountability:** toda decisão relevante é registrada, com responsável, data, fundamento e evidência
  associada, permitindo demonstração posterior do cumprimento desta Política.

## 4. Adoção do DAIR como abordagem dinâmica e iterativa

A organização adota o DAIR (*Dynamic Approach to Incident Response*) como abordagem de referência para a
execução operacional da resposta: Preparação, Detecção, Verificação e Triagem, seguidas do Loop de Ações de
Resposta (Escopo, Contenção, Erradicação, Recuperação), executado de forma iterativa — com reentradas sempre que
surgirem novas evidências, falhas de contenção, persistência do agente de ameaça ou mudança de escopo — e
Debrief ao final. Esta Política não impõe uma sequência rígida de etapas; a sequência operacional e os critérios
de reentrada estão detalhados no Plano de Resposta a Incidentes e no Playbook DAIR.

## 5. Papéis e responsabilidades

| Papel | Responsabilidade típica nesta Política |
|---|---|
| Direção / Diretoria designada | Autoridade final sobre decisões que afetem a continuidade do negócio, a reputação institucional ou exijam recursos extraordinários; aprova esta Política e o Plano. |
| Líder de Resposta a Incidentes | Coordena a resposta operacional, aciona os demais papéis e aplica a autoridade de declaração e priorização descrita na seção 6. |
| Segurança da Informação | Detecção, análise técnica, contenção, erradicação e recuperação, em articulação com TI. |
| Tecnologia da Informação (TI) | Suporte à contenção e à recuperação de sistemas, infraestrutura e serviços afetados. |
| Áreas de Negócio | Avaliação do impacto operacional e nas atividades de tratamento de dados sob sua responsabilidade; apoio à comunicação com clientes e parceiros. |
| Continuidade de Negócio | Articulação entre contenção/erradicação e planos de continuidade, quando a indisponibilidade afetar operações críticas. |
| Comunicação | Preparação e aprovação de mensagens internas, institucionais e, quando aplicável, públicas. |
| Jurídico | Avaliação de obrigações legais e contratuais, apoio à governança da decisão regulatória (seção 10) e à gestão de fornecedores (seção 12). |
| Encarregado (DPO) | Avaliação do risco aos titulares, condução da trilha de privacidade e, operacionalmente, preparação das comunicações à ANPD e aos titulares. |
| Controlador | Titular da obrigação legal de comunicar incidentes à ANPD e aos titulares (LGPD, arts. 48); pode delegar a execução, nunca a responsabilidade. |
| Operadores | Obrigação de informar o controlador sem demora injustificada ao identificar incidente envolvendo dados pessoais tratados em seu nome. |

A organização deve nomear, para cada papel acima, o(s) titular(es) e seus substitutos no Plano de Resposta a
Incidentes (que detalha contatos, canais e critérios de acionamento), mantendo esta Política livre de dados
pessoais ou nominais específicos. A tabela acima estabelece responsabilidades em termos gerais; a organização
deve detalhá-las em uma matriz RACI (Responsável, Aprovador, Consultado, Informado — ver
[`governanca/01-papeis-e-escalonamento.md`](01-papeis-e-escalonamento.md) para um exemplo) por atividade de
resposta, revisada e validada em exercício tabletop.

## 6. Autoridade de resposta

O Líder de Resposta a Incidentes, ou quem o substitua conforme definido no Plano, tem autoridade para:

- declarar formalmente a abertura de um incidente e sua classificação inicial;
- priorizar recursos internos para a resposta, inclusive realocando temporariamente pessoal técnico;
- determinar a contenção de acessos, contas, sessões, credenciais ou segmentos de rede;
- suspender temporariamente serviços ou funcionalidades, quando a continuidade representar risco desproporcional;
- autorizar o início da recuperação operacional, uma vez validada a contenção;
- declarar o encerramento da resposta ativa, sem prejuízo do acompanhamento residual (ver seção 14).

Decisões que envolvam risco relevante à continuidade do negócio, exposição reputacional significativa ou
compromisso de recursos extraordinários requerem ciência ou aprovação da Direção, conforme os limites que a
organização definir no Plano. O Plano detalha esta autoridade em uma matriz decisão a decisão (quem decide, quem
substitui, qual evidência a decisão deve deixar — ver Plano, seção 2.5), que complementa, sem substituir, a
matriz RACI da seção 5: RACI cobre participação e coordenação; a matriz de autoridade cobre o direito de decidir.

## 7. Obrigação interna de reportar

Todo colaborador, terceiro ou fornecedor com acesso a sistemas, serviços ou dados da organização tem o dever de
reportar, sem demora injustificada, qualquer suspeita ou confirmação de incidente de segurança, por canal
definido pela organização. A organização deve estabelecer, no Plano de Resposta a Incidentes, os canais
primários e alternativos de reporte e os substitutos para cada papel de recebimento, incluindo cobertura fora do
horário comercial.

## 8. Critérios gerais de classificação e escalonamento

A classificação de severidade técnica (impacto em sistemas, serviços e dados) é distinta da avaliação de risco
aos titulares (impacto a pessoas naturais) exigida pela LGPD e pela Resolução CD/ANPD nº 15/2024 (RCIS). Uma
severidade técnica baixa não afasta, por si só, risco relevante aos titulares, e vice-versa. Os critérios
operacionais de classificação e os limiares de escalonamento constam do Plano de Resposta a Incidentes; a
avaliação fundamentada do risco regulatório aos titulares é registrada no Instrumento 2, nunca decidida de forma
automática a partir de uma pontuação isolada (ver seção 11).

## 9. Diretrizes de coleta e proteção de evidências

- A coleta de evidências deve priorizar métodos que preservem integridade e cadeia de custódia, documentando
  ferramenta, responsável, data/hora e limitações.
- O acesso a evidências é restrito a quem tenha necessidade de conhecimento para a resposta ou para eventual
  processo regulatório, contratual ou judicial.
- Evidências relacionadas a incidentes envolvendo dados pessoais seguem também as regras de retenção da seção
  13 e as restrições de acesso aplicáveis a dados pessoais.
- Qualquer limitação ou lacuna na coleta (ex.: logs indisponíveis, janela de retenção expirada) deve ser
  registrada como tal, nunca omitida.

## 10. Diretrizes de comunicação

A comunicação em resposta a um incidente tem quatro destinatários possíveis, com requisitos distintos:

- **Interna:** entre os papéis envolvidos na resposta e a Direção, conforme a seção 6 e o Plano.
- **Contratual:** a clientes, parceiros ou fornecedores com quem haja obrigação contratual de notificação.
- **Regulatória:** à ANPD e a outros reguladores setoriais aplicáveis, conforme a governança da decisão
  regulatória (seção 11).
- **Aos titulares:** quando exigida pela avaliação de risco (LGPD, art. 48), preparada e executada conforme o
  Instrumento 5 e o Plano.

Em nenhum caso a comunicação a um destinatário substitui ou presume a comunicação aos demais.

## 11. Governança da decisão regulatória

A decisão sobre a necessidade de comunicar um incidente à ANPD e aos titulares é tomada por avaliação
fundamentada, registrada e versionada — nunca de forma automática a partir de uma pontuação ou matriz de
severidade isolada. A avaliação deve considerar os critérios do art. 5º da Resolução CD/ANPD nº 15/2024 (RCIS) e
ser revisada sempre que novas evidências relevantes surgirem. Os papéis de elaboração, revisão e aprovação dessa
avaliação, bem como o instrumento de registro, constam da seção 5 e do Instrumento 2.

## 12. Gestão de fornecedores e comunicação operador-controlador

- Contratos com fornecedores que operem dados pessoais em nome da organização devem prever obrigação de
  notificação ao controlador, sem demora injustificada, ao identificar incidente de segurança.
- A organização, quando atuar como operadora, deve notificar o controlador correspondente sem demora
  injustificada, fornecendo as informações necessárias à avaliação de risco deste.
- O Jurídico, em conjunto com a área responsável pela gestão de fornecedores, deve manter inventário atualizado
  de fornecedores com acesso a dados pessoais e dos canais de notificação de incidente de cada um.

## 13. Registro e retenção

Todo incidente, suas decisões, evidências e comunicações devem ser registrados nos instrumentos operacionais
definidos (ver seção "Hierarquia dos documentos" no [`README.md`](../README.md)). O prazo mínimo de retenção
observa a legislação aplicável — por exemplo, o art. 10 da Resolução CD/ANPD nº 15/2024 exige retenção mínima de
5 anos para os registros de incidentes de segurança envolvendo dados pessoais — sem prejuízo de obrigações
adicionais decorrentes de contrato, processo judicial ou regulatório em curso, que podem estender esse prazo.
`[preencher, se aplicável: prazos de retenção adicionais definidos pela organização para outras categorias de
registro]`.

## 14. Critérios gerais de encerramento, acompanhamento residual e reabertura

O encerramento da resposta ativa é uma decisão explícita e atribuível (ver seção 6), distinta da conclusão das
comunicações regulatórias e aos titulares e do encerramento do acompanhamento regulatório — que podem se
estender após o encerramento técnico. Riscos residuais aceitos e pendências devem ser registrados com
responsável e prazo. Um incidente encerrado pode ser reaberto quando surgirem evidências de persistência,
reincidência ou escopo não identificado anteriormente. Os critérios operacionais detalhados de aceite para
encerramento constam do Plano de Resposta a Incidentes.

## 15. Treinamento, exercícios, revisão e melhoria contínua

- A organização deve promover treinamento periódico dos papéis definidos na seção 5 sobre esta Política e o
  Plano correspondente.
- Exercícios simulados (*tabletop* ou técnicos) devem ser realizados com periodicidade definida pela
  organização, utilizando cenários realistas e registrando resultados e lições aprendidas.
- Todo incidente encerrado deve gerar um debrief com ações corretivas, responsáveis e prazos, cujo cumprimento é
  acompanhado até a conclusão (ver Plano, seção "Encerramento e melhoria").
- Esta Política e o Plano devem ser revisados com a periodicidade definida na seção 0, ou antes disso, sempre
  que houver mudança relevante na legislação aplicável, na estrutura organizacional ou após lições aprendidas de
  incidentes reais ou exercícios.

## 16. Gestão de exceções e não conformidade

Qualquer exceção ao disposto nesta Política deve ser solicitada, justificada e aprovada pela autoridade
competente definida pela organização, com registro da exceção, do prazo de vigência e do responsável. O
tratamento de não conformidades com esta Política segue as normas internas de conduta, disciplinares e de
compliance já vigentes na organização — esta Política não cria nem prevê sanções próprias.
`[preencher: referência à norma interna de conduta/disciplinar aplicável, se houver]`.

## 17. Documentos relacionados, aprovação e controle de versão

| Documento | Relação com esta Política |
|---|---|
| Plano de Resposta a Incidentes | Operacionaliza a mobilização e a coordenação da resposta com base nesta Política. |
| Playbook DAIR | Detalha os procedimentos técnicos de execução da resposta. |
| Instrumentos 1–5 | Registram, avaliam e controlam a execução, conforme a tabela de integração do `README.md`. |

Histórico de versões:

| Versão | Data | Alteração | Aprovador |
|---|---|---|---|
| `[preencher]` | `[preencher]` | `[preencher: ex.: versão inicial]` | `[preencher]` |

---

*Este é um modelo institucional reutilizável, de caráter educacional. Não contém dados de nenhuma organização
específica e não produz efeitos até ser adaptado aos campos de configuração da seção 0 e formalmente aprovado
pela autoridade competente da organização adotante. Licenciado sob CC BY 4.0 — Felipe Nicácio.*
