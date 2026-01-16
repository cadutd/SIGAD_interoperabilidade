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

- cadeia de custódia digital,
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
**Finalidade:** entidade técnica (arquivo, mídia, item) vinculada a Documento; concentra metadados técnicos e de preservação.

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
| `earqComponenteFormato` | `Formato` (`$ref`) | Não |  |  |
| `earqComponenteLocalizacao` | `Localização` (`$ref`) | Não |  |  |
| `earqComponenteSuporte` | `string (enum)` | Não | inclui `HD`, `CD-ROM`, `DVD`, `PAPEL`, etc. | Suporte físico/lógico. |
| `earqComponenteSoftwareLeitura` | `Software` (`$ref`) | Não |  |  |
| `earqComponenteHardware` | `Hardware` (`$ref`) | Não |  |  |
| `earqComponenteOutrasDependencias[]` | `Dependência` (`$ref`) | Não |  |  |
| `earqComponenteRelacionamentos[]` | `Relacionamento` (`$ref`) | Não |  |  |
| `earqComponenteFixidade` | `Fixidade` (`$ref`) | Não |  | Hash do componente. |
| `earqComponenteAssinaturas[]` | `Assinatura` (`$ref`) | Não |  | Assinaturas associadas. |
| `earqEventos[]` | `Evento` (`$ref`) | Não |  | Trilha de eventos do componente. |

---

### 4.13 `documento_schema.json` — Documento
**Finalidade:** entidade intelectual e de gestão (metadados descritivos, classificação, acesso, vínculos e lista de componentes e eventos).

**Campos principais (seleção, por relevância):**

| Campo | Tipo | Obrigatório | Valores/Regra | Observações |
|---|---|---:|---|---|
| `earqDocumentoId` | `string` | (Sim, intenção do schema) |  | Identificador do documento. ⚠️ `required` usa outro *case*. |
| `DcTitle` | `string` | Sim |  | Título. |
| `DcDescription` | `string` | Não |  | Descrição. |
| `DcSubject[]` | `array(string)` | Não |  | Assuntos. |
| `dc.relation[]` | `array(obj)` | Não |  | Relações com outros documentos (mínimo `EarqDocumentoId`). |
| `dc.date.issued` | `date` | Sim |  | Definida como “data de produção/finalização” (comentário no schema). |
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
| `dc.language` | `string (pattern)` | Não | `^[a-z]{3}$` | Idioma (3 letras). |
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
