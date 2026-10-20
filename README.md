# DAIR Privacy Incident Response Playbook

**Versão:** 0.1.0 (MVP) · **Data:** 20/10/2026 · **Autor da adaptação:** Felipe Nicácio

> Projeto independente, educacional e de uso operacional adaptável. Não é publicação oficial da SANS, da ANPD ou do NIST. O incidente e a organização retratados são inteiramente fictícios. Os registros preenchidos representam decisões simuladas, não atos efetivamente praticados ou comunicações enviadas.

## Objetivo

Aplicar o **Dynamic Approach to Incident Response (DAIR)**, criado por Joshua Wright e publicado pela SANS Institute, à resposta a incidentes de segurança envolvendo dados pessoais, integrando investigação, contenção, recuperação e decisões sobre riscos aos titulares e comunicação regulatória no contexto brasileiro.

## Comece por aqui

1. Leia [Cenário e premissas](cenario/01-cenario-e-premissas.md).
2. Siga o [procedimento operacional](playbook/01-procedimento-operacional.md).
3. Compare suas decisões com a [linha do tempo](cenario/02-linha-do-tempo-e-decisoes.md).
4. Revise a [avaliação de risco](templates/01-avaliacao-risco-titulares.md) e o [registro de incidente](templates/02-registro-incidente.md).
5. Consulte a [comunicação simulada](templates/03-comunicacao-simulada.md) — **não protocolada**.
6. Conclua com o [relatório de encerramento](cenario/03-relatorio-encerramento.md) e os [critérios de aceite](governanca/02-criterios-aceite.md).

## Escopo do MVP

Comprometimento de conta privilegiada, acesso não autorizado ao CRM, exportação de dados pessoais, duas iterações do Response Actions Loop (Scope, Contain, Eradicate, Recover), avaliação dinâmica de risco aos titulares, decisão de comunicação, registro de evidências, cronologia e encerramento.

## Documentação

- [Procedimento DAIR](playbook/01-procedimento-operacional.md)
- [Cenário](cenario/01-cenario-e-premissas.md)
- [Decisões e cronologia](cenario/02-linha-do-tempo-e-decisoes.md)
- [Relatório final](cenario/03-relatorio-encerramento.md)
- [Risco aos titulares](templates/01-avaliacao-risco-titulares.md)
- [Registro de incidente](templates/02-registro-incidente.md)
- [Comunicações simuladas](templates/03-comunicacao-simulada.md)
- [Checklist](templates/04-checklist-operacional.md)
- [Papéis e escalonamento](governanca/01-papeis-e-escalonamento.md)
- [Critérios de aceite](governanca/02-criterios-aceite.md)
- [Fontes e atribuições](referencias/fontes-e-atribuicoes.md)

## Distinções importantes

- Incidente técnico não implica automaticamente comunicação regulatória.
- Escopo confirmado deve ser separado das hipóteses, estimativas e lacunas.
- O controlador fundamenta a avaliação de risco aos titulares nos requisitos vigentes da LGPD e do Regulamento de Comunicação de Incidente de Segurança (RCIS) da ANPD.
- A análise jurídica acompanha a resposta técnica, e novas evidências podem exigir reavaliação.

## Proteção de informações

**Nunca publique logs reais, credenciais, tokens, IPs internos sensíveis, dados de titulares ou evidências forenses em issues ou pull requests públicos.** Casos reais exigem ambiente privado, classificação apropriada e cadeia de custódia.

## Licenciamento e atribuição

Textos originais disponibilizados sob **CC BY 4.0** (veja [LICENSE.md](LICENSE.md)). DAIR é criação de **Joshua Wright / SANS Institute**. Este repositório não sugere associação ou endosso e não substitui assessoria jurídica, investigação forense ou avaliação regulatória aplicável.
