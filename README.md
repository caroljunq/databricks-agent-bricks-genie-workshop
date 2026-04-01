# Databricks Agent Bricks & Genie Workshop

Este repositório contém o material prático para o workshop focado em **GenAI** e **Processamento de Linguagem Natural** utilizando o ecossistema Databricks. O objetivo principal é demonstrar como combinar o **Databricks Genie** e o **Agent Bricks** para criar uma interface unificada que consulta dados estruturados e não estruturados.

O projeto ilustra uma solução de "conversar com seus dados", permitindo que usuários façam perguntas tanto sobre tabelas SQL (propriedades químicas) quanto sobre documentos técnicos (manuais e normas).

---

## 🚀 Tecnologias Utilizadas

* **Databricks Genie:** BI conversacional para dados estruturados via linguagem natural.
* **AgentBricks:** Framework para orquestração de agentes inteligentes e leitura de dados não estruturados.
* **Unity Catalog:** Governança centralizada e gerenciamento de ativos de dados.

---

## 📂 Conteúdo dos Notebooks

A estrutura do workshop segue uma ordem lógica de implementação:

1.  **`passo-a-passo.ipynb`**
    * Passo a passo do workshop
    * Instruções de como seguir dentro da console da Databricks

2.  **`dataset-generation-contratos.ipynb`**
    * Execute somente se quiser gerar os dataset CSVs novamente

3.  **`pdf-generation.ipynb`**
    * Execute somente se quiser gerar os dataset PDFs

---

## 📊 Dataset: `quimica_dataset`

Este documento descreve as colunas de cada arquivo CSV presente no dataset, incluindo tipo de dado e descrição semântica.

---

### 1. `fornecedores.csv`

Cadastro de fornecedores da empresa química, com informações de identificação, contato e avaliação.

| Coluna | Tipo | Descrição |
|---|---|---|
| `id_fornecedor` | STRING | Identificador único do fornecedor. Formato: `FORN-XXXX`. Chave primária da tabela. |
| `razao_social` | STRING | Razão social da empresa fornecedora. |
| `cnpj` | STRING | CNPJ do fornecedor no formato `XX.XXX.XXX/XXXX-XX`. |
| `categoria` | STRING | Categoria de serviços ou produtos oferecidos (ex: *Equipamentos e Peças*, *Serviços de Engenharia*, *Produtos Químicos e Catalisadores*). |
| `cidade` | STRING | Cidade onde o fornecedor está localizado. |
| `estado` | STRING | Sigla do estado (UF) do fornecedor. |
| `telefone` | STRING | Telefone de contato no formato `(XX) XXXXX-XXXX`. |
| `email` | STRING | Endereço de e-mail de contato do fornecedor. |
| `status_cadastro` | STRING | Status do cadastro do fornecedor. Valores possíveis: `Ativo`, `Inativo`, `Bloqueado`. |
| `data_cadastro` | DATE | Data de cadastro do fornecedor no sistema. Formato: `YYYY-MM-DD`. |
| `avaliacao_desempenho` | FLOAT | Nota de avaliação de desempenho do fornecedor em escala de 0 a 10 (ex: `9.3`). |

---

### 2. `contratos.csv`

Contratos firmados com fornecedores, descrevendo escopo, valores, vigência e documentação associada.

| Coluna | Tipo | Descrição |
|---|---|---|
| `id_contrato` | STRING | Identificador único do contrato. Formato: `CTR-XXXXX`. Chave primária da tabela. |
| `numero_contrato` | STRING | Número formal do contrato. Formato: `CT-AAAA-XXXXX`. |
| `id_fornecedor` | STRING | Referência ao fornecedor contratado. Chave estrangeira para `fornecedores.id_fornecedor`. |
| `tipo_contrato` | STRING | Modalidade ou tipo do contrato (ex: *Serviços de Construção e Montagem*, *Contratação de Serviços de Inspeção*). |
| `descricao_escopo` | STRING | Descrição detalhada do escopo de trabalho contratado. |
| `valor_total` | FLOAT | Valor total do contrato em moeda corrente. |
| `moeda` | STRING | Código da moeda do contrato (ex: `BRL`). |
| `data_inicio` | DATE | Data de início da vigência do contrato. Formato: `YYYY-MM-DD`. |
| `data_fim` | DATE | Data de término da vigência do contrato. Formato: `YYYY-MM-DD`. |
| `status` | STRING | Status atual do contrato. Valores possíveis: `Ativo`, `Encerrado`, entre outros. |
| `unidade_industrial` | STRING | Unidade industrial responsável pelo contrato (ex: *Polo Petroquímico de Santa Grande*). |
| `area_responsavel` | STRING | Área interna da empresa responsável pela gestão do contrato (ex: *Engenharia de Processos*, *Planejamento e Controle de Manutenção (PCM)*). |
| `documento_contrato` | STRING | Nome do arquivo PDF do documento oficial do contrato. |
| `procedimento_referencia` | STRING | Nome do arquivo PDF do procedimento técnico ou de segurança de referência associado ao contrato. |

---

### 3. `ordens_servico.csv`

Ordens de serviço emitidas no âmbito dos contratos, registrando execução, valores e prazos.

| Coluna | Tipo | Descrição |
|---|---|---|
| `id_ordem` | STRING | Identificador único da ordem de serviço. Formato: `OS-XXXXX`. Chave primária da tabela. |
| `numero_ordem` | STRING | Número formal da ordem de serviço. Formato: `OS-AAAA-XXXXX`. |
| `id_contrato` | STRING | Referência ao contrato vinculado. Chave estrangeira para `contratos.id_contrato`. |
| `id_fornecedor` | STRING | Referência ao fornecedor executor. Chave estrangeira para `fornecedores.id_fornecedor`. |
| `tipo_ordem` | STRING | Tipo da ordem de serviço. Valores possíveis: `Fornecimento`, `Emergencial`, `Serviço`, entre outros. |
| `descricao` | STRING | Descrição do serviço ou fornecimento solicitado. |
| `valor_ordem` | FLOAT | Valor financeiro desta ordem de serviço. |
| `data_emissao` | DATE | Data de emissão da ordem de serviço. Formato: `YYYY-MM-DD`. |
| `data_prevista_conclusao` | DATE | Data prevista para conclusão da ordem. Formato: `YYYY-MM-DD`. |
| `data_conclusao` | DATE | Data efetiva de conclusão da ordem. Formato: `YYYY-MM-DD`. Pode estar vazia caso a ordem ainda esteja em andamento. |
| `status` | STRING | Status atual da ordem. Valores possíveis: `Em Andamento`, `Concluída`, entre outros. |
| `prioridade` | STRING | Nível de prioridade da ordem. Valores possíveis: `Crítica`, `Alta`, `Média`, `Baixa`. |
| `unidade_industrial` | STRING | Unidade industrial onde o serviço será ou foi executado. |
| `area_solicitante` | STRING | Área interna que solicitou a ordem de serviço (ex: *Manutenção Industrial*, *Automação e Instrumentação*). |

---

### Relacionamentos entre tabelas

```
fornecedores (id_fornecedor)
    │
    ├──< contratos (id_fornecedor)
    │         │
    │         └──< ordens_servico (id_contrato)
    │
    └──< ordens_servico (id_fornecedor)
```


## **Dados Não Estruturados (Documentos)**

 ### PDFs de contratos                                        
* Um arquivo por contrato, padrão CONTRATO_CT-AAAA-XXXXX.pdf                  
* Contém o instrumento jurídico-comercial completo                            
                                                                                
  ### PDFs de procedimentos)                               
  * Documentos normativos internos reutilizáveis entre contratos                
  * Organizados por 6 prefixos de área: SMS (Segurança), QLD (Qualidade), ENG   
  (Engenharia), AMB (Ambiental), MNT (Manutenção), LOG (Logística)            
    

---

## 🛠️ Como utilizar

1.  **Importação:** Clone este repositório no seu Workspace Databricks utilizando a funcionalidade de *Repos*.
2.  **Cluster:** Utilize um cluster com **Runtime 14.3 LTS ML** ou superior.
3.  **Execução:** Execute o notebook **passo-a-passo.ipynb**
4.  **Permissões:** Certifique-se de que o seu usuário tem permissões para criar tabelas no Unity Catalog.

---

**Mantenedora:** [caroljunq](https://github.com/caroljunq)
