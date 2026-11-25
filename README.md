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