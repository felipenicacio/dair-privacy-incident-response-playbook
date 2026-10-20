# Fontes, atribuições e limites

Correção pós-auditoria (achado A17): cada fonte abaixo identifica título, edição/data, URL específica e data de
consulta, e distingue as fontes **efetivamente utilizadas** para fundamentar o conteúdo normativo dos
instrumentos (metodológicas e legais) das referências **complementares** (citadas como leitura relacionada,
sem terem sido usadas para fundamentar uma exigência específica). Listar uma publicação nesta página não
declara conformidade com ela — ver "Limites" ao final.

## Metodologia (fonte efetivamente utilizada)

- **DAIR — Dynamic Approach to Incident Response**, criado por **Joshua Wright**, publicado pela **SANS Institute**.
  Material de referência consultado nesta correção: anexo fornecido pelo autor do projeto (arquivo `dynamicir.pdf`),
  conferido em 20/10/2026. A autoria do framework original pertence ao criador; este projeto é adaptação
  independente voltada a um exercício de privacidade, mantendo os waypoints e a natureza iterativa do Response
  Actions Loop conforme a formulação original. Verificar a publicação oficial mais recente da SANS Institute e
  suas condições de reutilização antes de reproduzir figuras e trechos — não se trata de minuta nem de
  recomendação não vinculante: é o próprio material metodológico de referência, usado aqui apenas para
  terminologia e estrutura (não é norma jurídica).

## Fontes legais brasileiras (fontes efetivamente utilizadas)

- **Lei nº 13.709/2018 (LGPD)**, com as alterações vigentes em 20/10/2026 — texto consolidado:
  https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2018/lei/l13709.htm (consultado em 20/10/2026). Usada para
  a referência ao art. 48 (dever de comunicar incidente) e para o enquadramento geral de titulares/controlador/operador.
- **Resolução CD/ANPD nº 15/2024 (RCIS)**, de 24/04/2024 — "Regulamento de Comunicação de Incidente de Segurança":
  publicação no Diário Oficial da União, Seção 1, edição de 26/04/2024 (https://www.in.gov.br/web/dou/-/resolucao-cd/anpd-n-15-de-24-de-abril-de-2024-556243024)
  e página da ANPD sobre comunicado de incidente de segurança (https://www.gov.br/anpd/pt-br/canais_atendimento/agente-de-tratamento/comunicado-de-incidente-de-seguranca-cis),
  ambas consultadas em 20/10/2026. Artigos efetivamente utilizados para fundamentar o conteúdo dos Instrumentos 2 e 5:
  - **Art. 5º, caput e incisos I a VI** — condições cumulativas de risco/dano relevante (afetação significativa +
    ao menos uma categoria: I. dados sensíveis; II. crianças/adolescentes/idosos; III. dados financeiros;
    IV. dados de autenticação; V. sigilo legal/judicial/profissional; VI. larga escala). Correção A05 desta
    versão: a redação anterior citava apenas o inciso II como se fosse a única hipótese da Condição B.
  - **Art. 6º** — comunicação à ANPD: prazo de 3 dias úteis (caput e §1º), conteúdo mínimo de 12 itens (§2º),
    complementação em até 20 dias úteis contados da comunicação efetivamente realizada (§3º), formulário
    eletrônico/SEI (§4º), comprovação de representação (§§5º–7º) e prazo em dobro para pequeno porte (§8º).
  - **Art. 7º** — pedido fundamentado de sigilo de informações protegidas por segredo comercial/industrial.
  - **Art. 8º** — poder da ANPD de solicitar informações adicionais a qualquer tempo.
  - **Art. 9º** — comunicação aos titulares: prazo de 3 dias úteis (caput), linguagem simples e comunicação
    direta/individualizada preferencial (§1º–§2º), divulgação pública alternativa por no mínimo 3 meses quando a
    comunicação direta for inviável (§3º), declaração de que a comunicação foi feita (§4º) e prazo em dobro para
    pequeno porte (§6º).
  - **Art. 10** — registro de todos os incidentes, inclusive não comunicados, por no mínimo 5 anos (caput),
    elementos mínimos do registro (§1º) e exceção de guarda permanente para entidades do art. 23 da LGPD, setor
    público (§2º).
  - Este regulamento não define um limiar numérico para "larga escala" (inciso VI) nem critérios objetivos de
    enquadramento de "pequeno porte" — a Resolução CD/ANPD nº 2/2022 trata do enquadramento de agentes de
    pequeno porte e deve ser consultada separadamente para essa finalidade; não foi usada como fonte direta de
    conteúdo normativo nesta correção, apenas referenciada para o enquadramento correto do ônus de prazo em dobro.
- A contagem de prazos, o enquadramento de pequeno porte, as regras setoriais específicas e demais obrigações
  aplicáveis a um caso concreto devem ser sempre confirmados pelo Jurídico na data do incidente — este
  repositório não substitui essa validação.

## Referências complementares (citadas como leitura relacionada — não usadas para fundamentar exigência específica)

- **NIST SP 800-61 Rev. 3**, *Incident Response Recommendations and Considerations for Cybersecurity Risk
  Management*, publicação de abril de 2025: https://csrc.nist.gov/pubs/sp/800/61/r3/final — referência de boas
  práticas de resposta a incidentes; não é norma brasileira e não fundamenta nenhuma exigência regulatória
  deste repositório.
- **ISO/IEC 27035** (gestão de incidentes de segurança da informação), **ISO/IEC 27001 e 27002** (gestão e
  controles de segurança da informação), **ISO/IEC 27701** (sistema de gestão de informações de privacidade) e
  **ISO 22301** (continuidade de negócios) — citadas como vocabulário e referência de mercado; **a menção a
  essas publicações não declara nem implica conformidade do repositório ou de quem o utiliza com qualquer
  dessas normas**. Confirmar edição vigente diretamente com a ISO/ABNT antes de qualquer referência formal.

## Licenciamento

Materiais originais deste repositório, inclusive os arquivos XLSX e DOCX autorais (instrumentos operacionais):
**CC BY 4.0**, de autoria de Felipe Nicácio. Essa licença cobre apenas o conteúdo original criado para este
projeto (estrutura, texto, fórmulas e catálogos). Direitos de terceiros — inclusive marcas, figuras, textos e
conteúdo do DAIR (SANS Institute/Joshua Wright), textos legais oficiais e qualquer logotipo da ANPD — são
independentes desta licença e permanecem com seus respectivos titulares; nenhuma reprodução de marca ou logo
oficial está incluída nos arquivos. Não se afirma endosso da SANS, ANPD ou NIST a este projeto (A18).

## Limites

Este repositório é material de treinamento e adaptação, não documentação oficial, assessoria jurídica ou
substituto de investigação forense. Listar uma publicação nesta página identifica uma fonte consultada, não
uma declaração de conformidade com ela. Todos os dados do incidente fictício (INC-2026-001) são fictícios.
