



@echo off
chcp 1252
SET ds_Instancia=.
SET ds_UseDatabase=cin2
SET ds_DirRaiz=%CD%
 
rem reset errorlevel para zero
ver >nul
cls
echo. -------------------------------
echo. Iniciando execucao de scripts
echo. -------------------------------
echo.
echo.
for /R %%j in (*.sql) do (
    echo == Executando: %%j
 
    SQLCMD -b -i "%%j" -S %ds_Instancia% -E -d %ds_UseDatabase% -o "%ds_DirRaiz%\Result.txt"
     
    if errorlevel 1 (
        echo !!!ERRO!!!
        echo ERRO: %%j >> "%ds_DirRaiz%\log_Scripts_Executados.txt"
        echo ERRO: %%j >> "%ds_DirRaiz%\log_Scripts_Executados_Detalhe.txt"
        if exist "%ds_DirRaiz%\Result.txt" copy "%ds_DirRaiz%\Result.txt" "%%j.erro.txt"
    ) else (
        echo OK
        echo OK: %%j >> "%ds_DirRaiz%\log_Scripts_Executados.txt"
        echo OK: %%j >> "%ds_DirRaiz%\log_Scripts_Executados_Detalhe.txt"
        )
    echo.
    if exist "%ds_DirRaiz%\Result.txt" type "%ds_DirRaiz%\Result.txt" >> "%ds_DirRaiz%\log_Scripts_Executados_Detalhe.txt"
    echo. >> "%ds_DirRaiz%\log_Scripts_Executados_Detalhe.txt"
 
)
del "%ds_DirRaiz%\Result.txt"
echo. -------------------------------
echo. Concluido!
echo. -------------------------------
echo.
echo.
echo.
Pause
