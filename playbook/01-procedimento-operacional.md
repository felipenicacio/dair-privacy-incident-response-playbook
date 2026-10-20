# Procedimento operacional — DAIR aplicado a dados pessoais

## Princípio

DAIR é um modelo dinâmico e iterativo. Os waypoints orientam decisões; **não constituem sequência rígida de fases ou obrigação de percorrer todos em uma única direção**. O Response Actions Loop envolve **Scope, Contain, Eradicate, Recover** com revisitas conforme o quadro muda.

## Prepare

**Objetivo:** estabelecer capacidade de resposta.

1. Designar líder IR, SOC, IAM, forense, dono do serviço, encarregado, Jurídico e representante autorizado do controlador.
2. Inventariar fontes de logs e capacidade de revogar contas, sessões, tokens e integrações.
3. Definir canais de crise, cadeia de custódia, classificações e autoridades de aprovação.
4. Criar trilha específica de avaliação de risco aos titulares, aplicável durante todo o incidente.

**Saída:** capacidade e responsabilidades conhecidas; deficiências aceitas ou tratadas.

## Detect

**Gatilho:** E01, login privilegiado atípico às 09h10.

- Abrir INC-2026-001, preservar alerta bruto e registrar timezone.
- Registrar hipóteses concorrentes sem tratá-las como fato.
- Correlacionar IdP, CRM, API e SIEM.
- Não aguardar confirmação total para preservar evidências.

**Saída:** evento rastreável, investigação aberta.

## Verify & Triage

**Gatilho:** E02 confirma acesso indevido a 200 registros às 09h25.

- Confirmar natureza do acesso, identidade afetada, dados pessoais e sistemas.
- Fixar marco fundamentado de conhecimento pelo controlador.
- Priorizar risco operacional e acionar encarregado/Jurídico.
- Abrir avaliação regulatória **v1**; não equiparar automaticamente incidente técnico a comunicação obrigatória.

**Saída:** incidente declarado, prioridade e próxima decisão.

## Response Actions Loop — primeira iteração

### Scope 1
- Delimitar conta, dispositivo, logs e 200 registros; registrar o que é desconhecido.
- Investigar sessões ativas, tokens de API, permissões de exportação e possíveis acessos correlatos.

### Contain 1
- Bloquear conta e trocar credenciais (E03).
- Validar revogação de sessões e tokens; **a falha em revogar token é deliberada no cenário**.
- Preservar evidências antes de ações destrutivas sempre que compatível com contenção.

### Eradicate 1
- Revisar privilégios, mecanismo de acesso e hipóteses de persistência.
- Não declarar remoção completa sem teste correspondente.

### Recover 1
- Restabelecer suporte de forma restrita, com monitoração adicional.
- Manter pendências e não declarar encerramento técnico.

## Reentrada no loop — segunda iteração

**Nova evidência crítica:** E04 identifica exportação de 30.000 registros via token persistente às 10h42.

### Scope 2
- Validar 30.000 titulares distintos e tipos de dados (E05).
- Confirmar telemetria da cópia externa (E06) e registrar destino/distribuição desconhecidos.
- Reclassificar severidade e atualizar matriz de risco **v2**.

### Contain 2
- Revogar token usado (E07), revisar outros tokens e sessões.
- Reduzir capacidades de exportação; aplicar bloqueios adicionais proporcionais.
- Validar tecnicamente que não há novas exportações pelo mecanismo conhecido.

### Eradicate 2
- Corrigir controles de revogação, eliminar acessos indevidos, revisar integrações.
- Investigar phishing provável sem confundir hipótese corroborada com causa definitivamente comprovada.

### Recover 2
- Validar serviço e controles, aumentar observabilidade, acompanhar indicadores.
- Documentar lacunas de cobertura E08 e risco de redistribuição impossível de eliminar apenas com recuperação técnica.

## Trilha transversal de privacidade

Em cada fato novo relevante, perguntar:

1. Quais dados pessoais foram **confirmadamente** afetados? Quais são apenas hipótese?
2. Existe potencial de afetação significativa de direitos/interesses fundamentais?
3. Está presente alguma categoria prevista no art. 5º, II, do RCIS?
4. Os requisitos cumulativos de comunicação foram atendidos?
5. Qual o marco de conhecimento do controlador, o prazo aplicável e as exceções?
6. Quais dados permanecem desconhecidos e como serão complementados?
7. Há riscos residuais, obrigações contratuais/setoriais ou medidas aos titulares?

A equipe técnica aporta fatos; encarregado/Jurídico fundamentam avaliação; o controlador delibera por seus representantes autorizados.

## Debrief

- Reconstruir cronologia, fatos, versões das decisões e ações executadas.
- Avaliar a falha da contenção inicial, tempo de detecção, preservação e resposta.
- Gerar relatório final com risco residual, responsáveis e plano de ação.
- Encerrar resposta técnica somente mediante aprovação, mantendo tarefas regulatórias abertas quando necessário.

## Regras de evidência e documentação

Distinguir **confirmado**, **hipótese**, **inferido**, **desconhecido** e **não aplicável**. Registrar origem, horário/timezone, coletor, integridade calculada quando há arquivo real, custodiantes e transferências. Não inventar hashes, provas ou protocolos.

**Uso real:** adaptar às leis vigentes, políticas organizacionais, procedimentos do operador e acordos de nível de serviço.
