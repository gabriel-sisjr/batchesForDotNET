# Install Packages & ADD References & Do the Scaffold on Tables

```sh
echo off
title BAT Scaffold
cls
dotnet ef
@REM Checando se o comando anterior rodou sem erros.
set erros= %errorlevel%
IF %erros%==0 (
	:Packages
	set /p installPacks=Deseja instalar os pacotes para geracao automatica do scaffold "Caso repita, selecione novamente"? [y/n]: 
	
	@REM ==== Condições da primeira instalação ====
	IF "%installPacks%"=="y" goto Install
	IF "%installPacks%"=="n" goto Scaffold
	IF "%installPacks%"=="" goto Packages
	@REM ==== FIM Condições da primeira instalação ====
	
	:Scaffold
    	dotnet ef dbcontext scaffold ^
	"server=YourServer;user=YourUser;password=YourPass;database=YourDB" ^
	Microsoft.EntityFrameworkCore.SqlServer -v -f ^
	--context-dir Context -c ProBdCartaoContext ^
	-o ../Domain/Entidades ^
	-t TB_X -t TB_Y ^
	--no-onconfiguring ^
	--no-pluralize

	dotnet add reference ../Domain/Domain.csproj
	dotnet restore
	pause
	exit
	
	:Install
	dotnet add package Microsoft.EntityFrameworkCore.Design
	dotnet add package Microsoft.EntityFrameworkCore.Tools
	dotnet add package Microsoft.EntityFrameworkCore.SqlServer
	set /p gerarScaffold= Pacotes Instalados, gerar scaffold? [y/n]: 
	IF "%gerarScaffold%"=="y" goto Scaffold
	pause
)

IF NOT %erros%==0 (
	:DotTool
    set /p dotTool= Voce nao possui a ferramenta de scaffold instalada globalmente, deseja instalar? [y/n]: 
	IF "%dotTool%"=="y" (
		dotnet tool install --global dotnet-ef
		echo Ferramenta Instalada, pressione uma tecla para instalar os pacotes...
		pause >nul
		goto Install
	)
	IF "%dotTool%"=="" goto DotTool
	
	pause
)
```
