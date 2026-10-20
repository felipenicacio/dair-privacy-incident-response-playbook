# Linha do tempo e decisões simuladas

Horários de Brasília (UTC−03:00). **Nenhuma comunicação foi realmente enviada.**

| Data/hora | Evento / waypoint | Decisão |
|---|---|---|
| 19/10 09h10 | Detect: login suspeito E01 | Abrir INC-2026-001 e preservar logs |
| 19/10 09h25 | Verify & Triage: E02 confirma acesso a dados pessoais | Escalar ao encarregado e Jurídico; marcar conhecimento do controlador |
| 19/10 09h35 | Contain 1: conta bloqueada E03 | Verificar sessões e tokens, pendência ainda aberta |
| 19/10 10h00 | Recover 1: serviço restrito restabelecido | Manter monitoramento, sem encerrar investigação |
| 19/10 10h42 | Detect/Scope 2: exportação E04 | Reabrir Scope e Contain |
| 19/10 10h50 | E05/E06: 30.000 titulares e dados financeiros relacionados a pedidos | Ampliar avaliação de risco e comunicação |
| 19/10 11h05 | Contain 2: token revogado E07 | Revisar tokens, integrações e sessões |
| 19/10 12h10 | Privacy decision v2 | Decisão simulada de comunicação devida |
| 19/10 15h00 | Eradicate/Recover: E08 | Manter observação; documentar limitações |
| 20/10 14h00 | Debrief / saída da resposta ativa | Encerrar contenção ativa, manter follow-up regulatório |
| 21/10 10h00 | Comunicação ANPD planejada | **Não executada** |
| 22/10 10h00 | Comunicação aos titulares planejada | **Não executada** |

## Registro de decisões

**D-01 — 09h25.** Acesso indevido a 200 registros confirmado; abertura de incidente com dados pessoais. Ainda não há prova de exportação. Líder IR e encarregado determinam avaliação de risco v1.

**D-02 — 10h42.** Token persistente utilizado após bloqueio da conta: primeira contenção insuficiente. O líder IR determina nova iteração do loop e suspensão de exportações.

**D-03 — 12h10.** Com 30.000 titulares, dados financeiros relativos a pedidos e cópia externa, o controlador fictício considera atendidos os requisitos cumulativos do art. 5º do RCIS e determina elaboração de comunicações regulatórias, sujeitas à validação jurídica.

**D-04 — 20/10 às 14h00.** Encerramento da resposta técnica ativa autorizado somente após revogação de tokens, avaliação de acessos, acompanhamento reforçado e registro explícito do risco residual. Comunicação e monitoramento seguem como pendências.

## Aprendizado-chave

A descoberta posterior de exportação não é uma fase seguinte de checklist: **altera hipóteses anteriores e exige regressar aos waypoints necessários**. Decisões anteriores devem permanecer auditáveis.
