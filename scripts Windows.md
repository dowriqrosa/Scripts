# Comandos windows

## abrir programa como administrado

```sh
runas /user:nome_do_usuario_administrador cmd
```
* pode ser utilizado para rodar scripts e programas

## acesso remoto CMD

```sh
PsExec.exe \\ipOuNome -s cmd
```

## alteração de senha VBS

```sh
Const WshFinished = 1
Const WshFailed = 2
strCommand = "net user"

Set WshShell = CreateObject("WScript.Shell")
Set WshShellExec = WshShell.Exec(strCommand)

While WshShellExec.Status = WshRunning
    WScript.Sleep 50
Wend

Select Case WshShellExec.Status
   Case WshFinished
       strOutput = WshShellExec.StdOut.ReadAll
   Case WshFailed
       strOutput = WshShellExec.StdErr.ReadAll
End Select

'WScript.Echo strOutput          'write results to default output
strOutput = InStr(1,strOutput,"administrator",1)
               'write results in a message box

const strComputer = "."
Set colAccounts = GetObject("WinNT://" & strComputer & ",computer")

If strOutput = 0 Then
	Set objUser = GetObject("WinNT://" & strComputer & "/administrador, user")
	objUser.SetPassword "1234522"
	objUser.SetInfo
	MsgBox strOutput 
Else
	Set objUser = GetObject("WinNT://" & strComputer & "/administrator, user")
	objUser.SetPassword "1234522"
	objUser.SetInfo
	MsgBox strOutput 
End If
```

## Apagar aquivos somente extensão especifica

```sh
del /q /s /f *.tmp
```

## Ativar instaler modo de segurança 

```sh
REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\SafeBoot\Minimal\MSIServer" /VE /T REG_SZ /F /D "Service"

REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\SafeBoot\Network\MSIServer" /VE /T REG_SZ /F /D "Service"

net start msiserver
```

## BAT bkp banco mysql

```sh
@echo off
cls

REM Define o usuário e senha do banco de dados
set dbUser=root
set dbPassword=root

REM Define a pasta que será feito o backup no padrão ...\<dia do mês>\<hora atual>
set backupDir=\\SRV-BKP\e$\invent\%date:~0,2%\%time:~0,2%\

REM Nome do arquivo que será gerado
set file=invent.sql

REM Caminho dos executáveis do mysqldump.exe, para executar o dump, e do 7z.exe, para compactar o arquivo
set mysqldump="C:\Program Files\MySQL\MySQL Server 8.0\bin\mysqldump.exe"
set zip="C:\Program Files\7-Zip\7z.exe"

REM Cria a pasta de backup caso não exista
if not exist "%backupDir%" (
    mkdir "%backupDir%"
)

REM Executa o dump, aqui precisa configurar o host e o nome do banco de dados (locais com xxx)
%mysqldump% --host="127.0.0.1" --user=%dbUser% --password=%dbPassword% invent > "%backupDir%\%file%"

REM Compacta o arquivo com o dump
%zip% a -tgzip "%backupDir%\%file%.gz" "%backupDir%\%file%"

REM Exclui o arquivo .sql original
del "%backupDir%\%file%"
```

## comando para exporta usuarios do grupo 
```sh
dsquery group -name NOME_DO_GRUPO | dsget group -members -expand | dsget user -fn -ln >> C:/LISTA.TXT
```

## delete todos os arquivos de uma pasta 

```sh
del /s /f /q C:\Windows\Temp\*.*
```

## Desabilitar firewall

```sh
netsh advfirewall set allprofiles state off
```

## Desabilitar tarefa agendada 

```sh
schtasks /Change /TN "\Microsoft\Windows\UpdateOrchestrator\Reboot" /disable
```

## Exporta chave de registro

```sh
reg export HKEY_CURRENT_USER\Printers\Connections  \\10.52.12.150\printReg\%COMPUTERNAME%-%USERNAME%-user.txt
reg export HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Print\Printers\ \\10.52.12.150\printReg\%COMPUTERNAME%-%USERNAME%-local.txt

Psexec reg export HKEY_CURRENT_USER\Printers\Connections c:\intel\puser.txt
Psexec reg export HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Print\Printers\ c:\intel\pcomputer.txt
```

## Limpeza WSUS

```sh
@echo on 

net stop wuauserv 

net stop cryptSvc

net stop msiserver

rd C:\Windows\SoftwareDistribution /s /q

rd C:\Windows\System32\catroot2 /s /q

REG add "HKLM\SYSTEM\CurrentControlSet\services\wuauserv" /v Start /t REG_DWORD /d 3 /f 

REG DELETE "HKLM\Software\Microsoft\Windows\CurrentVersion\WindowsUpdate" /v SusClientId /f

net start wuauserv

net start cryptSvc

net start msiserver

gpupdate /force

wuauclt /resetauthorization /detectnow /updatenow /ReportNow

wuauclt /resetauthorization /detectnow /updatenow /ReportNow

wuauclt /resetauthorization /detectnow /updatenow /ReportNow

wuauclt /resetauthorization /detectnow /updatenow /ReportNow

wuauclt /resetauthorization /detectnow /updatenow /ReportNow

wuauclt /resetauthorization /detectnow /updatenow /ReportNow

wuauclt /resetauthorization /detectnow /updatenow /ReportNow

wuauclt /resetauthorization /detectnow /updatenow /ReportNow

wuauclt /resetauthorization /detectnow /updatenow /ReportNow 
```

## Criar um link de uma pasta 

```sh
mklink /D Documentum \\ECMSTORAGE01\STORAGE01\PGE_PROD
```

## Lista de porta 

```sh
netstat -aon | findstr 8080
```

## habilitar netlogon e registro remoto

```sh
sc config Netlogon start=auto
sc start Netlogon

sc config remoteregistry start = auto
sc start remoteregistry
```

## Tomar propiedade da pasta 

```sh
takeown /f "pasta" /r /d y
```

## Redimensionar pasta System volume information

```sh
vssadmin resize shadowstorage /on=C: /For=C: /Maxsize=4GB
```

## Refazer partição de boot

```sh
O Que você vai precisar:
• 1 CD ou Pendrive com instalação do Windows ou disco de recuperação
• 1 Pendrive ou HD externo caso queira fazer backup.

1. Dê o boot com o a mídia de instalação ou recuperação do Windows

2. Pressione uma tecla caso seja solicitado.

3. Selecione teclado e idioma e clique em "Avançar".

4. Na próxima tela clique em "Reparar o computador" \ "Solução de Problemas" \ Opções avançadas \ Prompt de comando.

5. Digite diskpart e pressione ENTER para entrar no diskpart

6. Dentro do diskpart digite list disk para listar os discos conectados ao computador.

7. Digite select disk mais o número do disco onde fica o Windows por exemplo:
 
Select disk 0 (onde 0 é o disco que você quer selecionar)

8. Digite list volume para mostrar os volumes

9. Memorize a letra de unidade da partição do Windows e de seu pendrive (backup)

10. Especifique o seguinte comando:

bcdedit /export F:\md\backup\BCD\PC\boot

Onde F:\md\backup\BCD\PC é o caminho para onde desaja salvar e "boot" é o arquivo de configuração de boot.

11. Volte ao diskpart

12. Selecione o disco onde está o Windows

select disk 0

13. Selecione a patição EFI (Ela tem apróximadamente 100 MB e esta no formato de sistemas de arquivos Fat32)

ex:
list partition
select partition 2 (onde 2 é a sua partição EFI)
delete partition override

14. Crie uma nova partição EFI e atribua uma letra a ela:

create partition efi
format quick fs=fat32
assign letter=s (onde s pode ser qualquer letra a sua escolha)

15. Saia do diskpart

exit

16. Agora vamos gravar novos arquivos de boot na partição EFI:

bcdboot c:\windows /f UEFI /s s: (onde s: é letra que você atribuiu a partição EFi)

17. Use o comando bootrec para adicionar entradas ao arquivo de configuração de boot:

bootrec /rebuildbcd

18. Agora digite exit para sair do prompt de comando e reinicie o Windows.
```

## refazer refistro DNS

```sh
ipconfig /registerdns
```
## Reiniciar

```sh
shutdown -r -f -t 0
```

## senhas wifi salvas

```sh
netsh wlan export profile key=clear
```

## tamanho de nomes de arquivo

```sh
fsutil behavior set disable8dot3 `f`: 0
```

## Terminar processos de um usuario 

```sh
tasklist /S srv /U ba\forescout /FI "USERNAME eq ba\forescout"
taskkill /S SRV /U ba\crosa /FI "USERNAME eq ba\forescout"
```

## discpart comandos

```sh
@echo on

@echo ">>>>>>>>>>>>>> parada sercico cluster <<<<<<<<<<<<<<<<<" >> C:\Temp\log.log

sc query ClusSvc | find /I "STATE" | find "STOPPED" 
if errorlevel 1 goto :stopCluster
goto :changeLetter

:stopCluster
@echo "Stopping Cluster... " >> C:\Temp\log.log
sc stop ClusSvc
:whait_10S
rem cause a ~10 second sleep before checking the service state
ping 127.0.0.1 -n 10 -w 1000 > nul

sc query ClusSvc | find /I "STATE" | find "STOPPED" 
if errorlevel 1 goto :whait_10S

:changeLetter
diskpart /s C:\Temp\scriptDiskpart.txt
```

## Popup chrome app

```sh
if exist %programfiles(x86)%\Google\Chrome\Application\chrome.exe(
	"%programfiles(x86)%\Google\Chrome\Application\chrome.exe" --app="https://pt.surveymonkey.com/r/pgeterceirizadoseestagiarios"
) else (
	"%programfiles%\Google\Chrome\Application\chrome.exe" --app="https://pt.surveymonkey.com/r/pgeterceirizadoseestagiarios"
)
```


# Scripts voltados para o AD Active directory

## atualizar opção de usuario em lote 

```sh
Get-ADUser -SearchBase "OU" -Filter * -Properties PasswordNeverExpires | ? {$_.PasswordNeverExpires -eq "" -and $_.Name -notlike "HealthMailbox*"} | % {Set-ADUser -Identity $_.SamAccountName -PasswordNeverExpires $true -Confirm:$true}
```

## Desabilitar contas de usuarios .ps1

```sh
 $patch = Get-Content C:\temp\teste.txt
 
 foreach ($tree in $patch) {
   Disable-ADAccount  -Identity $tree
 }
```

## atualizar politicas de grupo 

```sh
gpupdate /force /boot
```

* Basicamente, como isso funciona é (uma vez que não recebe nenhuma política quando você executa o comando), ele aplica uma política vazia, que efetivamente remove a política presa de uma vez por todas.

## mover maquinas 

```sh
$computador = cat .\lista.txt
$ini = "CN="
$fim = ",OU"


$computador | % {Move-ADObject $ini$_$fim -TargetPath "OU Destino" }
```


## relatorio de usuario com dados especificos

```sh
Get-AdUser -SearchBase "OU" -Filter * -Properties *| Select-Object displayname, mail, SamAccountname, pager, company, telephoneNumber, uidNumber, uid, title, department, description, physicalDeliveryOfficeName  | Export-Csv C:\Estagiarios.csv -NoTypeInformation -Encoding Unicode
```

## Relatorio de usarios por grupo 

```sh
dsquery group -name <NOME DO GRUPO>| dsget group -members -expand
```

## Mover objetos(usuario, computadores....) desabilitados

```sh
Search-ADAccount -SearchBase "OU=" -AccountDisabled | Where {$_.DistinguishedName -notlike "*OU=DESABILITADO*"} | Move-ADObject -TargetPath "OU=DESABILITADO"
```

## Lista objetos(usuario, computadores....) desabilitado

```sh
Search-ADAccount -SearchBase "OU=" -AccountDisabled | Where {$_.DistinguishedName -notlike "*OU=DESABILITADO*"} | Select-Object Name, DistinguishedName > c:\txt.txt
```

## Desabilitar objetos(usuario, computadores....) por tempo de inatividade

```sh
$timespan = New-Timespan -Days 90
Search-ADAccount -SearchBase "OU=ESTAGIARIOS/TERCERIZADOS,OU=PGE,DC=PGEBA,DC=INTRANET" –UsersOnly –AccountInactive –TimeSpan $timespan | Disable-ADAccount
```
