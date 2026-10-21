# Avaliação de risco aos titulares: INC-2026-001

**Controlador fictício:** Aurora Comércio Digital S.A.  
**Referência:** LGPD e art. 5º da Resolução CD/ANPD nº 15/2024 (RCIS).

## Método

O exercício aplica avaliação cumulativa:

- **Condição A:** o incidente pode afetar significativamente interesses e direitos fundamentais dos titulares?
- **Condição B:** envolve, cumulativamente, ao menos uma categoria prevista nos incisos I a VI do art. 5º do RCIS: I. dados pessoais sensíveis; II. dados de crianças, adolescentes ou idosos; III. dados financeiros; IV. dados de autenticação; V. dados sob sigilo legal/judicial/profissional; VI. tratamento em larga escala?

A resposta depende dos fatos e critérios da norma; **volume ou CPF isolado não determinam automaticamente comunicação**.

## Avaliação v1: 19/10/2026, 09h25

- **Fatos confirmados:** 200 registros consultados indevidamente, com dados identificáveis.
- **Sem confirmação:** exportação, redistribuição ou maior universo de titulares.
- **Condição A:** risco potencial sob apuração; informação insuficiente para conclusão definitiva.
- **Condição B:** categorias e alcance ainda sob apuração.
- **Decisão:** manter avaliação aberta, aprofundar Scope, preservar evidências e monitorar prazo regulatório.

## Avaliação v2: 19/10/2026, 12h10

- **Fatos confirmados:** exportação de 30.000 registros de pessoas distintas para fora do ambiente autorizado.
- **Dados envolvidos:** nome, CPF, telefone, e-mail, endereço, valores e datas dos pedidos; não há evidência de cartões completos ou credenciais de clientes no arquivo.
- **Condição A:** **sim**, potencial de fraude e engenharia social direcionada com dados verdadeiros de compra/identificação, com alcance material.
- **Condição B:** **sim**, fundamentada separadamente em dois incisos distintos do art. 5º: **III (dados financeiros)**, pela natureza dos dados de pedidos exportados (independente da quantidade); e **VI (larga escala)**, pela combinação de volume absoluto (30.000 titulares distintos), abrangência (toda a base de pedidos do período) e forma não segmentada da exportação, e não apenas pelo número isolado de titulares, que o RCIS não fixa como limiar automático.
- **Medidas mitigadoras:** revogação de token (testada tecnicamente, não apenas presumida pelo log de execução), bloqueio de conta, revisão de acessos e monitoramento. Não removem dados já exportados.
- **Incertezas:** circulação e uso dos dados após a exportação.
- **Decisão simulada:** comunicar ANPD e titulares, observadas formalidades e prazos legais.

## Marco de prazo

Conhecimento pelo controlador: **segunda-feira, 19/10/2026, 09h25**. No cenário, assume-se prazo geral de três dias úteis e programação conservadora de comunicação até **22/10/2026**, sujeita a validação jurídica do marco, regras de contagem, feriados e exceções. Comunicações planejadas **não executadas**. Eventual complementação tem prazo e contagem específicos conforme RCIS.

## Regra de atualização

Toda evidência que modifique natureza dos dados, universo dos titulares, condições de risco, mitigação ou causa provável produz nova versão da avaliação, com data, responsável e justificativa.
