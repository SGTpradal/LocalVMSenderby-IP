# LocalVMSenderby-IP

# Windows HTTP File Share GUI

Ferramenta em PowerShell com interface gráfica para compartilhar temporariamente um arquivo local via HTTP.

O objetivo é facilitar a transferência rápida de arquivos entre um computador Windows e uma máquina de destino, como uma VM Linux, servidor local ou host na mesma rede.

## Funcionalidades

* Interface gráfica simples em Windows Forms.
* Compartilhamento de um único arquivo via HTTP.
* Geração automática do comando `wget`.
* Restrição de acesso por IP de destino.
* Criação automática de regra temporária no Firewall do Windows.
* Remoção da regra de firewall ao parar o compartilhamento.
* Compatível com execução via `.bat`.
* Funciona independente da pasta onde o script esteja salvo.

## Arquivos

```text
Share-File-GUI.bat
Share-File-GUI.ps1
```

O arquivo `.bat` é usado apenas para iniciar o script PowerShell com permissões administrativas.

## Requisitos

* Windows 10 ou superior.
* PowerShell 5.1 ou superior.
* Permissão de administrador.
* Máquina de destino acessível pela rede.
* Porta TCP liberada localmente pelo script.

## Como usar

1. Baixe os arquivos do projeto.
2. Mantenha o `.bat` e o `.ps1` na mesma pasta.
3. Execute o arquivo `.bat`.
4. Aceite o prompt de administrador do Windows.
5. Preencha os campos:

   * IP do Windows/remetente;
   * IP da máquina destino;
   * arquivo que será compartilhado;
   * porta TCP.
6. Clique em **Iniciar compartilhamento**.
7. Copie o comando `wget` gerado.
8. Execute o comando na máquina destino.

Exemplo de comando gerado:

```bash
wget "http://192.168.56.1:8000/arquivo.zip"
```

## Exemplo de uso

Cenário:

* Windows/remetente: `192.168.56.1`
* Linux/VM destino: `192.168.56.101`
* Porta: `8000`
* Arquivo: `arquivo.zip`

O script gera uma URL HTTP temporária e permite que a máquina destino baixe o arquivo com `wget`.

## Segurança

Este projeto foi feito para uso local, controlado e temporário.

Pontos importantes:

* O script abre uma porta HTTP temporária no Windows.
* O acesso é limitado ao IP de destino informado.
* Não há autenticação por usuário e senha.
* A proteção é baseada em restrição de IP e regra de firewall.
* Não é recomendado usar em redes públicas ou desconhecidas.
* O compartilhamento deve ser encerrado após a transferência.

Ao clicar em **Parar** ou fechar a janela, o script remove a regra temporária criada no Firewall do Windows.

## Por que precisa de administrador?

O script precisa ser executado como administrador porque cria e remove regras temporárias no Firewall do Windows usando `New-NetFirewallRule` e `Remove-NetFirewallRule`.

Sem permissão administrativa, o Windows não permite alterar regras de firewall.

## Sobre ExecutionPolicy Bypass

O arquivo `.bat` pode executar o PowerShell com:

```powershell
-ExecutionPolicy Bypass
```

Isso afeta somente o processo atual do PowerShell. A política de execução do sistema não é alterada permanentemente.

Esse parâmetro é usado para permitir que o script rode sem exigir alteração manual na política de execução do Windows.

## Limitações

* Compartilha apenas um arquivo por vez.
* Não possui autenticação.
* Não possui HTTPS.
* Não é indicado para exposição na internet.
* O acesso depende da conectividade entre origem e destino.
* Firewalls externos, NAT ou regras de rede podem bloquear a conexão.

## Estrutura técnica

O script usa:

* `System.Windows.Forms` para a interface gráfica.
* `System.Net.HttpListener` para o servidor HTTP local.
* `New-NetFirewallRule` para liberar temporariamente a porta.
* `Start-Job` para manter o servidor HTTP em segundo plano.
* `wget` como comando de download na máquina destino.

## Encerramento do compartilhamento

O compartilhamento pode ser encerrado de duas formas:

* clicando no botão **Parar**;
* fechando a janela do aplicativo.

Em ambos os casos, o script tenta parar o job em segundo plano e remover a regra temporária do firewall.

## Aviso

Use apenas em ambientes autorizados.

Este projeto é voltado para administração, suporte técnico, laboratório, testes e transferência temporária de arquivos entre máquinas conhecidas.


