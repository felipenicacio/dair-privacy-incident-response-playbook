# Checklist operacional: INC-2026-001

Estados referem-se apenas ao **exercício fictício**.

## Prepare
- [x] Definir equipes, encarregado e cadeia de decisão.
- [x] Identificar fontes de log e funções do CRM.
- [ ] Automatizar revogação de tokens (falha deliberada do cenário).

## Detect / Verify and Triage
- [x] Preservar alerta original e horários.
- [x] Confirmar acesso não autorizado.
- [x] Identificar dados pessoais afetados e marco de conhecimento.
- [x] Acionar IR, encarregado e Jurídico.

## Primeiro ciclo: Scope / Contain / Eradicate / Recover
- [x] Delimitar conta e consulta a 200 registros.
- [x] Bloquear conta e alterar credencial.
- [ ] Revogar token de API (falha inicial intencional).
- [x] Recuperar serviço de suporte de forma limitada.

## Segundo ciclo: após nova evidência
- [x] Validar exportação de 30.000 registros.
- [x] Identificar categorias de dados e titulares distintos.
- [x] Revogar token e verificar sessões e integrações.
- [x] Reavaliar risco aos titulares e comunicação.
- [x] Testar mecanismos conhecidos de acesso indevido.
- [x] Formalizar risco residual e decisão de saída do loop.

## Debrief e privacidade
- [x] Preservar cronologia, autoria e versões das decisões.
- [x] Preparar comunicação simulada e monitorar prazo.
- [ ] Protocolar comunicação à ANPD (não aplicável no exercício).
- [ ] Comunicar titulares reais (não aplicável no exercício).
- [x] Documentar cadeia de custódia, retenção e lições aprendidas.
