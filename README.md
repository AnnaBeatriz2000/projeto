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