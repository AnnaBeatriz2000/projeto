# Dicion?rio de dados ? PetSocial

**Status:** rascunho t?cnico baseado nos modelos Django atuais; confirmar com o dicion?rio aprovado pela equipe antes de tratar estes tipos e restri??es como definitivos. Os nomes de tabela informados s?o os `db_table` do Django. `FK` significa chave estrangeira; `M2M`, rela??o muitos-para-muitos. Campos `blank=True` s?o opcionais na valida??o do formul?rio; campos com `null=True` tamb?m aceitam `NULL` no banco.

## Conven??es

- IDs usam `BigAutoField`, salvo os campos herdados do Django.
- Datas usam `DateTimeField` com valor inicial de hora atual.
- Uploads usam `FileField` e armazenam caminho para arquivo em `media/` no desenvolvimento.
- A senha ? gerenciada pelo Django e armazenada somente como hash; nunca armazenar senha em texto puro.
- A aplica??o cria/altera as tabelas por migrations; n?o executar DDL manual para estas tabelas.

## Entidades

### `usuarios` ? `Usuario`

| Campo | Tipo/limite | Regras e rela??es |
| --- | --- | --- |
| `id_usuario` | BigAutoField | Chave prim?ria. |
| `nome` | CharField(120) | Obrigat?rio. |
| `email` | EmailField(191) | Obrigat?rio, ?nico; identificador de login. Limite de 191 compatibiliza ?ndices com o MySQL/WAMP usado. |
| `password` | CharField(128), herdado | Hash de senha Django; nunca texto puro. |
| `tipo_usuario` | CharField(30) | `adotante`, `tutor` ou `ong`; padr?o `adotante`. |
| `telefone` | CharField(30) | Opcional. |
| `bio` | TextField | Opcional. |
| `foto_perfil` | FileField | Opcional; arquivo em `perfis/`. |
| `data_cadastro` | DateTimeField | Data de cadastro. |
| `last_login` | DateTimeField | Herdado; aceita nulo. |
| `is_active`, `is_staff`, `is_superuser` | BooleanField | Estado da conta e permiss?es Django. |
| `groups`, `user_permissions` | M2M | Rela??es de autoriza??o herdadas do Django. |

### `pets` ? `Pet`

| Campo | Tipo/limite | Regras e rela??es |
| --- | --- | --- |
| `id_pet` | BigAutoField | Chave prim?ria. |
| `id_usuario` | FK para `usuarios` | Dono/tutor do an?ncio; exclus?o do usu?rio exclui seus pets. |
| `nome` | CharField(100) | Obrigat?rio. |
| `especie` | CharField(60) | `cao`, `gato` ou `outro`; padr?o `cao`. |
| `raca` | CharField(100) | Opcional. |
| `idade` | CharField(30) | Opcional; atualmente texto, formato precisa ser decidido. |
| `porte` | CharField(30) | Opcional: `Pequeno`, `M?dio` ou `Grande`. |
| `descricao` | TextField | Obrigat?ria. |
| `status_adocao` | CharField(30) | `disponivel`, `em-processo` ou `adotado`; padr?o `disponivel`. |
| `data_cadastro` | DateTimeField | Data de cria??o. |

### `publicacoes` ? `Publicacao`

| Campo | Tipo/limite | Regras e rela??es |
| --- | --- | --- |
| `id_publicacao` | BigAutoField | Chave prim?ria. |
| `id_usuario` | FK para `usuarios` | Autor da publica??o. |
| `id_pet` | OneToOne para `pets` | Cada pet pode estar associado a uma ?nica publica??o. Confirmar regra com a equipe. |
| `conteudo` | TextField | Texto da publica??o. |
| `data_publicacao` | DateTimeField | Data da publica??o. |

### `comentarios` ? `Comentario`

`id_comentario` (PK, BigAutoField), `id_publicacao` (FK para `publicacoes`), `id_usuario` (FK para `usuarios`), `comentario` (TextField obrigat?rio) e `data_comentario` (DateTimeField). Ordena??o crescente por data; endpoint ainda pendente.

### `curtidas` ? `Curtida`

`id_curtida` (PK, BigAutoField), `id_publicacao` (FK para `publicacoes`), `id_usuario` (FK para `usuarios`) e `data_curtida` (DateTimeField). Par publica??o/usu?rio ?nico.

### `adocoes` ? `Adocao`

`id_adocao` (PK, BigAutoField), `id_pet` (FK para `pets`, protegido contra exclus?o enquanto houver ado??o), `id_adotante` (FK para `usuarios`), `data_adocao` (DateTimeField) e `status` (CharField(30): `solicitada`, `aprovada`, `concluida` ou `recusada`; padr?o `solicitada`). Restri??es sobre solicita??es simult?neas e transi??es ainda precisam ser especificadas.

### `mensagens` ? `Mensagem`

`id_mensagem` (PK, BigAutoField), `id_remetente` e `id_destinatario` (FKs para `usuarios`), `mensagem` (TextField), `data_envio` (DateTimeField) e `lida` (BooleanField, padr?o falso).

### `seguidores` ? `Seguidor`

`id_seguidor` (PK, BigAutoField), `id_usuario` e `id_usuario_seguido` (FKs para `usuarios`) e `data_seguimento` (DateTimeField). O par ? ?nico e uma pessoa n?o pode seguir a si mesma; API ainda pendente.

### `imagens_pet` ? `ImagemPet`

`id_imagem` (PK, BigAutoField), `id_pet` (FK para `pets`), `url_imagem` (FileField obrigat?rio, caminho em `pets/`), `descricao` (TextField opcional) e `data_upload` (DateTimeField).

### `notificacoes` ? `Notificacao`

`id_notificacao` (PK, BigAutoField), `id_usuario` (FK para `usuarios`), `titulo` (CharField(150)), `mensagem` (TextField), `lida` (BooleanField, padr?o falso) e `data_notificacao` (DateTimeField). Endpoint/fluxo de leitura ainda pendente.

### `stories_pet` ? `StoryMoment`

`id_story` (PK, BigAutoField), `usuario_id` (FK para `usuarios`), `nome_pet` (CharField(100)), `imagem` (FileField em `stories/`) e `data_criacao` (DateTimeField). A consulta da API considera somente os ?ltimos 24 horas; n?o h? expira??o autom?tica do arquivo documentada.

### `historias_sucesso` ? `HistoriaSucesso`

`id_historia` (PK, BigAutoField), `usuario_id` (FK para `usuarios`), `nome_pet` (CharField(100)), `tipo` (CharField(30), valores ainda n?o enumerados), `descricao` (TextField), `foto_antes` e `foto_depois` (FileFields em `historias/antes/` e `historias/depois/`) e `data_publicacao` (DateTimeField).

### `curtidas_historias` ? `CurtidaHistoria`

`id_curtida` (PK, BigAutoField), `historia_id` (FK para `historias_sucesso`), `usuario_id` (FK para `usuarios`) e `data_curtida` (DateTimeField). Par hist?ria/usu?rio ?nico.

## Relacionamentos resumidos

- Um usu?rio pode cadastrar v?rios pets, publica??es, coment?rios, mensagens, ado??es, imagens de perfil, notifica??es, stories e hist?rias.
- Um pet pertence a um usu?rio e tem v?rias imagens; atualmente pode ter no m?ximo uma publica??o.
- Publica??es t?m coment?rios e curtidas.
- Solicita??es de ado??o ligam pet e adotante; mensagens ligam remetente e destinat?rio.
- Seguidores ligam dois usu?rios; curtidas de hist?rias ligam usu?rio e hist?ria.

## Pontos a confirmar com a equipe

Definir cat?logo final de esp?cies, portes, idades e tipos de hist?ria; campos obrigat?rios e opcionais; limite e formatos de imagem; regras de exclus?o; se pet pode ter v?rias publica??es; integridade entre tutor da publica??o e tutor do pet; e cardinalidade/limites de solicita??es de ado??o.
## Recorte do dicionário original recebido

A imagem enviada mostra a tabela completa `usuarios` e apenas o início da tabela `animais`. Os campos abaixo reproduzem somente o trecho legível; não foram inferidos os campos que ficaram fora do recorte.

### `usuarios` (definição do documento recebido)

| Campo | Tipo e tamanho | Chave | Nulo | Descrição informada |
| --- | --- | --- | --- | --- |
| `id_usuario` | INT | PK | Não | Identificador único do usuário. |
| `nome` | VARCHAR(100) | — | Não | Nome completo do usuário. |
| `email` | VARCHAR(150) | UNIQUE | Não | E-mail utilizado para login. |
| `senha` | VARCHAR(255) | — | Não | O documento descreve como “senha criptografada”; implementar e documentar como hash não reversível. |
| `tipo_usuario` | ENUM | — | Não | Valores visíveis: `ADOTANTE`, `ONG_DOADOR` ou `ADMIN`. Confirmar se `ONG_DOADOR` representa ONGs e tutores/protetores em um único papel. |
| `telefone` | VARCHAR(20) | — | Sim | Número de telefone. |
| `data_cadastro` | DATETIME | — | Não | Data de cadastro no sistema. |

### `animais` (trecho visível)

| Campo | Tipo e tamanho | Chave | Nulo | Descrição informada |
| --- | --- | --- | --- | --- |
| `id_animal` | INT | PK | Não | Identificador do animal. |
| `nome` | VARCHAR(100) | — | Não | Nome do animal. |
| `especie` | VARCHAR(50) | — | Não | Espécie (cão, gato etc.). |
| `raca` | VARCHAR(100) | — | Sim | Raça do animal. |

A captura adicional mostra mais campos de `animais` e a tabela `solicitacoes_adocao`; as demais páginas/tabelas ainda não estão legíveis no material recebido.

## Diferenças entre o recorte original e o modelo Django atual

| Item | Dicionário recebido | Modelo atual | Decisão pendente |
| --- | --- | --- | --- |
| Entidade de animais | Tabela `animais`, chave `id_animal` | Tabela `pets`, chave `id_pet` | Confirmar se o nome canônico deve ser `animais` ou `pets`. |
| Identificador | `INT` | `BigAutoField` | Escolher o tipo conforme o banco e os requisitos de crescimento. |
| Nome de usuário | `VARCHAR(100)` | `CharField(120)` | Alinhar o limite aprovado. |
| E-mail | `VARCHAR(150)`, único | `EmailField(191)`, único | Alinhar tamanho e validação; o limite atual atende à instalação MySQL/WAMP usada. |
| Senha | `VARCHAR(255)` | Campo Django de senha com hash | Registrar que é hash de senha, não criptografia reversível; confirmar o tamanho necessário para os hasheadores adotados. |
| Tipo de usuário | `ADOTANTE`, `ONG_DOADOR`, `ADMIN` | `adotante`, `tutor`, `ong`, além de flags de administrador Django | Definir papéis oficiais e mapear migração/permissões antes de alterar o sistema. |
| Telefone | `VARCHAR(20)`, opcional | `CharField(30)`, opcional | Alinhar limite. |
| Espécie | `VARCHAR(50)` | `CharField(60)` limitado às opções `cao`, `gato`, `outro` | Confirmar lista de espécies e se o campo deve ser livre ou enumerado. |
| Raça | `VARCHAR(100)`, opcional | `CharField(100)`, opcional | Compatível no trecho visível. |

Este recorte atualiza a fonte de requisitos, mas não altera automaticamente os modelos nem as migrations: o banco `petsocial` já recebeu migrations e a equipe precisa aprovar nomes, tipos e papéis antes de uma migração de esquema.
## Complemento do dicionário original — captura adicional

A nova captura permite completar os campos restantes da tabela `animais` e registrar a tabela `solicitacoes_adocao`.

### Campos adicionais de `animais`

| Campo | Tipo e tamanho | Chave | Nulo | Descrição informada |
| --- | --- | --- | --- | --- |
| `idade` | INT | — | Sim | Idade em anos. |
| `sexo` | ENUM | — | Sim | `M` ou `F`. |
| `porte` | ENUM | — | Sim | Pequeno, Médio ou Grande. |
| `descricao` | TEXT | — | Sim | Características do animal. |
| `localizacao` | VARCHAR(150) | — | Sim | Cidade ou região. |
| `status_adocao` | ENUM | — | Não | Disponível, Em análise ou Adotado. |
| `data_cadastro` | DATETIME | — | Não | Data de cadastro. |
| `id_responsavel` | INT | FK | Não | Identificador da ONG ou doador responsável. |

### `solicitacoes_adocao` (definição do documento recebido)

| Campo | Tipo e tamanho | Chave | Nulo | Descrição informada |
| --- | --- | --- | --- | --- |
| `id_solicitacao` | INT | PK | Não | Identificador da solicitação. |
| `id_animal` | INT | FK | Não | Animal solicitado. |
| `id_adotante` | INT | FK | Não | Usuário interessado. |
| `data_solicitacao` | DATETIME | — | Não | Data da solicitação. |
| `status_solicitacao` | ENUM | — | Não | Pendente, Aprovada ou Rejeitada. |
| `observacoes` | TEXT | — | Sim | Informações complementares. |

### Diferenças adicionais para decidir antes de alterar o banco

- O dicionário define `idade` como número de anos e inclui `sexo` e `localizacao`; o modelo Django atual usa idade textual e não possui esses dois campos.
- `status_adocao` no documento usa “Em análise”; o código atual usa `em-processo`. Definir valores canônicos e um mapeamento antes de migrar.
- `id_responsavel` do documento corresponde semanticamente ao responsável do pet, mas o código usa `id_usuario`.
- O documento chama a tabela de solicitações `solicitacoes_adocao`, com `data_solicitacao` e `observacoes`; o código usa `adocoes`, `data_adocao` e não tem observações. Os estados também diferem (`Pendente/Aprovada/Rejeitada` versus `solicitada/aprovada/concluida/recusada`).

Essas diferenças foram registradas para revisão; não alterei modelos, migrations nem dados existentes.
