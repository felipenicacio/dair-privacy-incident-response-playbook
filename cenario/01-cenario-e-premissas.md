# Cenário fictício: INC-2026-001

## Contexto

A **Aurora Comércio Digital S.A.**, sociedade fictícia, é controladora de dados de clientes de uma plataforma brasileira de comércio eletrônico, com aproximadamente 180 mil clientes cadastrados. Utiliza CRM SaaS, provedor de identidade (IdP), EDR e SIEM. Neste exercício, o fornecedor do CRM atua como operador.

## Evento inicial

**19/10/2026, 09h10 (America/Sao_Paulo):** alerta de login de conta de suporte privilegiada em dispositivo não gerenciado. Suspeita-se de phishing e reutilização de sessão. A causa inicial é hipótese até confirmação adicional.

## Evidências do exercício

| ID | Fato | Estado |
|---|---|---|
| E01 | Login incomum da conta de suporte às 09h10 | Confirmado |
| E02 | Consulta indevida a 200 registros às 09h18 | Confirmado |
| E03 | Senha alterada e conta bloqueada às 09h35 | Confirmado |
| E04 | Exportação de 30.000 registros às 10h42 mediante token de API anterior ao bloqueio | Confirmado |
| E05 | Dados: nome, CPF, telefone, e-mail, endereço, valores e datas dos pedidos | Confirmado |
| E06 | Cópia para fora do ambiente autorizado; redistribuição posterior desconhecida | Confirmado / desconhecido |
| E07 | Revogação do token às 11h05 | Confirmado |
| E08 | Verificação de outros tokens sem indicadores adicionais às 15h | Confirmado, cobertura limitada |
| E09 | Evidências de e-mail e IdP corroboram phishing | Hipótese corroborada |

Não há números completos de cartões nem credenciais de clientes entre os campos identificados.

## Premissas

- Conhecimento pelo controlador de afetação de dados pessoais: **19/10/2026 às 09h25**, após confirmação de acesso indevido a registros identificáveis.
- O controlador não é agente de pequeno porte para as regras específicas de prazo neste exercício.
- Não há feriados relevantes na contagem simulada; uso real exige verificação jurídica.
- E-mails e pessoas são fictícios; nenhum dado real de cliente é utilizado.

## Objetivos de aprendizagem

1. Distinguir detecção, confirmação, hipótese e evidência.
2. Identificar falha de contenção: bloquear conta não invalida necessariamente tokens.
3. Executar nova iteração do DAIR ao mudar o escopo.
4. Reavaliar riscos aos titulares pelo RCIS.
5. Registrar decisões, pendências e prazos sem fingir envio de comunicações.
