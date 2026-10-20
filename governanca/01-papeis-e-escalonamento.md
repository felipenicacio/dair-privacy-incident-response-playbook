# Governança e escalonamento do incidente

## Responsabilidades funcionais

| Atividade | Responsável (R) | Aprovador (A) | Consultados (C) | Informados (I) |
|---|---|---|---|---|
| Declarar incidente e prioridade | Líder IR | CISO | SOC, TI | Direção, Encarregado |
| Coletar e preservar evidências | Analista forense | Líder IR | Operador do CRM | CISO |
| Conter conta, sessões e tokens | IAM | Líder IR | Dono do CRM | CISO, TI |
| Avaliar risco aos titulares | Encarregado / Privacidade | Controlador / diretoria designada | Jurídico, IR | Comunicação |
| Deliberar comunicação regulatória | Jurídico / Encarregado | Representante autorizado do controlador | CISO, Comunicação | Direção, IR |
| Validar recuperação operacional | TI / CRM | Dono do serviço | IR, Negócio | CISO |
| Autorizar encerramento técnico | Líder IR | CISO | Encarregado, Jurídico | Direção, Negócio |

R: executa; A: responde pela aprovação (um único aprovador por atividade); C: consultado em via de mão dupla
antes da decisão; I: mantido informado do andamento, sem participar da decisão. A responsabilidade legal pela
comunicação é do controlador; a matriz não transfere obrigações legais. Ver também a
[Matriz de autoridade de decisão](plano-resposta-incidentes.md#25-matriz-de-autoridade-de-decisão) do Plano, que
nomeia quem decide (e quem substitui) para as decisões mais consequentes, com a documentação que cada uma deve
deixar — RACI cobre participação; a matriz de autoridade cobre direito de decidir.

## Regras de escalonamento

- Acionar encarregado e Jurídico assim que a ocorrência envolvendo dados pessoais for confirmada.
- Ampliar severidade e informar direção ao confirmar exfiltração, impacto relevante ou dados de risco elevado.
- Preservar decisões com horário, responsável, evidência, incertezas e versão.
- Conflitos entre preservação de evidências, contenção e continuidade exigem decisão registrada de autoridade competente.
- O prazo regulatório não depende da conclusão forense; acompanhar desde o marco de conhecimento aplicável.
