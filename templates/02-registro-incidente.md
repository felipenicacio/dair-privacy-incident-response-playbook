# Registro estruturado do incidente — exemplo preenchido

**ID:** INC-2026-001 · **Status:** resposta técnica encerrada; privacidade em acompanhamento · **Controlador:** Aurora Comércio Digital S.A. (fictícia)

| Campo | Registro do exercício |
|---|---|
| Conhecimento de dados pessoais afetados | 19/10/2026 09h25 BRT |
| Período investigado | 19/10 09h10–15h00; observação até 20/10 14h |
| Natureza | Comprometimento de confidencialidade |
| Dados | Nome, CPF, contato, endereço, valores e datas de pedidos |
| Titulares | 30.000 pessoas distintas (simulação) |
| Ambientes | IdP, CRM SaaS, API |
| Risco | Potencial de fraude; avaliação v2 atende condições cumulativas do RCIS |
| Contenção | Conta bloqueada, token revogado, permissões revisadas |
| Comunicação | Planejada no tabletop, **não enviada** |
| Risco residual | Redistribuição externa dos dados desconhecida |
| Encerramento | Decisão D-04 em 20/10 às 14h; obrigações posteriores pendentes |

## Inventário e custódia

| ID | Origem | Aquisição | Integridade e custódia |
|---|---|---|---|
| E01 | SIEM / IdP | 19/10 09h15 | Preservação simulada pelo SOC |
| E02 | CRM | 19/10 09h28 | Cópia de logs somente leitura, analista forense |
| E03 | IdP | 19/10 09h36 | Histórico de mudança, IAM |
| E04–E06 | CRM / API | 19/10 10h48–11h00 | Logs e resposta do operador, analista forense |
| E07 | API | 19/10 11h07 | Log de revogação, IAM |
| E08–E09 | IdP / e-mail / CRM | 19/10 15h00 | Coleta limitada e hipóteses identificadas |

Para incidentes reais, registrar **hash SHA-256 calculado**, origem, método, arquivo, hora/timezone, coletor, local seguro de armazenamento, cada transferência e conferência de integridade. Nunca inventar hashes nem publicar evidências reais neste repositório.

## Retenção

O art. 10 do RCIS prevê registro de incidentes envolvendo dados pessoais, inclusive os não comunicados, por pelo menos **cinco anos**, observadas regras aplicáveis. Armazenar com controles de acesso, sigilo, integridade e descarte seguro.
