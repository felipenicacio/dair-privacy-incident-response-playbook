# Jornada 3: Exercício de mesa (tabletop) guiado

**Público:** equipes de IR, SOC, IAM, Privacidade, Jurídico, negócio e continuidade.  
**Formato:** exercício conduzido por facilitador; não envolve sistemas reais.  
**Duração sugerida:** entre 60 e 90 minutos.  
**Objetivo:** observar qualidade de decisões, comunicação, evidências e capacidade de replanejamento.

## Preparação do facilitador

1. Ler o [cenário completo](../cenario/01-cenario-e-premissas.md), a [cronologia](../cenario/02-linha-do-tempo-e-decisoes.md) e o [relatório final](../cenario/03-relatorio-encerramento.md).
2. Definir participantes e autoridade de decisão segundo a [matriz funcional](../governanca/01-papeis-e-escalonamento.md).
3. Designar um registrador para documentar decisões, hipóteses, responsáveis, evidências exigidas e prazos.
4. Entregar aos participantes somente o contexto inicial abaixo. **Não compartilhar a cronologia completa antecipadamente**, pois contém o desfecho.

## Rodada 1: Detecção e confirmação

**Informação entregue:** 19/10, 09h10. O SIEM alerta para autenticação privilegiada no CRM a partir de dispositivo não gerenciado.

**Decisões solicitadas:** quem assume o incidente; quais logs preservar; como validar acesso; quais ações urgentes são proporcionais; quando envolver encarregado e Jurídico?

**Nova informação:** às 09h25, logs confirmam consulta indevida a 200 registros identificáveis. Solicitar avaliação inicial, classificação de confiança e registro do conhecimento pelo controlador.

## Rodada 2: Contenção aparentemente concluída

**Informação entregue:** às 09h35, a conta é bloqueada e a senha redefinida. A operação de suporte é restabelecida de forma restrita.

**Decisões solicitadas:** que testes demonstram contenção efetiva? O que verificar em sessões, API, tokens e integrações? É possível declarar erradicação e recuperação completa?

Registrar o que a equipe faria **antes** da próxima injeção.

## Rodada 3: Mudança material de escopo

**Informação entregue:** às 10h42, é detectada exportação de 30.000 registros por token de API ainda ativo. O arquivo inclui nome, CPF, contatos, endereço, valores e datas de pedidos; o destino externo é identificado, mas redistribuição posterior permanece desconhecida.

**Decisões solicitadas:** que waypoints serão revisitados? Como conter o acesso remanescente? Como dimensionar titulares afetados? O que muda na avaliação de risco e na estratégia de comunicação?

**Nova informação:** token revogado às 11h05; investigação dos demais tokens apresenta cobertura limitada.

## Rodada 4: Decisão, recuperação e encerramento

**Informação entregue:** o encarregado e o Jurídico precisam recomendar uma decisão fundamentada sobre comunicação. A equipe técnica solicita autorização para encerrar a resposta ativa.

**Decisões solicitadas:** quais condições cumulativas do RCIS são atendidas? Qual marco de prazo foi registrado? O que falta investigar? Qual risco residual fica aberto? Quem aprova o encerramento técnico e quem acompanha obrigações posteriores?

## Registro de desempenho

Avaliar de 0 a 2 cada dimensão, conforme evidência do exercício: **0 = não demonstrado; 1 = parcial; 2 = demonstrado**.

| Dimensão | Evidência observável |
|---|---|
| Detecção e confirmação | Eventos preservados, fatos separados de hipóteses |
| Governança | Decisores, escalonamento e autoridades explícitos |
| Contenção | Sessões/tokens revisados; validação técnica exigida |
| Resposta dinâmica | Nova iteração após exportação |
| Privacidade | Análise fundamentada e prazo rastreado |
| Recuperação | Critérios de serviço e risco residual registrados |
| Documentação | Decisões e evidências rastreáveis |

**Pontuação máxima: 14.** A escala é didática e **não representa maturidade validada, conformidade normativa ou certificação**. Priorizar lacunas observadas e testes posteriores.

## Debrief

Comparar resultados com a [linha do tempo de referência](../cenario/02-linha-do-tempo-e-decisoes.md) e registrar três melhorias prioritárias, com donos e evidências de verificação. Revisar o [checklist operacional](../templates/04-checklist-operacional.md).

**Limite desta versão:** o tabletop é conduzido manualmente. Não existe sistema de injeção automática, formulário interativo persistente ou geração de relatório; os instrumentos operacionais da [Jornada 4](04-utilizar-instrumentos.md) podem apoiar o registro manual do exercício, mas não automatizam a condução do tabletop.
