# Manual de Interoperabilidade de Documentos Arquivísticos

## Sumário
1. [Visão geral do padrão](#1-visão-geral-do-padrão)  
   1.1 [Objetivo](#11-objetivo)  
   1.2 [Estrutura de referências ($ref)](#12-estrutura-de-referências-ref)  
2. [Regras e convenções de preenchimento](#2-regras-e-convenções-de-preenchimento)  
   2.1 [Tipos e formatos](#21-tipos-e-formatos)  
   2.2 [Campos obrigatórios](#22-campos-obrigatórios)  
   2.3 [“Eventos” como trilha de auditoria](#23-eventos-como-trilha-de-auditoria)  
3. [Pontos de atenção (importante para a versão em desenvolvimento)](#3-pontos-de-atenção-importante-para-a-versão-em-desenvolvimento)  
4. [Dicionário de dados por schema](#4-dicionário-de-dados-por-schema)  
   4.1 [`dependencia_schema.json` — Dependência](#41-dependencia_schemajson--dependência)  
   4.2 [`assinatura_schema.json` — Assinatura](#42-assinatura_schemajson--assinatura)  
   4.3 [`agente_schema.json` — Agente](#43-agente_schemajson--agente)  
   4.4 [`evento_schema.json` — Evento](#44-evento_schemajson--evento)  
   4.5 [`fixidade_schema.json` — Fixidade](#45-fixidade_schemajson--fixidade)  
   4.6 [`localizacao_schema.json` — Localização](#46-localizacao_schemajson--localização)  
   4.7 [`relacionamento_schema.json` — RelacionamentoComponente](#47-relacionamento_schemajson--relacionamentocomponente)  
   4.8 [`inibidor_schema.json` — Inibidor](#48-inibidor_schemajson--inibidor)  
   4.9 [`software_schema.json` — Software](#49-software_schemajson--software)  
   4.10 [`hardware_schema.json` — Hardware](#410-hardware_schemajson--hardware)  
   4.11 [`formato_schema.json` — Formato](#411-formato_schemajson--formato)  
   4.12 [`componente_schema.json` — Componente](#412-componente_schemajson--componente)  
   4.13 [`documento_schema.json` — Documento](#413-documento_schemajson--documento)  
5. [Exemplo mínimo de uso (modelo mental)](#5-exemplo-mínimo-de-uso-modelo-mental)  
   5.1 [Documento com 1 componente e 2 eventos (exemplo conceitual)](#51-documento-com-1-componente-e-2-eventos-exemplo-conceitual)  
6. [Checklist de implementação (para quem vai produzir/consumir JSON)](#6-checklist-de-implementação-para-quem-vai-produzirconsumir-json)
7. [Padrão de Armazenamento e Trâmite em Sistema de Arquivos](#7-padrao-de-armazenamento-e-tramite-em-sistema-de-arquivos)  
   7.1 [Objetivo](#71-objetivo)  
   7.2 [Princípios Gerais](#72-principios-gerais)  
   7.3 [Estrutura de Diretórios Recomendada](#73-estrutura-de-diretorios-recomendada)  
   7.4 [Convenções de Nomeação](#74-convencoes-de-nomeacao)  
   7.5 [Empacotamento para Trâmite](#75-empacotamento-para-tramite)  
8. [Empacotamento para Envio a RDC-Arq (SIP)](#8-empacotamento-para-envio-a-rdc-arq-sip)  
   8.1 [Objetivo](#81-objetivo)  
   8.2 [Modelo OAIS e Pacotes de Informação](#82-modelo-oais-e-pacotes-de-informacao)  
   8.3 [Padrão de Empacotamento](#83-padrao-de-empacotamento)  
   8.4 [Nomeação do Pacote SIP](#84-nomeacao-do-pacote-sip)  
   8.5 [Estrutura Interna do SIP](#85-estrutura-interna-do-sip)  
   8.6 [Ficha Espelho de Transferência](#86-ficha-espelho-de-transferencia)  
   8.7 [Composição do Conteúdo do SIP](#87-composicao-do-conteudo-do-sip)  
   8.8 [Validação do SIP](#88-validacao-do-sip)  
   8.9 [Pacote de Informação de Representação da Guia](#89-pacote-de-informacao-de-representacao-da-guia)  
      8.9.1 [Padrão de Empacotamento](#891-padrao-de-empacotamento)  
      8.9.2 [Nomeação do Pacote](#892-nomeacao-do-pacote)  
      8.9.3 [Estrutura do Pacote](#893-estrutura-do-pacote)  
      8.9.4 [Conteúdo do Pacote](#894-conteudo-do-pacote)  
      8.9.5 [Relação com os SIPs](#895-relacao-com-os-sips)  

---

## 1) Visão geral do padrão

### 1.1 Objetivo
Os schemas descrevem uma estrutura mínima para representar documentos e processos com metadados de gestão (e-ARQ) e preservação (conceitos alinhados ao PREMIS), especialmente:

- **Documento** como entidade intelectual (metadados descritivos e de contexto).
- **Componente** como entidade técnica/material (arquivo digital, mídia, item físico, etc.).
- **Evento** como trilha de ações ocorridas no ciclo de vida (captura, tramitação, assinatura, verificação de fixidez, migração etc.).
- **Agente** como responsável por eventos e papéis (pessoa, organização, software, hardware).

### 1.2 Estrutura de referências ($ref)
Os schemas se relacionam por **`$ref`** (referência por arquivo). Exemplos:

- `evento_schema.json` referencia `agente_schema.json` em `EarqEventoAgente`.
- `componente_schema.json` referencia `software_schema.json`, `hardware_schema.json`, `formato_schema.json`, etc.
- `documento_schema.json` referencia `componente_schema.json` e `evento_schema.json`.

**Recomendação prática:** mantenha todos os `.json` de schema no mesmo diretório para facilitar a resolução relativa dos `$ref`.

---

## 2) Regras e convenções de preenchimento

### 2.1 Tipos e formatos
- `format: date` → data no padrão ISO (ex.: `2026-01-15`).
- `format: date-time` → data/hora ISO 8601 (ex.: `2026-01-15T10:30:00-03:00`).

> Observação: a aplicação dessas convenções aparece, por exemplo, em `evento_schema.json`.

### 2.2 Campos obrigatórios
Cada schema define uma lista **`required`**. Em interoperabilidade, isso significa:

- O produtor deve garantir a presença dos campos obrigatórios.
- O consumidor pode rejeitar objetos inválidos (ou registrar inconsistência).

### 2.3 “Eventos” como trilha de auditoria
Tanto **Documento** quanto **Componente** possuem `earqEventos` (lista de **Evento**).

Isso permite modelar:

- cadeia de preservação
- histórico de validações (fixidez, assinatura),
- ações de preservação (migração, normalização),
- tramitações e atos processuais.

---

## 3) Pontos de atenção (importante para a versão em desenvolvimento)

Há inconsistências de *case* entre `properties` e `required` em alguns schemas:

- Em **Agente**, os `properties` estão como `earqAgenteId`, `earqAgenteNome` etc., mas o `required` lista `EarqAgenteId`, `EarqAgenteNome`, `EarqAgenteStatus` (com “E” maiúsculo). Isso tende a falhar na validação.
- Em **Documento**, `required` lista `EarqDocumentoId`, mas em `properties` aparece `earqDocumentoId`.

**Recomendação de padronização (para próxima revisão):**
- escolher um padrão único (ex.: `camelCase` com prefixo `earq...` e `dc...`) e alinhar `required` aos nomes exatos em `properties`.

Também há regras condicionais (`allOf/if/then`) que parecem referenciar caminhos que não existem do jeito escrito (ex.: condição no Evento olha `EarqEventoAgente.tipo`, mas no schema de Agente o campo é `agentType`).

➡️ Isso merece ajuste para que a regra condicional funcione.

---

## 4) Dicionário de dados por schema
Abaixo, para cada schema: **finalidade, campos, tipo, obrigatoriedade, valores controlados e observações**.

---

### 4.1 `dependencia_schema.json` — Dependência
**Finalidade:** apontar dependências genéricas (ex.: dependência técnica, dependência externa, vínculo a outro recurso).

| Campo | Tipo | Obrigatório | Observações |
|---|---|---:|---|
| `tipo` | `string` | Sim | Tipo de dependência (vocabulário ainda aberto). |
| `id` | `string` | Sim | Identificador da dependência (URI, UUID, código interno). |

---

### 4.2 `assinatura_schema.json` — Assinatura
**Finalidade:** registrar assinatura (digital ou outra forma) associada ao componente.

| Campo | Tipo | Obrigatório | Observações |
|---|---|---:|---|
| `codificacao` | `string` | Sim | Ex.: base64, hex, DER etc. |
| `signatario` | `Agente` (`$ref`) | Sim | Quem assinou. |
| `metodo` | `string` | Sim | Método/algoritmo (ex.: CMS/PKCS#7, PAdES, etc.). |
| `valor` | `string` | Sim | Valor/artefato da assinatura (string). |
| `regrasValidacao` | `string` | Não | Regras ou política de validação. |
| `chave` | `string` | Não | Chave/certificado/identificador correlato. |

---

### 4.3 `agente_schema.json` — Agente
**Finalidade:** representar pessoa, organização, hardware ou software responsável por ações/eventos. Inclui metadados alinháveis ao PREMIS (`agentType`, `agentVersion`, `agentNote`).

| Campo | Tipo | Obrigatório | Valores/Regra | Observações |
|---|---|---:|---|---|
| `earqAgenteId` | `string` | (Sim, intenção do schema) |  | Identificador único (UUID/URI/código). |
| `earqAgenteNome` | `string` | (Sim, intenção do schema) |  | Nome do agente. |
| `earqFormaDoNome` | `string` | Não |  | Forma do nome (completo/abreviado etc.). |
| `earqAgenteStatus` | `string` | (Sim, intenção do schema) | `Ativo` \| `Inativo` | Situação do agente. |
| `agentType` | `string` | Sim | `Pessoa` \| `Organização` \| `hardware` \| `software` | Tipo do agente. |
| `agentVersion` | `string` | Condicional | obrigatório se `agentType = software` ou `hardware` | Versão do agente técnico. |
| `agentNote` | `string` | Não |  | Nota para desambiguação. |
| `earqDatasDeExistencia.inicio` | `date` | Não |  | Data de início. |
| `earqDatasDeExistencia.fim` | `date` | Não |  | Data de fim. |
| `earqIdentificadoresExternos[]` | `array(obj)` | Não |  | Ex.: ORCID, CPF/CNPJ, VIAF, Wikidata. |
| `earqContacto.email` | `email` | Não |  | Contato (se aplicável). |
| `earqContacto.telefone` | `string` | Não |  |  |
| `earqContacto.endereco` | `string` | Não |  |  |

⚠️ **Atenção (validação):** alinhar `required` ao mesmo *case* de `properties` (hoje há divergência).

---

### 4.4 `evento_schema.json` — Evento
**Finalidade:** registrar evento associado a Documento/Componente ao longo do ciclo de vida.

| Campo | Tipo | Obrigatório | Valores/Regra | Observações |
|---|---|---:|---|---|
| `EarqEventoId` | `string` | Sim |  | Identificador único do evento. |
| `EarqEventoTipo` | `string (enum)` | Sim | Lista codificada (`ECV*`, `EPROT*`, `ECLA*`, `EPRES*`) | Vocabulário controlado (em evolução). |
| `EarqEventoDataHora` | `date-time` | Sim | ISO 8601 | Data/hora do evento. |
| `EarqEventoAgente` | `Agente` (`$ref`) | Sim |  | Responsável pelo evento. |
| `EarqEventoDetalhe` | `string` | Não |  | Descrição/parametrização. |
| `EarqEventoResultado` | `string (enum)` | Condicional | `sucesso` \| `falha` | Obrigatório “quando agente for software” (regra condicional precisa alinhar com `agentType`). |

⚠️ **Atenção (regra condicional):** o `if/then` verifica `EarqEventoAgente.tipo = software`, mas o schema de Agente usa `agentType`. Ajustar para funcionar.

---

### 4.5 `fixidade_schema.json` — Fixidade
**Finalidade:** registrar fixidez (hash) de componente.

| Campo | Tipo | Obrigatório | Observações |
|---|---|---:|---|
| `algoritmo` | `string` | Sim | Ex.: `sha256`, `sha512`. |
| `hash` | `string` | Sim | Valor do hash (string). |

---

### 4.6 `localizacao_schema.json` — Localização
**Finalidade:** indicar onde o componente/documento pode ser encontrado (URI ou localização física).

| Campo | Tipo | Obrigatório | Valores |
|---|---|---:|---|
| `tipo` | `string (enum)` | Sim | `URI` \| `LOCALIZACAOFISICA` |
| `valor` | `string` | Sim | URI, caminho, referência física etc. |

---

### 4.7 `relacionamento_schema.json` — RelacionamentoComponente
**Finalidade:** registrar relações entre componentes (origem/destino).

| Campo | Tipo | Obrigatório | Observações |
|---|---|---:|---|
| `relacaoIDComponenteOrigem` | `string` | Sim | ID do componente de origem. |
| `relacaoIDComponenteDestino` | `string` | Sim | ID do componente destino. |
| `relacaoTipo` | `string` | Não | Tipo de relação (vocabulário a definir). |

---

### 4.8 `inibidor_schema.json` — Inibidor
**Finalidade:** representar inibidores (ex.: restrições, impedimentos, bloqueios, DRM, sigilo técnico etc.).

| Campo | Tipo | Obrigatório | Observações |
|---|---|---:|---|
| `tipo` | `string` | Sim | Categoria do inibidor (vocabulário aberto). |
| `chave` | `string` | Não | Token/identificador associado ao inibidor (quando aplicável). |

---

### 4.9 `software_schema.json` — Software
**Finalidade:** descrever software relacionado à criação/leitura do componente.

| Campo | Tipo | Obrigatório | Observações |
|---|---|---:|---|
| `nome` | `string` | Sim | Nome do software. |
| `versao` | `string` | Não | Versão do software. |
| `tipo` | `string` | Não | Categoria/tipo (vocabulário aberto). |

---

### 4.10 `hardware_schema.json` — Hardware
**Finalidade:** descrever hardware necessário/associado ao componente.

| Campo | Tipo | Obrigatório | Observações |
|---|---|---:|---|
| `nome` | `string` | Não | Nome/modelo. |
| `tipo` | `string` | Não | Categoria (scanner, drive, etc.). |
| `outrasInformacoes` | `string` | Não | Observações. |

---

### 4.11 `formato_schema.json` — Formato
**Finalidade:** descrever formato do componente (ex.: PDF/A-2b, TIFF, WAV).

| Campo | Tipo | Obrigatório | Observações |
|---|---|---:|---|
| `nome` | `string` | Sim | Nome do formato. |
| `versao` | `string` | Não | Versão/perfil do formato. |

---

### 4.12 `componente_schema.json` — Componente
**Finalidade:** entidade técnica (arquivo digitais) vinculada a Documento; concentra metadados técnicos e de preservação.

| Campo | Tipo | Obrigatório | Valores/Regra | Observações |
|---|---|---:|---|---|
| `earqComponenteId` | `string` | Sim |  | Identificador do componente. |
| `earqComponenteNomeOriginal` | `string` | Sim |  | Nome original do arquivo/item. |
| `earqComponenteTamanho` | `integer` | Não | ≥ 0 | Tamanho numérico. |
| `earqComponenteUnidadeMedidaTamanho` | `string (enum)` | Não | `KB` `MB` `GB` `TB` `CAIXA` `METRO LINEAR` | Mistura unidades digitais e físicas (ok, mas convém definir regra de uso). |
| `earqComponenteSoftwareCriacao` | `Software` (`$ref`) | Não |  | Software de criação. |
| `earqComponenteDataCriacao` | `date` | Não |  | Data de criação. |
| `earqComponenteNivelComposicao` | `integer (enum)` | Não | `0` ou `1` | Nível de composição (precisa semântica explícita no padrão). |
| `earqComponenteInibidor` | `Inibidor` (`$ref`) | Não |  |  |
| `earqComponenteFormato` | `Formato` (`$ref`) | Não |  | É Altamente recomendado o preenchimento deste metadado quando o formato é conhecido, pois ele permite mapaer quais softwares são capazes de visualizar a informação do componente |
| `earqComponenteLocalizacao` | `Localização` (`$ref`) | Não |  |  |
| `earqComponenteSuporte` | `string (enum)` | Não | inclui `HD`, `CD-ROM`, `DVD`, `PAPEL`, etc. | Suporte físico/lógico. |
| `earqComponenteSoftwareLeitura` | `Software` (`$ref`) | Não |  |  |
| `earqComponenteHardware` | `Hardware` (`$ref`) | Não |  |  |
| `earqComponenteOutrasDependencias[]` | `Dependência` (`$ref`) | Não |  |  |
| `earqComponenteRelacionamentos[]` | `Relacionamento` (`$ref`) | Não |  | Alguns compoentes estabelcem relações com outros compoentes, a exemplo um componete pode ser parte de outra, aprofundando esse exemplo um comoente que represernta um arquivo de folha de estilos "CSS" é relacionado com o compoente que presenta um arquivo html. É Altamente recomendado o preenchimento deste metadado.   |
| `earqComponenteFixidade` | `Fixidade` (`$ref`) | Não |  | Hash do componente. |
| `earqComponenteAssinaturas[]` | `Assinatura` (`$ref`) | Não |  | Assinaturas associadas. |
| `earqEventos[]` | `Evento` (`$ref`) | Não |  | Trilha de eventos do componente. |

---

### 4.13 `documento_schema.json` — Documento
**Finalidade:** entidade intelectual e de gestão (metadados descritivos, classificação, acesso, vínculos e lista de componentes e eventos).

**Campos principais (seleção, por relevância):**

| Campo | Tipo | Obrigatório | Valores/Regra | Observações |
|---|---|---:|---|---|
| `earqDocumentoId` | `string` | (Sim, intenção do schema) |  | Identificador do documento. |
| `DcTitle` | `string` | Sim |  | Título. |
| `DcDescription` | `string` | Não |  | Descrição. |
| `DcSubject[]` | `array(string)` | Não |  | Assuntos. |
| `dc.relation[]` | `array(obj)` | Não |  | Relações com outros documentos (mínimo `EarqDocumentoId`). |
| `dc.date.issued` | `date` | Sim |  | Definida como “data de produção/finalização”. este elemento do padrão dublin core é o que melhor representa a data de produção do documento, ou seja a data em que o documento é finalizado, assinado e passa a ter efeito |
| `earqDocumentoNumero` | `string` | Não |  | Número do documento. |
| `earqDocumentoProtocolo` | `string` | Não |  | Protocolo. |
| `earqProcessoDossieId` | `string` | Não |  | Vincula a dossiê/processo. |
| `earqVolumeId` | `string` | Não |  | Vincula a volume. |
| `earqDocumentoMeio` | `string (enum)` | Sim | `digital` \| `não digital` \| `híbrido` | Meio. |
| `earqDocumentoStatus` | `string (enum)` | Sim | minuta/original/cópia/... | Status. |
| `earqDocumentoVersao` | `integer` | Sim | ≥ 1 | Versão. |
| `earqDocumentoAutor` | `Agente` (`$ref`) | Sim |  | Autor. |
| `earqDocumentoDestinatario` | `Agente` (`$ref`) | Sim |  | Destinatário. |
| `earqDocumentoOriginador` | `Agente` (`$ref`) | Não |  | Originador. |
| `earqDocumentoRedator` | `Agente` (`$ref`) | Sim |  | Redator. |
| `earqDocumentoInteressado` | `Agente` (`$ref`) | Não |  | Interessado. |
| `earqDocumentoGenero` | `string (enum)` | Não | textual/audiovisual/... | Gênero documental. |
| `earqDocumentoEspecie` | `string` | Não |  | Espécie. |
| `earqDocumentoTipo` | `string` | Não |  | Tipo. |
| `dc.language` | `string (pattern)` | Não | `^[a-z]{3}$` | Idioma (3 letras), conforme ISO 639-2:1998|
| `earqDocumentoQtFolha` | `integer` | Não | ≥ 0 | Qt. folhas. |
| `earqDocumentoSequencia` | `string` | Não |  | Sequência. |
| `earqDocumentoAnexo` | `boolean` | Não |  | Se é anexo. |
| `earqDocumentoPrevisaoDesclassificacao` | `date` | Não |  | Data prevista. |
| `earqClasseId` | `object` | Não | `codigo` obrigatório | Classe/código/plano. |
| `earqDocumentoDestinacao` | `string (enum)` | Não | transferência/eliminação/recolhimento | Destinação. |
| `earqDocumentoPrazoDeGuarda` | `date` | Não |  | Prazo (como data). |
| `earqDocumentoLocalizacao` | `object` | Não |  | Localização física administrativa (endereço/sigla/obs). |
| `earqNivelDeAcessoId` | `string (enum)` | Não | público/restrito/reservado/classificado | Nível de acesso. |
| `earqDocumentoIndicacaoAnotacao` | `boolean` | Não |  | Indicação de anotação. |
| `earqDocumentoSetorExecucao` | `string` | Não |  | Setor. |
| `earqComponentes[]` | `array(Componente)` | Não |  | Componentes associados. |
| `earqEventos[]` | `array(Evento)` | Não |  | Trilha de eventos do documento. |

⚠️ **Atenção (validação):** alinhar `required` com os nomes de `properties` (há divergências de *case* e também campos requeridos que não aparecem com o mesmo nome).

#### 4.13.1 Detalhamento das definições (pendências de especificação)
- **Detalhar melhor** o metadado `earqDocumentoGenero`.
- `earqDocumentoPrevisaoDesclassificacao`: detalhar o porquê de esse metadado não existir no padrão e existir no e-ARQ Brasil. Discutimos que a existência deste metadado é para uma possível estratégia de implementação, podendo ele ser calculado a partir do evento de classificação de sigilo (`ECV6` e `ECV7`).
- `earqDocumentoDestinacao`: não é obrigatório no padrão, apesar de ser no e-ARQ Brasil, para o caso do uso do padrão ser trâmite externo; neste caso, a destinação final será dada no destinatário do trâmite (órgão acumulador final do documento).
- `earqDocumentoPrazoDeGuarda`: mesmo caso de `earqDocumentoDestinacao`.
- `earqDocumentoSetorExecucao`: não é obrigatório no padrão, apesar de ser no e-ARQ Brasil, para o caso do uso do padrão ser trâmite externo; neste caso, o órgão receptor do trâmite é quem define o setor de execução.

---

## 5) Exemplo mínimo de uso (modelo mental)

### 5.1 Documento com 1 componente e 2 eventos (exemplo conceitual)

- **Documento** descreve o que é (título, data, autor etc.).
- **Componente** descreve em que forma existe (arquivo PDF, tamanho, formato, fixidez).
- **Eventos** descrevem o que aconteceu (ex.: “captura”, “verificação de fixidez”).

Como os schemas ainda estão em desenvolvimento e com inconsistências de nomenclatura, recomendo usar este exemplo como “guia de preenchimento” e só depois ajustar para validação estrita quando os nomes/cases forem estabilizados.

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
- desmembrar o conteúdo para ingestão em repositório;
- gerar novo pacote para trâmite subsequente.



## 8 Empacotamento para Envio a RDC-Arq (SIP)

### 8.1 Objetivo

Esta seção define o padrão de empacotamento para envio de Documentos, Processos e Dossiês a um **Repositório Arquivístico Digital Confiável (RDC-Arq)**.

O empacotamento descrito nesta seção corresponde à criação de um **SIP (Submission Information Package)**, conforme definido no modelo OAIS.

O objetivo é:

- garantir interoperabilidade entre sistemas produtores e o RDC-Arq;
- assegurar integridade e autenticidade da informação transferida;
- permitir validação automatizada do pacote de ingestão;
- manter rastreabilidade da transferência arquivística;
- facilitar processos de ingestão automatizada no repositório.

---

### 8.2 Modelo OAIS e Pacotes de Informação

O **OAIS (Open Archival Information System)** é um modelo de referência internacional para sistemas de preservação digital de longo prazo.

O modelo define três tipos principais de pacotes de informação:

| Tipo de Pacote | Função |
|---|---|
| SIP — Submission Information Package | Pacote submetido ao repositório para ingestão |
| AIP — Archival Information Package | Pacote armazenado e preservado pelo repositório |
| DIP — Dissemination Information Package | Pacote gerado para acesso ao usuário |

Neste manual, esta seção trata da criação do **SIP**, que é o pacote enviado ao RDC-Arq durante processos de transferência arquivística, recolhimento ou ingestão programada de documentos digitais.

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
| ID_GUIA | Identificador da guia de transferência ou recolhimento |
| CODIGO_CLASSIFICACAO_COMPLETO | Código de classificação completo |
| DATAHORAEMPACOTAMENTO | Data e hora de criação do pacote |

Regras:

- utilizar "_" como separador do código de classificação;
- não utilizar acentos;
- evitar espaços;
- utilizar apenas caracteres alfanuméricos, hífen e underscore.

Exemplo:

```
SIP-AN-MINISTERIO_DA_JUSTICA-GT2026-04-02_01_03-20260402T143500
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
- ingestão;
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
SIP-IR-AN-MINISTERIO_DA_JUSTICA-GT2026
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
