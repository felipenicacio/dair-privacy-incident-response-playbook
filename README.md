# DAIR Privacy Incident Response Playbook

**Versão 0.3.0 — Instrumentos operacionais** · **20 de outubro de 2026** · **Felipe Nicácio**

> Projeto aberto e independente de apoio à resposta a incidentes envolvendo dados pessoais. O cenário é fictício. Não é publicação oficial ou endossada pela SANS, ANPD ou NIST e não substitui decisões técnicas ou jurídicas contextualizadas.

O projeto adapta o **Dynamic Approach to Incident Response (DAIR)**, de Joshua Wright / SANS Institute, a um caso de acesso indevido e exportação de dados pessoais, integrando resposta técnica, avaliação de risco aos titulares e governança LGPD/ANPD. Seu diferencial é **reavaliar escopo, contenção, erradicação e recuperação quando surgem novas evidências**, em vez de presumir um fluxo linear.

## Escolha sua jornada

| Quero... | Por onde começar | O que obtenho |
|---|---|---|
| **Aprender** o modelo | 📖 [Jornada 1 — Aprender](guias/01-aprender.md) | Estudo guiado de um incidente e suas duas iterações DAIR |
| **Implementar** na minha organização | 🛠️ [Jornada 2 — Implementar](guias/02-implementar.md) | Sequência de adaptação, responsabilidades, controles e validação |
| **Testar minha equipe** | 🧪 [Jornada 3 — Tabletop](guias/03-testar-equipe.md) | Exercício manual com quatro rodadas, perguntas e avaliação |
| **Usar os instrumentos editáveis** | 📊 [Jornada 4 — Utilizar os instrumentos](guias/04-utilizar-instrumentos.md) | Planilhas e documentos Word prontos para uso operacional |

## Novidade da v0.3 — Instrumentos operacionais editáveis

Cinco instrumentos em Excel e Word, independentes do cenário fictício, prontos para adaptação e uso real:

| # | Instrumento | Formato |
|---|---|---|
| 1 | [Registro e Gestão de Incidentes](instrumentos/excel/DAIR_Registro_e_Gestao_de_Incidentes_v0.3.xlsx) | `.xlsx` |
| 2 | [Avaliação de Riscos aos Titulares e Decisão de Comunicação (ANPD)](instrumentos/excel/DAIR_Avaliacao_Riscos_Titulares_ANPD_v0.3.xlsx) | `.xlsx` |
| 3 | [Checklist Operacional Dinâmico](instrumentos/excel/DAIR_Checklist_Operacional_v0.3.xlsx) | `.xlsx` |
| 4 | [Relatório de Resposta ao Incidente](instrumentos/word/DAIR_Relatorio_Resposta_Incidente_v0.3.docx) | `.docx` |
| 5 | [Kit de Comunicações Regulatórias e aos Titulares](instrumentos/word/DAIR_Modelos_Comunicacao_ANPD_Titulares_v0.3.docx) | `.docx` |

Um [conjunto demonstrativo](instrumentos/exemplos/) preenchido com o cenário fictício INC-2026-001 acompanha os modelos, exclusivamente para fins de treinamento. Siga a [Jornada 4](guias/04-utilizar-instrumentos.md) para saber qual instrumento usar, quando e por quem. Ver também o [relatório de validação técnica da v0.3](validacao/relatorio-validacao-v0.3.md).

**Início rápido:** quem dispõe de 15 minutos pode ler o [cenário](cenario/01-cenario-e-premissas.md), consultar as [decisões](cenario/02-linha-do-tempo-e-decisoes.md) e identificar por que o bloqueio da conta não encerrou o acesso via token. Para uso operacional, siga a jornada de implementação completa.

## Cenário didático

Em 19/10/2026, uma empresa fictícia detecta acesso anômalo em sua conta administrativa do CRM. Após confirmação de consulta a **200 registros**, a equipe bloqueia a conta. Pouco depois, uma exportação via **token ainda ativo** revela comprometimento de **30.000 titulares**. A equipe precisa reabrir a investigação, executar nova contenção, reavaliar risco e deliberar sobre obrigações de comunicação. **Todas as decisões e comunicações registradas neste repositório são simulações.**

## Materiais disponíveis

### Referência técnica
- [Procedimento operacional DAIR](playbook/01-procedimento-operacional.md)
- [Cenário e premissas](cenario/01-cenario-e-premissas.md)
- [Linha do tempo e decisões](cenario/02-linha-do-tempo-e-decisoes.md)
- [Relatório de encerramento](cenario/03-relatorio-encerramento.md)

### Governança e privacidade
- [Papéis e escalonamento](governanca/01-papeis-e-escalonamento.md)
- [Critérios de aceite](governanca/02-criterios-aceite.md)
- [Avaliação de riscos aos titulares — exemplo preenchido](templates/01-avaliacao-risco-titulares.md)
- [Registro de incidente e evidências — exemplo preenchido](templates/02-registro-incidente.md)
- [Comunicações simuladas — não enviadas](templates/03-comunicacao-simulada.md)
- [Checklist operacional — exercício](templates/04-checklist-operacional.md)

### Instrumentos operacionais (v0.3)
- [Guia de utilização dos instrumentos](guias/04-utilizar-instrumentos.md)
- [Instrumentos em Excel](instrumentos/excel/) · [Instrumentos em Word](instrumentos/word/) · [Conjunto demonstrativo](instrumentos/exemplos/)
- [Relatório de validação técnica v0.3](validacao/relatorio-validacao-v0.3.md)

### Referências
- [Fontes, atribuições e limitações](referencias/fontes-e-atribuicoes.md)
- [Licença dos conteúdos autorais](LICENSE.md)

## Como usar com segurança

1. **Para aprender:** acompanhe o caso simulado, faça suas próprias decisões antes de ler a cronologia completa e compare as justificativas.
2. **Para implementar:** adapte os procedimentos e os registros a sistemas, contratos, responsabilidades, controles e critérios jurídicos reais. Os arquivos preenchidos **não são formulários prontos para uso em incidentes reais**.
3. **Para testar:** um facilitador deve entregar as evidências gradualmente e manter a cronologia de referência reservada até o debrief.

**Não publique dados pessoais reais, tokens, credenciais, registros forenses ou informações restritas em issues, discussões ou pull requests públicos.** Use ambiente interno controlado para incidentes efetivos.

## Limitações da versão 0.3

Os instrumentos não possuem sincronização automática entre si, não há automação SOAR nem geração automática de relatório a partir das planilhas, e os cálculos de prazo regulatório são apoio em dias úteis simples — não substituem validação jurídica (feriados, regras de pequeno porte e exceções específicas devem ser confirmados caso a caso). A avaliação regulatória deve sempre ser validada no contexto do incidente, conforme normas vigentes. Ver o [relatório de validação](validacao/relatorio-validacao-v0.3.md) para o detalhamento.

## Próximas evoluções

- **v0.4:** exercícios com injeção de evidências e material para facilitadores;
- **v1.0:** validação com profissionais e refinamento técnico-regulatório.

## Créditos e licença

Adaptação e materiais autorais: **Felipe Nicácio**, sob [CC BY 4.0](LICENSE.md). O DAIR é de autoria de **Joshua Wright**, divulgado pela **SANS Institute**. Este projeto não implica endosso de terceiros.
