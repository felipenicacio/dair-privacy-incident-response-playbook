# Relatório de encerramento simulado — INC-2026-001

**Controlador:** Aurora Comércio Digital S.A. (fictícia)  
**Status em 20/10/2026, 14h00:** resposta técnica ativa encerrada; monitoramento e comunicações simuladas pendentes.

## Síntese executiva

Houve comprometimento de conta administrativa do CRM, seguido de consulta indevida a registros e exportação confirmada de dados de **30.000 pessoas** mediante token ainda válido após bloqueio da conta. Os dados incluem identificadores, contatos e dados financeiros associados a pedidos. Não foi identificada exportação de senhas de clientes ou números completos de cartões, conforme escopo de evidências analisado.

## Causa e fatores contribuintes

- Vetor inicial **provável:** phishing de conta privilegiada, corroborado por telemetria; análise não exclui outras hipóteses.
- Deficiência comprovada: política de revogação de sessões/tokens não foi aplicada integralmente no primeiro bloqueio.
- Exportação permitida a conta de suporte, com privilégios excessivos e detecção tardia.

## Resposta realizada no exercício

1. Preservação de logs e investigação de identidade/CRM.
2. Bloqueio de conta e redefinição de credencial.
3. Segunda iteração após identificação da exportação via API.
4. Revogação do token, redução de permissões e monitoramento de integrações.
5. Avaliação v2 de risco e elaboração de comunicações **não enviadas**.

## Risco residual

A exportação consumada não pode ser revertida. Permanecem desconhecidas eventual redistribuição e utilização futura dos dados; a investigação sobre tokens alternativos teve cobertura limitada. Registrar donos, datas e testes de conclusão das ações residuais.

## Plano de melhoria

| Prioridade | Ação | Responsável funcional | Evidência de conclusão |
|---|---|---|---|
| Crítica | Revogação automática de sessões e tokens após bloqueio de identidade | IAM | Teste de revogação fim a fim |
| Alta | Reduzir privilégios de exportação de suporte | Dono do CRM | Revisão de papéis e acessos |
| Alta | Alertas de exportação em massa e comportamento atípico | SOC | Caso de uso SIEM validado |
| Alta | Testar comunicação e escalonamento LGPD/ANPD | Jurídico / Encarregado | Tabletop e ata |
| Média | Reforçar controles contra phishing | Segurança | Exercício e indicadores |

## Encerramento condicionado

A liberação de saída do loop não equivale a risco zero, nem ao cumprimento de todas as obrigações regulatórias. O incidente permanece registrado e sujeito às rotinas de retenção e monitoramento previstas no RCIS.
