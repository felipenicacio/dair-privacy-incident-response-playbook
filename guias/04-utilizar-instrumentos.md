# Jornada 4: Utilizar os instrumentos operacionais (v0.3)

**Público:** líder de IR, SOC, IAM, analista forense, encarregado/DPO, Jurídico, donos de serviço.
**Objetivo:** usar os cinco instrumentos editáveis da v0.3 durante um incidente real, mantendo rastreabilidade entre eles.

> Esta jornada complementa as jornadas [Aprender](01-aprender.md), [Implementar](02-implementar.md) e [Testar equipe](03-testar-equipe.md); não as substitui. Leia o [procedimento operacional DAIR](../playbook/01-procedimento-operacional.md) antes de usar os instrumentos pela primeira vez.

## Os cinco instrumentos

| Instrumento | Arquivo | Quando usar |
|---|---|---|
| 1. Registro e Gestão de Incidentes | [`instrumentos/excel/DAIR_Registro_e_Gestao_de_Incidentes_v0.3.xlsx`](../instrumentos/excel/DAIR_Registro_e_Gestao_de_Incidentes_v0.3.xlsx) | Do momento da detecção até o encerramento; é o registro central do incidente |
| 2. Avaliação de Riscos aos Titulares e ANPD | [`instrumentos/excel/DAIR_Avaliacao_Riscos_Titulares_ANPD_v0.3.xlsx`](../instrumentos/excel/DAIR_Avaliacao_Riscos_Titulares_ANPD_v0.3.xlsx) | A partir da confirmação de que dados pessoais foram afetados; revisado a cada nova evidência relevante |
| 3. Checklist Operacional Dinâmico | [`instrumentos/excel/DAIR_Checklist_Operacional_v0.3.xlsx`](../instrumentos/excel/DAIR_Checklist_Operacional_v0.3.xlsx) | Durante a execução técnica da resposta, em paralelo ao Instrumento 1 |
| 4. Relatório de Resposta ao Incidente | [`instrumentos/word/DAIR_Relatorio_Resposta_Incidente_v0.3.docx`](../instrumentos/word/DAIR_Relatorio_Resposta_Incidente_v0.3.docx) | No debrief e no encerramento, consolidando os demais instrumentos |
| 5. Kit de Preparação e Controle das Comunicações ANPD/Titulares | [`instrumentos/word/DAIR_Kit_Preparacao_Comunicacoes_ANPD_Titulares_v0.3.docx`](../instrumentos/word/DAIR_Kit_Preparacao_Comunicacoes_ANPD_Titulares_v0.3.docx) | Quando o Instrumento 2 indicar necessidade de preparar a comunicação à ANPD e/ou aos titulares, antes de abrir o peticionamento no SEI!ANPD |

Um conjunto demonstrativo, preenchido com o cenário fictício INC-2026-001 já usado neste repositório, está em [`instrumentos/exemplos/`](../instrumentos/exemplos/); use-o apenas para aprender o preenchimento, nunca como modelo de registro real.

## 1. Qual instrumento utilizar e em que momento

1. **Abertura do incidente (Detect / Verify and Triage):** abrir o Instrumento 1 (aba Cadastro_Incidente) e registrar o primeiro evento na Linha_do_Tempo. Se houver indício de dados pessoais afetados, abrir também o Instrumento 2 (aba Caracterizacao, versão 1).
2. **Durante o Response Actions Loop (Scope/Contain/Eradicate/Recover):** usar o Instrumento 3 para acompanhar atividades técnicas e o Instrumento 1 (Linha_do_Tempo, Registro_Decisoes, Gestao_Evidencias) para o registro factual. Cada reentrada no loop gera um novo Ciclo DAIR (CIC-NN), usado nos três instrumentos.
3. **Nova evidência que altera natureza, escopo ou risco:** registrar uma nova versão no Instrumento 2 (Caracterizacao, Avaliacao_Regulatoria, Decisao_Comunicacao, todas com o mesmo ID de avaliação e versão incrementada) e resumir a mudança na aba Historico.
4. **Decisão de comunicar:** preencher o Instrumento 5 (Partes A e B) a partir da avaliação aprovada no Instrumento 2, e controlar a execução na Parte C.
5. **Debrief e encerramento:** consolidar tudo no Instrumento 4, remetendo aos IDs dos demais instrumentos em vez de duplicar o conteúdo integralmente.

## 2. Quem deve preencher cada instrumento

Use a [matriz de papéis e escalonamento](../governanca/01-papeis-e-escalonamento.md) como referência de responsabilidade. Em resumo:

- **Instrumento 1:** líder de IR (cadastro, linha do tempo, decisões), analista forense (evidências), responsáveis técnicos (plano de ações).
- **Instrumento 2:** encarregado/DPO elabora a análise; Jurídico e representante autorizado do controlador aprovam a decisão de comunicação.
- **Instrumento 3:** cada responsável técnico designado preenche sua atividade; o líder de IR acompanha o painel geral e o Controle_Ciclos.
- **Instrumento 4:** líder de IR consolida, com contribuição do encarregado (seções 13 e 14) e do Jurídico (revisão geral).
- **Instrumento 5:** encarregado/DPO prepara o conteúdo; Jurídico e representante autorizado aprovam antes de qualquer envio pelos canais oficiais.

## 3. Como registrar e atualizar informações

- Preencha apenas as células com fundo amarelo (entrada manual); não edite células com fundo cinza (cálculo automático).
- Use sempre os menus suspensos quando disponíveis.
- Quando um dado ainda não for conhecido, use os estados explícitos oferecidos ("Não informado", "Desconhecido", "Hipótese", "Em apuração") em vez de deixar a célula em branco sem explicação ou presumir um valor.
- Nunca sobrescreva uma versão anterior no Instrumento 2: registre uma nova linha com versão incrementada.
- No Instrumento 3, duplique a atividade em uma nova linha com um novo Ciclo DAIR ao reabrir Scope/Contain/Eradicate/Recover; não há ordem obrigatória nem limite de reentradas.

## 4. Como relacionar um instrumento aos demais

Todos os instrumentos compartilham os mesmos prefixos de identificador:

| Prefixo | Significado | Onde nasce |
|---|---|---|
| `INC-AAAA-NNN` | Incidente | Instrumento 1, aba Cadastro_Incidente |
| `EVT-NNN` | Evento da linha do tempo | Instrumento 1, aba Linha_do_Tempo |
| `EVD-NNN` | Evidência | Instrumento 1, aba Gestao_Evidencias |
| `DEC-NNN` | Decisão | Instrumento 1, aba Registro_Decisoes |
| `ACA-NNN` | Ação (operacional ou corretiva) | Instrumento 1, aba Plano_Acoes |
| `AVR-NN` | Avaliação de risco (versionada) | Instrumento 2, aba Caracterizacao |
| `CIC-NN` | Ciclo DAIR (iteração do loop) | Instrumento 3, aba Controle_Ciclos |
| `CHK-NNN` | Atividade do checklist | Instrumento 3, aba Checklist |

**Correção pós-auditoria (A02/A03):** a partir desta versão, todos os identificadores acima são **digitados manualmente**: não são mais gerados por fórmula a partir da posição da linha. Isso significa que: (1) o mesmo ID nunca deve ser reutilizado ou renumerado, mesmo após inserir, excluir ou reordenar linhas; (2) no Instrumento 2, é a mesma ID `AVR-NN`, e não a posição da linha, que liga os registros de Caracterizacao, Avaliacao_Regulatoria, Decisao_Comunicacao e a linha correspondente em Historico; preencha sempre a mesma ID nas quatro abas para a mesma versão. Cada planilha sinaliza em vermelho e bloqueia a gravação de um ID duplicado dentro da mesma coluna.

**Não há sincronização automática entre os arquivos nesta versão.** Para transferir informação de um instrumento a outro sem perder rastreabilidade: copie o identificador relevante (não o conteúdo integral) entre as planilhas e documentos, e, no Instrumento 4, remeta aos IDs em vez de retranscrever os dados. O Instrumento 1 inclui ainda a aba **Operador_e_Acompanhamento**, para registrar o aviso do operador ao controlador (quando houver operador) e o acompanhamento de requisições/determinações da ANPD após a comunicação; distinga ali o encerramento técnico interno (decisão da equipe de resposta) da eventual extinção do processo pela autoridade (ato da ANPD).

## 5. Validações e aprovações necessárias

- O **resultado da avaliação regulatória** (Instrumento 2) é sempre manual e exige fundamentação registrada: nenhuma fórmula decide isso.
- A **matriz de probabilidade x impacto** (Instrumento 2, aba Matriz_Apoio) é apoio interno de priorização e nunca fundamento de decisão de comunicação.
- Os **prazos** (Instrumento 2, aba Decisao_Comunicacao) têm uma "Data Calculada de Apoio" (fórmula WORKDAY, 3 ou 6 dias úteis conforme elegibilidade de pequeno porte, sem considerar feriados) e um campo separado, manual, de "Prazo Aplicável Validado": é este último que vale, com fundamento, calendário de referência e responsável pela validação registrados. A elegibilidade de pequeno porte nunca deve ser presumida pelo nome ou porte aparente da organização. A complementação à ANPD (até 20 dias úteis) conta a partir da data em que a comunicação foi **efetivamente realizada**, nunca de uma data apenas planejada.
- Nenhuma comunicação deve ser registrada como "Enviado"/"Realizada" (Instrumento 5, Parte C) sem evidência comprobatória correspondente: a Parte C acompanha separadamente a comunicação inicial à ANPD, cada complementação (sem limite numérico predefinido), a comunicação aos titulares, a declaração do art. 9º, §4º, e outras providências ou requisições do processo; enviar à ANPD não significa que os titulares também foram comunicados, e juntar documento no SEI não realiza a comunicação aos titulares.
- O Instrumento 5 organiza e aprova internamente o conteúdo antes do preenchimento do formulário oficial do SEI!ANPD; ele não é o formulário oficial, não substitui o peticionamento eletrônico e não constitui protocolo por si só.
- O encerramento da resposta técnica (Instrumento 1, Instrumento 3, aba Controle_Ciclos, Instrumento 4 seção 18) exige decisão explícita e atribuível, conforme o modelo DAIR original.

## 6. Cuidados com dados pessoais e evidências sensíveis

- **Nunca** insira dados pessoais reais, credenciais, tokens, hashes reais ou evidências forenses reais nestes arquivos quando eles estiverem em um repositório público.
- Para uso em um incidente real, mantenha os instrumentos preenchidos em ambiente interno controlado, com controle de acesso e trilha de auditoria; nunca em repositório público.
- O art. 10 do RCIS exige retenção de registros de incidentes com dados pessoais por no mínimo cinco anos, inclusive os não comunicados; preserve todas as versões do Instrumento 2, mesmo as substituídas.
- Ao usar o conjunto demonstrativo como referência de preenchimento, lembre-se de que todos os seus dados são fictícios (ver [cenário e premissas](../cenario/01-cenario-e-premissas.md)).

## Limitações desta versão

Não há sincronização automática entre arquivos, não há geração automática de relatório a partir das planilhas, e os cálculos de prazo são apoio, não substituto de validação jurídica. Ver [relatório de validação v0.3](../validacao/relatorio-validacao-v0.3.md) para o detalhamento dos testes executados e das limitações conhecidas.
