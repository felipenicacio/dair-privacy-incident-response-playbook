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

**Correção (e uma correção dentro da correção):** o ID (`AVR-NN`) agora é digitado manualmente e é ele — não a
linha — que liga o registro entre as quatro abas. A primeira versão desta correção usou uma formatação
condicional que comparava a coluna "Versão" com os dois últimos dígitos da própria ID (ex.: ID `AVR-02` deveria
ter Versão `2`) — um mecanismo frágil e artificial: ele força o número da versão a estar embutido no texto da
ID, o que não prova reconciliação alguma entre abas (duas abas podem ter a mesma ID com dígitos "corretos" e,
ainda assim, dados de versões diferentes uma da outra) e quebraria em casos legítimos em que a numeração da ID
não seguisse estritamente a sequência de versões. **Corrigido nesta revisão:** a formatação condicional agora
testa, por fórmula, se a MESMA combinação ID+Versão desta linha também existe nas outras abas relacionadas
(`COUNTIFS` cruzado entre abas, não extração de dígitos da ID) — por exemplo, em Caracterizacao:
`COUNTIFS(Avaliacao_Regulatoria!$A$4:$A$53,$A4,Avaliacao_Regulatoria!$B$4:$B$53,$B4)=0` sinaliza em vermelho se
aquele par ID+Versão não existir em Avaliacao_Regulatoria. Esse é o teste de reconciliação que o achado A03
realmente pede ("relacionar de forma inequívoca incidente/avaliação/versão... sem depender de alinhamento pela
mesma linha").

**Teste executado:** reconstruído o conjunto demonstrativo com `AVR-01` (v1, informações insuficientes) e
`AVR-02` (v2, condições potencialmente atendidas) consistentes nas quatro abas, incluindo a linha de Historico
que agora traz `AVR-02` (não mais `AVR-01`) como ID de chegada. Recálculo: 0 erros. Inspeção de valores: a
fórmula de Condição B (coluna calculada a partir dos incisos I–VI) retornou `"Em apuração"` para AVR-01 e
`"Sim"` para AVR-02, consistente com a fundamentação textual registrada. **Teste de regressão (simulando
novamente o bug original encontrado pela auditoria):** em uma cópia do conjunto demonstrativo, a ID da linha de
Historico foi deliberadamente revertida de `AVR-02` para `AVR-01` (reintroduzindo exatamente a divergência
original), mantendo a Versão = 2. Simulação programática da fórmula de reconciliação confirmou que a nova regra
identifica corretamente a inconsistência (`('AVR-01', '2')` não encontrado nas três abas restantes), enquanto o
mesmo teste sobre os dados consistentes (sem a reversão) não aponta nenhuma inconsistência — confirmando que a
regra funciona nos dois sentidos (não falsos positivos sobre dados corretos, não falsos negativos sobre o bug
original).

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

### 10.1 — Investigação específica de `Painel!B10` (achado A16)

**Correção de referência nesta revisão do relatório:** uma versão anterior deste relatório (e o exercício da
seção 15) referia-se a esta célula como `Painel!B9`. B9 é "Eventos registrados na linha do tempo" — um
indicador diferente, não afetado pelo achado da auditoria. A célula correta, confirmada por leitura direta do
arquivo publicado (`A10` = "Ciclos DAIR distintos identificados (CIC-01 a CIC-30)"), é **`Painel!B10`**. Todas
as referências foram corrigidas nesta revisão, incluindo o exercício de alteração indevida da seção 15.

A auditoria identificou divergência entre o valor salvo e o motor de recálculo independente na fórmula antiga de
"Ciclos DAIR distintos identificados" (`Painel!B10`), que usava `SUMPRODUCT((intervalo<>"")/COUNTIF(intervalo,intervalo&""))`.
Esse padrão é sensível a como cada motor trata células vazias no critério `""` do `COUNTIF` — uma causa plausível
e tecnicamente coerente da divergência relatada, mas não confirmada contra o Microsoft Excel real nesta sessão
(apenas o LibreOffice estava disponível). Em vez de presumir qual motor está "certo", a fórmula foi substituída
por uma contagem por correspondência exata de padrão (`"CIC-"&TEXT(n,"00")`), sem divisão nem dependência de como
cada motor conta strings vazias. **Teste executado:** no conjunto demonstrativo (2 ciclos reais, CIC-01 e
CIC-02), a nova fórmula retornou exatamente `2`, valor correto e verificável manualmente (confirmado por leitura
direta do valor calculado em `Painel!B10` com `data_only=True`, não apenas pela ausência de erro no recálculo).

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
| DAIR_Modelos_Comunicacao_ANPD_Titulares_v0.3.docx | Aprovada (195 parágrafos) | 6 (passou por 5, 4 e 5 em iterações intermediárias — ver histórico abaixo) | Todas as páginas inspecionadas em quatro rodadas, incluindo duas com conteúdo realista preenchido (não apenas células vazias) |

A Parte C (Instrumento 5) passou por três rodadas de correção, cada uma revelando um problema que a anterior não
cobria — documentadas aqui para que o histórico fique rastreável:

**1ª iteração:** a tabela, ao ser ampliada para 10 colunas (A07), foi definida inicialmente com larguras
somando mais que a largura útil da página (11800 DXA contra 9000 DXA), causando quebra de palavra em cada
caractere nos cabeçalhos. Corrigida a soma das larguras para exatamente a largura útil.

**2ª iteração:** mesmo com a largura total corrigida, a inspeção visual página a página revelou que cabeçalhos
longos em colunas estreitas (ex.: "Processo/Protocolo (SEI ou outro)", "Responsável") ainda quebravam caractere
a caractere, por excederem a largura da coluna sem um ponto de quebra natural. Corrigida reduzindo o tamanho da
fonte dos cabeçalhos desta tabela (de 10pt para 7pt) e abreviando os rótulos mais longos.

**3ª iteração (achado durante os exercícios de cenário da seção 15, não durante a inspeção com células vazias):**
ao preencher a tabela com conteúdo realista (exercício 15.1 — "Encarregado", "Formulário eletrônico SEI/ANPD"
etc.), as células de CONTEÚDO (não apenas os cabeçalhos) voltaram a quebrar caractere a caractere — o problema
nunca tinha aparecido porque as inspeções anteriores usaram a tabela vazia. Reduzir a fonte do cabeçalho não
resolve o corpo da tabela, e aumentar a fonte do corpo pioraria a quebra. **Correção estrutural:** a tabela
única de 10 colunas foi substituída por cinco blocos verticais (rótulo | valor), um por registro de
comunicação, com a coluna de valor em largura plena (6400 DXA de 9000). Testado `layout: TableLayoutType.FIXED`
antes dessa mudança — não resolveu a quebra por si só, confirmando que o problema era de espaço disponível, não
de modo de cálculo de layout da tabela. Reconstrução e nova inspeção visual com o mesmo conteúdo do exercício
15.1 confirmaram texto legível, sem quebra de caractere, em nenhuma célula. Efeito colateral aceito: o documento
cresceu de 4 para 6 páginas.

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

**Nota sobre uma revisão anterior deste relatório:** a primeira versão desta seção testou estes três exercícios
apenas como manipulações técnicas da planilha (duplicar um ID, sobrescrever uma fórmula) — o que comprova
controles de integridade de arquivo, mas não testa os três cenários que a tarefa pede, que são tipos de
**incidente envolvendo dados pessoais** (um envio incorreto de comunicação regulatória, uma alteração indevida
de dados pessoais, uma indisponibilidade sem exfiltração confirmada). Os três exercícios foram refeitos como
cenários fictícios completos, abaixo. O exercício técnico de sobrescrita de fórmula foi mantido — é uma
evidência legítima sobre um limite técnico diferente (A11) — mas estava testando a célula errada (ver 15.4).

Todos os exercícios abaixo foram executados em cópias de verificação dos arquivos publicados (não incorporados
ao repositório), para comprovar o comportamento dos instrumentos e dos controles sem alterar os arquivos
entregues nem o conjunto demonstrativo oficial (INC-2026-001).

### 15.1 — Envio incorreto (comunicação enviada à ANPD com erro, corrigida por complementação)

Cenário: a comunicação inicial à ANPD (Instrumento 5, Parte C) é enviada informando **300** titulares afetados;
o erro é percebido depois do envio (o número correto é 30.000) e corrigido por complementação, dentro do prazo
do art. 6º, §3º (20 dias úteis da comunicação efetivamente realizada).

Em uma cópia do Instrumento 5, preenchidos os dois primeiros blocos da Parte C:
- **Comunicação inicial à ANPD** — Conteúdo/Versão: "v1 — comunicação inicial (art. 6º, §2º)"; Data de
  Envio/Juntada: 19/10/2026; Situação: "Enviado (erro identificado após o envio — ver Complementação 1)".
- **Complementação 1 à ANPD** — Conteúdo/Versão: "v2 — correção do quantitativo de titulares (de 300 para
  30.000) e atualização das medidas mitigatórias"; Prazo Aplicável: "Até 20 dias úteis da comunicação inicial
  (16/11/2026)"; Data de Envio/Juntada: 21/10/2026; Situação: "Enviado".

**Resultado:** o kit registra o envio incorreto e a correção como dois registros distintos e rastreáveis — a
comunicação original não é apagada nem sobrescrita, preservando o histórico de que houve um erro e de que foi
corrigido dentro do prazo de complementação. `validate.py` aprovado; inspeção visual confirmou que os dois
registros aparecem completos e legíveis, sem corte de texto (ver achado adicional de legibilidade em 15.5).

### 15.2 — Alteração indevida (incidente de integridade envolvendo dados pessoais, sem exportação)

Cenário: 42 registros cadastrais de titulares (endereço de entrega e dados bancários de reembolso) são alterados
sem autorização no CRM, identificado por auditoria de integridade — sem evidência de exportação ou acesso
externo associado.

Em cópias dos Instrumentos 1, 2 e 3: Cadastro_Incidente preenchido com a descrição do cenário (natureza:
comprometimento de integridade, não confidencialidade); Checklist, atividade "Confirmar a natureza do
incidente", Resultado Observado = "Comprometimento de integridade confirmado: 42 registros cadastrais alterados
sem autorização. Sem evidência de exfiltração associada — confirmação não depende de exportação de dados.";
Avaliação Regulatória (Instrumento 2), inciso III (dados financeiros) marcado "Sim", fundamentado por "a
alteração compromete a confiabilidade dos dados usados para reembolso, podendo gerar prejuízo financeiro direto
ao titular — independentemente de exportação".

**Resultado:** os três instrumentos registraram o cenário sem exigir adaptação estrutural — não há campo que
condicione o registro de um incidente à existência de exportação ou de comprometimento de confidencialidade.
Recálculo dos três arquivos: 0 erros. A Condição B (Instrumento 2) calculou corretamente `"Sim"` a partir do
inciso III, demonstrando que a fundamentação de risco não depende de exfiltração confirmada.

### 15.3 — Indisponibilidade sem exfiltração (ransomware, exfiltração não confirmada)

Cenário: indisponibilidade do CRM por 14 horas por ransomware em servidor de aplicação; backups preservados e
restaurados; investigação de exfiltração em andamento, sem evidência de exportação até o momento. Detecção
reportada por um fornecedor de SOC terceirizado, sem acionamento de ferramenta de monitoramento interna.

Preenchimento análogo ao cenário 15.2: Cadastro_Incidente com a descrição acima (quantidade de titulares:
"Desconhecido", já que a exfiltração não está confirmada); Checklist, Resultado Observado = "Comprometimento de
disponibilidade confirmado (ransomware). Detecção reportada pelo SOC terceirizado, sem acionamento de
ferramenta de monitoramento interna. Exfiltração não confirmada — investigação em andamento; ausência de
evidência de exportação não é tratada como prova de ausência de risco."; Avaliação Regulatória, incisos I/VI
marcados "Em apuração" (não "Não"), com fundamentação de que o volume de titulares impactados pelo período de
indisponibilidade ainda está sendo levantado.

**Resultado:** recálculo dos três arquivos: 0 erros. A Condição B calculou `"Em apuração"` — nem "Sim" nem "Não"
— refletindo corretamente que a avaliação está em curso e não foi encerrada pela ausência de confirmação de
exfiltração (o que seria o erro apontado pelo achado A08). O catálogo do Instrumento 3 aceitou a detecção
reportada por um fornecedor, sem exigir uma ferramenta de monitoramento interna (A15).

### 15.4 — Alteração indevida de uma célula calculada (regressão técnica — correção de referência)

Em uma cópia do modelo em branco do Instrumento 1, a fórmula de `Painel!B10` ("Ciclos DAIR distintos
identificados" — a fórmula corrigida pelo achado A09/A16, não `Painel!B9` como uma versão anterior deste
relatório indicava por engano) foi deliberadamente substituída por um valor fixo incorreto (`999`), simulando
alguém sobrescrevendo uma célula calculada. Resultado: o recálculo automatizado (`recalc.py`) reportou sucesso e
zero erros — uma sobrescrita de fórmula por um valor estático não gera, por si só, um erro de fórmula detectável
por ferramentas automatizadas. Esse é um limite técnico real (consistente com a nota de A11 de que o
sombreamento de células é controle de usabilidade, não de segurança): a proteção contra alteração indevida de
uma célula calculada depende de controles organizacionais (controle de versão, permissões de edição na camada
de distribuição/armazenamento do arquivo), não de um mecanismo interno da planilha. Recomenda-se tratar os
modelos em branco como somente leitura para a maioria dos usuários e distribuir cópias de trabalho versionadas.

### 15.5 — Achado adicional de legibilidade encontrado ao preencher os cenários acima (Instrumento 5, Parte C)

Ao preencher o exercício 15.1 com conteúdo realista (nomes de papéis como "Encarregado", "Formulário eletrônico
SEI/ANPD" etc.), a tabela de 10 colunas da Parte C — mesmo após a correção de largura e de fonte de cabeçalho
relatada anteriormente nesta seção 11 — voltou a quebrar palavras caractere a caractere nas células de conteúdo,
porque nenhuma coluna de valor tinha largura suficiente para uma palavra comum nesse tamanho de fonte. Isso só
foi percebido ao testar com dados representativos, não com células vazias. **Correção:** a tabela única de 10
colunas foi substituída por cinco blocos verticais (rótulo | valor), um por registro (comunicação inicial,
complementação 1, complementação 2, comunicação aos titulares, declaração art. 9º §4º), cada um com a coluna de
valor em largura plena (6400 DXA). Nova inspeção visual com o mesmo conteúdo do exercício 15.1 confirmou texto
legível, sem quebra de caractere, em todas as linhas. O documento passou de 4 para 6 páginas como consequência
do layout vertical — uma troca aceita em favor da legibilidade.

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
