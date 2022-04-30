# Configure-PowerShell

- Para configurar o POWER SHELL faça a instalação do [POSH-GIT](https://github.com/dahlbyk/posh-git)

`comando de instalação:
 PowerShellGet\Install-Module posh-git -Scope CurrentUser -Force`


- Após essa instalação execute o comando no terminal:

`code  $profile.CurrentUserCurrentHost`

> Precisa ter o VSCODE INSTALADO para executar o comando "code" no terminal.


- Cole as informações abaixo dentro do arquivo que será aberto.
> Não esqueça de deixar a linha 1 vazia.
```bash
# Limpando Console
Clear-Host

# Importando módulos
Import-Module posh-git
Import-Module PSReadLine

# Autocomplete - Teclas de atalho
Set-PSReadLineKeyHandler -Key Tab -Function MenuComplete
Set-PSReadLineKeyHandler -Key UpArrow -Function HistorySearchBackward
Set-PSReadLineKeyHandler -Key DownArrow -Function HistorySearchForward  
# Set-PSReadLineKeyHandler -Key Tab -Function Complete

# autocomplete - Funcionamento
Set-PSReadLineOption -ShowToolTips;
Set-PSReadLineOption -HistoryNoDuplicates;
Set-PSReadLineOption -PredictionSource History;

$ROOT = Split-Path -Parent $MyInvocation.MyCommand.Path

# Sobrescrevendo a função ls
Import-Module $ROOT\Scripts\powerls.psm1
Set-Alias -Name ls -Value PowerLS -Option AllScope
```

- Instale o [PSReadLine](https://github.com/PowerShell/PSReadLine)

```
Install-Module PSReadLine -AllowPrerelease -Force

```


- Apos isso dê um ```echo $ROOT``` no seu terminal, abra o caminho. 
- Abra a pasta SCRIPT e crie um arquivo com o nome powerls.psm1 
- após isso entre no arquivo que foi criado e cole o seguinte código:

```
# Copiei de https://github.com/JRJurman/PowerLS
# Só alterei a cor dos 'normal files'

<#
 .Synopsis
  Powershell unix-like ls
  Written by Jesse Jurman (JRJurman)

 .Description
  A colorful ls

 .Parameter Redirect
  The first month to display.

 .Example
   # List the current directory
   PowerLS

 .Example
   # List the parent directory
   PowerLS ../
#>
function PowerLS {
    param(
      [string]$redirect = "."
    )
      write-host "" # add newline at top
  
      # get the console buffersize
      $buffer = Get-Host
      $bufferwidth = $buffer.ui.rawui.buffersize.width
  
      # get all the files and folders
      $childs = Get-ChildItem $redirect
  
      # get the longest string and get the length
      $lnStr = $childs | select-object Name | sort-object { "$_".length } -descending | select-object -first 1
      $len = $lnStr.name.length
  
      # keep track of how long our line is so far
      $count = 0
  
      # extra space to give some breather space
      $breather = 4
  
      # for every element, print the line
      foreach ($e in $childs) {
  
        $newName = $e.name + (" "*($len - $e.name.length+$breather))
        $count += $newName.length
  
        # determine color we should be printing
        # Blue for folders, Green for files, and Gray for hidden files
        if (($newName -match "^\..*$") -and (Test-Path ($redirect + "\" + $e) -pathtype container)) { #hidden folders
          $newName = $e.name + "\" + (" "*($len - $e.name.length+$breather - 1))
          write-host $newName -nonewline -foregroundcolor darkcyan
        }
        elseif (Test-Path ($redirect + "\" + $e) -pathtype container) { #normal folders
          $newName = $e.name + "\" + (" "*($len - $e.name.length+$breather - 1))
          write-host $newName -nonewline -foregroundcolor cyan
        }
        elseif ($newName -match "^\..*$") { #hidden files
          write-host $newName -nonewline -foregroundcolor darkgray
        }
        elseif ($newName -match "\.[^\.]*") { #normal files
          write-host $newName -nonewline -foregroundcolor Yellow
        }
        else { #others...
          write-host $newName -nonewline -foregroundcolor gray
        }
  
        if ( $count -ge ($bufferwidth - ($len+$breather)) ) {
          write-host ""
          $count = 0
        }
  
      }
  
      write-host "" # add newline at bottom
      write-host "" # add newline at bottom
  
  }
  
  export-modulemember -function PowerLS
  
```

PRONTO seu terminal está configurado.
