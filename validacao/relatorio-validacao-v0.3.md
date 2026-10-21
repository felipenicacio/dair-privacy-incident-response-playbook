# Relatório de validação técnica: v0.3 (correção pós-auditoria)

**Data desta validação:** 20/10/2026 · **Responsável pela execução:** sessão assistida por IA, sob supervisão de Felipe Nicácio.
**Base de partida:** commit `f81732b3e674535ee66393df1efb2dc7d0d0af29` (v0.3 original, auditada). Este relatório substitui
integralmente o relatório anterior e documenta apenas os testes efetivamente executados nesta correção: nenhum
teste aqui descrito foi presumido ou declarado sem execução correspondente. Ver a [matriz de achados A01 a A18](matriz-achados-A01-A18.md)
para o rastreamento achado a achado.

## 1. Escopo testado

- 3 planilhas Excel (modelos em branco): `DAIR_Registro_e_Gestao_de_Incidentes_v0.3.xlsx`, `DAIR_Avaliacao_Riscos_Titulares_ANPD_v0.3.xlsx`, `DAIR_Checklist_Operacional_v0.3.xlsx`.
- 3 planilhas Excel (conjunto demonstrativo fictício): as mesmas três, com sufixo `_EXEMPLO-INC-2026-001`.
- 2 documentos Word: `DAIR_Relatorio_Resposta_Incidente_v0.3.docx`, `DAIR_Kit_Preparacao_Comunicacoes_ANPD_Titulares_v0.3.docx`.
- Todos os Markdown do repositório (playbook, cenário, governança, templates, guias, referências).

## 2. Motor e tipos de teste utilizados (distinção exigida pela auditoria)

| Tipo de teste | Ferramenta | O que comprova | O que NÃO comprova |
|---|---|---|---|
| Recálculo completo | `recalc.py` (LibreOffice headless) | Todas as fórmulas avaliam sem erro de fórmula (`#REF!`, `#DIV/0!` etc.) no motor LibreOffice | Compatibilidade com Microsoft Excel real (não testado nesta sessão: ver Limitações) nem correção lógica do resultado |
| Validação estrutural | `validate.py` (schema XML OOXML) | O `.docx` é um arquivo bem-formado, abre sem erro de estrutura | Qualidade visual, paginação ou conteúdo |
| Inspeção visual | Conversão para PDF + leitura das páginas renderizadas | Layout, cortes de tabela, cabeçalho/rodapé, paginação, legibilidade | Comportamento interativo no Word/Excel real |
| Inspeção de valores calculados | Leitura programática do arquivo recalculado (`data_only=True`) | O **resultado lógico** de uma fórmula (não apenas ausência de erro) | N/A |
| Revisão de conteúdo jurídico | Leitura de fonte primária (RCIS, LGPD) e reconciliação manual do texto | Que o texto do instrumento corresponde ao artigo citado | Validade jurídica para um caso concreto: exige Jurídico |

## 3. Correção A02: Identificadores manuais e estáveis

**Mudança:** todas as colunas de ID (EVT-, DEC-, EVD-, ACA- no Instrumento 1; AVR- no Instrumento 2; CIC-, CHK- no
Instrumento 3) deixaram de ser fórmulas baseadas em `ROW()` e passaram a ser campos de entrada manual, com:
- validação de dados customizada que bloqueia (`showErrorMessage=True`, `errorStyle="stop"`) a gravação de um ID
  duplicado dentro da mesma coluna;
- formatação condicional que sinaliza em vermelho qualquer duplicidade já existente.

**Teste executado:** inserida uma linha de evento isolada no Instrumento 1 (EVT-021, linha 24, fora do bloco
inicial de preenchimento) e recalculado o arquivo: 0 erros de fórmula, validação e formatação presentes na
linha. O Painel contou corretamente 13 eventos (12 do cenário + 1 de teste), confirmando que a contagem não
depende da posição da linha nem de uma fórmula de ID.

## 4. Correção A03: Reconciliação de versões da avaliação regulatória

**Problema identificado pela auditoria:** no exemplo anterior, a versão 2 da avaliação aparecia como `AVR-02` nas
abas Caracterizacao/Avaliacao_Regulatoria/Decisao_Comunicacao, mas como `AVR-01` na aba Historico: porque o ID
era calculado pela posição da linha em cada aba, de forma independente.

**Correção (e uma correção dentro da correção):** o ID (`AVR-NN`) agora é digitado manualmente e é ele: não a
linha: que liga o registro entre as quatro abas. A primeira versão desta correção usou uma formatação
condicional que comparava a coluna "Versão" com os dois últimos dígitos da própria ID (ex.: ID `AVR-02` deveria
ter Versão `2`): um mecanismo frágil e artificial: ele força o número da versão a estar embutido no texto da
ID, o que não prova reconciliação alguma entre abas (duas abas podem ter a mesma ID com dígitos "corretos" e,
ainda assim, dados de versões diferentes uma da outra) e quebraria em casos legítimos em que a numeração da ID
não seguisse estritamente a sequência de versões. **Corrigido nesta revisão:** a formatação condicional agora
testa, por fórmula, se a MESMA combinação ID+Versão desta linha também existe nas outras abas relacionadas
(`COUNTIFS` cruzado entre abas, não extração de dígitos da ID): por exemplo, em Caracterizacao:
`COUNTIFS(Avaliacao_Regulatoria!$A$4:$A$53,$A4,Avaliacao_Regulatoria!$B$4:$B$53,$B4)=0` sinaliza em vermelho se
aquele par ID+Versão não existir em Avaliacao_Regulatoria. Esse é o teste de reconciliação que o achado A03
realmente pede ("relacionar de forma inequívoca incidente/avaliação/versão... sem depender de alinhamento pela
mesma linha").

**Teste executado:** reconstruído o conjunto demonstrativo com `AVR-01` (v1, informações insuficientes) e
`AVR-02` (v2, condições potencialmente atendidas) consistentes nas quatro abas, incluindo a linha de Historico
que agora traz `AVR-02` (não mais `AVR-01`) como ID de chegada. Recálculo: 0 erros. Inspeção de valores: a
fórmula de Condição B (coluna calculada a partir dos incisos I a VI) retornou `"Em apuração"` para AVR-01 e
`"Sim"` para AVR-02, consistente com a fundamentação textual registrada. **Teste de regressão (simulando
novamente o bug original encontrado pela auditoria):** em uma cópia do conjunto demonstrativo, a ID da linha de
Historico foi deliberadamente revertida de `AVR-02` para `AVR-01` (reintroduzindo exatamente a divergência
original), mantendo a Versão = 2. Simulação programática da fórmula de reconciliação confirmou que a nova regra
identifica corretamente a inconsistência (`('AVR-01', '2')` não encontrado nas três abas restantes), enquanto o
mesmo teste sobre os dados consistentes (sem a reversão) não aponta nenhuma inconsistência: confirmando que a
regra funciona nos dois sentidos (não falsos positivos sobre dados corretos, não falsos negativos sobre o bug
original).

## 5. Correção A04: Reconciliação de efetividade, evidência e encerramento

**Problema identificado pela auditoria:** o exemplo anterior registrava "token revogado" e "validação de
contenção" como atividades concluídas e efetivas apoiadas apenas no log de execução da revogação: sem um
registro de teste técnico posterior que comprovasse que o acesso indevido deixou de funcionar.

**Correção:** acrescentados dois eventos fictícios explícitos de teste técnico no Instrumento 1 (`EVT-004`: teste da 1ª contenção, revelando falha; `EVT-009`: teste da 2ª contenção, confirmando efetividade), com
referência cruzada no Instrumento 3 (atividades de Contenção, coluna "Efetividade Comprovada" ajustada de
"Testada: parcialmente efetiva" para "Testada: inefetiva" na 1ª iteração, e "Testada: efetiva" na 2ª,
cada uma referenciando a evidência e o teste correspondentes). A redução emergencial de privilégios (bloqueio de
conta) foi distinguida textualmente da revogação do token (melhoria estrutural完成 apenas no Ciclo 2). A janela de
observação pós-recuperação (atividade "Acompanhar indicadores pós-recuperação") permanece "Em andamento" no
encerramento do exercício, não "Concluída": refletindo que a observação continuada é uma situação aberta, não um
evento pontual.

**Teste executado:** recálculo dos três arquivos Excel do conjunto demonstrativo após a reconciliação: 0 erros.
Revisão manual cruzada: cada afirmação de "Testada: efetiva/inefetiva" no Instrumento 3 agora tem uma
evidência vinculada (`EVD-003` ou `EVD-005`) que, na Gestão de Evidências do Instrumento 1, descreve o teste
específico (método, data/hora e resultado), não apenas o log de execução administrativa.

## 6. Correção A05: Referências ao art. 5º do RCIS

**Problema identificado pela auditoria:** diversos pontos do repositório (Instrumento 2: cabeçalho e texto de
instruções; Instrumento 4, seção 13; `templates/01-avaliacao-risco-titulares.md`; `playbook/01-procedimento-operacional.md`)
citavam "art. 5º, II" como se fosse a única hipótese da Condição B, quando o inciso II trata especificamente de
dados de crianças/adolescentes/idosos: dados financeiros estão no inciso III e larga escala no inciso VI.

**Correção:** todas as ocorrências foram revisadas e corrigidas para referenciar "art. 5º, caput e incisos I a
VI", com a lista completa (I. sensíveis; II. crianças/adolescentes/idosos; III. financeiros; IV. autenticação;
V. sigilo legal/judicial/profissional; VI. larga escala) nos pontos em que o texto já detalhava os incisos. O
Instrumento 2 passou a ter seis colunas estruturadas (uma por inciso), e a condição B é calculada por fórmula a
partir delas: corrigindo também a lógica cumulativa (A11).

**Teste executado:** `grep` em todos os arquivos Markdown e nos geradores dos instrumentos confirmando ausência
de qualquer referência remanescente a "art. 5º, II" como hipótese única.

## 7. Correção A06: Controle de prazos

**Mudança no Instrumento 2 (Decisao_Comunicacao):** a antiga coluna única "Prazo Regulatório (apoio)" foi
desdobrada em: (1) "Data Calculada de Apoio" (fórmula `WORKDAY`, 3 ou 6 dias úteis conforme elegibilidade de
pequeno porte, sem feriados); (2) "Prazo Aplicável Validado" (campo manual); (3) "Fundamento da Validação do
Prazo"; (4) "Calendário de Referência Utilizado"; (5) "Responsável pela Validação do Prazo". A elegibilidade de
pequeno porte é um campo manual próprio ("Não avaliado" por padrão), nunca presumida. A complementação (até 20
dias úteis) passou a ser calculada a partir da "Data da Comunicação à ANPD Efetivamente Realizada": se essa
data não estiver preenchida, o campo exibe explicitamente "Sem data de comunicação efetiva registrada", em vez
de calcular a partir de uma data planejada.

**Teste executado:** no conjunto demonstrativo, a "Data Calculada de Apoio" para AVR-01 e AVR-02 (marco
19/10/2026 09h25, pequeno porte "Não elegível") recalculou corretamente para 22/10/2026: consistente com o
texto já publicado no cenário (v0.2). A "Necessidade de Complementação" retornou corretamente a mensagem
explícita de ausência de data efetiva, já que, no exercício, nenhuma comunicação foi de fato enviada.

## 8. Correção A07: Acompanhamento das comunicações

**Mudança no Instrumento 5 (Parte C):** a tabela única "ANPD x Titulares" foi substituída por um controle que
acompanha separadamente a comunicação inicial à ANPD, as complementações, a comunicação aos titulares e a
declaração do art. 9º, §4º: cada uma com processo/protocolo, conteúdo/versão enviada, responsável, aprovação,
prazo aplicável, data de envio, meio utilizado, evidência comprobatória e situação. Acrescentada a seção C.1
(classificação de acesso dos documentos anexados e conferência de representação) e C.2 (divulgação pública
alternativa: justificativa, canais, datas e evidência de manutenção pelo período mínimo de 3 meses). O texto
agora destaca explicitamente que o envio à ANPD não implica que os titulares também foram comunicados.

**Correção adicional nesta rodada (adequação do Instrumento 5 à continuidade institucional: Política/Plano
v0.3):** o Instrumento 5 foi retitulado para "Kit de Preparação e Controle das Comunicações ANPD/Titulares" e
renomeado para `DAIR_Kit_Preparacao_Comunicacoes_ANPD_Titulares_v0.3.docx`, para deixar claro que o documento
organiza informações e aprovações internas ANTES do preenchimento do formulário oficial do SEI!ANPD (que já
oferece modalidades preliminar e completa no Documento Principal), e que não constitui protocolo nem substitui
esse formulário. O limite de "até duas complementações" foi removido: era artificial: a ANPD não fixa um
número máximo de complementações, apenas o prazo de 20 dias úteis do art. 6º, §3º. A Parte C agora traz um único
bloco "Complementação à ANPD: registro nº 1", com instrução explícita para duplicá-lo sem limite, e um novo
bloco "Outras providências ou requisições relacionadas ao processo", também duplicável, para registrar pedidos
de informação adicional (art. 8º do RCIS) ou determinações da ANPD no mesmo processo. A classificação de acesso
e a conferência de representação (antiga seção C.1) foram deslocadas para a Parte A (seção A.3), por serem parte
da preparação de documentos a anexar, e não da execução; a preparação da divulgação pública alternativa (antiga
seção C.2, parte justificativa/canais/prazo) foi deslocada para a Parte B (seção B.3), mantendo-se em C apenas o
controle da sua execução (data de início, cumprimento do período mínimo, evidência de manutenção). Validado por
`validate.py` (OOXML aprovado) e por conversão a PDF com inspeção visual de todas as páginas, incluindo uma
rodada com todas as tabelas preenchidas com conteúdo realista de teste para confirmar ausência de quebra de
texto caractere a caractere nos novos blocos (A.3 e nos registros adicionais de C): ver seção 11.

**Teste executado:** conversão para PDF e inspeção visual de todas as páginas (ver seção 10): tabela renderiza
sem cortes após ajuste de largura de colunas (ver achado de teste registrado abaixo).

## 9. Correção A08: Fundamentação e estados decisórios

**Mudança no Instrumento 2 (Avaliacao_Regulatoria):** os seis incisos do art. 5º passaram a ser colunas
individuais (Sim/Não/Em apuração), com fundamentação própria por inciso assinalado: corrigindo o tratamento de
30 mil titulares como limiar automático de larga escala (a fundamentação agora exige descrever volume,
abrangência, duração e proporção da base, não apenas o número). Acrescentada a coluna "Estado da Análise"
(Parecer em elaboração / Análise concluída aguardando aprovação / Análise aprovada / Decisão de comunicar
confirmada / Autorização de envio emitida), distinta do campo "Resultado da Avaliação". O texto contraditório
"Não: aprofundar Scope" (que combinava uma negativa com uma ação afirmativa) foi substituído por "Sim: aprofundar Scope e confirmar perfil etário dos titulares antes de concluir o inciso II".

**Teste executado:** inspeção de valores do conjunto demonstrativo: AVR-01 retorna "Em apuração" (não "Não") no
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
indicadores calculados: por desenho, não por omissão.

Todos os seis arquivos abrem sem mensagem de corrupção no LibreOffice (motor de verificação disponível nesta
sessão). **Não foi possível testar a abertura no Microsoft Excel real**: ver Limitações.

### 10.1: Investigação específica de `Painel!B10` (achado A16)

**Correção de referência nesta revisão do relatório:** uma versão anterior deste relatório (e o exercício da
seção 15) referia-se a esta célula como `Painel!B9`. B9 é "Eventos registrados na linha do tempo": um
indicador diferente, não afetado pelo achado da auditoria. A célula correta, confirmada por leitura direta do
arquivo publicado (`A10` = "Ciclos DAIR distintos identificados (CIC-01 a CIC-30)"), é **`Painel!B10`**. Todas
as referências foram corrigidas nesta revisão, incluindo o exercício de alteração indevida da seção 15.

A auditoria identificou divergência entre o valor salvo e o motor de recálculo independente na fórmula antiga de
"Ciclos DAIR distintos identificados" (`Painel!B10`), que usava `SUMPRODUCT((intervalo<>"")/COUNTIF(intervalo,intervalo&""))`.
Esse padrão é sensível a como cada motor trata células vazias no critério `""` do `COUNTIF`: uma causa plausível
e tecnicamente coerente da divergência relatada, mas não confirmada contra o Microsoft Excel real nesta sessão
(apenas o LibreOffice estava disponível). Em vez de presumir qual motor está "certo", a fórmula foi substituída
por uma contagem por correspondência exata de padrão (`"CIC-"&TEXT(n,"00")`), sem divisão nem dependência de como
cada motor conta strings vazias. **Teste executado:** no conjunto demonstrativo (2 ciclos reais, CIC-01 e
CIC-02), a nova fórmula retornou exatamente `2`, valor correto e verificável manualmente (confirmado por leitura
direta do valor calculado em `Painel!B10` com `data_only=True`, não apenas pela ausência de erro no recálculo).

### 10.2: Testes de expansão (achado A10)

| Teste exigido pela auditoria | Resultado |
|---|---|
| 21º evento (Linha_do_Tempo) | Registrado na linha 24 (fora do bloco inicial); recálculo sem erro; Painel contou 13 eventos (12 do cenário + 1 de teste) |
| 17ª revisão de avaliação (Instrumento 2, Historico) | Testado em cópia de verificação (`AVR-17`, linha 20): recálculo sem erro, fórmulas e validação presentes: não incorporado ao conjunto demonstrativo publicado, por não fazer parte do cenário fictício |
| 13º ciclo (Controle_Ciclos) | Registrado (`CIC-13`, linha 16 do conjunto demonstrativo) como linha de teste isolada; recálculo sem erro |
| 31ª atividade (Checklist) | Registrada (`CHK-031`, linha 34) como linha de teste isolada; recálculo sem erro |
| Registros além da linha 200 | As quatro abas de registro do Instrumento 1 têm fórmulas/validação pré-formatadas até a linha 503 (testado: linha 503 tem validação de waypoint e preenchimento ativos) |

As faixas são fixas e generosas (não ilimitadas): ver `guias/04-utilizar-instrumentos.md` para como estender
manualmente além do limite pré-formatado.

## 11. Testes em arquivos Word

**Ferramentas:** validação de esquema XML (`validate.py`), conversão para PDF e inspeção visual de todas as
páginas renderizadas.

| Arquivo | Validação de esquema | Páginas (PDF) | Inspeção visual |
|---|---|---|---|
| DAIR_Relatorio_Resposta_Incidente_v0.3.docx | Aprovada (329 parágrafos) | 8 | Capa, sumário e seções 1 a 20 inspecionadas; seção 13 corrigida (inciso correto) |
| DAIR_Kit_Preparacao_Comunicacoes_ANPD_Titulares_v0.3.docx | Aprovada (229 parágrafos, após retitulação e reescrita das Partes A/B/C nesta rodada) | 7 vazio / 10 com todas as tabelas preenchidas com conteúdo realista de teste | Todas as páginas inspecionadas em cinco rodadas ao todo (quatro da correção A07 original, mais uma desta rodada com o documento retitulado e as seções A.2/A.3/B.3 novas e os registros de C sem limite de complementação, novamente com conteúdo realista preenchido): sem quebra de texto caractere a caractere em nenhum bloco |

A Parte C (Instrumento 5) passou por três rodadas de correção, cada uma revelando um problema que a anterior não
cobria: documentadas aqui para que o histórico fique rastreável:

**1ª iteração:** a tabela, ao ser ampliada para 10 colunas (A07), foi definida inicialmente com larguras
somando mais que a largura útil da página (11800 DXA contra 9000 DXA), causando quebra de palavra em cada
caractere nos cabeçalhos. Corrigida a soma das larguras para exatamente a largura útil.

**2ª iteração:** mesmo com a largura total corrigida, a inspeção visual página a página revelou que cabeçalhos
longos em colunas estreitas (ex.: "Processo/Protocolo (SEI ou outro)", "Responsável") ainda quebravam caractere
a caractere, por excederem a largura da coluna sem um ponto de quebra natural. Corrigida reduzindo o tamanho da
fonte dos cabeçalhos desta tabela (de 10pt para 7pt) e abreviando os rótulos mais longos.

**3ª iteração (achado durante os exercícios de cenário da seção 15, não durante a inspeção com células vazias):**
ao preencher a tabela com conteúdo realista (exercício 15.1: "Encarregado", "Formulário eletrônico SEI/ANPD"
etc.), as células de CONTEÚDO (não apenas os cabeçalhos) voltaram a quebrar caractere a caractere: o problema
nunca tinha aparecido porque as inspeções anteriores usaram a tabela vazia. Reduzir a fonte do cabeçalho não
resolve o corpo da tabela, e aumentar a fonte do corpo pioraria a quebra. **Correção estrutural:** a tabela
única de 10 colunas foi substituída por cinco blocos verticais (rótulo | valor), um por registro de
comunicação, com a coluna de valor em largura plena (6400 DXA de 9000). Testado `layout: TableLayoutType.FIXED`
antes dessa mudança: não resolveu a quebra por si só, confirmando que o problema era de espaço disponível, não
de modo de cálculo de layout da tabela. Reconstrução e nova inspeção visual com o mesmo conteúdo do exercício
15.1 confirmaram texto legível, sem quebra de caractere, em nenhuma célula. Efeito colateral aceito: o documento
cresceu de 4 para 6 páginas.

**Dados fictícios misturados a modelos reutilizáveis:** confirmado que não há: os Instrumentos 4 e 5 contêm
apenas rótulos de campo e instruções de preenchimento em itálico, sem nenhum dado do incidente fictício
INC-2026-001 (reconferido após as edições desta correção).

## 12. Consistência entre instrumentos e Markdown

- Nomenclatura de identificadores (`INC-`, `EVT-`, `EVD-`, `DEC-`, `ACA-`, `AVR-`, `CIC-`, `CHK-`) revisada e
  consistente nos três instrumentos Excel, no conjunto demonstrativo e no `guias/04-utilizar-instrumentos.md`.
- Referências cruzadas de evidência (`EVD-NNN`) conferidas em todos os arquivos do conjunto demonstrativo: mantida a numeração EVD-001 a EVD-006 já usada na v0.2/v0.3 original, agora reconciliada com os novos eventos
  de teste técnico (EVT-004, EVT-009).
- Referências ao art. 5º do RCIS (A05) revisadas em todos os Markdown e nos geradores dos instrumentos.
- `templates/02-registro-incidente.md` atualizado para refletir a falha de contenção testada e a revogação
  confirmada por teste técnico (A04), e `templates/01-avaliacao-risco-titulares.md` atualizado para fundamentar
  separadamente os incisos III e VI (A05/A08).

## 13. Fontes externas consultadas nesta correção

Ver `referencias/fontes-e-atribuicoes.md` (reescrito nesta correção: A17) para URLs específicas, artigos
efetivamente utilizados, data de consulta (20/10/2026) e a distinção entre fontes efetivamente utilizadas e
referências complementares.

## 14. Limitações conhecidas

- Os testes de abertura e recálculo foram executados no LibreOffice (motor de verificação disponível nesta
  sessão), não no Microsoft Excel/Word real. A estrutura dos arquivos (`.xlsx`/`.docx` nativos, sem recursos
  proprietários) reduz o risco de incompatibilidade, mas uma verificação adicional no Microsoft Office é
  recomendada antes do uso em produção: nenhuma afirmação de compatibilidade perfeita é feita aqui.
- Os cálculos de prazo regulatório (Instrumento 2) usam `WORKDAY` sem lista de feriados nacionais/municipais: o campo que efetivamente vale é o "Prazo Aplicável Validado" (manual, com fundamento e responsável), não a
  "Data Calculada de Apoio".
- Não há sincronização automática entre os cinco instrumentos; a integração depende de disciplina manual no uso
  dos identificadores compartilhados, agora manuais e com detecção de duplicidade (A02).
- As faixas de linhas pré-formatadas (A10) são fixas, embora generosas (até ~500 por aba no Instrumento 1; até
  53 no Instrumento 2; até 153 atividades e 43 ciclos no Instrumento 3): não há capacidade ilimitada.
- O exercício de "envio incorreto/alteração indevida/indisponibilidade sem exfiltração" (seção 5 do escopo da
  auditoria) foi conduzido como revisão de conteúdo (A15: catálogo do Instrumento 3 agora aceita incidentes de
  integridade/disponibilidade/autenticidade sem atacante ou exportação, e detecção por pessoas/fornecedores sem
  ferramenta de monitoramento): não foi conduzido como simulação dinâmica adicional de tabletop nesta correção.
- O conjunto demonstrativo cobre os três instrumentos em Excel; os Instrumentos 4 e 5 permanecem, por decisão de
  projeto, modelos em branco sem dados do cenário fictício (ver `guias/04-utilizar-instrumentos.md`).

## 15. Exercícios adicionais: envio incorreto, alteração indevida e indisponibilidade sem exfiltração

**Nota sobre uma revisão anterior deste relatório:** a primeira versão desta seção testou estes três exercícios
apenas como manipulações técnicas da planilha (duplicar um ID, sobrescrever uma fórmula): o que comprova
controles de integridade de arquivo, mas não testa os três cenários que a tarefa pede, que são tipos de
**incidente envolvendo dados pessoais** (um envio incorreto de comunicação regulatória, uma alteração indevida
de dados pessoais, uma indisponibilidade sem exfiltração confirmada). Os três exercícios foram refeitos como
cenários fictícios completos, abaixo. O exercício técnico de sobrescrita de fórmula foi mantido: é uma
evidência legítima sobre um limite técnico diferente (A11): mas estava testando a célula errada (ver 15.4).

Todos os exercícios abaixo foram executados em cópias de verificação dos arquivos publicados (não incorporados
ao repositório), para comprovar o comportamento dos instrumentos e dos controles sem alterar os arquivos
entregues nem o conjunto demonstrativo oficial (INC-2026-001).

### 15.1: Envio incorreto (comunicação enviada à ANPD com erro, corrigida por complementação)

Cenário: a comunicação inicial à ANPD (Instrumento 5, Parte C) é enviada informando **300** titulares afetados;
o erro é percebido depois do envio (o número correto é 30.000) e corrigido por complementação, dentro do prazo
do art. 6º, §3º (20 dias úteis da comunicação efetivamente realizada).

Em uma cópia do Instrumento 5, preenchidos os dois primeiros blocos da Parte C:
- **Comunicação inicial à ANPD**: Conteúdo/Versão: "v1: comunicação inicial (art. 6º, §2º)"; Data de
  Envio/Juntada: 19/10/2026; Situação: "Enviado (erro identificado após o envio: ver Complementação 1)".
- **Complementação 1 à ANPD**: Conteúdo/Versão: "v2: correção do quantitativo de titulares (de 300 para
  30.000) e atualização das medidas mitigatórias"; Prazo Aplicável: "Até 20 dias úteis da comunicação inicial
  (16/11/2026)"; Data de Envio/Juntada: 21/10/2026; Situação: "Enviado".

**Resultado:** o kit registra o envio incorreto e a correção como dois registros distintos e rastreáveis: a
comunicação original não é apagada nem sobrescrita, preservando o histórico de que houve um erro e de que foi
corrigido dentro do prazo de complementação. `validate.py` aprovado; inspeção visual confirmou que os dois
registros aparecem completos e legíveis, sem corte de texto (ver achado adicional de legibilidade em 15.5).

### 15.2: Alteração indevida (incidente de integridade envolvendo dados pessoais, sem exportação)

Cenário: 42 registros cadastrais de titulares (endereço de entrega e dados bancários de reembolso) são alterados
sem autorização no CRM, identificado por auditoria de integridade: sem evidência de exportação ou acesso
externo associado.

Em cópias dos Instrumentos 1, 2 e 3: Cadastro_Incidente preenchido com a descrição do cenário (natureza:
comprometimento de integridade, não confidencialidade); Checklist, atividade "Confirmar a natureza do
incidente", Resultado Observado = "Comprometimento de integridade confirmado: 42 registros cadastrais alterados
sem autorização. Sem evidência de exfiltração associada: confirmação não depende de exportação de dados.";
Avaliação Regulatória (Instrumento 2), inciso III (dados financeiros) marcado "Sim", fundamentado por "a
alteração compromete a confiabilidade dos dados usados para reembolso, podendo gerar prejuízo financeiro direto
ao titular: independentemente de exportação".

**Resultado:** os três instrumentos registraram o cenário sem exigir adaptação estrutural: não há campo que
condicione o registro de um incidente à existência de exportação ou de comprometimento de confidencialidade.
Recálculo dos três arquivos: 0 erros. A Condição B (Instrumento 2) calculou corretamente `"Sim"` a partir do
inciso III, demonstrando que a fundamentação de risco não depende de exfiltração confirmada.

### 15.3: Indisponibilidade sem exfiltração (ransomware, exfiltração não confirmada)

Cenário: indisponibilidade do CRM por 14 horas por ransomware em servidor de aplicação; backups preservados e
restaurados; investigação de exfiltração em andamento, sem evidência de exportação até o momento. Detecção
reportada por um fornecedor de SOC terceirizado, sem acionamento de ferramenta de monitoramento interna.

Preenchimento análogo ao cenário 15.2: Cadastro_Incidente com a descrição acima (quantidade de titulares:
"Desconhecido", já que a exfiltração não está confirmada); Checklist, Resultado Observado = "Comprometimento de
disponibilidade confirmado (ransomware). Detecção reportada pelo SOC terceirizado, sem acionamento de
ferramenta de monitoramento interna. Exfiltração não confirmada: investigação em andamento; ausência de
evidência de exportação não é tratada como prova de ausência de risco."; Avaliação Regulatória, incisos I/VI
marcados "Em apuração" (não "Não"), com fundamentação de que o volume de titulares impactados pelo período de
indisponibilidade ainda está sendo levantado.

**Resultado:** recálculo dos três arquivos: 0 erros. A Condição B calculou `"Em apuração"`: nem "Sim" nem "Não": refletindo corretamente que a avaliação está em curso e não foi encerrada pela ausência de confirmação de
exfiltração (o que seria o erro apontado pelo achado A08). O catálogo do Instrumento 3 aceitou a detecção
reportada por um fornecedor, sem exigir uma ferramenta de monitoramento interna (A15).

### 15.4: Alteração indevida de uma célula calculada (regressão técnica: correção de referência)

Em uma cópia do modelo em branco do Instrumento 1, a fórmula de `Painel!B10` ("Ciclos DAIR distintos
identificados": a fórmula corrigida pelo achado A09/A16, não `Painel!B9` como uma versão anterior deste
relatório indicava por engano) foi deliberadamente substituída por um valor fixo incorreto (`999`), simulando
alguém sobrescrevendo uma célula calculada. Resultado: o recálculo automatizado (`recalc.py`) reportou sucesso e
zero erros: uma sobrescrita de fórmula por um valor estático não gera, por si só, um erro de fórmula detectável
por ferramentas automatizadas. Esse é um limite técnico real (consistente com a nota de A11 de que o
sombreamento de células é controle de usabilidade, não de segurança): a proteção contra alteração indevida de
uma célula calculada depende de controles organizacionais (controle de versão, permissões de edição na camada
de distribuição/armazenamento do arquivo), não de um mecanismo interno da planilha. Recomenda-se tratar os
modelos em branco como somente leitura para a maioria dos usuários e distribuir cópias de trabalho versionadas.

### 15.5: Achado adicional de legibilidade encontrado ao preencher os cenários acima (Instrumento 5, Parte C)

Ao preencher o exercício 15.1 com conteúdo realista (nomes de papéis como "Encarregado", "Formulário eletrônico
SEI/ANPD" etc.), a tabela de 10 colunas da Parte C: mesmo após a correção de largura e de fonte de cabeçalho
relatada anteriormente nesta seção 11: voltou a quebrar palavras caractere a caractere nas células de conteúdo,
porque nenhuma coluna de valor tinha largura suficiente para uma palavra comum nesse tamanho de fonte. Isso só
foi percebido ao testar com dados representativos, não com células vazias. **Correção:** a tabela única de 10
colunas foi substituída por cinco blocos verticais (rótulo | valor), um por registro (comunicação inicial,
complementação 1, complementação 2, comunicação aos titulares, declaração art. 9º §4º), cada um com a coluna de
valor em largura plena (6400 DXA). Nova inspeção visual com o mesmo conteúdo do exercício 15.1 confirmou texto
legível, sem quebra de caractere, em todas as linhas. O documento passou de 4 para 6 páginas como consequência
do layout vertical: uma troca aceita em favor da legibilidade.

## 16. Pontos que exigem revisão humana

- Validação jurídica da contagem de prazos (feriados, regra de pequeno porte, prazo setorial específico) e da
  aplicabilidade de cada inciso do art. 5º do RCIS ao caso concreto: o instrumento aponta a necessidade, não
  decide por fórmula.
- Confirmação por um usuário de Microsoft Excel/Word real antes de uso em produção, dado que a verificação
  automatizada usou o LibreOffice.
- Revisão jurídica da redação da Parte A.2 do Instrumento 5 (pequeno porte, representação, sigilo) frente ao
  caso concreto de cada organização.
- Decisão sobre se as faixas de expansão (A10) atendem ao volume esperado de incidentes da organização, ou se
  devem ser ampliadas antes do uso.
- Aprovação formal da Política e do Plano de Resposta a Incidentes pela autoridade competente da organização,
  com preenchimento real dos campos de configuração (seção 0 de ambos): nenhum dos dois produz efeito sem essa
  aprovação.
- Exercício tabletop efetivo do Plano (Jornada 3), com a lista real de contatos, canais e critérios de
  severidade da organização: o walkthrough textual desta rodada (seção 17, item 7) não substitui essa prática.
- Definição, pela organização, dos limiares de severidade técnica do Plano (seção 2.1) e dos limites de valor/
  recurso que exigem aprovação da Direção (Plano, seção 6, e Política, seção 6).

## 17. Validação desta rodada: Instrumento 5 ajustado, Política, Plano e integração

Ferramentas e versões: Node.js com o pacote `docx` (geração), `/mnt/skills/public/docx/scripts/office/validate.py`
(validação de schema OOXML), LibreOffice (headless, `--convert-to pdf`) com `pdftoppm`/`pdfinfo` para conversão e
inspeção visual, `/mnt/skills/public/xlsx/scripts/recalc.py` (recálculo de fórmulas via LibreOffice headless),
`python-docx` (preenchimento de células para os testes de legibilidade com conteúdo realista) e verificação de
links por varredura de todos os arquivos `.md` do repositório.

1. **Referências legais e metodológicas confrontadas com fontes primárias.** O procedimento vigente de
   comunicado de incidente de segurança (CIS) foi consultado diretamente em
   `gov.br/anpd/.../comunicado-de-incidente-de-seguranca-cis` nesta rodada (ver seção 13) e usado para
   fundamentar as seções A.2/A.3 do Instrumento 5 e a seção 4 do Plano: nenhuma informação sobre o procedimento
   do SEI foi inferida de memória. Os artigos do RCIS já verificados nas rodadas anteriores (seção 13 e
   `referencias/fontes-e-atribuicoes.md`) não foram alterados; a Política e o Plano reutilizam as mesmas
   referências, sem introduzir novo conteúdo normativo não verificado.
2. **Compatibilidade de responsabilidades e critérios entre Política, Plano, Playbook e Instrumentos.** Os
   papéis nomeados na Política (seção 5) e no Plano (seções 1.3 e 2.2) reutilizam os mesmos nomes de papel já
   empregados em `governanca/01-papeis-e-escalonamento.md` (Líder IR/Líder de Resposta, CISO/Segurança, SOC, TI,
   IAM, Encarregado, Jurídico, Controlador) e nos Instrumentos 1 e 2 (Responsável, Aprovação, Encarregado,
   representante autorizado do controlador): não foram criados nomes de papel paralelos. O waypoint do Plano
   (seção 3) usa os mesmos nomes de fase do Playbook (Prepare, Detect, Verify and Triage, Scope/Contain/
   Eradicate/Recover, Debrief) e dos Instrumentos 1/3.
3. **Ausência de dados do cenário fictício ou de aprovação simulada nos modelos limpos.** Busca por
   `INC-2026-001`, identificadores do cenário (`EVT-`, `AVR-0`, datas e números do caso fictício) e por valores
   de aprovação preenchidos nos arquivos `governanca/politica-resposta-incidentes.md`,
   `governanca/plano-resposta-incidentes.md` e nos dois `.docx` correspondentes: nenhuma ocorrência. Os campos
   de aprovação, owner, versão e vigência permanecem como `[preencher]` em ambos os documentos.
4. **Links e referências testados após as renomeações.** Varredura automatizada de todos os links relativos em
   arquivos `.md` do repositório (regex sobre `](...)`, resolvendo o caminho contra o diretório de cada arquivo):
   0 links quebrados. Busca adicional por ocorrências remanescentes do nome antigo do Instrumento 5
   (`DAIR_Modelos_Comunicacao_ANPD_Titulares_v0.3`): nenhuma.
5. **Renderização e inspeção de todas as páginas dos DOCX novos/alterados.** `validate.py` aprovado para os
   quatro arquivos Word afetados nesta rodada (Instrumento 5 reescrito: 229 parágrafos; Política: 143
   parágrafos; Plano: 282 parágrafos; Instrumento 4 reconstruído apenas para atualizar a referência ao
   Instrumento 5 no Anexo: 329 parágrafos, conteúdo normativo inalterado). Conversão para PDF e inspeção visual
   de todas as páginas de cada um (7, 6 e 7 páginas vazias, respectivamente, para Instrumento 5/Política/Plano).
6. **Teste de leiaute com preenchimento fictício realista em cópias descartáveis.** Todas as tabelas e campos
   `[preencher]`/vazios da Política e do Plano foram preenchidos com um texto de teste deliberadamente longo
   (para forçar o pior caso de quebra de linha) em cópias descartáveis (`teste_plano.docx`, não versionadas),
   convertidos para PDF e inspecionados: sem quebra de texto caractere a caractere em nenhuma célula, inclusive
   na tabela de contatos (5 colunas) e nos blocos verticais de waypoint do Plano. O Instrumento 5 já havia
   recebido o mesmo teste na rodada anterior (seção 15.5) e foi reconfirmado após a reescrita das Partes A/B/C.
7. **Walkthrough textual dos três cenários adicionais e do cenário original com o novo Plano.** Este é um
   exercício de leitura cruzada do texto do Plano contra os registros já existentes dos cenários: **não é um
   exercício operacional executado** (não houve simulação ao vivo, cronometragem nem tabletop com os novos
   documentos nesta rodada):
   - *Cenário original (INC-2026-001, acesso indevido + exfiltração via token):* mapeia diretamente à seção 3 do
     Plano: Detect (E01) → Verify and Triage (E02, abertura de AVR-01) → Scope 1/Contain 1/Eradicate 1/Recover 1
     → reentrada por nova evidência (E04, 30.000 registros) → Scope 2/Contain 2/Eradicate 2/Recover 2. A trilha
     de privacidade (seção 4 do Plano) corresponde à reavaliação AVR-01→AVR-02 já registrada no Instrumento 2.
   - *Cenário 15.1, envio incorreto:* mapeia à seção 4, item 6 a 7 do Plano (preparação/envio/comprovação
     distintos; complementação sem limite numérico) e à Parte C do Instrumento 5: o registro "Complementação à
     ANPD" corrigiu o número de titulares sem reabrir o prazo original, consistente com a seção 4, item 5.
   - *Cenário 15.2, alteração indevida (integridade, sem exportação):* mapeia à seção 3 do Plano, nota de
     abrangência ("incidentes de confidencialidade, integridade, disponibilidade e autenticidade... inclusive
     acidentais") e à Condição B via inciso III do art. 5º do RCIS, já registrada no Instrumento 2.
   - *Cenário 15.3, indisponibilidade sem exfiltração (ransomware):* mapeia à mesma nota de abrangência do
     Plano e à seção 2.4 (distinção entre severidade técnica e avaliação regulatória): a indisponibilidade de
     14h foi tecnicamente grave sem que a exfiltração estivesse confirmada, exigindo avaliação regulatória
     própria ("Em apuração"), consistente com a seção 2.4.
   Em todos os quatro casos, o texto do Plano permite identificar, sem ambiguidade, o documento orientador, o
   instrumento de registro e o responsável típico: conferido manualmente linha a linha, não apenas por
   amostragem.
8. **Identificação de quem decide, quem executa, qual registro usar e quais pendências restam.** Confirmado pela
   tabela de integração do `README.md` (seção "Hierarquia dos documentos") cruzada com a tabela de waypoints do
   Plano (seção 3) e com a matriz de responsabilidades de `governanca/01-papeis-e-escalonamento.md`: para cada
   uma das oito atividades principais há exatamente um documento orientador, um instrumento de registro e um
   responsável típico nomeado: sem sobreposição nem lacuna identificada nesta conferência.
9. **Regressão proporcional nos instrumentos afetados.** O Instrumento 4 foi reconstruído (apenas a linha do
   Anexo que referencia o Instrumento 5) e revalidado por `validate.py` (329 parágrafos, schema aprovado); os
   Instrumentos 1, 2 e 3 não foram alterados nesta rodada: testados, em cópias descartáveis não versionadas,
   por `recalc.py`: 150, 12 e 0 fórmulas recalculadas, respectivamente, 0 erros em cada um, confirmando que a
   adição da Política/Plano e o ajuste do Instrumento 5 não afetaram os instrumentos em Excel.
10. **Registro de ferramentas, versões, testes e limitações.** Ver o parágrafo de abertura desta seção para
    ferramentas e versões. Limitações desta rodada: (a) o walkthrough do item 7 é uma conferência textual, não um
    tabletop executado: a organização deve exercitar efetivamente o Plano (Jornada 3) antes de confiar nele em
    um incidente real; (b) a lista de contatos do Plano (seção 1.3) e os critérios de severidade (seção 2.1) são
    campos de configuração vazios por desenho: este repositório não pode testá-los com dados reais de nenhuma
    organização; (c) a verificação automatizada de DOCX usou LibreOffice, não Microsoft Word: mantém-se a
    recomendação já registrada na seção 16 de confirmação em Word real antes do uso em produção.

## 18. Validação desta rodada: Matriz de autoridade de decisão e ajustes de fidelidade ao DAIR

Esta rodada decorre da avaliação completa de aderência ao DAIR e ao procedimento vigente da ANPD (seção 17
cobriu a rodada anterior; esta cobre as melhorias então propostas e agora implementadas). Ferramentas e versões:
as mesmas da seção 17 (Node.js + `docx`, `validate.py`, LibreOffice headless com `pdftoppm`/`pdfinfo`,
`openpyxl` para varredura de conteúdo, `/mnt/skills/public/xlsx/scripts/recalc.py` para recálculo).

1. **Matriz de autoridade de decisão adicionada ao Plano (nova seção 2.5) e referenciada pela Política (seções
   5 e 6) e por `governanca/01-papeis-e-escalonamento.md`.** Reproduz o padrão do livro-fonte DAIR (seção "Document
   Incident Response Decision Authority"): 8 decisões (declarar incidente, conter com impacto relevante, expandir
   escopo, sair do Loop de Ações de Resposta, aceitar risco residual, autorizar comunicação à ANPD, autorizar
   comunicação aos titulares, declarar encerramento do acompanhamento regulatório), cada uma com colunas Decisão |
   Autoridade | Delegado | Documentação exigida. Texto explicita a distinção frente à matriz RACI já existente
   (RACI cobre participação/coordenação; a matriz de autoridade cobre o direito de decidir): nenhuma das duas
   substitui a outra. Mantida como modelo limpo, sem nomes próprios ou titulares reais (apenas papéis).
2. **Coluna "Informados (I)" completada em `governanca/01-papeis-e-escalonamento.md`.** A tabela de
   responsabilidades, que já tinha R/A/C, ganhou a quarta coluna do RACI para as 7 atividades existentes, com
   legenda atualizada (R/A/C/I) e cross-reference à nova matriz de autoridade do Plano.
3. **Terceira categoria de causa: "ambiental": adicionada ao lado de maliciosa/acidental.** Corrigido na
   abrangência do Plano (seção 3, Markdown e DOCX) e no Instrumento 3 (linha "Verify and Triage"/"Confirmar a
   natureza do incidente", descrição e evidência esperada), alinhando com o livro-fonte, que trata as três causas
   como igualmente cobertas pelos waypoints e pelo loop, apesar do foco predominante do livro em incidentes
   maliciosos.
4. **Debrief com os dois objetivos nomeados pelo DAIR: Incident Closure e Organizational Learning.** Seção 6.4
   do Plano reescrita (Markdown e DOCX) distinguindo os dois objetivos explicitamente, com novo item de bullet
   sobre avaliação/decisão de retenção, modificação ou descomissionamento de cada ativo temporário criado durante
   a resposta (isolamento de sistemas, imagens forenses, contas temporárias, monitoramento reforçado): elemento
   do Incident Closure ausente na redação anterior.
5. **Padronização ortográfica "Verify and Triage" (grafia canônica do livro-fonte, sem "&").** Corrigida em ~10
   arquivos Markdown (`guias/04-utilizar-instrumentos.md`, `guias/01-aprender.md`,
   `validacao/relatorio-validacao-v0.3.md`, `validacao/matriz-achados-A01-A18.md`,
   `cenario/02-linha-do-tempo-e-decisoes.md`, `templates/04-checklist-operacional.md`,
   `playbook/01-procedimento-operacional.md`, `governanca/plano-resposta-incidentes.md`), em 2 scripts geradores
   de planilha (`instrumento1.py`/`exemplo1.py`, via `common.py` compartilhado; `instrumento3.py`) e em 2
   geradores de Word (`instrumento4.js`, `plano.js`). Confirmado por varredura de conteúdo via `openpyxl` nos
   quatro arquivos `.xlsx` reconstruídos (Registro + EXEMPLO, Checklist + EXEMPLO): 0 ocorrências remanescentes
   da grafia antiga em cada um.
6. **Recálculo de fórmulas sem regressão.** `recalc.py` nos quatro arquivos `.xlsx` afetados: Registro: 12
   fórmulas recalculadas, 0 erros (igual à linha de base pré-mudança, confirmando que a correção textual não
   afetou fórmulas); Checklist: 0 fórmulas (esperado, planilha não tem fórmulas), 0 erros. Mesmo resultado nas
   cópias EXEMPLO correspondentes.
7. **Validação de schema e inspeção visual dos três DOCX afetados.** `validate.py` aprovado para os três:
   Política (143 parágrafos), Plano (322 parágrafos, incluindo a nova tabela de 4 colunas e 8 linhas da matriz de
   autoridade), Relatório de Resposta ao Incidente (329 parágrafos: reconstruído apenas para herdar a correção
   ortográfica no resumo de waypoints do Anexo, conteúdo normativo inalterado). Conversão para PDF e inspeção
   visual: Política (6 páginas, páginas 3 a 4 inspecionadas), Plano (8 páginas, páginas 4, 6 e 8 inspecionadas,
   incluindo a tabela da nova matriz de autoridade: larguras de coluna [2600, 2200, 1600, 2600] renderizam sem
   quebra de palavra em nenhuma célula), Relatório (8 páginas, páginas 1 e 4 inspecionadas, incluindo a linha
   "Verify and Triage" na tabela de metodologia DAIR aplicada: sem quebra de texto).
8. **Verificação de links e de ausência de dados do cenário fictício.** Varredura automatizada de todos os links
   relativos em arquivos `.md` do repositório: 100 links verificados, 0 quebrados (uma ocorrência de `](...)` é
   texto literal descrevendo o próprio regex de varredura nesta seção, não um link real). Nenhuma das edições
   desta rodada introduziu identificadores do cenário fictício (`INC-2026-001`, `EVT-`, `AVR-0`) ou valores de
   aprovação preenchidos nos modelos limpos: as mudanças foram puramente textuais (nova seção, nova coluna, troca
   de grafia, nova frase de abrangência).
9. **Limitações desta rodada.** (a) A matriz de autoridade é um ponto de partida com papéis genéricos: a
   organização deve nomear titulares e delegados reais no Plano, seção 1.3, e pode precisar adaptar as 8 decisões
   listadas à sua própria estrutura de governança; (b) como nas rodadas anteriores, a verificação de DOCX usou
   LibreOffice, não Microsoft Word: recomenda-se confirmação em Word real antes do uso em produção (seção 16);
   (c) nenhum exercício tabletop foi executado para validar a matriz de autoridade na prática: a organização deve
   testá-la junto com o Plano (Jornada 3).

## 19. Validação desta rodada: eliminação de travessões/meias-riscas e formatação ABNT NBR 14724 nos 4 DOCX

Esta rodada atende a duas exigências independentes: (a) eliminar travessão (—, U+2014) e meia-risca (–, U+2013)
usados como pontuação em todo o texto do repositório, reescrevendo as ligações entre ideias com vírgulas,
ponto e vírgula, dois-pontos, parênteses, pontos finais ou conjunções; e (b) reformatar os 4 instrumentos DOCX
segundo a NBR 14724. Ferramentas e versões: as mesmas das seções 17 e 18 (Node.js + `docx` 9.8.1, `validate.py`,
LibreOffice headless com `pdftoppm`/`pdfinfo`, `openpyxl`, `recalc.py`), mais um script Python de varredura por
expressão regular (`[—–]`) usado como critério objetivo de conclusão, em vez de `grep -c`, cuja
contagem de caracteres multibyte se mostrou inconsistente neste ambiente.

1. **Escopo da eliminação de travessões.** Todos os 21 arquivos Markdown do repositório (os mais extensos:
   `validacao/relatorio-validacao-v0.3.md`, 132 ocorrências; `validacao/matriz-achados-A01-A18.md`, 45;
   `governanca/plano-resposta-incidentes.md`, 19) e os 11 scripts geradores dos instrumentos
   (`instrumento1.py`, `instrumento2.py`, `instrumento3.py`, `exemplo1.py`, `exemplo2.py`, `exemplo3.py`,
   `common.py`, `instrumento4.js`, `instrumento5.js`, `plano.js`, `politica.js`), cujo texto embutido (valores de
   célula, comentários/notas do Excel, parágrafos do Word) também conta como conteúdo do repositório. O hífen
   simples (-) foi preservado onde faz parte da grafia correta de identificadores (`EVT-`, `AVR-`, `INC-2026-001`
   etc.) e não foi tocado em nenhuma correção; o sinal de menos (−, U+2212, usado em `UTC−03:00`) é um caractere
   distinto e também não foi alterado.
2. **Método.** Substituição por regra: travessão/meia-risca ladeado por espaços (o padrão predominante, "rótulo —
   explicação") virou dois-pontos; meia-risca ligando diretamente tokens alfanuméricos sem espaço (faixas como
   "E04–E06", "I–XII") virou " a "; o caractere isolado usado como marcador de célula de tabela ("—" sozinho)
   virou "N/A". Cada arquivo foi revisado manualmente após a substituição automática para corrigir os casos em
   que um parentético duplamente delimitado por travessões ("termo — explicação —, continuação") gerou
   construções artificiais ("termo: explicação: , continuação"): reescritas com parênteses ("termo (explicação),
   continuação") em `plano-resposta-incidentes.md`, `matriz-achados-A01-A18.md`, `plano.js`, `instrumento3.py`
   e `instrumento5.js`. Um caso à parte: o regime de faixa converteu
   indevidamente a cadeia de quatro etapas "Scope–Contain–Eradicate–Recover" (`plano.js`) em "Scope a Contain a
   Eradicate a Recover"; corrigido manualmente para a notação de barra ("Scope/Contain/Eradicate/Recover") já
   usada no restante do repositório.
3. **Verificação.** Varredura Python (`re.findall('[—–]', texto)`) confirmando 0 ocorrências em: todos
   os 21 arquivos `.md` do repositório; os 11 scripts geradores; os 6 arquivos `.xlsx` reconstruídos (valores de
   célula e comentários/notas, via `openpyxl`); os 4 arquivos `.docx` reconstruídos (texto extraído via
   `python-docx`). `node --check` e `python3 -c "ast.parse(...)"` confirmaram a integridade sintática dos 11
   scripts após as edições. `recalc.py` nos 6 `.xlsx`: 0 erros de fórmula, contagem de fórmulas inalterada frente
   à linha de base da seção 18.
4. **Formatação ABNT NBR 14724 aplicada aos 4 DOCX.** Fonte Arial, corpo de texto 12 pt (escolha confirmada pelo
   usuário frente à alternativa Times New Roman 12 pt); margens 3 cm (superior/esquerda) e 2 cm
   (inferior/direita); espaçamento de 1,5 linha e alinhamento justificado nos parágrafos de texto corrido;
   recuo de primeira linha de 1,25 cm nos parágrafos de corpo (não aplicado a títulos nem a itens de lista, que
   seguem a convenção de recuo próprio de cada um). Numeração de página em algarismos arábicos no canto superior
   direito (cabeçalho), suprimida na capa (`titlePage: true` combinado com cabeçalho/rodapé vazios na primeira
   página); rodapé "Página X de Y" removido. A capa dos 4 instrumentos segue o padrão institucional já adaptado
   nas rodadas anteriores (campos de organização/versão/data, não autor/instituição acadêmica: escolha
   confirmada pelo usuário).
5. **Escopo deliberadamente limitado no Instrumento 5 (Kit de Preparação e Controle das Comunicações).** Apenas
   os elementos de documento (fonte, margens, espaçamento/justificação do estilo padrão) foram aplicados; os
   tamanhos de fonte das funções auxiliares de tabela/campo (`instr`, `fillField`, `recordBlock`, `cell`) foram
   mantidos nos valores originais (18-24 half-points), sem o aumento para 12 pt aplicado aos parágrafos de texto
   corrido dos demais instrumentos. Decisão de risco: a troca de família de fonte (Calibri → Arial, que já ocorre
   neste arquivo) altera a largura de caractere; um aumento adicional de tamanho nas mesmas larguras de coluna
   historicamente frágeis (achado A07, seção 8) poderia reintroduzir quebra de texto caractere a caractere. A
   inspeção visual desta rodada (item 7 abaixo) confirmou que a troca de fonte, sozinha, não quebrou nenhuma
   célula da Parte C.
6. **Validação de schema (`validate.py`), sem alteração na contagem de parágrafos frente à rodada anterior:**
   Política 143 parágrafos, Plano 322, Relatório 329, Kit 229. Todos aprovados.
7. **Inspeção visual (conversão para PDF, LibreOffice headless, `pdftoppm`).** Capa dos 4 documentos: sem
   cabeçalho nem numeração de página, texto limpo. Página 2 dos 4 documentos: numeração arábica confirmada no
   canto superior direito, corpo de texto justificado com recuo de primeira linha legível. Tabela da seção 2.5
   do Plano (Matriz de autoridade de decisão, 4 colunas, larguras [2600, 2200, 1600, 2600], página 5-6 do PDF
   reformatado): renderiza sem quebra de palavra em nenhuma célula. Blocos verticais da Parte C do Kit
   (Comunicação inicial à ANPD, Complementação nº 1, Comunicação aos titulares, páginas 6-7 do PDF reformatado):
   sem quebra de caractere em nenhum rótulo ou campo, confirmando que a decisão do item 5 foi suficiente.
8. **Crescimento de paginação, atribuído à formatação (não a conteúdo adicionado).** Política: 6 → 10 páginas;
   Plano: 8 → 12; Relatório: 8 → 9; Kit: 7 → 9. Consistente com a soma de três fatores: espaçamento 1,5 (de
   simples), margens maiores (3 cm vs. 2 cm nas bordas superior/esquerda) e justificação com recuo de primeira
   linha, todos aplicados apenas aos parágrafos de texto corrido (não às tabelas, que mantiveram suas larguras e
   fontes originais).
9. **Limitações desta rodada.** (a) A reformatação ABNT foi aplicada à estrutura do documento (fonte, margens,
   espaçamento, numeração); elementos de capa acadêmica tradicionalmente exigidos pela norma (nome de instituição,
   cidade, ano) permanecem fora do escopo por serem substituídos, por decisão do usuário, pelos campos
   institucionais já existentes (organização, versão, data de aprovação); (b) como nas rodadas anteriores, a
   verificação de DOCX usou LibreOffice, não Microsoft Word: recomenda-se confirmação em Word real antes do uso
   em produção; (c) o Instrumento 5 manteve tamanhos de fonte de tabela menores que 12 pt por decisão de risco
   (item 5): uma organização que priorize a conformidade tipográfica integral sobre a estabilidade de layout pode
   reavaliar esse ponto especificamente, testando o aumento de fonte nas tabelas da Parte C com conteúdo real
   antes de adotar.
