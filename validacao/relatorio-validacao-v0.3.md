# Relatório de validação técnica — v0.3

**Data da validação:** 20/10/2026 · **Responsável pela execução:** sessão assistida por IA, sob supervisão de Felipe Nicácio.

Este relatório documenta apenas os testes efetivamente executados nesta versão. Nenhum teste aqui descrito foi presumido ou declarado sem execução correspondente.

## 1. Escopo testado

- 3 planilhas Excel (modelos em branco): `DAIR_Registro_e_Gestao_de_Incidentes_v0.3.xlsx`, `DAIR_Avaliacao_Riscos_Titulares_ANPD_v0.3.xlsx`, `DAIR_Checklist_Operacional_v0.3.xlsx`.
- 3 planilhas Excel (conjunto demonstrativo fictício): as mesmas três, com sufixo `_EXEMPLO-INC-2026-001`.
- 2 documentos Word: `DAIR_Relatorio_Resposta_Incidente_v0.3.docx`, `DAIR_Modelos_Comunicacao_ANPD_Titulares_v0.3.docx`.

## 2. Testes em arquivos Excel

**Ferramenta:** recálculo completo via LibreOffice headless (`recalc.py`), que abre cada arquivo, recalcula todas as fórmulas e relata erros de fórmula.

| Arquivo | Fórmulas | Erros encontrados |
|---|---|---|
| DAIR_Registro_e_Gestao_de_Incidentes_v0.3.xlsx | 91 | 0 |
| DAIR_Avaliacao_Riscos_Titulares_ANPD_v0.3.xlsx | 80 | 0 |
| DAIR_Checklist_Operacional_v0.3.xlsx | 42 | 0 |
| DAIR_Registro_e_Gestao_de_Incidentes_v0.3_EXEMPLO-INC-2026-001.xlsx | 91 | 0 |
| DAIR_Avaliacao_Riscos_Titulares_ANPD_v0.3_EXEMPLO-INC-2026-001.xlsx | 80 | 0 |
| DAIR_Checklist_Operacional_v0.3_EXEMPLO-INC-2026-001.xlsx | 42 | 0 |

Todos os seis arquivos abrem sem mensagem de corrupção no LibreOffice (usado como motor de verificação; não foi possível testar a abertura no Microsoft Excel real nesta sessão — ver Limitações).

**Falhas encontradas e corrigidas durante a validação** (documentadas para transparência):

1. No Instrumento 1, as fórmulas do Painel que contavam linhas preenchidas usavam `COUNTIF`/`COUNTIFS` sobre a coluna de ID (que é ela própria uma fórmula retornando `""` quando vazia). O Excel/LibreOffice conta células com fórmula `""` como não-vazias para o critério `"<>"`, o que inflava os indicadores (ex.: "Ações pendentes" aparecia como 17 em vez de 3 no conjunto demonstrativo). Corrigido para contar sobre uma coluna de entrada manual real (ex.: "Ação", "Fato Observado", "Descrição", "Decisão Adotada").
2. A fórmula de "Tempo entre detecção e confirmação" referenciava as células erradas do Cadastro do Incidente (deslocamento de duas linhas). Corrigida para referenciar as células corretas e ajustado o formato para exibir horas e minutos.
3. A célula de "Prazo Regulatório" no Instrumento 2 (função `WORKDAY`) não tinha formato de data aplicado, exibindo o número de série em vez da data. Corrigido.
4. No conjunto demonstrativo, identificadores de evidência (`EVD-NNN`) referenciados na Linha do Tempo, no Registro de Decisões, no Plano de Ações e no Checklist estavam desalinhados com a numeração sequencial real da aba Gestão de Evidências do Instrumento 1. Todas as referências cruzadas foram revisadas e corrigidas para apontar ao mesmo identificador em todos os arquivos.

**Validações, fórmulas e filtros:** confirmados nas três planilhas (listas suspensas em todas as colunas de estado/classificação; formatação condicional no Painel do Instrumento 1; fórmulas de ID automático, contagem e prazo recalculando corretamente).

**Tratamento de campos vazios:** as fórmulas de ID e do Painel usam `IF(...="","",...)` e `IFERROR(...)`, retornando vazio ou mensagem explicativa em vez de erro quando a linha ainda não foi preenchida.

**Múltiplas iterações DAIR:** testado no conjunto demonstrativo — o Instrumento 3 registra a atividade "Revogar tokens de API" duas vezes, uma vez no Ciclo 1 (não concluída, falha de contenção) e uma vez no Ciclo 2 (concluída e validada), demonstrando o funcionamento não-linear do checklist.

**Indicadores não confundem ausência de informação com resultado positivo:** os campos de resultado regulatório (Instrumento 2) usam os estados explícitos "Avaliação pendente" e "Informações insuficientes" em vez de um valor padrão que poderia ser lido como aprovação.

## 3. Testes em arquivos Word

**Ferramentas:** conversão para PDF via LibreOffice headless e inspeção visual das páginas renderizadas (capa, sumário e ao menos uma seção de conteúdo de cada arquivo); validação de esquema XML via `validate.py`.

| Arquivo | Validação de esquema | Páginas (PDF) | Inspeção visual |
|---|---|---|---|
| DAIR_Relatorio_Resposta_Incidente_v0.3.docx | Aprovada | 8 | Capa, sumário e seção 1–4 inspecionadas |
| DAIR_Modelos_Comunicacao_ANPD_Titulares_v0.3.docx | Aprovada | 3 | Capa com aviso de não-oficialidade inspecionada |

**Falha encontrada e corrigida:** a primeira versão do Instrumento 4 usava um campo de sumário automático (TOC) do Word, que é renderizado vazio por leitores que não executam a etapa de "atualizar campo" (incluindo a pré-visualização em PDF usada nesta validação). Substituído por uma lista estática das 20 seções, legível em qualquer leitor sem ação adicional do usuário.

**Editabilidade:** ambos os arquivos são `.docx` nativos, sem proteção de documento, com estilos de título padrão do Word (Heading 1/2), permitindo edição direta e navegação pelo painel de estrutura do documento.

**Cabeçalhos, rodapés e controle de versão:** ambos os documentos têm cabeçalho identificando o instrumento e rodapé com numeração de página; o Instrumento 4 inclui tabela de histórico de versões na seção 1.2.

**Dados fictícios misturados a modelos reutilizáveis:** confirmado que não há — os Instrumentos 4 e 5 contêm apenas rótulos de campo e instruções de preenchimento em itálico, sem nenhum dado do incidente fictício INC-2026-001.

## 4. Consistência entre instrumentos

- Nomenclatura de identificadores (`INC-`, `EVT-`, `EVD-`, `DEC-`, `ACA-`, `AVR-`, `CIC-`, `CHK-`) aplicada de forma consistente nos três instrumentos Excel e documentada no [guia de utilização](../guias/04-utilizar-instrumentos.md).
- Dados do cenário de demonstração (INC-2026-001) conferidos contra os arquivos já existentes no repositório ([cenário e premissas](../cenario/01-cenario-e-premissas.md), [linha do tempo e decisões](../cenario/02-linha-do-tempo-e-decisoes.md)): horários, eventos E01–E09, decisões D-01–D-04 e o prazo de comunicação planejado para 22/10/2026 são consistentes entre os arquivos novos e os já publicados na v0.2.
- Referências cruzadas de evidência (`EVD-NNN`) revisadas e corrigidas em todos os arquivos do conjunto demonstrativo (ver item 2, falha 4).
- Referências legais e metodológicas (DAIR/SANS, LGPD, RCIS) conferidas em fontes oficiais durante o desenvolvimento desta versão — ver seção 5.

## 5. Fontes externas consultadas

- SANS Institute — página oficial e pôster da metodologia DAIR (waypoints Prepare, Detect, Verify & Triage, Response Actions Loop [Scope, Contain, Eradicate, Recover], Debrief; caráter iterativo confirmado).
- Lei nº 13.709/2018 (LGPD) — referência mantida conforme já constava no repositório.
- Resolução CD/ANPD nº 15/2024 (RCIS) — art. 5º (condições cumulativas), art. 6º (comunicação à ANPD, prazo de 3 dias úteis, conteúdo mínimo, complementação em até 20 dias úteis), art. 9º (comunicação aos titulares) e art. 10 (registro e retenção mínima de 5 anos) conferidos para fundamentar o conteúdo dos Instrumentos 2 e 5.
- NIST SP 800-61 Rev. 3 — identificação confirmada (publicação de abril/2025, "Incident Response Recommendations and Considerations for Cybersecurity Risk Management").

## 6. Limitações conhecidas

- Os testes de abertura foram executados no LibreOffice (motor de verificação disponível nesta sessão), não no Microsoft Excel/Word. A estrutura dos arquivos (`.xlsx`/`.docx` nativos, sem recursos proprietários) reduz o risco de incompatibilidade, mas uma verificação adicional no Microsoft Office é recomendada antes do uso em produção.
- Os cálculos de prazo regulatório (Instrumento 2) usam dias úteis simples (função `WORKDAY` sem lista de feriados) e não consideram a regra de prazo em dobro para agentes de pequeno porte — exigem validação jurídica caso a caso, como indicado no próprio instrumento.
- Não há sincronização automática entre os cinco instrumentos; a integração depende de disciplina manual no uso dos identificadores compartilhados.
- O conjunto demonstrativo cobre os três instrumentos em Excel; não foram gerados Instrumentos 4 e 5 preenchidos com o cenário fictício, por decisão de manter esses dois documentos estritamente como modelos em branco (conforme exigido para o Instrumento 4 na especificação desta versão) — o relatório e o kit de comunicação de exemplo podem ser elaborados pela organização a partir dos dados já disponíveis no conjunto demonstrativo das planilhas.
- A matriz de probabilidade e impacto (Instrumento 2) não foi testada com dados do conjunto demonstrativo, por ser uma ferramenta de apoio interna e genérica, não vinculada a um incidente específico.

## 7. Pontos que exigem revisão humana

- Validação jurídica da contagem de prazos e da aplicabilidade das categorias do art. 5º, II, do RCIS ao caso concreto.
- Confirmação de que a terminologia em português adotada para os waypoints do DAIR (mantidos em inglês, conforme o original) está adequada ao público interno da organização que adotar o playbook.
- Revisão por um usuário de Microsoft Excel/Word real antes de uso em produção, dado que a verificação automatizada usou o LibreOffice.
- Decisão sobre se os Instrumentos 4 e 5 devem ganhar, em versão futura, um conjunto demonstrativo próprio preenchido com o cenário fictício.
