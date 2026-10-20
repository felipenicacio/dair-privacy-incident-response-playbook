# Governança e escalonamento do incidente

## Responsabilidades funcionais

| Atividade | Responsável (R) | Aprovador (A) | Consultados (C) |
|---|---|---|---|
| Declarar incidente e prioridade | Líder IR | CISO | SOC, TI |
| Coletar e preservar evidências | Analista forense | Líder IR | Operador do CRM |
| Conter conta, sessões e tokens | IAM | Líder IR | Dono do CRM |
| Avaliar risco aos titulares | Encarregado / Privacidade | Controlador / diretoria designada | Jurídico, IR |
| Deliberar comunicação regulatória | Jurídico / Encarregado | Representante autorizado do controlador | CISO, Comunicação |
| Validar recuperação operacional | TI / CRM | Dono do serviço | IR, Negócio |
| Autorizar encerramento técnico | Líder IR | CISO | Encarregado, Jurídico |

R: executa; A: responde pela aprovação. A responsabilidade legal pela comunicação é do controlador; a matriz não transfere obrigações legais.

## Regras de escalonamento

- Acionar encarregado e Jurídico assim que a ocorrência envolvendo dados pessoais for confirmada.
- Ampliar severidade e informar direção ao confirmar exfiltração, impacto relevante ou dados de risco elevado.
- Preservar decisões com horário, responsável, evidência, incertezas e versão.
- Conflitos entre preservação de evidências, contenção e continuidade exigem decisão registrada de autoridade competente.
- O prazo regulatório não depende da conclusão forense; acompanhar desde o marco de conhecimento aplicável.
