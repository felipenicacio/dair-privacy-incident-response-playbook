# Jornada 2: Adaptar o playbook à organização

**Público:** CISO, coordenador de resposta, SOC, IAM, encarregado, Jurídico, continuidade e donos de serviços.  
**Objetivo:** transformar um exemplo fictício em procedimento interno validado.

## Passo 0: Adaptar e aprovar Política e Plano

Antes de adaptar procedimentos técnicos, submeta a [Política](../governanca/politica-resposta-incidentes.md) e o
[Plano de Resposta a Incidentes](../governanca/plano-resposta-incidentes.md) à aprovação formal da organização,
preenchendo os campos de configuração (organização, owner, aprovador, versão, vigência) e a lista real de
contatos, canais e critérios de severidade do Plano. Os passos 1 a 6 abaixo detalham essa adaptação; nenhum deles
substitui a aprovação formal destes dois documentos antes do uso operacional.

## Passo 1: Definir o contexto

Mapear serviços críticos, operações de tratamento, papéis de controlador/operador, contratos, fontes de evidência e recursos de investigação. Identificar regulamentações setoriais adicionais aplicáveis.

## Passo 2: Definir pessoas e decisões

Usar [papéis e escalonamento](../governanca/01-papeis-e-escalonamento.md) como referência. Atribuir autoridade real para declarar incidentes, suspender serviços, autorizar restauração, avaliar riscos aos titulares e aprovar comunicações. Confirmar substitutos e canais de crise.

## Passo 3: Adaptar os procedimentos

Revisar cada waypoint no [playbook operacional](../playbook/01-procedimento-operacional.md). Para cada ação, definir:

- Gatilho e objetivo;
- Responsável e aprovador;
- Ferramentas e permissões necessárias;
- Evidência de execução;
- Critério de validação e condição de reentrada no loop;
- Impacto na continuidade e dependências com fornecedores.

Atenção especial à revogação de **contas, sessões, tokens e integrações**: bloquear credenciais pode não encerrar sessões existentes.

## Passo 4: Preparar registros controlados

Adaptar a [avaliação de riscos](../templates/01-avaliacao-risco-titulares.md), o [registro de incidente](../templates/02-registro-incidente.md), as [comunicações simuladas](../templates/03-comunicacao-simulada.md) e o [checklist](../templates/04-checklist-operacional.md).

**Os arquivos publicados estão preenchidos com um cenário fictício; não devem ser reutilizados como registros de um incidente real.** Criar modelos internos limpos, sob controle de versão e acesso.

## Passo 5: Validar o tratamento regulatório

O encarregado e o Jurídico devem verificar os critérios cumulativos do regulamento da ANPD, o marco de conhecimento do controlador, a contagem dos prazos, o conteúdo obrigatório das comunicações, as regras de complementação e obrigações adicionais. Nenhum score interno substitui essa fundamentação.

## Passo 6: Exercitar antes do uso

Executar um [tabletop guiado](03-testar-equipe.md). Testar canais de crise, acesso a logs, revogação efetiva de tokens, aprovações, evidências de comunicação e recuperação. Abrir ações corretivas com responsáveis e comprovação de eficácia.

## Critério de saída

A organização dispõe de responsabilidades aprovadas, procedimento revisado, controles testados, registros protegidos e treinamento documentado. Usar os [critérios de aceite](../governanca/02-criterios-aceite.md) como ponto de partida, não como certificação.

## Segurança e uso responsável

Não inserir evidências reais, identificadores de titulares, segredos ou informações restritas neste GitHub público. Para casos reais, utilizar sistemas internos com controle de acesso, cadeia de custódia e política de retenção adequada.
