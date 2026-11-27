# projetos

## Configuração no VS Code

Abra o VS Code, selecione **File → Open Folder** e escolha a pasta `portal_cursos`. Em seguida, abra o terminal integrado pelo menu **Terminal → New Terminal**.

### 3. Criando o ambiente virtual Python

O ambiente virtual permite instalar as bibliotecas do projeto sem interferir em outras instalações Python. Execute no terminal do VS Code:

```powershell
python -m venv venv
```

Ative o ambiente virtual no PowerShell:

```powershell
.\venv\Scripts\Activate.ps1
```
Se o PowerShell bloquear a execução do script, permita a execução apenas na sessão atual:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
```

### 4. Instalando Django e o conector MySQL

Com o ambiente virtual ativado, instale o Django diretamente pelo terminal do VS Code:

```powershell
pip install django
```

Instale o conector para comunicação com o MySQL:

```powershell
pip install mysqlclient
```

### 5. Criando o projeto Django

No terminal do VS Code, execute:

```powershell
django-admin startproject portal_cursos .
```

A estrutura criada será semelhante a esta (`venv/` foi criada na etapa 3):

```text
portal_cursos/                 # pasta do projeto
├── manage.py
├── portal_cursos/              # configurações do Django
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   ├── asgi.py
│   └── wsgi.py
└── venv/
```

O Django não cria um arquivo `requirements.txt` com esse comando.

Execute o servidor de desenvolvimento:

```powershell
python manage.py runserver
```

Abra [http://127.0.0.1:8000/](http://127.0.0.1:8000/) no navegador. Se a página inicial do Django aparecer, o projeto foi criado corretamente. Para interromper o servidor, pressione **Ctrl + C** no terminal.

### 6. Criando o banco de dados MySQL

Com o MySQL instalado, abra o MySQL Workbench, conecte-se ao servidor local e execute:

```sql
CREATE DATABASE portal_cursos;
```

### 6.1 Configurando a conexão no Django

Abra `portal_cursos/settings.py`, localize `DATABASES` e configure a conexão usando o `mysqlclient` instalado anteriormente:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'portal_cursos',
        'USER': 'root',
        'PASSWORD': 'SUA_SENHA',
        'HOST': 'localhost',
        'PORT': '3306',
    }
}
```

Substitua `SUA_SENHA` localmente pela senha configurada no MySQL. Não publique nem faça commit da sua senha real.

Para testar a conexão e criar as tabelas iniciais do Django, execute:

```powershell
python manage.py migrate
```

### 7. Criando a primeira aplicação Django

Um projeto Django pode ser dividido em aplicações menores, cada uma responsável por um conjunto de funcionalidades. Para o Portal de Cursos, vamos começar com a aplicação `cursos`:

```powershell
python manage.py startapp cursos
```

Abra `portal_cursos/settings.py` e inclua `cursos` na lista `INSTALLED_APPS`:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'cursos',
]
```

A estrutura inicial do projeto está pronta para começar a desenvolver as funcionalidades.

### 8. Iniciando o desenvolvimento com Vibe Coding

Utilize o GitHub Copilot Chat no VS Code como agente de apoio. Quando disponível, selecione o modo **Agent**. O agente poderá analisar arquivos, propor alterações, criar arquivos e executar comandos conforme as permissões concedidas.

Os alunos devem revisar as alterações e ter cuidado especial com comandos que instalem pacotes, alterem configurações ou executem operações no banco de dados.

#### 8.1 Preparando a documentação

Antes de solicitar implementações, organize os documentos produzidos pela equipe em uma pasta `documentacao` na raiz do projeto:

```text
portal_cursos/                     # raiz do projeto
├── documentacao/
│   ├── documento_requisitos.md
│   ├── dicionario_dados.md
│   └── prototipo.md
├── cursos/
├── portal_cursos/                  # configurações do Django
├── manage.py
├── requirements.txt               # lista de dependências, mantida pela equipe
└── venv/                           # ambiente criado na etapa 3
```

Os documentos devem reunir o problema e os objetivos, os atores envolvidos, os requisitos funcionais e não funcionais, as regras de negócio, o modelo de dados e os protótipos das telas. O Django não cria `requirements.txt` automaticamente; mantenha esse arquivo com as dependências do projeto.

#### 8.2 Primeiro passo: apresentar o projeto à IA

Antes de pedir implementações, envie ao Copilot Chat uma apresentação do projeto e solicite uma análise. Você pode usar este prompt:

```text
Estamos desenvolvendo um Portal de Cursos como Projeto Integrador do curso Técnico em Informática.

O projeto utiliza Python, Django, MySQL e Bootstrap.

Você será nosso assistente de desenvolvimento e deverá nos auxiliar na construção da aplicação.

Antes de implementar qualquer código, analise os documentos disponíveis na pasta documentacao, considerando:
- Problema e objetivos;
- Atores envolvidos;
- Requisitos funcionais e não funcionais;
- Regras de negócio;
- Modelo de dados;
- Protótipos das telas.

Apresente uma análise geral do projeto, identificando possíveis inconsistências e informações ausentes.

Sugira uma organização das aplicações Django e das principais funcionalidades.
```

## PetSocial — integração da aplicação com Django e MySQL

O PetSocial é um portal para adoção de animais e conexão entre adotantes, tutores e ONGs. A interface web foi ligada a um backend Django com persistência MySQL. A prévia antiga do GitHub Pages continua estática; para usar cadastro, login, publicações, mensagens e uploads, é necessário executar o Django.

### Tecnologias e estrutura

- **Backend:** Python, Django 5.2 e `mysqlclient`.
- **Banco:** MySQL, configurado pelo arquivo local `.env`; migrations do Django criam as tabelas.
- **Frontend:** páginas HTML existentes, JavaScript e Bootstrap, servidas pelo Django.
- `api/`: modelos, administração, endpoints, URLs, migrations e testes.
- `config/`: configurações, URLs e ASGI/WSGI do projeto.
- `static/js/api.js`: helper same-origin para chamadas HTTP, CSRF, erros e escape de HTML.
- `static/js/chat-widget.js`: widget de chat.
- `media/`: uploads de imagens durante o desenvolvimento.
- `manage.py`: comandos do Django.

### Funcionalidades conectadas

- Cadastro, login, logout e sessão com autenticação Django e senhas armazenadas com hash.
- Anúncios de pets com imagem, espécie, idade, porte, descrição e status de adoção.
- Feed com curtidas; tutores/ONGs podem publicar e administrar seus anúncios.
- Solicitação de adoção, mensagem inicial ao tutor e notificação.
- Conversas e envio/leitura de mensagens entre usuários.
- Edição de perfil e foto.
- Stories de pets (visíveis por 24 horas) e histórias de sucesso com reações.
- Painel administrativo do Django.

A interface principal está em `index.html`; `historias.html` usa a API de histórias de sucesso. As páginas antigas `petsocial.html`, `login.html` e `chat.html` redirecionam para o fluxo Django. A autenticação usa sessões e proteção CSRF; imagens enviadas são validadas e limitadas a 800 KB.

### Modelos e dados

O app `api` define `Usuario`, `Pet`, `Publicacao`, `Comentario`, `Curtida`, `Adocao`, `Mensagem`, `Seguidor`, `ImagemPet` e `Notificacao`, além de `StoryMoment`, `HistoriaSucesso` e `CurtidaHistoria` para os recursos de stories. Há restrições de unicidade para impedir curtidas ou relacionamentos duplicados. O e-mail do usuário tem limite de 191 caracteres para compatibilidade com o limite de índices da instalação MySQL/WAMP usada no desenvolvimento.

### Configuração local no Windows

Requisitos: Python 3.10 ou superior, MySQL Server/WAMP e MySQL Workbench. Na raiz do projeto:

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
Copy-Item .env.example .env
```

Se a ativação do ambiente virtual for bloqueada pelo PowerShell, permita scripts somente na sessão atual e ative novamente:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
.\.venv\Scripts\Activate.ps1
```

No Workbench, crie o banco vazio (se ainda não existir):

```sql
CREATE DATABASE IF NOT EXISTS petsocial CHARACTER SET utf8mb4;
```

Edite `.env` com o nome do banco, host, porta e usuário da sua instalação MySQL. Defina uma chave aleatória em `DJANGO_SECRET_KEY`; por exemplo:

```powershell
python -c "import secrets; print(secrets.token_urlsafe(50))"
```

Em instalações WAMP locais nas quais `root` realmente não tem senha, `DB_PASSWORD=` pode ficar vazio. Isso é apenas uma configuração de desenvolvimento local; não use `root` sem senha em produção. O `.env` é ignorado pelo Git e não deve ser compartilhado. Use `.env.example` como referência, nunca coloque credenciais reais no repositório.

O projeto foi configurado para usar o banco `petsocial`. O schema existente `petsocial_db` não faz parte dessa configuração e deve permanecer separado. **Não crie as tabelas manualmente no Workbench:** o Django administra a estrutura por migrations. Execute:

```powershell
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```

Abra <http://127.0.0.1:8000/>. O painel administrativo está em <http://127.0.0.1:8000/admin/>. Para desligar o servidor, pressione **Ctrl+C** no terminal.

### API principal

| Método e rota | Uso |
| --- | --- |
| `GET /api/session/` | Consultar a sessão atual |
| `POST /api/register/`, `POST /api/login/`, `POST /api/logout/` | Cadastro e autenticação |
| `GET/POST /api/pets/` | Listar e publicar pets |
| `DELETE /api/pets/<id>/`, `POST /api/pets/<id>/status/` | Remover anúncio e alterar status (com autorização) |
| `POST /api/pets/<id>/likes/`, `POST /api/pets/<id>/adoptions/` | Curtir e solicitar adoção |
| `GET /api/conversations/`, `GET/POST /api/messages/` | Listar conversas e consultar/enviar mensagens |
| `PATCH /api/profile/`, `POST /api/profile/photo/` | Editar perfil e foto |
| `GET/POST /api/pet-stories/` | Consultar e publicar stories de pets |
| `GET/POST /api/success-stories/`, `POST /api/success-stories/<id>/likes/` | Histórias de sucesso e reações |

### Testes e banco

Os testes do app usam SQLite em memória, sem alterar o banco MySQL:

```powershell
python manage.py test api --settings=config.test_settings
```

As migrations do PetSocial foram aplicadas com sucesso ao schema `petsocial` na instalação WAMP de desenvolvimento. Para conferir as tabelas pelo Workbench, selecione/atualize `petsocial` e execute `SHOW TABLES;`.
## Análise preliminar do projeto PetSocial

> **Limitação da análise:** no repositório `AnnaBeatriz2000/projeto` só foi localizado este `README.md`; não há pasta `documentacao/` nem arquivos separados de requisitos, dicionário de dados ou protótipo. A análise abaixo é preliminar, baseada na descrição do README e na implementação atual do PetSocial. Para confirmar o alinhamento com o trabalho da equipe, esses documentos ainda precisam ser adicionados.

### Problema, objetivo e atores

O PetSocial busca conectar pessoas interessadas em adotar a animais disponíveis, aproximando adotantes, tutores e ONGs. Os atores identificados na aplicação são **adotante**, **tutor**, **ONG** e **administrador**. Ainda é necessário formalizar as responsabilidades e permissões de cada perfil e definir como medir o sucesso do serviço.

### Requisitos identificados

As funcionalidades presentes ou previstas na aplicação incluem cadastro, login, perfis, anúncios de pets, feed e curtidas, solicitação de adoção, conversas e mensagens, stories temporários e histórias de sucesso. Comentários, seguidores e notificações têm modelos no banco, mas não têm operações completas na API atual.

Python, Django, MySQL e Bootstrap estão definidos como tecnologias. Requisitos não funcionais ainda precisam ser acordados e testáveis: acessibilidade, uso em dispositivos móveis, desempenho, privacidade, retenção de dados, backups e disponibilidade. A autenticação usa sessões Django e proteção CSRF; imagens enviadas têm limite de tamanho.

### Regras de negócio observadas e decisões pendentes

- Tutores e ONGs podem publicar anúncios de pets; adotantes não podem.
- Um usuário não pode solicitar a adoção do próprio pet; solicitações duplicadas são verificadas pela aplicação.
- As curtidas possuem restrição para evitar duplicidade.
- Stories de pets ficam disponíveis por 24 horas.
- O fluxo de aprovação ou recusa de adoções e os efeitos de cada etapa no status do anúncio ainda precisam ser definidos.
- É necessário decidir quem pode publicar histórias de sucesso, quais categorias são aceitas e como denúncias e moderação funcionam.

### Modelo de dados e inconsistências a validar

Os modelos incluem usuários, pets, publicações, comentários, curtidas, adoções, mensagens, seguidores, imagens, notificações, stories e histórias de sucesso. Confirme se cada publicação deve estar ligada a exatamente um pet e se o tutor da publicação deve sempre ser o tutor cadastrado do pet. A idade está armazenada como texto, embora seja preciso decidir se o requisito pede uma faixa etária ou idade numérica com unidade. Também faltam definições documentadas de campos obrigatórios, formatos, estados válidos e regras de exclusão/retensão.

### Protótipos e lacunas

O README aponta para um [protótipo no Figma](https://www.figma.com/make/1kT8CnOWGkEgfTolfZbFzh/Pet-Social-Login-Page?t=GaJowEEYZSWFmBiV-20&fullscreen=1), mas não há telas exportadas ou especificações de fluxo para comparar com os requisitos. Recomenda-se incluir na documentação os fluxos de cadastro, publicação de pet, solicitação de adoção, conversa e administração, além de registrar inconsistências e decisões aprovadas pela equipe.

### Organização Django sugerida

Para o MVP, a aplicação atual `api` pode continuar concentrando os endpoints. À medida que o projeto crescer, uma divisão por responsabilidade facilitaria a manutenção:

- `accounts`: usuários, perfis, autenticação e papéis;
- `pets`: cadastro, imagens, anúncios e status dos pets;
- `adoptions`: solicitações e fluxo de aprovação;
- `social`: publicações, comentários, curtidas, seguidores e notificações;
- `messaging`: conversas e mensagens;
- `stories`: stories temporários e histórias de sucesso.

A configuração do projeto permanece em `config`. Antes de ampliar funcionalidades, a equipe deve consolidar `documento_requisitos.md`, `dicionario_dados.md` e `prototipo.md` em `documentacao/`.
