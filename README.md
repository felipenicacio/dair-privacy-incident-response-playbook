# DAIR Privacy Incident Response Playbook

**Versão:** 0.1.0 (MVP) · **Data:** 20/10/2026 · **Autor da adaptação:** Felipe Nicácio

> Projeto independente, educacional e de uso operacional adaptável. Não é publicação oficial da SANS, da ANPD ou do NIST. O incidente e a organização retratados são fictícios. Os registros preenchidos são decisões simuladas, não atos efetivos.

## Objetivo

Aplicar o **Dynamic Approach to Incident Response (DAIR)** de Joshua Wright, publicado pela SANS Institute, à resposta a incidentes envolvendo dados pessoais, integrando a atuação técnica e a avaliação regulatória sob a LGPD e a ANPD.

## Como executar o exercício

1. Leia o [cenário e as premissas](cenario/01-cenario-e-premissas.md).
2. Execute o [procedimento operacional](playbook/01-procedimento-operacional.md).
3. Compare com a [linha do tempo e as decisões](cenario/02-linha-do-tempo-e-decisoes.md).
4. Analise [risco aos titulares](templates/01-avaliacao-risco-titulares.md) e [registro do incidente](templates/02-registro-incidente.md).
5. Examine a [comunicação simulada](templates/03-comunicacao-simulada.md).
6. Conclua com [relatório de encerramento](cenario/03-relatorio-encerramento.md) e [critérios de aceite](governanca/02-criterios-aceite.md).

## Estrutura documental

- [Cenário de incidente](cenario/01-cenario-e-premissas.md)
- [Cronologia e pontos de decisão](cenario/02-linha-do-tempo-e-decisoes.md)
- [Relatório final](cenario/03-relatorio-encerramento.md)
- [Governança e escalonamento](governanca/01-papeis-e-escalonamento.md)
- [Critérios de aceite](governanca/02-criterios-aceite.md)
- [Playbook DAIR](playbook/01-procedimento-operacional.md)
- [Matriz de avaliação de riscos](templates/01-avaliacao-risco-titulares.md)
- [Registro do incidente](templates/02-registro-incidente.md)
- [Modelos de comunicação](templates/03-comunicacao-simulada.md)
- [Checklist operacional](templates/04-checklist-operacional.md)
- [Fontes e atribuições](referencias/fontes-e-atribuicoes.md)

## Limitações e segurança

Este MVP usa cenário totalmente fictício. Não automatiza SIEM/SOAR nem substitui investigação forense, decisão do controlador ou assessoria jurídica. **Não publique dados pessoais reais, tokens, credenciais nem evidências sensíveis em repositórios públicos.**

Os conteúdos autorais são licenciados em [CC BY 4.0](LICENSE.md). DAIR é criação de Joshua Wright / SANS Institute; projeto independente, sem endosso institucional.