# Documento de requisitos ? PetSocial

**Status:** rascunho para valida??o da equipe  
**Base:** README do projeto e implementa??o atual do PetSocial. N?o substitui a aprova??o dos requisitos pelos respons?veis.

## 1. Problema e objetivo

O PetSocial busca facilitar a conex?o entre pessoas interessadas em adotar animais, tutores e organiza??es de prote??o animal. A plataforma oferece an?ncios de pets, contato entre usu?rios e compartilhamento de hist?rias. Os indicadores de sucesso (por exemplo, solicita??es atendidas ou ado??es conclu?das) ainda devem ser definidos pela equipe.

## 2. Atores

| Ator | Responsabilidade prevista |
| --- | --- |
| Visitante | Consultar conte?do p?blico e iniciar cadastro ou login. |
| Adotante | Consultar pets, curtir publica??es, solicitar ado??o, conversar com tutores e manter seu perfil. |
| Tutor | Manter perfil, publicar e administrar an?ncios pr?prios, publicar stories e responder contatos. |
| ONG | Usar as fun??es de tutor em nome da organiza??o; validar necessidade de campos institucionais e verifica??o. |
| Administrador | Administrar usu?rios e conte?do por meio do painel administrativo. |

Confirmar se visitantes podem ver todos os an?ncios e hist?rias e se ONGs precisam de verifica??o documental.

## 3. Requisitos funcionais

Os estados indicam o que existe na implementa??o atual; n?o representam aprova??o formal do requisito.

| ID | Requisito | Situa??o observada |
| --- | --- | --- |
| RF-01 | Criar conta com nome, e-mail, senha e tipo de perfil; autenticar e encerrar sess?o. | Implementado com sess?o Django e hash de senha. |
| RF-02 | Consultar e editar nome, biografia e foto de perfil. | Implementado. |
| RF-03 | Consultar o feed de pets e filtrar an?ncios por idade/porte. | Implementado na interface; definir pagina??o e filtros adicionais. |
| RF-04 | Tutor/ONG publicar an?ncio com nome, esp?cie, idade, porte, descri??o e imagem. | Implementado. |
| RF-05 | Autor ou administrador remover an?ncio e alterar seu status. | Implementado com verifica??o de permiss?o. |
| RF-06 | Usu?rio autenticado curtir/descurtir an?ncio, sem duplicar curtida. | Implementado com restri??o de unicidade. |
| RF-07 | Adotante solicitar pet dispon?vel; registrar solicita??o e notificar/contatar tutor. | Solicita??o e mensagem inicial implementadas; fluxo de decis?o ainda incompleto. |
| RF-08 | Consultar conversas e enviar mensagens entre usu?rios. | Implementado. Definir regras de contato, bloqueio e den?ncia. |
| RF-09 | Tutor/ONG publicar story de pet e consultar stories ativos. | Implementado; dura??o de 24 horas. |
| RF-10 | Usu?rio autenticado publicar hist?ria de sucesso com fotos de antes/depois e reagir. | Implementado; definir se publica??o deve ser restrita a tutor/ONG e categorias v?lidas. |
| RF-11 | Administrador gerenciar registros pelo painel Django. | Painel Django dispon?vel; validar quais a??es ser?o autorizadas. |
| RF-12 | Comentar, seguir usu?rios e consultar/marcar notifica??es. | Existem modelos de dados; endpoints e interface completos ainda n?o est?o dispon?veis. |
| RF-13 | Recuperar senha por e-mail. | Ainda n?o implementado; a interface informa que o recurso n?o est? configurado. |

## 4. Requisitos n?o funcionais

| ID | Requisito / decis?o | Situa??o |
| --- | --- | --- |
| RNF-01 | Backend Python/Django, banco MySQL e interface HTML/JavaScript/Bootstrap. | Tecnologias definidas no projeto. |
| RNF-02 | Proteger credenciais: hash de senha, sess?o e prote??o CSRF. | Implementado pelo Django. |
| RNF-03 | Validar uploads e limitar tamanho; armazenar arquivos de m?dia fora dos dados bin?rios do banco. | H? limite de 800 KB e `FileField`; validar conte?do real, tipos permitidos e armazenamento de produ??o. |
| RNF-04 | Responsividade, acessibilidade, navegadores suportados e metas de desempenho. | Crit?rios e testes precisam ser definidos. |
| RNF-05 | Privacidade, reten??o, backup, recupera??o e modera??o. | Pol?ticas ainda n?o documentadas. |
| RNF-06 | Segredos e credenciais n?o devem ser versionados; usar configura??o local por ambiente. | `.env` local ? ignorado pelo Git; revisar configura??o para produ??o. |

## 5. Regras de neg?cio observadas

1. Os tipos de conta existentes s?o `adotante`, `tutor` e `ong`.
2. Somente tutor e ONG podem publicar pets e stories tempor?rios.
3. Um usu?rio n?o pode solicitar a ado??o do pr?prio pet; o an?ncio precisa estar dispon?vel.
4. Curtidas de uma mesma pessoa na mesma publica??o n?o podem ser duplicadas; um usu?rio n?o pode seguir a si mesmo.
5. Stories tempor?rios s?o listados somente durante as 24 horas seguintes ? publica??o.
6. O propriet?rio do an?ncio ou administrador pode remov?-lo/alterar seu status.
7. Status de an?ncio: `disponivel`, `em-processo` e `adotado`. Status de solicita??o: `solicitada`, `aprovada`, `concluida` e `recusada`.

A equipe ainda precisa definir quem pode aprovar/recusar solicita??o, se um pet pode ter v?rias solicita??es abertas, o que acontece com as demais ap?s uma aprova??o e quais transi??es de status s?o permitidas.

## 6. Quest?es para valida??o da equipe

- Qual ? o fluxo completo de ado??o, incluindo an?lise, aprova??o, recusa e conclus?o?
- Idade ser? faixa (filhote/adulto) ou valor num?rico com unidade? Quais esp?cies, portes e condi??es de sa?de ser?o aceitos?
- Quais informa??es e documentos verificam uma ONG? Quem pode publicar hist?rias de sucesso?
- Coment?rios, seguidores, notifica??es e recupera??o de senha fazem parte do escopo atual?
- Quais dados s?o p?blicos, por quanto tempo s?o retidos e como o usu?rio solicita exclus?o?
- Quais metas de acessibilidade, suporte m?vel, desempenho, disponibilidade e backup s?o exigidas?
- Quais telas e fluxos do prot?tipo constituem crit?rios de aceite?