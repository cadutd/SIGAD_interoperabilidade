# Manual de Interoperabilidade de Documentos Arquivísticos

## Sumário
- [Manual de Interoperabilidade de Documentos Arquivísticos](#manual-de-interoperabilidade-de-documentos-arquivísticos)
  - [Sumário](#sumário)
  - [1) Introdução](#1-introdução)
  - [2) Visão geral do padrão](#2-visão-geral-do-padrão)
    - [2.1 Objetivo](#21-objetivo)
    - [2.2 Estrutura de referências ($ref)](#22-estrutura-de-referências-ref)
  - [3) Regras e convenções de preenchimento](#3-regras-e-convenções-de-preenchimento)
    - [3.1 Tipos e formatos](#31-tipos-e-formatos)
    - [3.2 Campos obrigatórios](#32-campos-obrigatórios)
    - [3.3 “Eventos” como trilha de auditoria](#33-eventos-como-trilha-de-auditoria)
  - [4) Dicionário de dados por schema](#4-dicionário-de-dados-por-schema)
    - [4.1 `documento_schema.json` — Documento](#41-documento_schemajson--documento)
    - [4.2 `componente_schema.json` — Componente](#42-componente_schemajson--componente)
    - [4.3 `processo_dossie_schema.json` — Processo / Dossiê](#43-processo_dossie_schemajson--processo--dossiê)
    - [4.4 `volume_schema.json` — Volume](#44-volume_schemajson--volume)
    - [4.5 `agente_schema.json` — Agente](#45-agente_schemajson--agente)
    - [4.6 `evento_schema.json` — Evento](#46-evento_schemajson--evento)
    - [4.7 `assinatura_schema.json` — Assinatura](#47-assinatura_schemajson--assinatura)
    - [4.8 `fixidade_schema.json` — Fixidade](#48-fixidade_schemajson--fixidade)
    - [4.9 `dependencia_schema.json` — Dependência](#49-dependencia_schemajson--dependência)
    - [4.10 `hardware_schema.json` — Hardware](#410-hardware_schemajson--hardware)
    - [4.11 `software_schema.json` — Software](#411-software_schemajson--software)
    - [4.12 `inibidor_schema.json` — Inibidor](#412-inibidor_schemajson--inibidor)
    - [4.13 `formato_schema.json` — Formato](#413-formato_schemajson--formato)
    - [4.14 `localizacao_schema.json` — Localização](#414-localizacao_schemajson--localização)
    - [4.15 `relacionamento_schema.json` — Relacionamento](#415-relacionamento_schemajson--relacionamento)
  - [5) Exemplo mínimo de uso (modelo mental)](#5-exemplo-mínimo-de-uso-modelo-mental)
    - [5.1 Documento com 1 componente e 2 eventos (exemplo conceitual)](#51-documento-com-1-componente-e-2-eventos-exemplo-conceitual)
  - [6) Checklist de implementação (para quem vai produzir/consumir JSON)](#6-checklist-de-implementação-para-quem-vai-produzirconsumir-json)
- [7) Padrão de Armazenamento em Sistemas de Arquivos](#7-padrão-de-armazenamento-em-sistemas-de-arquivos)
  - [7.1 Objetivo](#71-objetivo)
  - [7.2 Princípios Gerais](#72-princípios-gerais)
    - [7.2.1 Hierarquia Arquivística](#721-hierarquia-arquivística)
    - [7.2.2 Identificador como Elemento Estruturante](#722-identificador-como-elemento-estruturante)
    - [7.2.3 Separação entre Metadados e Conteúdo Binário](#723-separação-entre-metadados-e-conteúdo-binário)
    - [7.2.4 Imutabilidade Lógica](#724-imutabilidade-lógica)
  - [7.3 Estrutura de Diretórios Recomendada](#73-estrutura-de-diretórios-recomendada)
  - [7.3.1 Estrutura Base](#731-estrutura-base)
    - [a) Documento (DA)](#a-documento-da)
    - [b) Processo (PR)](#b-processo-pr)
    - [c) Dossiê (DS)](#c-dossiê-ds)
  - [7.4 Convenções de Nomeação](#74-convenções-de-nomeação)
    - [7.4.1 Nomeação de Diretórios Raiz](#741-nomeação-de-diretórios-raiz)
    - [7.4.2 Nomeação de Arquivos de Metadados](#742-nomeação-de-arquivos-de-metadados)
    - [7.4.3 Nomeação de Componentes Digitais](#743-nomeação-de-componentes-digitais)
    - [7.4.4 Sensibilidade a Maiúsculas e Minúsculas](#744-sensibilidade-a-maiúsculas-e-minúsculas)
    - [7.4.5 Proibições](#745-proibições)
    - [7.4.6 Compatibilidade com Preservação Digital](#746-compatibilidade-com-preservação-digital)
  - [7.5 Empacotamento para Trâmite](#75-empacotamento-para-trâmite)
    - [7.5.1 Objetivo](#751-objetivo)
    - [7.5.2 Padrão BagIt](#752-padrão-bagit)
      - [Estrutura Geral do BagIt](#estrutura-geral-do-bagit)
      - [Arquivos Obrigatórios](#arquivos-obrigatórios)
      - [Arquivos Opcionais](#arquivos-opcionais)
    - [7.5.3 Estrutura do Pacote de Trâmite](#753-estrutura-do-pacote-de-trâmite)
    - [7.5.4 Estrutura Interna do Bag](#754-estrutura-interna-do-bag)
      - [Regras de Composição](#regras-de-composição)
    - [7.5.5 Integridade e Validação](#755-integridade-e-validação)
    - [7.5.6 Considerações de Preservação](#756-considerações-de-preservação)
  - [8 Empacotamento para Envio a RDC-Arq (SIP)](#8-empacotamento-para-envio-a-rdc-arq-sip)
    - [8.1 Objetivo](#81-objetivo)
    - [8.2 Modelo OAIS e Pacotes de Informação](#82-modelo-oais-e-pacotes-de-informação)
    - [8.3 Padrão de Empacotamento](#83-padrão-de-empacotamento)
    - [8.4 Nomeação do Pacote SIP](#84-nomeação-do-pacote-sip)
    - [8.5 Estrutura Interna do SIP](#85-estrutura-interna-do-sip)
    - [8.6 Ficha Espelho de Transferência](#86-ficha-espelho-de-transferência)
    - [8.7 Composição do Conteúdo do SIP](#87-composição-do-conteúdo-do-sip)
    - [8.8 Validação do SIP](#88-validação-do-sip)
    - [8.9 Pacote de Informação de Representação da Guia](#89-pacote-de-informação-de-representação-da-guia)
    - [8.9.1 Padrão de Empacotamento](#891-padrão-de-empacotamento)
    - [8.9.2 Nomeação do Pacote](#892-nomeação-do-pacote)
    - [8.9.3 Estrutura do Pacote](#893-estrutura-do-pacote)
    - [8.9.4 Conteúdo do Pacote](#894-conteúdo-do-pacote)
    - [8.9.5 Relação com os SIPs](#895-relação-com-os-sips)
  - [9. Considerações Finais](#9-considerações-finais)

---

## 1) Introdução

A transformação digital da administração pública e das organizações privadas ampliou de forma significativa a produção, tramitação, armazenamento e preservação de documentos em meio digital. Processos administrativos eletrônicos, sistemas corporativos, plataformas colaborativas e aplicações especializadas passaram a registrar atividades, decisões e evidências institucionais em formatos digitais, convertendo esses registros em parte essencial do patrimônio arquivístico contemporâneo. Nesse contexto, garantir a autenticidade, a integridade, a confiabilidade, a acessibilidade e a preservação de longo prazo desses documentos tornou-se um desafio estratégico para a gestão documental e para a governança da informação.

Embora existam iniciativas consolidadas no Brasil voltadas à gestão e preservação de documentos arquivísticos digitais — como o **e-ARQ Brasil**, a **NOBRADE**, as diretrizes para **RDC-Arq** e outros referenciais normativos — ainda persistem dificuldades práticas relacionadas à interoperabilidade entre sistemas produtores, tramitadores, custodiais e preservadores de documentos digitais. Em muitos cenários, diferentes soluções tecnológicas utilizam estruturas de dados incompatíveis, metadados heterogêneos e modelos próprios de empacotamento, o que dificulta transferências, recolhimentos, migrações tecnológicas e ações coordenadas de preservação digital.

Este **Manual de Interoperabilidade de Documentos Arquivísticos Digitais** surge como resposta a esse cenário. Seu propósito é oferecer um modelo técnico comum para representação, troca, armazenamento e encaminhamento de documentos arquivísticos digitais ao longo de todo o seu ciclo de vida. Para isso, o documento estabelece esquemas estruturados em **JSON Schema**, define metadados essenciais, organiza relações entre documentos, componentes digitais, eventos e agentes, e propõe convenções para armazenamento em sistema de arquivos e empacotamento com base em padrões amplamente reconhecidos, como o **BagIt** e o modelo **OAIS**.

A proposta adota uma visão arquivística integrada, na qual o documento digital é compreendido não apenas como um arquivo de computador, mas como uma unidade documental contextualizada por metadados, vínculos orgânicos, histórico de eventos, assinaturas, controles de acesso e requisitos de preservação. Dessa forma, busca-se assegurar que diferentes sistemas possam produzir, interpretar, transferir e custodiar documentos digitais sem perda de significado, contexto ou valor probatório.

Mais do que uma especificação técnica, este manual pretende servir como instrumento de cooperação institucional. Ao favorecer padrões abertos e compartilhados, contribui para a construção de ecossistemas interoperáveis entre órgãos públicos, arquivos permanentes, plataformas de processo eletrônico, sistemas de negócio e repositórios digitais confiáveis. Seu uso pode apoiar tanto iniciativas locais quanto políticas nacionais voltadas à modernização administrativa, à transparência pública, à memória institucional e à preservação do patrimônio documental digital brasileiro.

## 2) Visão geral do padrão

### 2.1 Objetivo
Os schemas descrevem uma estrutura mínima para representar documentos e processos com metadados de gestão (e-ARQ) e preservação (conceitos alinhados ao PREMIS), especialmente:

- **Documento** como entidade intelectual (metadados descritivos e de contexto).
- **Componente** como entidade técnica/material (arquivo digital, mídia, item físico, etc.).
- **Evento** como trilha de ações ocorridas no ciclo de vida (captura, tramitação, assinatura, verificação de fixidez, migração etc.).
- **Agente** como responsável por eventos e papéis (pessoa, organização, software, hardware).

### 2.2 Estrutura de referências ($ref)
Os schemas se relacionam por **`$ref`** (referência por arquivo). Exemplos:

- `evento_schema.json` referencia `agente_schema.json` em `EarqEventoAgente`.
- `componente_schema.json` referencia `software_schema.json`, `hardware_schema.json`, `formato_schema.json`, etc.
- `documento_schema.json` referencia `componente_schema.json` e `evento_schema.json`.

**Recomendação prática:** mantenha todos os `.json` de schema no mesmo diretório para facilitar a resolução relativa dos `$ref`.

---

## 3) Regras e convenções de preenchimento

### 3.1 Tipos e formatos
- `format: date` → data no padrão ISO (ex.: `2026-01-15`).
- `format: date-time` → data/hora ISO 8601 (ex.: `2026-01-15T10:30:00-03:00`).

> Observação: a aplicação dessas convenções aparece, por exemplo, em `evento_schema.json`.

### 3.2 Campos obrigatórios
Cada schema define uma lista **`required`**. Em interoperabilidade, isso significa:

- O produtor deve garantir a presença dos campos obrigatórios.
- O consumidor pode rejeitar objetos inválidos (ou registrar inconsistência).

### 3.3 “Eventos” como trilha de auditoria
Tanto **Documento** quanto **Componente** possuem `earqEventos` (lista de **Evento**).

Isso permite modelar:

- cadeia de preservação
- histórico de validações (fixidez, assinatura),
- ações de preservação (migração, normalização),
- tramitações e atos processuais.

---

## 4) Dicionário de dados por schema
Abaixo, para cada schema: **finalidade, campos, tipo, obrigatoriedade, valores controlados e observações**.

---

### 4.1 `documento_schema.json` — Documento

**Finalidade:** representar o documento arquivístico como entidade intelectual e orgânica, reunindo metadados de identificação, contexto, classificação, destinação, acesso, autoria, relacionamento, componentes e eventos associados ao longo do ciclo de vida.

| Campo | Tipo | Obrigatório | Valores/Regra | Observações |
|---|---|---:|---|---|
| `earqDocumentoId` | `string` | Sim |  | Identificador único do documento. Recomenda-se UUID, URI ou código interno estável. |
| `DcTitle` | `string` | Sim |  | Título do documento. Corresponde ao elemento Dublin Core utilizado para identificação e recuperação. |
| `DcDescription` | `string` | Não |  | Descrição resumida do conteúdo, assunto ou contexto do documento. |
| `DcSubject[]` | `array(string)` | Não |  | Lista de assuntos, descritores ou palavras-chave associados ao documento. |
| `dc.relation[]` | `array(object)` | Não | Pode conter `earqDocumentoId`, `earqProcessoId`, `DcTitle` | Relações com outros documentos ou processos. Permite registrar vínculos intelectuais, processuais ou referenciais. |
| `dc.date.issued` | `string` | Sim | `format: date` | Data de emissão, produção ou expedição do documento,  ou seja a data em que o documento é finalizado, assinado e passa a ter efeito no padrão ISO 8601 (`AAAA-MM-DD`). |
| `earqDocumentoNumero` | `string` | Não |  | Número formal do documento, quando existente (ex.: ofício, portaria, memorando). |
| `earqDocumentoProtocolo` | `string` | Não |  | Número de protocolo associado ao documento. |
| `earqProcessoDossieId` | `string` | Não |  | Identificador do processo ou dossiê ao qual o documento pertence. |
| `earqVolumeId` | `string` | Não |  | Identificador do volume em que o documento está inserido. |
| `earqDocumentoMeio` | `string` | Sim | `digital`, `não digital`, `híbrido` | Indica a natureza material do documento: exclusivamente digital, exclusivamente não digital ou híbrido. |
| `earqDocumentoStatus` | `string` | Sim | `minuta`, `original`, `cópia`, `versão autenticada`, `conferida`, `assinada digitalmente` | Estado diplomático ou situação documental da peça. |
| `earqDocumentoVersao` | `integer` | Sim | mínimo `1` | Número da versão do documento. Utilizado para controle evolutivo de conteúdo. |
| `earqDocumentoAutor[]` | `array(Agente)` | Sim | mínimo `1` item | Lista de autores do documento. Cada item referencia `agente_schema.json`. |
| `earqDocumentoDestinatario[]` | `array(Agente)` | Sim |  | Lista de destinatários do documento. Utilizado para documentos expedidos ou comunicacionais. |
| `earqDocumentoInteressado[]` | `array(Agente)` | Não |  | Lista de pessoas ou entidades interessadas no conteúdo ou efeitos do documento. |
| `earqDocumentoOriginador` | `Agente` | Não | `$ref` | Agente originador do documento. Pode representar unidade produtora, sistema de origem ou pessoa responsável pela emissão. |
| `earqDocumentoRedator` | `Agente` | Sim | `$ref` | Agente redator do documento. Representa quem redigiu ou materializou o conteúdo documental. |
| `earqDocumentoGenero` | `string` | Não | `TEXTUAL`, `CARTOGRAFICO`, `ICONOGRAFICO`, `SONORO`, `AUDIOVISUAL`, `TRIDIMENSIONAL` | Gênero documental segundo características de linguagem e suporte informacional. |
| `earqDocumentoEspecie` | `string` | Não |  | Espécie documental (ex.: ofício, requerimento, relatório, parecer). |
| `earqDocumentoTipo` | `string` | Não |  | Tipo documental derivado da atividade que originou o documento. |
| `dc.language` | `string` | Não | regex `^[a-z]{3}$` | Idioma principal do documento em código ISO 639-2 de três letras. |
| `earqDocumentoQtFolha` | `integer` | Não | mínimo `1` | Quantidade de folhas/páginas associadas ao documento, quando aplicável. |
| `earqDocumentoAnexo` | `boolean` | Não | `true` / `false` | Indica se o documento possui algum anexo vinculado a ele. |
| `earqClasseId` | `object` | Sim | obrigatório `codigo` | Classe de classificação arquivística atribuída ao documento. Pode conter `codigo`, `descricao` e `planoDeClassificacaoId`. |
| `earqDocumentoDestinacao` | `string` | Não | `transferência`, `eliminação`, `recolhimento` | Destinação prevista conforme tabela de temporalidade. |
| `earqDocumentoPrazoDeGuarda` | `string` | Não |  | Indicação do prazo estabelecido em tabela de temporalidade e destinação. Deve ser preenchido com a quantidade de meses de guarda. |
| `earqDocumentoLocalizacao` | `object` | Não | pode conter `enderecoFisico`, `siglaUnidade`, `observacao` | Localização física ou administrativa do documento quando aplicável, especialmente para documentos não digitais ou híbridos. |
| `earqNivelDeAcessoId` | `string` | Sim | valor padrão `ostensivo` | Nível de acesso do documento. Define restrições de consulta conforme norma aplicável. |
| `earqDocumentoIndicacaoAnotacao` | `boolean` | Sim | `true` / `false` | Indica a existência de anotações, despachos, vistos ou marcas de tramitação no documento. |
| `earqDocumentoSetorExecucao` | `string` | Não |  | Unidade administrativa ou setor responsável pela execução relacionada ao documento. |
| `earqComponentes[]` | `array(Componente)` | Não | `$ref` | Lista de componentes documentais. Representa arquivos digitais, anexos, mídias ou partes técnicas do documento. |
| `earqEventos[]` | `array(Evento)` | Não | `$ref` | Lista de eventos associados ao documento ao longo de seu ciclo de vida (gestão, preservação, acesso, tramitação etc.). |

### 4.2 `componente_schema.json` — Componente

**Finalidade:** representar a entidade técnica vinculada ao documento arquivístico. O componente corresponde ao arquivo digital, objeto físico, mídia ou parte material que contém a informação registrada. Reúne metadados técnicos, dependências tecnológicas, fixidez, assinaturas, localização e eventos associados ao longo do ciclo de vida.

| Campo | Tipo | Obrigatório | Valores/Regra | Observações |
|---|---|---:|---|---|
| `earqComponenteId` | `string` | Sim |  | Identificador único do componente. Recomenda-se UUID, URI ou código interno persistente. |
| `earqComponenteNomeOriginal` | `string` | Sim |  | Nome original do arquivo, objeto ou item no momento de sua captura ou incorporação ao sistema. |
| `earqComponenteTamanho` | `integer` | Sim | mínimo `0` | Tamanho numérico do componente. Deve ser interpretado em conjunto com `earqComponenteUnidadeMedidaTamanho`. |
| `earqComponenteUnidadeMedidaTamanho` | `string` | Sim | `B`, `KB`, `MB`, `GB`, `TB` | Unidade de medida do tamanho informado. Recomenda-se uso de bytes (`B`) para maior precisão técnica. |
| `earqComponenteSoftwareCriacao` | `Software` | Não | `$ref` | Software utilizado na criação ou geração do componente. Referencia `software_schema.json`. |
| `earqComponenteDataCriacao` | `string` | Sim | `format: date-time` | Data e hora de criação do componente no padrão ISO 8601. |
| `earqComponenteNivelComposicao` | `integer` | Não | `0`, `1` | Indica se o componente está submetido a compressão, empacotamento, criptografia ou encapsulamento. Valor `0` indica ausência; valor `1` indica presença de composição. |
| `earqComponenteNumeroOrdem` | `integer` | Não | mínimo `1` | Ordem de apresentação do componente dentro do documento quando houver múltiplos componentes. |
| `earqComponenteInibidor` | `Inibidor` | Não | `$ref` | Inibidor associado ao componente, como restrição técnica, criptografia, DRM, bloqueio ou mecanismo impeditivo. Referencia `inibidor_schema.json`. |
| `earqComponenteFormato` | `Formato` | Não | `$ref` | Formato técnico do componente (ex.: PDF/A, TIFF, WAV, XML). Metadado altamente recomendável para preservação e acesso. Referencia `formato_schema.json`. |
| `earqComponenteLocalizacao` | `Localizacao` | Sim | `$ref` | Localização lógica, física ou URI onde o componente pode ser encontrado. Referencia `localizacao_schema.json`. |
| `earqComponenteSuporte` | `string` | Não | `FITA_MAGNETICA`, `HD`, `SSD`, `CARTAO_MEMORIA`, `DISCO_OPTICO`, `NAOIDENTIFICADO` | Tipo de suporte físico de armazenamento associado ao componente. |
| `earqComponenteSoftwareLeitura` | `Software` | Não | `$ref` | Software necessário ou recomendado para leitura, renderização ou interpretação do componente. Referencia `software_schema.json`. |
| `earqComponenteHardware` | `Hardware` | Não | `$ref` | Hardware necessário ou relacionado ao acesso, captura ou leitura do componente. Referencia `hardware_schema.json`. |
| `earqComponenteOutrasDependencias[]` | `array(Dependencia)` | Não | `$ref` | Lista de dependências adicionais necessárias ao uso do componente, como bibliotecas, codecs, esquemas externos ou serviços correlatos. |
| `earqComponenteRelacionamentos[]` | `array(Relacionamento)` | Não | `$ref` | Relações com outros componentes, como derivação, dependência, versão correlata, representação alternativa ou vínculo estrutural. |
| `earqComponenteFixidade` | `Fixidade` | Sim | `$ref` | Informação de fixidez utilizada para verificação de integridade do componente, normalmente hash criptográfico. Referencia `fixidade_schema.json`. |
| `earqComponenteAssinaturas[]` | `array(Assinatura)` | Não | `$ref` | Lista de assinaturas associadas ao componente, incluindo assinaturas digitais e outras formas de autenticação registradas. |
| `earqEventos[]` | `array(Evento)` | Não | `$ref` | Lista de eventos associados ao componente ao longo de seu ciclo de vida, como captura, migração, verificação de fixidez, acesso, assinatura e preservação. |


### 4.3 `processo_dossie_schema.json` — Processo / Dossiê

**Finalidade:** representar o processo ou dossiê como unidade de arquivamento que agrega documentos, volumes, metadados de contexto, classificação, destinação, acesso e eventos associados ao longo de seu ciclo de vida.

| Campo | Tipo | Obrigatório | Valores/Regra | Observações |
|---|---|---:|---|---|
| `earqProcessoId` | `string` | Sim |  | Identificador único do processo ou dossiê. Recomenda-se UUID, URI ou código interno persistente. |
| `dc.relation[]` | `array(object)` | Não | Pode conter `earqProcessoId`, `earqDocumentoId`, `DcTitle` | Relações com outros processos, dossiês ou documentos. Permite registrar vínculos processuais, documentais ou referenciais. |
| `dc.date.issued` | `string` | Sim | `format: date` | Data de emissão, autuação, abertura ou produção do processo/dossiê no padrão ISO 8601 (`AAAA-MM-DD`). |
| `earqProcessoNumero` | `string` | Não |  | Número formal do processo ou dossiê, quando existente. |
| `earqProcessoProtocolo` | `string` | Não |  | Número de protocolo associado ao processo ou dossiê. |
| `earqProcessoDossieId` | `string` | Não |  | Identificador correlato do processo ou dossiê, útil para relacionamento com outros sistemas ou estruturas de negócio. |
| `earqProcessoMeio` | `string` | Sim | `digital`, `não digital`, `híbrido` | Indica a natureza material do processo ou dossiê: exclusivamente digital, exclusivamente não digital ou híbrido. |
| `earqProcessoStatus` | `string` | Sim | `tramitando`, `sobrestado`, `arquivado` | Estado do processo ou dossiê no fluxo de trabalho ou no ciclo de vida arquivístico. |
| `earqProcessoAutor[]` | `array(Agente)` | Sim | mínimo `1` item | Lista de autores do processo. Cada item referencia `agente_schema.json`. |
| `earqProcessoDestinatario[]` | `array(Agente)` | Não |  | Lista de destinatários do processo ou dossiê, quando aplicável. Cada item referencia `agente_schema.json` |
| `earqProcessoInteressado[]` | `array(Agente)` | Não |  | Lista de interessados no processo ou dossiê. Cada item referencia `agente_schema.json`|
| `earqProcessoOriginador` | `Agente` | Não | `$ref` | Agente originador do processo ou dossiê. Pode representar unidade produtora, sistema de origem ou pessoa responsável pela abertura. Referencia `agente_schema.json` |
| `earqProcessoRedator` | `Agente` | Não | `$ref` | Agente redator vinculado à elaboração ou formalização do processo ou dossiê. Referencia `agente_schema.json`|
| `earqProcessoEspecie` | `string` | Não |  | Espécie documental do processo ou dossiê. |
| `earqProcessoTipo` | `string` | Não |  | Tipo documental derivado da atividade que originou o processo ou dossiê. |
| `dc.language` | `string` | Não | regex `^[a-z]{3}$` | Idioma principal do processo ou dossiê em código ISO 639-2 de três letras. |
| `earqProcessoQtFolha` | `integer` | Não | mínimo `1` | Quantidade de folhas associadas ao processo ou dossiê, quando aplicável. |
| `earqClasseId` | `object` | Sim | obrigatório `codigo` | Classe de classificação arquivística atribuída ao processo ou dossiê. Pode conter `codigo`, `descricao` e `planoDeClassificacaoId`. |
| `earqProcessoDestinacao` | `string` | Não | `transferência`, `eliminação`, `recolhimento` | Destinação prevista conforme tabela de temporalidade. |
| `earqProcessoPrazoDeGuarda` | `string` | Não |  | Indicação do prazo estabelecido em tabela de temporalidade e destinação de documentos para o cumprimento da destinação. Deve ser preenchido com a quantidade de meses que o processo deve ser guardado. |
| `earqProcessoLocalizacao` | `object` | Não | pode conter `enderecoFisico`, `siglaUnidade`, `observacao` | Localização física ou administrativa do processo ou dossiê, especialmente relevante para documentos não digitais ou híbridos. |
| `earqNivelDeAcessoId` | `string` | Sim |  | Nível de acesso do processo ou dossiê. Define restrições de consulta conforme norma aplicável. |
| `earqProcessoIndicacaoAnotacao` | `boolean` | Sim | `true` / `false` | Indica a existência de anotações, despachos, vistos ou marcas de tramitação no processo ou dossiê. |
| `earqProcessoSetorExecucao` | `string` | Não |  | Unidade administrativa ou setor responsável pela execução relacionada ao processo ou dossiê. |
| `earqVolumes[]` | `array(object)` | Não | mínimo `1` item por array, quando presente | Lista ordenada de volumes pertencentes ao processo, com numeração e ordem de apresentação. Cada item exige `ordemVolume` e `earqVolume`. |
| `earqVolumes[].ordemVolume` | `integer` | Sim, dentro do item | mínimo `1` | Ordem sequencial do volume dentro do processo ou dossiê. |
| `earqVolumes[].earqVolume` | `Volume` | Sim, dentro do item | `$ref` | Volume associado ao processo ou dossiê. Referencia `volume_schema.json`. |
| `earqEventos[]` | `array(Evento)` | Não | `$ref` | Lista de eventos associados ao processo ou dossiê ao longo de seu ciclo de vida (gestão, preservação, acesso, tramitação etc.). |

### 4.4 `volume_schema.json` — Volume

**Finalidade:** representar o volume como unidade subordinada ao processo ou dossiê, utilizada para organizar e controlar a ordenação material ou lógica de conjuntos documentais, reunindo identificação, datas, localização e a lista ordenada de documentos que o compõem.

| Campo | Tipo | Obrigatório | Valores/Regra | Observações |
|---|---|---:|---|---|
| `earqVolumeId` | `string` | Sim |  | Identificador único do volume. Recomenda-se UUID, URI ou código interno persistente. |
| `earqVolumeNumero` | `string` | Sim |  | Número ou designação do volume dentro do processo ou dossiê. |
| `earqVolumeQtFolha` | `integer` | Não | mínimo `1` | Quantidade de folhas associadas ao volume, quando aplicável. |
| `earqDataAbertura` | `string` | Sim | `format: date` | Data de abertura do volume no padrão ISO 8601 (`AAAA-MM-DD`). |
| `earqDataEncerramento` | `string` | Não | `format: date` | Data de encerramento do volume no padrão ISO 8601 (`AAAA-MM-DD`). |
| `earqVolumeLocalizacao` | `Localizacao` | Não | `$ref` | Localização lógica, física ou URI associada ao volume. Referencia `localizacao_schema.json`. |
| `earqDocumentos[]` | `array(object)` | Não | mínimo `1` item por array, quando presente; `uniqueItems: true` | Lista ordenada de documentos pertencentes ao volume, com ordem de apresentação. O `uniqueItems: true` impede repetição do objeto completo, mas não garante sozinho unicidade isolada de `earqDocumentoId` ou de `ordemApresentacao`. |
| `earqDocumentos[].ordemApresentacao` | `integer` | Sim, dentro do item | mínimo `1` | Ordem de apresentação do documento dentro do volume. |
| `earqDocumentos[].earqDocumentoId` | `string` | Sim, dentro do item |  | Identificador do documento pertencente ao volume. |

### 4.5 `agente_schema.json` — Agente

**Finalidade:** representar agentes envolvidos na produção, gestão, preservação, acesso e registro de eventos relacionados aos documentos arquivísticos digitais. O agente pode corresponder a pessoa, organização, hardware ou software.

| Campo | Tipo | Obrigatório | Valores/Regra | Observações |
|---|---|---:|---|---|
| `earqAgenteId` | `string` | Sim |  | Identificador único do agente. Ex.: UUID, URI ou código interno. |
| `earqAgenteNome` | `string` | Sim |  | Nome do agente (pessoa, organização, hardware ou software). |
| `earqFormaDoNome` | `string` | Não |  | Forma do nome. Ex.: nome completo, abreviado, nome artístico, sigla institucional ou denominação técnica. |
| `earqAgenteStatus` | `string` | Sim | `Ativo`, `Inativo` | Situação cadastral ou operacional do agente. |
| `agentType` | `string` | Sim | `Pessoa`, `Organização`, `hardware`, `software` | Tipo do agente. Metadado alinhado ao PREMIS. |
| `agentVersion` | `string` | Condicional | obrigatório quando `agentType = software` ou `hardware` | Versão do agente referenciado em `earqAgenteNome`, quando se tratar de software ou hardware. Metadado PREMIS relevante para reprodutibilidade e preservação. |
| `agentNote` | `string` | Não |  | Informação adicional para desambiguar, contextualizar ou descrever o agente. Metadado PREMIS. |
| `earqDatasDeExistencia` | `object` | Não | pode conter `inicio`, `fim` | Período de existência, vigência ou atividade do agente. |
| `earqDatasDeExistencia.inicio` | `string` | Não | `format: date` | Data inicial de existência, atuação ou vigência no padrão ISO 8601 (`AAAA-MM-DD`). |
| `earqDatasDeExistencia.fim` | `string` | Não | `format: date` | Data final de existência, atuação ou vigência no padrão ISO 8601 (`AAAA-MM-DD`). |
| `earqIdentificadoresExternos[]` | `array(object)` | Não | pode conter `sistema`, `identificador` | Identificadores adicionais em sistemas externos, como ORCID, CPF, CNPJ, Wikidata, VIAF ou outros cadastros de autoridade. |
| `earqIdentificadoresExternos[].sistema` | `string` | Não |  | Nome do sistema externo de identificação. |
| `earqIdentificadoresExternos[].identificador` | `string` | Não |  | Valor do identificador no sistema informado. |
| `earqContacto` | `object` | Não | pode conter `email`, `telefone`, `endereco` | Informações de contato do agente, quando aplicável. |
| `earqContacto.email` | `string` | Não | `format: email` | Endereço eletrônico de contato. |
| `earqContacto.telefone` | `string` | Não |  | Telefone de contato. |
| `earqContacto.endereco` | `string` | Não |  | Endereço físico ou postal de contato. |

### 4.6 `evento_schema.json` — Evento

**Finalidade:** representar eventos associados a documentos, processos, dossiês ou componentes ao longo de seu ciclo de vida. Os eventos registram ações de gestão, preservação, tramitação, captura, assinatura, verificação de fixidez, classificação, acesso e outras ocorrências relevantes para a cadeia de custódia digital.

| Campo | Tipo | Obrigatório | Valores/Regra | Observações |
|---|---|---:|---|---|
| `EarqEventoId` | `string` | Sim |  | Identificador único do evento. Ex.: UUID, URI ou código interno. |
| `EarqEventoTipo` | `string` | Sim | `ECV1...ECV7`, `EPROT1...EPROT10`, `ECLA1...ECLA10`, `EPRES1...EPRES9` | Tipo do evento conforme codificação adotada no modelo e-ARQ Brasil. Pode representar ações como captura, tramitação, classificação, assinatura, verificação de fixidez, migração, acesso e preservação. |
| `EarqEventoDataHora` | `string` | Sim | `format: date-time` | Data e hora do evento no padrão ISO 8601. |
| `EarqEventoAgente` | `Agente` | Sim | `$ref` | Agente responsável pelo evento. Pode corresponder a pessoa, organização, software ou hardware. Referencia `agente_schema.json`. |
| `EarqEventoDetalhe` | `string` | Não |  | Detalhe ou descrição do evento, incluindo parâmetros relevantes, contexto, justificativa ou informação complementar sobre o que ocorreu. |
| `EarqEventoResultado` | `string` | Condicional | `sucesso`, `falha` | Resultado do evento. Obrigatório quando o agente responsável for do tipo `software`, conforme regra condicional do schema. |

### 4.7 `assinatura_schema.json` — Assinatura

**Finalidade:** representar assinaturas associadas a componentes documentais, contemplando tanto mecanismos baseados em autenticação por usuário e senha quanto assinaturas realizadas com certificado digital. O schema registra o signatário, o tipo jurídico da assinatura e os elementos técnicos necessários à verificação futura.

| Campo | Tipo | Obrigatório | Valores/Regra | Observações |
|---|---|---:|---|---|
| `signatario` | `Agente` | Sim | `$ref` | Agente que realizou a assinatura. Referencia `agente_schema.json`. |
| `dataHoraAssinatura` | `string` | Sim | `format: date-time` | Data e hora da assinatura no padrão ISO 8601. |
| `tipoAssinatura` | `string` | Sim | `SIMPLES`, `AVANCADA`, `QUALIFICADA` | Tipo de assinatura conforme a Lei nº 14.063/2020. |
| `assinaturaUsuarioSenha` | `object` | Condicional | exige um dos blocos técnicos do schema | Bloco destinado ao registro de assinatura baseada em autenticação de usuário. O schema exige a presença de pelo menos um mecanismo técnico de assinatura (`assinaturaUsuarioSenha` ou `assinaturaCertificadoDigital`). |
| `assinaturaUsuarioSenha.identificadorUsuario` | `string` | Sim, dentro do bloco |  | Identificador único usado pelo sistema de autenticação para identificar o usuário. |
| `assinaturaUsuarioSenha.sistemaAutenticacao` | `string` | Sim, dentro do bloco |  | Sistema informatizado que conhecia a identidade do usuário e realizou a autenticação no momento da assinatura. Ex.: SEI, LDAP, AD, Keycloak. |
| `assinaturaUsuarioSenha.fatorAutenticacao` | `string` | Não | `senha`, `senha_hash`, `sessao_autenticada`, `outro` | Fator ou evidência de autenticação utilizado no momento da assinatura. |
| `assinaturaUsuarioSenha.checksumConteudo` | `Fixidade` | Sim, dentro do bloco | `$ref` | Fixidez do conteúdo assinado. Permite comprovar a integridade do conteúdo vinculado à autenticação. Referencia `fixidade_schema.json`. |
| `assinaturaCertificadoDigital` | `object` | Condicional | exige um dos blocos técnicos do schema | Bloco destinado ao registro de assinatura baseada em certificado digital. O schema exige a presença de pelo menos um mecanismo técnico de assinatura (`assinaturaUsuarioSenha` ou `assinaturaCertificadoDigital`). |
| `assinaturaCertificadoDigital.tipoPadrao` | `string` | Não | `PKCS#7`, `CMS`, `CAdES`, `PAdES`, `XAdES`, `XMLDSig`, `OUTRO` | Padrão técnico utilizado para codificação da assinatura digital. |
| `assinaturaCertificadoDigital.formatoAssinatura` | `string` | Não | `EMBUTIDA`, `DESTACADA` | Forma de armazenamento da assinatura digital no componente. |
| `assinaturaCertificadoDigital.earqComponenteIdDestacado` | `string` | Condicional | obrigatório quando `formatoAssinatura = DESTACADA` | Identificador do componente que representa o arquivo de assinatura destacada. |
| `assinaturaCertificadoDigital.referenciaValidacao` | `string` | Não | `LCR`, `OCSP`, `OUTRO` | Referência para validação da assinatura. Pode indicar uso de Lista de Certificados Revogados (LCR), resposta OCSP ou outro mecanismo de validação. |

### 4.8 `fixidade_schema.json` — Fixidade

**Finalidade:** representar informações de fixidez utilizadas para comprovar a integridade de documentos, componentes ou conteúdos digitais ao longo do tempo. A fixidez permite verificar se o objeto permaneceu inalterado desde a geração do hash.

| Campo | Tipo | Obrigatório | Valores/Regra | Observações |
|---|---|---:|---|---|
| `algoritmo` | `string` | Sim | `MD5`, `SHA-1`, `SHA-256`, `SHA-512`, `OUTRO` | Algoritmo criptográfico utilizado para cálculo da fixidez. Recomenda-se o uso de algoritmos contemporaneamente confiáveis, como `SHA-256` ou superiores. |
| `hash` | `string` | Sim | mínimo `1` caractere | Valor do hash calculado para o conteúdo. Deve ser armazenado exatamente como produzido pelo algoritmo utilizado. |
| `originador` | `string` | Não |  | Identifica quem ou o que gerou a informação de fixidez. Pode representar sistema, software, equipamento, serviço ou agente responsável pelo cálculo. |

### 4.9 `dependencia_schema.json` — Dependência

**Finalidade:** representar dependências técnicas, lógicas ou externas necessárias para interpretação, execução, renderização, validação ou uso adequado de documentos e componentes digitais.

| Campo | Tipo | Obrigatório | Valores/Regra | Observações |
|---|---|---:|---|---|
| `tipo` | `string` | Sim |  | Tipo da dependência. Pode representar biblioteca, codec, esquema XML, serviço externo, base de dados, fonte tipográfica, componente auxiliar, módulo de software ou outra exigência necessária ao uso do objeto digital. |
| `id` | `string` | Sim |  | Identificador da dependência. Pode ser URI, UUID, nome técnico, código interno, versão de pacote ou outra referência inequívoca. |

### 4.10 `hardware_schema.json` — Hardware

**Finalidade:** representar equipamentos físicos necessários, utilizados ou associados à criação, captura, leitura, execução, preservação ou acesso de documentos e componentes digitais.

| Campo | Tipo | Obrigatório | Valores/Regra | Observações |
|---|---|---:|---|---|
| `nome` | `string` | Sim |  | Nome do equipamento, fabricante, modelo ou denominação técnica do hardware. |
| `tipo` | `string` | Não |  | Tipo ou categoria do hardware. Ex.: scanner, leitora óptica, servidor, drive magnético, estação de trabalho, dispositivo móvel, appliance ou periférico especializado. |
| `outrasInformacoes` | `string` | Não |  | Informações complementares sobre o hardware, como capacidade, interface, arquitetura, requisitos operacionais, número de série, observações técnicas ou contexto de uso. |

### 4.11 `software_schema.json` — Software

**Finalidade:** representar programas, aplicações, sistemas ou utilitários utilizados na criação, leitura, processamento, preservação, validação ou acesso de documentos e componentes digitais.

| Campo | Tipo | Obrigatório | Valores/Regra | Observações |
|---|---|---:|---|---|
| `nome` | `string` | Sim |  | Nome do software, sistema, aplicação ou ferramenta utilizada. |
| `versao` | `string` | Não |  | Versão do software. Informação relevante para reprodutibilidade, compatibilidade técnica e preservação digital. |
| `tipo` | `string` | Não |  | Tipo ou categoria do software. Ex.: editor de texto, visualizador, sistema corporativo, SIGAD, banco de dados, conversor, validador, antivírus, serviço web ou biblioteca técnica. |

### 4.12 `inibidor_schema.json` — Inibidor

**Finalidade:** representar mecanismos, condições ou restrições que possam impedir, limitar ou condicionar o acesso, a leitura, o processamento, a reprodução ou a preservação de documentos e componentes digitais.

| Campo | Tipo | Obrigatório | Valores/Regra | Observações |
|---|---|---:|---|---|
| `tipo` | `string` | Sim |  | Tipo do inibidor. Pode representar criptografia, DRM, bloqueio por senha, limitação de licença, sigilo técnico, dependência proprietária, restrição contratual, mecanismo de proteção ou outro fator impeditivo. |
| `chave` | `string` | Não |  | Chave, token, credencial, identificador ou referência associada ao inibidor, quando aplicável. |

### 4.13 `formato_schema.json` — Formato

**Finalidade:** representar o formato técnico do componente digital, informação essencial para preservação, interoperabilidade, renderização, migração tecnológica e definição de estratégias de acesso futuro.

| Campo | Tipo | Obrigatório | Valores/Regra | Observações |
|---|---|---:|---|---|
| `nome` | `string` | Sim |  | Nome do formato do componente digital. Ex.: PDF, PDF/A, TIFF, JPEG, WAV, XML, CSV, DOCX, MP4. |
| `versao` | `string` | Não |  | Versão, perfil ou variante do formato. Ex.: PDF/A-2b, TIFF 6.0, XML 1.0, CSV RFC4180, OOXML Transitional. |

### 4.14 `localizacao_schema.json` — Localização

**Finalidade:** representar a localização lógica, física ou referencial de documentos, volumes e componentes, permitindo identificar onde o objeto pode ser encontrado, acessado ou recuperado.

| Campo | Tipo | Obrigatório | Valores/Regra | Observações |
|---|---|---:|---|---|
| `tipo` | `string` | Sim | `URI`, `LOCALIZACAO_EM_DISCO`, `RELATIVA` | Tipo da localização informada. `URI` indica endereço universal (web, repositório ou identificador resolvível); `LOCALIZACAO_EM_DISCO` indica caminho absoluto ou referência local de armazenamento; `RELATIVA` indica caminho relativo dentro de pacote, diretório ou estrutura de empacotamento. |
| `valor` | `string` | Sim | mínimo `1` caractere | Valor concreto da localização conforme o tipo informado. Pode ser URL, URI, caminho de arquivo, diretório, identificador interno ou referência relativa. |

### 4.15 `relacionamento_schema.json` — Relacionamento

**Finalidade:** representar vínculos entre componentes digitais, permitindo expressar relações estruturais, funcionais, técnicas ou derivativas entre objetos associados ao mesmo documento ou a documentos distintos.

| Campo | Tipo | Obrigatório | Valores/Regra | Observações |
|---|---|---:|---|---|
| `relacaoIDComponenteDestino` | `string` | Sim |  | Identificador do componente de destino relacionado ao componente de origem. No modelo atual, a origem é implícita pelo contexto em que o relacionamento está declarado. |
| `relacaoTipo` | `string` | Não |  | Tipo da relação estabelecida entre os componentes. Pode representar derivação, dependência, representação alternativa, versão correlata, vínculo estrutural, parte integrante, conversão, assinatura associada ou outra relação semântica definida pela implementação. |

---

## 5) Exemplo mínimo de uso (modelo mental)

### 5.1 Documento com 1 componente e 2 eventos (exemplo conceitual)

- **Documento** descreve o que é (título, data, autor etc.).
- **Componente** descreve em que forma existe (arquivo PDF, tamanho, formato, fixidez).
- **Eventos** descrevem o que aconteceu (ex.: “captura”, “verificação de fixidez”).

---

## 6) Checklist de implementação (para quem vai produzir/consumir JSON)

1. Resolver `$ref` localmente (todos os schemas no mesmo diretório).
2. Padronizar nomes (`required` ⇄ `properties`) antes de exigir validação “hard fail”.
3. Definir vocabulários controlados pendentes:
   - `Dependencia.tipo`, `Relacionamento.relacaoTipo`, `Inibidor.tipo`, `Software.tipo`.
4. Formalizar semântica de campos ambíguos:
   - `earqComponenteNivelComposicao` (o que significa `0/1`).
5. Ajustar regra condicional do Evento para checar `agentType == "software"` (ou definir `tipo` no Agente e padronizar).

# 7) Padrão de Armazenamento em Sistemas de Arquivos

## 7.1 Objetivo

Definir um padrão lógico e previsível para organização de **Documento**, **Processo** e **Dossiê** em sistemas de arquivos (filesystem), garantindo:

- interoperabilidade entre sistemas;
- previsibilidade de localização;
- integridade da estrutura intelectual;
- rastreabilidade;
- compatibilidade com preservação digital de longo prazo;
- facilidade de migração para repositórios digitais confiáveis (RDC-Arq).

Este padrão não substitui sistemas de gestão, mas estabelece uma convenção mínima para armazenamento estruturado quando o meio for filesystem (NAS, storage local, NFS, S3 compatível, etc.).

## 7.2 Princípios Gerais

O padrão de armazenamento em sistema de arquivos deve observar princípios arquivísticos e técnicos que garantam coerência entre estrutura intelectual e estrutura física.

### 7.2.1 Hierarquia Arquivística

A estrutura deve refletir a organização intelectual da informação, podendo se apresentar de duas formas:

**1) Processo → Documento → Componente(s)**

```
Processo
   └── Documento
         └── Componente(s)
```

**2) Dossiê → Documento → Componente(s)**

```
Dossiê
   └── Documento
         └── Componente(s)
```

O Documento é sempre a unidade mínima intelectual estruturante dentro de Processo ou Dossiê.

O Componente representa a materialização técnica do Documento (arquivo digital, mídia física, objeto híbrido etc.).

---

### 7.2.2 Identificador como Elemento Estruturante

Sempre que possível, deve-se utilizar o identificador persistente da entidade como base para nomeação de diretórios e organização estrutural:

- `earqDocumentoId`
- `earqProcessoDossieId`
- outro identificador institucional equivalente

O identificador deve ser único, estável e não dependente de título ou descrição textual.

---

### 7.2.3 Separação entre Metadados e Conteúdo Binário

A organização física deve distinguir claramente:

- **Metadados estruturados** → arquivos `.json`
- **Componentes digitais** → arquivos binários (PDF, DOCX, TIFF, etc.)

Essa separação facilita:

- validação automática por schema
- indexação
- preservação digital
- auditoria

---

### 7.2.4 Imutabilidade Lógica

Uma vez encerrado o Processo ou Dossiê:

- A estrutura física não deve ser alterada sem registro de evento;
- Alterações devem ser registradas como novo evento;
- Recomenda-se evitar sobrescrita silenciosa de arquivos.

A imutabilidade lógica é requisito essencial para garantir integridade e rastreabilidade.


## 7.3 Estrutura de Diretórios Recomendada

Esta seção estabelece o padrão obrigatório de organização de **Documento**, **Processo** e **Dossiê** em sistemas de arquivos.

A estrutura deve:

- Utilizar identificadores únicos como base de nomeação;
- Manter os metadados em arquivo `.json` na raiz da entidade;
- Garantir previsibilidade estrutural;
- Permitir interoperabilidade e migração futura para repositórios digitais confiáveis;
- Evitar ambiguidade entre estrutura física e estrutura intelectual.

Regras gerais:

1. O diretório raiz de cada entidade deve seguir o prefixo normativo:
   - `DA-` para Documento
   - `PR-` para Processo
   - `DS-` para Dossiê

2. O identificador único deve ser persistente e corresponder ao identificador registrado nos metadados (`earqDocumentoId`, `earqProcessoDossieId` ou equivalente).

3. O arquivo de metadados (`.json`) deve estar sempre no nível raiz da entidade.

4. No caso de Documento, os componentes digitais devem estar no mesmo nível do arquivo `documento.json`.

---

## 7.3.1 Estrutura Base

### a) Documento (DA)

Regras:

- Deve existir uma pasta raiz com o padrão: `DA-<IdentificadorUnico>`.
- Dentro da pasta raiz deve existir o arquivo `documento.json`, contendo os metadados conforme os schemas definidos neste manual.
- No mesmo nível do `documento.json` devem estar os componentes digitais associados ao documento (PDF, DOCX, imagens, planilhas etc.).

Estrutura exemplo:

```text
DA-<IdentificadorUnico>/
├── documento.json
├── documento_principal.pdf
├── anexo_01.docx
└── imagem_01.tif
```

---

### b) Processo (PR)

Regras:

- Deve existir uma pasta raiz com o padrão: `PR-<IdentificadorUnico>`.
- Dentro da pasta raiz deve existir o arquivo `processo.json`, contendo os metadados do processo conforme os schemas definidos.
- Abaixo da pasta raiz deve existir uma pasta para cada Documento que compõe o processo.
- Cada pasta de Documento deve obedecer integralmente ao padrão definido para Documento (`DA-`).

Estrutura exemplo:

```text
PR-<IdentificadorUnico>/
├── processo.json
├── DA-<IdentificadorDocumento01>/
│   ├── documento.json
│   └── documento_01.pdf
└── DA-<IdentificadorDocumento02>/
    ├── documento.json
    ├── documento_02.pdf
    └── anexo_01.docx
```

---

### c) Dossiê (DS)

Regras:

- Deve existir uma pasta raiz com o padrão: `DS-<IdentificadorUnico>`.
- Dentro da pasta raiz deve existir o arquivo `dossie.json`, contendo os metadados do dossiê conforme os schemas definidos.
- Abaixo da pasta raiz deve existir uma pasta para cada Documento que compõe o dossiê.
- Cada pasta de Documento deve obedecer integralmente ao padrão definido para Documento (`DA-`).

Estrutura exemplo:

```text
DS-<IdentificadorUnico>/
├── dossie.json
├── DA-<IdentificadorDocumento01>/
│   ├── documento.json
│   └── documento_01.pdf
└── DA-<IdentificadorDocumento02>/
    ├── documento.json
    ├── documento_02.pdf
    └── anexo_01.docx
```
## 7.4 Convenções de Nomeação

As convenções de nomeação devem garantir:

- previsibilidade estrutural;
- interoperabilidade entre sistemas;
- estabilidade de identificação;
- compatibilidade com preservação digital de longo prazo.

As regras abaixo complementam a estrutura definida nas Seções 7.2 e 7.3.

---

### 7.4.1 Nomeação de Diretórios Raiz

Os diretórios raiz devem obedecer obrigatoriamente ao seguinte padrão:

- Documento:  
  `DA-<IdentificadorUnico>`

- Processo:  
  `PR-<IdentificadorUnico>`

- Dossiê:  
  `DS-<IdentificadorUnico>`

O `<IdentificadorUnico>` deve:

- corresponder ao identificador registrado nos metadados (`earqDocumentoId`, `earqProcessoDossieId` ou equivalente institucional);
- ser persistente;
- não depender de título ou descrição textual;
- não conter espaços;
- não conter acentos;
- não conter caracteres especiais;
- utilizar apenas caracteres alfanuméricos e hífen.

Exemplo recomendado:

```
DA-2026-000123
PR-2026-000456
DS-2026-000789
```

---

### 7.4.2 Nomeação de Arquivos de Metadados

Os arquivos de metadados devem ter nomes fixos e padronizados:

| Entidade   | Nome obrigatório |
|------------|------------------|
| Documento  | `documento.json` |
| Processo   | `processo.json`  |
| Dossiê     | `dossie.json`    |

Não é permitido:

- incluir versão no nome do arquivo de metadados (o versionamento deve ocorrer no campo `earqDocumentoVersao`);
- alterar a grafia padronizada;
- utilizar nomes derivados do título.

---

### 7.4.3 Nomeação de Componentes Digitais

Os componentes digitais devem:

- preservar, sempre que possível, o nome original do arquivo;
- estar registrados no campo `earqComponenteNomeOriginal`;
- evitar caracteres especiais e espaços excessivos;
- manter extensão coerente com o formato declarado em `earqComponenteFormato`.

Opcionalmente, pode-se utilizar prefixo numérico para ordenação:

```
01_oficio.pdf
02_anexo.pdf
03_memoria_calculo.xlsx
```

A ordenação física não substitui a ordenação lógica, que deve ser garantida pelos metadados.

---

### 7.4.4 Sensibilidade a Maiúsculas e Minúsculas

Recomenda-se que:

- nomes de diretórios e arquivos sejam tratados como case-sensitive;
- o padrão definido (DA-, PR-, DS-) seja mantido em letras maiúsculas;
- os nomes dos arquivos `.json` permaneçam em minúsculas.

Essa padronização evita inconsistências em ambientes Linux, Unix e sistemas baseados em objetos (S3 compatível).

---

### 7.4.5 Proibições

Não é permitido:

- utilizar títulos como nome de pasta raiz;
- renomear diretórios após sua consolidação;
- modificar identificadores sem registro formal de evento;
- criar estruturas paralelas fora do padrão definido.

---

### 7.4.6 Compatibilidade com Preservação Digital

As convenções de nomeação devem:

- permitir empacotamento em BagIt sem necessidade de renomeação;
- permitir indexação automática;
- ser compatíveis com armazenamento em NAS, NFS, storage local ou S3;
- não depender de caminhos absolutos para interpretação semântica.

A interpretação intelectual deve estar sempre garantida pelos metadados estruturados.

## 7.5 Empacotamento para Trâmite

### 7.5.1 Objetivo

Esta seção define o padrão de empacotamento para trâmite eletrônico de Documentos, Processos e Dossiês entre órgãos ou sistemas distintos.

O objetivo é:

- garantir interoperabilidade entre instituições;
- assegurar integridade e autenticidade do conteúdo transferido;
- manter rastreabilidade da origem e destino;
- permitir validação automatizada do pacote;
- assegurar compatibilidade com estratégias de preservação digital.

O empacotamento deve ser independente de sistema específico, baseado em padrão aberto e amplamente adotado.

---

### 7.5.2 Padrão BagIt

O empacotamento para trâmite deve utilizar o padrão **BagIt**, definido originalmente pela Library of Congress.

#### Estrutura Geral do BagIt

Um pacote BagIt possui a seguinte estrutura mínima:

```
<bag-root>/
├── bagit.txt
├── bag-info.txt (opcional)
├── manifest-<algoritmo>.txt
├── tagmanifest-<algoritmo>.txt (opcional)
└── data/
    └── (conteúdo transferido)
```

#### Arquivos Obrigatórios

- `bagit.txt`  
  Define a versão do padrão BagIt e a codificação utilizada.

- `manifest-<algoritmo>.txt`  
  Contém os hashes (checksums) dos arquivos presentes na pasta `data/`.

- Diretório `data/`  
  Contém o conteúdo efetivo do pacote.

#### Arquivos Opcionais

- `bag-info.txt`  
  Metadados administrativos do pacote (ex.: origem, contato, descrição).

- `tagmanifest-<algoritmo>.txt`  
  Hashes dos arquivos de tag (bagit.txt, bag-info.txt etc.).

O algoritmo de hash recomendado é `sha256` ou superior.

---

### 7.5.3 Estrutura do Pacote de Trâmite

O diretório raiz do pacote deve seguir obrigatoriamente o padrão:

```
TR-<NomeOrgaoOrigem>-<NomeOrgaoDestino>
```

Regras:

- Não utilizar acentos;
- Não utilizar espaços (substituir por hífen);
- Utilizar apenas caracteres alfanuméricos e hífen;
- O nome deve refletir claramente os órgãos envolvidos no trâmite.

Exemplo:

```
TR-MGI-ARQUIVONACIONAL
TR-MJ-MGI
```

---

### 7.5.4 Estrutura Interna do Bag

Dentro do BagIt, a pasta `data/` deverá conter a seguinte estrutura:

```
TR-<Origem>-<Destino>/
├── bagit.txt
├── manifest-sha256.txt
└── data/
    ├── tramite.json
    ├── DA-<IdentificadorDocumento>/
    ├── PR-<IdentificadorProcesso>/
    └── DS-<IdentificadorDossie>/
```

#### Regras de Composição

1. Deve existir obrigatoriamente na raiz da pasta `data/` o arquivo:

   ```
   tramite.json
   ```

2. O arquivo `tramite.json` deve conter:

   - identificação do órgão de origem;
   - identificação do órgão de destino;
   - data/hora do envio;
   - lista das entidades tramitadas;
   - identificador do pacote;
   - algoritmo de hash utilizado;
   - eventos associados ao envio.

3. Na pasta `data/` deve existir a pasta de cada entidade tramitada:

   - `DA-<IdentificadorUnico>` para Documento;
   - `PR-<IdentificadorUnico>` para Processo;
   - `DS-<IdentificadorUnico>` para Dossiê.

4. Cada pasta deve obedecer integralmente ao padrão estrutural definido nas Seções 7.2 e 7.3 deste manual.

---

### 7.5.5 Integridade e Validação

O pacote deve permitir:

- validação de integridade por verificação de `manifest-sha256.txt`;
- validação estrutural por schema dos arquivos `.json`;
- verificação de completude da estrutura;
- rastreabilidade institucional do trâmite.

Recomenda-se que o recebimento do pacote gere automaticamente um evento de validação e outro de recebimento.

---

### 7.5.6 Considerações de Preservação

O pacote de trâmite:

- não substitui um AIP (Archival Information Package);
- não constitui repositório definitivo;
- deve ser considerado estrutura de transporte interoperável.

Após o recebimento, o órgão destinatário poderá:

- manter o pacote como evidência do trâmite;
- desmembrar o conteúdo para admissão em repositório;
- gerar novo pacote para trâmite subsequente.



## 8 Empacotamento para Envio a RDC-Arq (SIP)

### 8.1 Objetivo

Esta seção define o padrão de empacotamento para envio de Documentos, Processos e Dossiês a um **Repositório Arquivístico Digital Confiável (RDC-Arq)**.

O empacotamento descrito nesta seção corresponde à criação de um **SIP (Submission Information Package)**, conforme definido no modelo OAIS.

O objetivo é:

- garantir interoperabilidade entre sistemas produtores e o RDC-Arq;
- assegurar autenticidade (integridade e identidade) da informação transferida;
- permitir validação automatizada do pacote de admissão;
- manter rastreabilidade da transferência ou recolhimento;
- facilitar processos de admissão automatizada no repositório.

---

### 8.2 Modelo OAIS e Pacotes de Informação

O **OAIS (Open Archival Information System)** é um modelo de referência internacional para sistemas de preservação digital de longo prazo.

O modelo define três tipos principais de pacotes de informação:

| Tipo de Pacote | Função |
|---|---|
| SIP — Submission Information Package | Pacote submetido ao repositório para admissão |
| AIP — Archival Information Package | Pacote armazenado e preservado pelo repositório |
| DIP — Dissemination Information Package | Pacote gerado para acesso ao usuário |

Neste manual, esta seção trata da criação do **SIP**, que é o pacote enviado ao RDC-Arq durante processos de transferência arquivística, recolhimento ou admissão programada de documentos digitais.

---

### 8.3 Padrão de Empacotamento

O SIP deve utilizar o padrão **BagIt**, conforme descrito na Seção 7.5 deste manual.

Estrutura geral:

```
<bag-root>/
├── bagit.txt
├── manifest-<algoritmo>.txt
├── bag-info.txt (opcional)
└── data/
    └── (conteúdo transferido)
```

O algoritmo de hash recomendado é **sha256** ou superior.

---

### 8.4 Nomeação do Pacote SIP

A pasta raiz do BagIt deve seguir o padrão:

```
SIP-<CODEARQ>-<NOME_DO_FUNDO>-<ID_GUIA>-<CODIGO_CLASSIFICACAO_COMPLETO>-<DATAHORAEMPACOTAMENTO>
```

Onde:

| Elemento | Descrição |
|---|---|
| CODEARQ | Código do cadastro nacional de entidades custoriadoras de acervos do arquivo responsável pela custódia |
| NOME_DO_FUNDO | Nome do fundo arquivístico, a ser informado pelo Arquivo de destino do pacote |
| ID_GUIA | Identificador da guia de transmissão (transfêrencia ou recolhimento) |
| CODIGO_CLASSIFICACAO_COMPLETO | Código de classificação completo |
| DATAHORAEMPACOTAMENTO | Data e hora de criação do pacote |

Regras:

- utilizar "_" como separador do código de classificação;
- não utilizar acentos;
- evitar espaços;
- utilizar apenas caracteres alfanuméricos, hífen e underscore.

Exemplo:

```
SIP-BRANRJ-MINISTERIO_DA_JUSTICA-GT022-001_130_131-20260402T143500
```

---

### 8.5 Estrutura Interna do SIP

Estrutura do pacote:

```
SIP-<...>/
├── bagit.txt
├── manifest-sha256.txt
└── data/
    ├── fichaEspelho.json
    ├── DA-<IdentificadorDocumento>/
    ├── PR-<IdentificadorProcesso>/
    └── DS-<IdentificadorDossie>/
```

---

### 8.6 Ficha Espelho de Transferência

Na raiz da pasta `data/` deve existir obrigatoriamente o arquivo:

```
fichaEspelho.json
```

Este arquivo contém os metadados administrativos da transferência arquivística.

Entre os elementos esperados estão:

- órgão produtor;
- fundo arquivístico;
- identificação da guia;
- período documental;
- código de classificação;
- quantidade de documentos;
- data do empacotamento;
- responsável pelo envio.

O schema deste arquivo será definido em versão posterior deste manual.

---

### 8.7 Composição do Conteúdo do SIP

Na pasta `data/` devem estar presentes as entidades arquivísticas transferidas:

- `DA-<IdentificadorUnico>` para Documento;
- `PR-<IdentificadorUnico>` para Processo;
- `DS-<IdentificadorUnico>` para Dossiê.

Cada entidade deve obedecer às regras estruturais definidas nas Seções 7.2, 7.3 e 7.4 deste manual.

---

### 8.8 Validação do SIP

Antes do envio ao RDC-Arq, o pacote deve permitir:

- validação de integridade por verificação de `manifest-sha256.txt`;
- validação estrutural da organização do pacote;
- validação dos arquivos JSON conforme seus schemas;
- verificação da consistência entre metadados e conteúdo.

Recomenda-se que o RDC-Arq registre automaticamente eventos de:

- validação do pacote;
- admissão;
- rejeição ou aceitação do SIP.

---

### 8.9 Pacote de Informação de Representação da Guia

Para cada **Guia de Transferência ou Recolhimento**, deve ser criado um pacote adicional contendo **Informações de Representação** necessárias para interpretação correta dos documentos enviados nos SIPs associados àquela guia.

Este pacote segue o modelo **OAIS**, que define a **Informação de Representação** como o conjunto de informações necessárias para interpretar corretamente os objetos digitais.

Este pacote deve ser criado **uma única vez por guia**.

---

### 8.9.1 Padrão de Empacotamento

O pacote também deve utilizar o padrão **BagIt**.

Estrutura:

```
<bag-root>/
├── bagit.txt
├── manifest-<algoritmo>.txt
└── data/
    └── (informações de representação)
```

---

### 8.9.2 Nomeação do Pacote

O nome do pacote deve seguir o padrão:

```
SIP-IR-<CODEARQ>-<NOME_DO_FUNDO>-<ID_GUIA>
```

Exemplo:

```
SIP-IR-AN-MINISTERIO_DA_JUSTICA-GT002
```

---

### 8.9.3 Estrutura do Pacote

Estrutura recomendada:

```
SIP-IR-<...>/
├── bagit.txt
├── manifest-sha256.txt
└── data/
    ├── representacao.json
    ├── fontes/
    ├── softwares/
    └── documentacao/
```

---

### 8.9.4 Conteúdo do Pacote

O arquivo principal é:

```
representacao.json
```

Este arquivo deve descrever os recursos técnicos necessários para interpretação dos documentos.

Exemplos de informação de representação:

- fontes tipográficas utilizadas;
- especificações de formatos;
- softwares necessários para leitura;
- dependências técnicas;
- documentação técnica de formatos ou sistemas produtores.

---

### 8.9.5 Relação com os SIPs

Os SIPs de conteúdo associados à mesma guia devem referenciar o pacote **SIP-IR** por meio de metadados no arquivo:

```
fichaEspelho.json
```

Isso permite que o RDC-Arq associe automaticamente os SIPs às informações de representação necessárias para interpretação dos objetos digitais.

## 9. Considerações Finais

O presente **Manual de Interoperabilidade de Documentos Arquivísticos Digitais** propõe um modelo técnico comum para representação, armazenamento, tramitação e transferência de documentos arquivísticos digitais, orientado por princípios arquivísticos e por padrões amplamente reconhecidos no campo da gestão documental e da preservação digital.

Ao estruturar metadados em **JSON Schema**, organizar entidades como documento, componente, processo, volume, agente e evento, e estabelecer convenções para empacotamento e intercâmbio com uso de padrões como **BagIt** e do modelo **OAIS**, o manual busca reduzir barreiras de interoperabilidade entre sistemas produtores, plataformas de processo eletrônico, sistemas de negócio, arquivos permanentes e repositórios digitais confiáveis.

A adoção deste modelo pode contribuir para:

- fortalecimento da autenticidade, integridade e confiabilidade dos documentos digitais;
- melhoria dos processos de transferência, recolhimento e custódia digital;
- maior independência tecnológica por meio de padrões abertos;
- preservação de longo prazo com manutenção de contexto, significado e valor probatório;
- integração entre instituições e soluções tecnológicas heterogêneas;
- estímulo à cooperação técnica entre órgãos e entidades custodiais.

Este documento não pretende encerrar o tema, mas servir como base evolutiva para futuras versões. A transformação tecnológica, o surgimento de novos formatos documentais, o avanço de assinaturas eletrônicas, a expansão da inteligência artificial e a crescente complexidade dos ecossistemas informacionais exigirão revisões periódicas, aperfeiçoamentos normativos e amadurecimento contínuo do modelo aqui apresentado.

Recomenda-se que sua implementação seja acompanhada por processos de governança, validação técnica, capacitação institucional e diálogo permanente entre áreas de arquivo, tecnologia da informação, gestão pública e preservação digital.

Por fim, reafirma-se que a interoperabilidade não é apenas um requisito tecnológico, mas condição essencial para assegurar direitos, transparência, memória institucional e continuidade administrativa em uma sociedade cada vez mais digital.