# Relatório de validação técnica — v0.3 (correção pós-auditoria)

**Data desta validação:** 20/10/2026 · **Responsável pela execução:** sessão assistida por IA, sob supervisão de Felipe Nicácio.
**Base de partida:** commit `f81732b3e674535ee66393df1efb2dc7d0d0af29` (v0.3 original, auditada). Este relatório substitui
integralmente o relatório anterior e documenta apenas os testes efetivamente executados nesta correção — nenhum
teste aqui descrito foi presumido ou declarado sem execução correspondente. Ver a [matriz de achados A01–A18](matriz-achados-A01-A18.md)
para o rastreamento achado a achado.

## 1. Escopo testado

- 3 planilhas Excel (modelos em branco): `DAIR_Registro_e_Gestao_de_Incidentes_v0.3.xlsx`, `DAIR_Avaliacao_Riscos_Titulares_ANPD_v0.3.xlsx`, `DAIR_Checklist_Operacional_v0.3.xlsx`.
- 3 planilhas Excel (conjunto demonstrativo fictício): as mesmas três, com sufixo `_EXEMPLO-INC-2026-001`.
- 2 documentos Word: `DAIR_Relatorio_Resposta_Incidente_v0.3.docx`, `DAIR_Modelos_Comunicacao_ANPD_Titulares_v0.3.docx`.
- Todos os Markdown do repositório (playbook, cenário, governança, templates, guias, referências).

## 2. Motor e tipos de teste utilizados (distinção exigida pela auditoria)

| Tipo de teste | Ferramenta | O que comprova | O que NÃO comprova |
|---|---|---|---|
| Recálculo completo | `recalc.py` (LibreOffice headless) | Todas as fórmulas avaliam sem erro de fórmula (`#REF!`, `#DIV/0!` etc.) no motor LibreOffice | Compatibilidade com Microsoft Excel real (não testado nesta sessão — ver Limitações) nem correção lógica do resultado |
| Validação estrutural | `validate.py` (schema XML OOXML) | O `.docx` é um arquivo bem-formado, abre sem erro de estrutura | Qualidade visual, paginação ou conteúdo |
| Inspeção visual | Conversão para PDF + leitura das páginas renderizadas | Layout, cortes de tabela, cabeçalho/rodapé, paginação, legibilidade | Comportamento interativo no Word/Excel real |
| Inspeção de valores calculados | Leitura programática do arquivo recalculado (`data_only=True`) | O **resultado lógico** de uma fórmula (não apenas ausência de erro) | — |
| Revisão de conteúdo jurídico | Leitura de fonte primária (RCIS, LGPD) e reconciliação manual do texto | Que o texto do instrumento corresponde ao artigo citado | Validade jurídica para um caso concreto — exige Jurídico |

## 3. Correção A02 — Identificadores manuais e estáveis

**Mudança:** todas as colunas de ID (EVT-, DEC-, EVD-, ACA- no Instrumento 1; AVR- no Instrumento 2; CIC-, CHK- no
Instrumento 3) deixaram de ser fórmulas baseadas em `ROW()` e passaram a ser campos de entrada manual, com:
- validação de dados customizada que bloqueia (`showErrorMessage=True`, `errorStyle="stop"`) a gravação de um ID
  duplicado dentro da mesma coluna;
- formatação condicional que sinaliza em vermelho qualquer duplicidade já existente.

**Teste executado:** inserida uma linha de evento isolada no Instrumento 1 (EVT-021, linha 24, fora do bloco
inicial de preenchimento) e recalculado o arquivo — 0 erros de fórmula, validação e formatação presentes na
linha. O Painel contou corretamente 13 eventos (12 do cenário + 1 de teste), confirmando que a contagem não
depende da posição da linha nem de uma fórmula de ID.

## 4. Correção A03 — Reconciliação de versões da avaliação regulatória

**Problema identificado pela auditoria:** no exemplo anterior, a versão 2 da avaliação aparecia como `AVR-02` nas
abas Caracterizacao/Avaliacao_Regulatoria/Decisao_Comunicacao, mas como `AVR-01` na aba Historico — porque o ID
era calculado pela posição da linha em cada aba, de forma independente.

**Correção:** o ID (`AVR-NN`) agora é digitado manualmente e é ele — não a linha — que liga o registro entre as
quatro abas. Uma formatação condicional compara a coluna "Versão" com os dois últimos dígitos da ID e sinaliza em
vermelho qualquer divergência (ex.: ID `AVR-02` com Versão diferente de `2`).

**Teste executado:** reconstruído o conjunto demonstrativo com `AVR-01` (v1, informações insuficientes) e
`AVR-02` (v2, condições potencialmente atendidas) consistentes nas quatro abas, incluindo a linha de Historico
que agora traz `AVR-02` (não mais `AVR-01`) como ID de chegada. Recálculo: 0 erros. Inspeção de valores: a
fórmula de Condição B (coluna calculada a partir dos incisos I–VI) retornou `"Em apuração"` para AVR-01 e
`"Sim"` para AVR-02, consistente com a fundamentação textual registrada.

## 5. Correção A04 — Reconciliação de efetividade, evidência e encerramento

**Problema identificado pela auditoria:** o exemplo anterior registrava "token revogado" e "validação de
contenção" como atividades concluídas e efetivas apoiadas apenas no log de execução da revogação — sem um
registro de teste técnico posterior que comprovasse que o acesso indevido deixou de funcionar.

**Correção:** acrescentados dois eventos fictícios explícitos de teste técnico no Instrumento 1 (`EVT-004` —
teste da 1ª contenção, revelando falha; `EVT-009` — teste da 2ª contenção, confirmando efetividade), com
referência cruzada no Instrumento 3 (atividades de Contenção, coluna "Efetividade Comprovada" ajustada de
"Testada — parcialmente efetiva" para "Testada — inefetiva" na 1ª iteração, e "Testada — efetiva" na 2ª,
cada uma referenciando a evidência e o teste correspondentes). A redução emergencial de privilégios (bloqueio de
conta) foi distinguida textualmente da revogação do token (melhoria estrutural完成 apenas no Ciclo 2). A janela de
observação pós-recuperação (atividade "Acompanhar indicadores pós-recuperação") permanece "Em andamento" no
encerramento do exercício, não "Concluída" — refletindo que a observação continuada é uma situação aberta, não um
evento pontual.

**Teste executado:** recálculo dos três arquivos Excel do conjunto demonstrativo após a reconciliação — 0 erros.
Revisão manual cruzada: cada afirmação de "Testada — efetiva/inefetiva" no Instrumento 3 agora tem uma
evidência vinculada (`EVD-003` ou `EVD-005`) que, na Gestão de Evidências do Instrumento 1, descreve o teste
específico (método, data/hora e resultado), não apenas o log de execução administrativa.

## 6. Correção A05 — Referências ao art. 5º do RCIS

**Problema identificado pela auditoria:** diversos pontos do repositório (Instrumento 2 — cabeçalho e texto de
instruções; Instrumento 4, seção 13; `templates/01-avaliacao-risco-titulares.md`; `playbook/01-procedimento-operacional.md`)
citavam "art. 5º, II" como se fosse a única hipótese da Condição B, quando o inciso II trata especificamente de
dados de crianças/adolescentes/idosos — dados financeiros estão no inciso III e larga escala no inciso VI.

**Correção:** todas as ocorrências foram revisadas e corrigidas para referenciar "art. 5º, caput e incisos I a
VI", com a lista completa (I. sensíveis; II. crianças/adolescentes/idosos; III. financeiros; IV. autenticação;
V. sigilo legal/judicial/profissional; VI. larga escala) nos pontos em que o texto já detalhava os incisos. O
Instrumento 2 passou a ter seis colunas estruturadas (uma por inciso), e a condição B é calculada por fórmula a
partir delas — corrigindo também a lógica cumulativa (A11).

**Teste executado:** `grep` em todos os arquivos Markdown e nos geradores dos instrumentos confirmando ausência
de qualquer referência remanescente a "art. 5º, II" como hipótese única.

## 7. Correção A06 — Controle de prazos

**Mudança no Instrumento 2 (Decisao_Comunicacao):** a antiga coluna única "Prazo Regulatório (apoio)" foi
desdobrada em: (1) "Data Calculada de Apoio" (fórmula `WORKDAY`, 3 ou 6 dias úteis conforme elegibilidade de
pequeno porte, sem feriados); (2) "Prazo Aplicável Validado" (campo manual); (3) "Fundamento da Validação do
Prazo"; (4) "Calendário de Referência Utilizado"; (5) "Responsável pela Validação do Prazo". A elegibilidade de
pequeno porte é um campo manual próprio ("Não avaliado" por padrão), nunca presumida. A complementação (até 20
dias úteis) passou a ser calculada a partir da "Data da Comunicação à ANPD Efetivamente Realizada" — se essa
data não estiver preenchida, o campo exibe explicitamente "Sem data de comunicação efetiva registrada", em vez
de calcular a partir de uma data planejada.

**Teste executado:** no conjunto demonstrativo, a "Data Calculada de Apoio" para AVR-01 e AVR-02 (marco
19/10/2026 09h25, pequeno porte "Não elegível") recalculou corretamente para 22/10/2026 — consistente com o
texto já publicado no cenário (v0.2). A "Necessidade de Complementação" retornou corretamente a mensagem
explícita de ausência de data efetiva, já que, no exercício, nenhuma comunicação foi de fato enviada.

## 8. Correção A07 — Acompanhamento das comunicações

**Mudança no Instrumento 5 (Parte C):** a tabela única "ANPD x Titulares" foi substituída por um controle que
acompanha separadamente a comunicação inicial à ANPD, até duas complementações, a comunicação aos titulares e a
declaração do art. 9º, §4º — cada uma com processo/protocolo, conteúdo/versão enviada, responsável, aprovação,
prazo aplicável, data de envio, meio utilizado, evidência comprobatória e situação. Acrescentada a seção C.1
(classificação de acesso dos documentos anexados e conferência de representação) e C.2 (divulgação pública
alternativa: justificativa, canais, datas e evidência de manutenção pelo período mínimo de 3 meses). O texto
agora destaca explicitamente que o envio à ANPD não implica que os titulares também foram comunicados.

**Teste executado:** conversão para PDF e inspeção visual de todas as páginas (ver seção 10) — tabela renderiza
sem cortes após ajuste de largura de colunas (ver achado de teste registrado abaixo).

## 9. Correção A08 — Fundamentação e estados decisórios

**Mudança no Instrumento 2 (Avaliacao_Regulatoria):** os seis incisos do art. 5º passaram a ser colunas
individuais (Sim/Não/Em apuração), com fundamentação própria por inciso assinalado — corrigindo o tratamento de
30 mil titulares como limiar automático de larga escala (a fundamentação agora exige descrever volume,
abrangência, duração e proporção da base, não apenas o número). Acrescentada a coluna "Estado da Análise"
(Parecer em elaboração / Análise concluída aguardando aprovação / Análise aprovada / Decisão de comunicar
confirmada / Autorização de envio emitida), distinta do campo "Resultado da Avaliação". O texto contraditório
"Não — aprofundar Scope" (que combinava uma negativa com uma ação afirmativa) foi substituído por "Sim —
aprofundar Scope e confirmar perfil etário dos titulares antes de concluir o inciso II".

**Teste executado:** inspeção de valores do conjunto demonstrativo — AVR-01 retorna "Em apuração" (não "Não") no
cálculo de Condição B, refletindo corretamente a incerteza sobre o inciso II; AVR-02 retorna "Sim", fundamentado
separadamente nos incisos III e VI.

## 10. Testes em arquivos Excel (recálculo)

**Ferramenta:** recálculo completo via LibreOffice headless (`recalc.py`).

| Arquivo | Fórmulas | Erros encontrados |
|---|---|---|
| DAIR_Registro_e_Gestao_de_Incidentes_v0.3.xlsx | 12 | 0 |
| DAIR_Avaliacao_Riscos_Titulares_ANPD_v0.3.xlsx | 150 | 0 |
| DAIR_Checklist_Operacional_v0.3.xlsx | 0 | 0 |
| DAIR_Registro_e_Gestao_de_Incidentes_v0.3_EXEMPLO-INC-2026-001.xlsx | 12 | 0 |
| DAIR_Avaliacao_Riscos_Titulares_ANPD_v0.3_EXEMPLO-INC-2026-001.xlsx | 150 | 0 |
| DAIR_Checklist_Operacional_v0.3_EXEMPLO-INC-2026-001.xlsx | 0 | 0 |

O Instrumento 3 (Checklist) tem 0 fórmulas porque todos os seus IDs passaram a ser manuais (A02) e não possui
indicadores calculados — por desenho, não por omissão.

Todos os seis arquivos abrem sem mensagem de corrupção no LibreOffice (motor de verificação disponível nesta
sessão). **Não foi possível testar a abertura no Microsoft Excel real** — ver Limitações.

### 10.1 — Investigação específica de `Painel!B9` (achado A16)

A auditoria identificou divergência entre o valor salvo e o motor de recálculo independente na fórmula antiga de
"Ciclos DAIR distintos identificados", que usava `SUMPRODUCT((intervalo<>"")/COUNTIF(intervalo,intervalo&""))`.
Esse padrão é sensível a como cada motor trata células vazias no critério `""` do `COUNTIF` — uma causa plausível
e tecnicamente coerente da divergência relatada, mas não confirmada contra o Microsoft Excel real nesta sessão
(apenas o LibreOffice estava disponível). Em vez de presumir qual motor está "certo", a fórmula foi substituída
por uma contagem por correspondência exata de padrão (`"CIC-"&TEXT(n,"00")`), sem divisão nem dependência de como
cada motor conta strings vazias. **Teste executado:** no conjunto demonstrativo (2 ciclos reais, CIC-01 e
CIC-02), a nova fórmula retornou exatamente `2`, valor correto e verificável manualmente.

### 10.2 — Testes de expansão (achado A10)

| Teste exigido pela auditoria | Resultado |
|---|---|
| 21º evento (Linha_do_Tempo) | Registrado na linha 24 (fora do bloco inicial); recálculo sem erro; Painel contou 13 eventos (12 do cenário + 1 de teste) |
| 17ª revisão de avaliação (Instrumento 2, Historico) | Testado em cópia de verificação (`AVR-17`, linha 20): recálculo sem erro, fórmulas e validação presentes — não incorporado ao conjunto demonstrativo publicado, por não fazer parte do cenário fictício |
| 13º ciclo (Controle_Ciclos) | Registrado (`CIC-13`, linha 16 do conjunto demonstrativo) como linha de teste isolada; recálculo sem erro |
| 31ª atividade (Checklist) | Registrada (`CHK-031`, linha 34) como linha de teste isolada; recálculo sem erro |
| Registros além da linha 200 | As quatro abas de registro do Instrumento 1 têm fórmulas/validação pré-formatadas até a linha 503 (testado: linha 503 tem validação de waypoint e preenchimento ativos) |

As faixas são fixas e generosas (não ilimitadas) — ver `guias/04-utilizar-instrumentos.md` para como estender
manualmente além do limite pré-formatado.

## 11. Testes em arquivos Word

**Ferramentas:** validação de esquema XML (`validate.py`), conversão para PDF e inspeção visual de todas as
páginas renderizadas.

| Arquivo | Validação de esquema | Páginas (PDF) | Inspeção visual |
|---|---|---|---|
| DAIR_Relatorio_Resposta_Incidente_v0.3.docx | Aprovada (329 parágrafos) | 8 | Capa, sumário e seções 1–20 inspecionadas; seção 13 corrigida (inciso correto) |
| DAIR_Modelos_Comunicacao_ANPD_Titulares_v0.3.docx | Aprovada (160 parágrafos) | 4 (antes: 5) | Todas as páginas inspecionadas em duas rodadas (antes e depois da correção de cabeçalho da Parte C) |

**Falha encontrada e corrigida nesta correção (1ª iteração):** a tabela da Parte C (Instrumento 5), ao ser
ampliada para 10 colunas (A07), foi definida inicialmente com larguras somando mais que a largura útil da
página, causando quebra de palavra em cada caractere nos cabeçalhos. Corrigida a soma das larguras para
exatamente a largura útil (9000 DXA).

**Falha encontrada e corrigida nesta correção (2ª iteração — inspeção completa de página):** mesmo após o
ajuste de largura total, a inspeção visual página a página (seção 11 completa, abaixo) revelou que cabeçalhos
longos em colunas estreitas (ex.: "Processo/Protocolo (SEI ou outro)", "Responsável") ainda quebravam
caractere a caractere, por excederem a largura da coluna sem um ponto de quebra natural. Corrigido reduzindo o
tamanho da fonte dos cabeçalhos desta tabela (de 10pt para 7pt) e abreviando os rótulos mais longos ("Processo/
Protocolo (SEI ou outro)" → "Protocolo (SEI/ outro)"; "Conteúdo/Versão Enviada" → "Conteúdo/ Versão"; "Data de
Envio/Juntada" → "Data de Envio"; "Evidência Comprobatória" → "Evidência"), com uma nota de rodapé explicando as
abreviações. Nova inspeção visual confirmou cabeçalhos legíveis, sem quebra de caractere.

**Dados fictícios misturados a modelos reutilizáveis:** confirmado que não há — os Instrumentos 4 e 5 contêm
apenas rótulos de campo e instruções de preenchimento em itálico, sem nenhum dado do incidente fictício
INC-2026-001 (reconferido após as edições desta correção).

## 12. Consistência entre instrumentos e Markdown

- Nomenclatura de identificadores (`INC-`, `EVT-`, `EVD-`, `DEC-`, `ACA-`, `AVR-`, `CIC-`, `CHK-`) revisada e
  consistente nos três instrumentos Excel, no conjunto demonstrativo e no `guias/04-utilizar-instrumentos.md`.
- Referências cruzadas de evidência (`EVD-NNN`) conferidas em todos os arquivos do conjunto demonstrativo —
  mantida a numeração EVD-001 a EVD-006 já usada na v0.2/v0.3 original, agora reconciliada com os novos eventos
  de teste técnico (EVT-004, EVT-009).
- Referências ao art. 5º do RCIS (A05) revisadas em todos os Markdown e nos geradores dos instrumentos.
- `templates/02-registro-incidente.md` atualizado para refletir a falha de contenção testada e a revogação
  confirmada por teste técnico (A04), e `templates/01-avaliacao-risco-titulares.md` atualizado para fundamentar
  separadamente os incisos III e VI (A05/A08).

## 13. Fontes externas consultadas nesta correção

Ver `referencias/fontes-e-atribuicoes.md` (reescrito nesta correção — A17) para URLs específicas, artigos
efetivamente utilizados, data de consulta (20/10/2026) e a distinção entre fontes efetivamente utilizadas e
referências complementares.

## 14. Limitações conhecidas

- Os testes de abertura e recálculo foram executados no LibreOffice (motor de verificação disponível nesta
  sessão), não no Microsoft Excel/Word real. A estrutura dos arquivos (`.xlsx`/`.docx` nativos, sem recursos
  proprietários) reduz o risco de incompatibilidade, mas uma verificação adicional no Microsoft Office é
  recomendada antes do uso em produção — nenhuma afirmação de compatibilidade perfeita é feita aqui.
- Os cálculos de prazo regulatório (Instrumento 2) usam `WORKDAY` sem lista de feriados nacionais/municipais —
  o campo que efetivamente vale é o "Prazo Aplicável Validado" (manual, com fundamento e responsável), não a
  "Data Calculada de Apoio".
- Não há sincronização automática entre os cinco instrumentos; a integração depende de disciplina manual no uso
  dos identificadores compartilhados, agora manuais e com detecção de duplicidade (A02).
- As faixas de linhas pré-formatadas (A10) são fixas, embora generosas (até ~500 por aba no Instrumento 1; até
  53 no Instrumento 2; até 153 atividades e 43 ciclos no Instrumento 3) — não há capacidade ilimitada.
- O exercício de "envio incorreto/alteração indevida/indisponibilidade sem exfiltração" (seção 5 do escopo da
  auditoria) foi conduzido como revisão de conteúdo (A15: catálogo do Instrumento 3 agora aceita incidentes de
  integridade/disponibilidade/autenticidade sem atacante ou exportação, e detecção por pessoas/fornecedores sem
  ferramenta de monitoramento) — não foi conduzido como simulação dinâmica adicional de tabletop nesta correção.
- O conjunto demonstrativo cobre os três instrumentos em Excel; os Instrumentos 4 e 5 permanecem, por decisão de
  projeto, modelos em branco sem dados do cenário fictício (ver `guias/04-utilizar-instrumentos.md`).

## 15. Exercícios adicionais — envio incorreto, alteração indevida e indisponibilidade sem exfiltração

Executados em cópias de verificação dos arquivos publicados (não incorporados ao repositório), para comprovar o
comportamento dos controles sem alterar os arquivos entregues.

**Envio incorreto (duplicidade de identificador):** em uma cópia do conjunto demonstrativo do Instrumento 1,
o identificador já existente `EVT-001` foi gravado deliberadamente em uma segunda linha (linha 25), simulando um
operador digitando um ID já usado. Resultado: a validação de dados (`showErrorMessage=True, errorStyle="stop"`)
e a formatação condicional cobrem exatamente a célula alterada (`A4:A503`); uma simulação programática do mesmo
critério usado pela fórmula de validação (`COUNTIF`) confirmou a duplicidade (`EVT-001` → 2 ocorrências), ou
seja, o controle teria bloqueado a gravação em uso interativo real (Excel/LibreOffice Calc) e sinalizaria em
vermelho qualquer duplicidade que já exista no arquivo.

**Alteração indevida (sobrescrita de fórmula):** em uma cópia do modelo em branco do Instrumento 1, a fórmula de
`Painel!B9` (`=COUNTIF(Linha_do_Tempo!C4:C503,"<>")`) foi deliberadamente substituída por um valor fixo
incorreto (`999`), simulando alguém sobrescrevendo uma célula calculada. Resultado: o recálculo automatizado
(`recalc.py`) reportou sucesso e zero erros — uma sobrescrita de fórmula por um valor estático não gera, por si
só, um erro de fórmula detectável por ferramentas automatizadas. Esse é um limite técnico real documentado nesta
correção (consistente com a nota de A11 de que o sombreamento de células é controle de usabilidade, não de
segurança): a proteção contra alteração indevida depende de controles organizacionais (controle de versão,
permissões de edição na camada de distribuição/armazenamento do arquivo), não de um mecanismo interno da
planilha. Recomenda-se tratar os modelos em branco como somente leitura para a maioria dos usuários e distribuir
cópias de trabalho versionadas.

**Indisponibilidade sem exfiltração:** confirmado por revisão de conteúdo (não requer alteração de arquivo) que
o catálogo do Instrumento 3 (atividades de Verify & Triage, linha "Confirmar a natureza do incidente") já não
pressupõe confidencialidade comprometida nem exportação de dados — o texto cobre explicitamente incidentes de
"confidencialidade, integridade, disponibilidade ou autenticidade... mesmo sem atacante identificado", e a
atividade de detecção aceita indício "de qualquer origem (ferramenta, pessoa ou fornecedor)". Um cenário de
indisponibilidade pura (ex.: ransomware sem exfiltração confirmada) é, portanto, registrável no instrumento sem
adaptação adicional.

## 16. Pontos que exigem revisão humana

- Validação jurídica da contagem de prazos (feriados, regra de pequeno porte, prazo setorial específico) e da
  aplicabilidade de cada inciso do art. 5º do RCIS ao caso concreto — o instrumento aponta a necessidade, não
  decide por fórmula.
- Confirmação por um usuário de Microsoft Excel/Word real antes de uso em produção, dado que a verificação
  automatizada usou o LibreOffice.
- Revisão jurídica da redação da Parte A.2 do Instrumento 5 (pequeno porte, representação, sigilo) frente ao
  caso concreto de cada organização.
- Decisão sobre se as faixas de expansão (A10) atendem ao volume esperado de incidentes da organização, ou se
  devem ser ampliadas antes do uso.
