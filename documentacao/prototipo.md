# Prot?tipo textual e mapa de telas ? PetSocial

**Status:** atualiza??o funcional em rascunho, baseada nas p?ginas e fluxos atualmente ligados ? API. ? um invent?rio/wireframe textual, n?o substitui as telas visuais de alta fidelidade nem a aprova??o do prot?tipo pela equipe.

**Refer?ncia visual existente:** [Prot?tipo PetSocial no Figma](https://www.figma.com/make/1kT8CnOWGkEgfTolfZbFzh/Pet-Social-Login-Page?t=GaJowEEYZSWFmBiV-20&fullscreen=1). Validar e atualizar no Figma ap?s aprovar requisitos, paleta, conte?do, responsividade e estados de cada tela.

## Mapa de navega??o

```text
Visitante -> In?cio/Feed -> Login ou Cadastro
Adotante -> Feed -> Detalhe do pet -> Solicitar ado??o -> Conversa com tutor
Tutor/ONG -> Feed -> Publicar pet -> Meus pets -> Atualizar status
Tutor/ONG -> Stories de pets -> Publicar story
Usu?rio autenticado -> Perfil -> Editar dados/foto
Usu?rio autenticado -> Hist?rias de sucesso -> Publicar e reagir
Administrador -> Painel Django
```

## Telas e componentes

### 1. In?cio e autentica??o (`/`)

- Estado visitante: apresenta??o do PetSocial e a??es para entrar/criar conta.
- Login: e-mail e senha; link para recupera??o, que ainda n?o est? configurada.
- Cadastro: nome, e-mail, senha e perfil (adotante, tutor ou ONG).
- Estados necess?rios no prot?tipo: valida??o, erro de credencial, e-mail j? cadastrado, carregamento e sucesso.

### 2. Feed de ado??o (`/` ap?s login)

- Cabe?alho com navega??o, sess?o e perfil.
- Faixa de stories ativos.
- Filtros de idade e porte.
- Cart?es de pets com foto, nome, tutor, descri??o, idade, porte e curtidas.
- A??es de curtir, abrir perfil/conversa e solicitar ado??o.
- Formul?rio de an?ncio mostrado apenas para tutor/ONG: nome, descri??o, esp?cie, idade, porte e foto.
- Estados vazios: nenhum an?ncio/story; estados de erro e carregamento precisam ser revisados.

### 3. Detalhe e solicita??o de ado??o

- Exibir an?ncio, respons?vel e status atual.
- Permitir solicitar apenas se autenticado, se o an?ncio estiver dispon?vel e se a pessoa n?o for a respons?vel pelo pet.
- Confirmar o envio e explicar que o tutor receber? uma mensagem/aviso.
- A tela de an?lise pelo tutor (aprovar, recusar ou concluir) ainda precisa ser desenhada e implementada.

### 4. Perfil

- Exibir nome, foto e biografia.
- Formul?rio de edi??o para nome, tipo de perfil, biografia e foto.
- Definir se o tipo de perfil pode ser alterado livremente ou exige valida??o, especialmente para ONG.

### 5. Conversas e mensagens

- Lista de conversas e ?rea de mensagens com identifica??o do contato e hor?rio.
- Campo para compor e enviar mensagem; indicar mensagens lidas quando houver suporte visual.
- Ainda definir estados vazios, den?ncia/bloqueio, falha de envio e atualiza??o da conversa.

### 6. Stories tempor?rios

- Lista com nome do pet e imagem; visualizador com avan?o e fechamento.
- Publica??o permitida a tutor/ONG com nome do pet e imagem.
- Informar que o story some da listagem ap?s 24 horas.

### 7. Hist?rias de sucesso (`/historias.html`)

- Filtros por tipo; cart?es com imagens antes/depois, nome do pet, respons?vel, data, descri??o e rea??es.
- Formul?rio de publica??o com nome, tipo, texto e duas fotos.
- Definir categorias, permiss?o de publica??o, modera??o e textos alternativos das imagens.

### 8. Administra??o (`/admin/`)

- Painel administrativo Django para gest?o interna. Definir quais fun??es administrativas s?o necess?rias e quem pode acess?-las.

## Wireframe textual: feed

```text
????????????????????????????????????????????????????????????
? PetSocial                         Perfil | Conversas | Sair?
????????????????????????????????????????????????????????????
? Stories: [pet] [pet] [pet]                                ?
? Filtros: [Idade ?] [Porte ?]                              ?
? Publicar pet (somente tutor/ONG)                           ?
????????????????????????????????????????????????????????????
? [Foto] Nome do pet                 Tutor/ONG              ?
?        Descri??o ? idade ? porte                           ?
?        [Curtir] [Conversar] [Solicitar ado??o]             ?
?                                                          ?
? [Pr?ximo an?ncio...]                                      ?
????????????????????????????????????????????????????????????
```

## Crit?rios visuais e pend?ncias

- Conferir todos os fluxos com o Figma e manter consist?ncia entre desktop e celular.
- Definir acessibilidade: contraste, foco de teclado, r?tulos, mensagens de erro e texto alternativo para imagens.
- Especificar estados de carregamento, vazio, sucesso, erro e confirma??o para cada a??o.
- Definir navega??o em telas pequenas, modais e comportamento ap?s login/logout.
- As p?ginas legadas `login.html`, `chat.html` e `petsocial.html` redirecionam para o fluxo principal; revisar se links externos ainda dependem dessas URLs.
- Atualizar este documento e o Figma quando a equipe aprovar requisitos e regras de neg?cio.
## Atualização visual: login e cadastro

### Direção visual

- Referência de linguagem visual de redes sociais como o Instagram: apresentação limpa, foco no formulário e hierarquia visual simples, sem reproduzir marca, logotipo ou elementos proprietários.
- Paleta suave e acolhedora: fundo rosa-claro/lilás, gradiente rosa-lilás nos botões, texto ameixa escuro e superfícies brancas.
- Cartão central com cantos arredondados, sombra suave, campos claros e rótulos visíveis. A identidade PetSocial usa patinhas e ilustrações de cães/gatos.
- Em telas largas, composição dividida: painel de identidade/boas-vindas à esquerda e formulário à direita. Em celular, o painel ilustrativo é ocultado e o formulário ocupa a largura disponível.
- Implementação usa classes do Bootstrap 5 para grid, formulário e botões, com CSS próprio para as cores e o acabamento; as funções existentes de autenticação e cadastro são preservadas.

### Fluxo de login

1. Cabeçalho “Entrar”, mensagem acolhedora, campo de e-mail e campo de senha.
2. Ação principal “Entrar”, link “Esqueci minha senha” e atalho para cadastro.
3. Acesso administrativo separado visualmente das ações do usuário.

### Fluxo de cadastro

1. Cabeçalho “Criar conta” e campos de nome, e-mail, senha e tipo de perfil.
2. Ação principal “Criar minha conta” e atalho para entrar.
3. Tipos apresentados: adotante, tutor e ONG.

O layout está aplicado ao formulário da página principal do PetSocial (`index.html`). O arquivo do Figma não foi alterado; usar esta especificação como referência ao atualizar o protótipo visual e validar os estados de erro, foco, carregamento e responsividade.
