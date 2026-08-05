# Sistema de Gestão de Contratos de Publicidade

## Case Study

Este projeto documenta a concepção e o desenvolvimento de um sistema para gestão do ciclo financeiro, documental e operacional de contratos de publicidade em um órgão público estadual.

O objetivo deste repositório é apresentar a arquitetura da solução, as decisões de produto, as regras de negócio e os principais desafios enfrentados durante o desenvolvimento. O código-fonte não é disponibilizado.

## Contexto

O processo era integralmente controlado por planilhas eletrônicas.

Planos financeiros, notas fiscais, checklists de conformidade, atestados, ofícios e pagamentos eram administrados manualmente, distribuídos em diversos arquivos independentes, sem integração, rastreabilidade ou regras automatizadas.

Além da complexidade operacional, o processo precisava atender requisitos típicos da administração pública:

- rastreabilidade das decisões;
- histórico completo de alterações;
- segregação de responsabilidades;
- conformidade documental;
- auditoria;
- acompanhamento de prazos legais.

O desafio não era apenas informatizar um processo existente, mas transformar um conjunto de rotinas administrativas em um sistema consistente, auditável e preparado para evoluir.

## Abordagem

Este projeto foi conduzido a partir da modelagem do processo de negócio.

Antes da implementação, foram mapeadas todas as etapas operacionais, identificadas as regras de negócio, definidos os fluxos, estados, exceções, permissões, indicadores e critérios de auditoria.

A implementação utilizou ferramentas de desenvolvimento assistido por inteligência artificial como aceleradoras do processo de engenharia.

A IA foi empregada como ferramenta de desenvolvimento, enquanto todas as decisões relacionadas à arquitetura, modelagem, regras de negócio, experiência do usuário e validação funcional permaneceram orientadas pelo conhecimento do domínio e pela evolução contínua do produto.

Mais do que demonstrar o desenvolvimento de um software, este projeto procura ilustrar uma forma de trabalho baseada na combinação entre análise de processos, engenharia de produto e utilização estratégica de inteligência artificial para acelerar a construção de soluções complexas.

## Principais funcionalidades

### Gestão completa do ciclo operacional

O sistema acompanha integralmente o fluxo de execução dos contratos:

```
Plano Financeiro
        ↓
Nota Fiscal
        ↓
Checklist de Conformidade
        ↓
Atestado ou Ofício
        ↓
Pagamento
```

Cada documento permanece vinculado ao mesmo processo durante todo o ciclo de vida.

### Validação automática de conformidade

A Nota Fiscal é validada automaticamente contra o Plano Financeiro correspondente.

São verificadas, entre outras informações:

- Secretaria
- Agência
- Campanha
- Empenho
- Fornecedor
- Valores
- Aplicação
- Identificadores do processo

Divergências impedem a aprovação da análise.

### Controle de SLA

O sistema acompanha automaticamente o prazo entre o recebimento da Nota Fiscal e a assinatura do Atestado.

Os indicadores são separados entre:

- Secretaria de Comunicação
- Demais Secretarias

Cada grupo possui metas próprias de atendimento e classificação automática por faixas de SLA.

### Gestão automática de vencimentos

Datas de vencimento são recalculadas automaticamente considerando:

- finais de semana;
- feriados cadastrados;
- regras específicas do processo.

### Emissão de documentos institucionais

O sistema gera automaticamente:

- Atestados;
- Ofícios.

Todos os documentos são produzidos em PDF utilizando modelos institucionais parametrizados pelos dados do processo.

### Importação de planilhas

Planilhas Excel podem ser importadas para o sistema.

As mesmas regras utilizadas na entrada manual são aplicadas durante a importação, evitando tratamentos diferentes entre processos automatizados e operações realizadas pelo usuário.

### Dashboards operacionais

O sistema fornece indicadores para acompanhamento de:

- produtividade;
- situação operacional;
- pagamentos;
- SLA;
- distribuição financeira;
- pendências.

Todos os gráficos e indicadores são integrados por filtros cruzados.

## Decisões de engenharia

### Auditoria como requisito estrutural

A auditoria foi concebida como parte da arquitetura do sistema.

Todos os registros financeiros possuem histórico completo de alterações, preservando rastreabilidade e trilha de auditoria.

### Alterações críticas controladas

Campos sensíveis não podem ser alterados diretamente.

Qualquer modificação gera uma Solicitação de Alteração que percorre um fluxo próprio de aprovação, registrando:

- solicitante;
- aprovador;
- justificativa;
- histórico completo da operação.

### Regras de negócio centralizadas

As regras responsáveis por cálculos, validações e classificações permanecem isoladas da interface.

Isso permite reutilização entre:

- telas;
- importação de planilhas;
- geração de documentos;
- dashboards;
- validações futuras.

### Design System próprio

Toda a interface foi construída sobre um Design System baseado em Design Tokens (CSS Custom Properties).

Essa abordagem garante consistência visual entre todos os módulos sem dependência de frameworks CSS de terceiros.

### Infraestrutura única

O mesmo artefato Docker é utilizado nos ambientes de:

- desenvolvimento;
- homologação;
- produção.

As diferenças entre ambientes são tratadas exclusivamente por configuração.

## Arquitetura

```
                              Usuário
                                │
                         Navegador (HTTPS)
                                │
                                ▼
                        ┌───────────────┐
                        │     Nginx     │
                        └───────┬───────┘
                                │
                                ▼
                    ┌────────────────────────┐
                    │ Django + Gunicorn      │
                    └──────┬───────────┬─────┘
                           │           │
                           ▼           ▼
                     PostgreSQL      Redis
```

A infraestrutura utiliza Docker Compose para orquestração dos serviços, comunicação por rede interna entre containers e renovação automática de certificados TLS através do Let's Encrypt.

## Stack tecnológica

- Python
- Django
- PostgreSQL
- Redis
- Gunicorn
- Nginx
- Docker
- Docker Compose
- Let's Encrypt
- HTML
- CSS (Design System próprio)

## O que este projeto demonstra

Além da implementação técnica, este projeto procura demonstrar uma abordagem para desenvolvimento de software orientada por domínio, onde a tecnologia atua como meio para resolver problemas de negócio.

O foco esteve na compreensão do processo, na modelagem das regras operacionais e na construção de uma solução capaz de transformar um fluxo administrativo complexo em um sistema consistente, auditável e preparado para evolução contínua.

## Imagens

As telas apresentadas utilizam exclusivamente dados fictícios. Nenhuma informação institucional ou processo administrativo real é exibido neste repositório.

![Login](screenshots/01_login.png)

![Dashboard](screenshots/02_dashboard.png)

![Notas Fiscais — Painel de SLA e contagem regressiva](screenshots/03_notas_fiscais_sla.png)

![Detalhe da Nota Fiscal — histórico de auditoria](screenshots/04_nota_fiscal_detalhe.png)

![Planos Financeiros](screenshots/05_planos_financeiros.png)

![Atestados e Ofícios](screenshots/07_atestados_consultar.png)
