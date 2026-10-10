# Gatehouse for Boundary

An independent desktop client for [HashiCorp Boundary](https://www.boundaryproject.io). Sign in, find your targets and open sessions from a clean window, without living in the terminal.

> Gatehouse for Boundary is an independent project. It is not affiliated with, endorsed by or sponsored by HashiCorp or IBM. "HashiCorp" and "Boundary" are trademarks of their respective owners.

**[Download the latest release](../../releases/latest)** · [Privacy policy](https://carlosdoki.github.io/gatehouse-for-boundary/privacy/) · [Report a problem](../../issues)

_Last updated: October 10, 2026 (version 1.0.3)_

Português (Brasil): [veja abaixo](#português-brasil)

## What's new in 1.0.3

- **Preferred clients for every protocol.** **Settings → Preferred Clients** now has RDP, SSH and TCP (browser) rows, each with built-in options per platform and a custom command. See [Preferred clients](#preferred-clients).
- **Choose how to connect.** **Connect** asks whether to open the preferred client, or only start the session and show the local port (with a button to copy it).
- **Copy command.** Right-click a target ID to copy `boundary connect [ssh|rdp] -target-id <id> -addr <cluster>`. TCP targets get no subcommand.
- **Fix (Windows):** the Boundary CLI lookup only accepts `boundary.exe`, so a stray extensionless `boundary` file in the `PATH` no longer breaks sign-in with "%1 is not a valid Win32 application".

## What's new in 1.0.2: Linux

This version brings Gatehouse for Boundary to Linux, with **.deb** (Debian/Ubuntu) and **.rpm** (CentOS Stream/RHEL/Rocky/Alma) packages.

**New**

- **Linux support:** sign-in (password and OIDC), target list, sessions and connect, just like on macOS and Windows.
- **Aliases on Linux:** reach targets by alias name (for example `https://site.domain.local`). The app answers the alias names and opens the Boundary session by itself on the first connection. System setup is done once, in **Settings → Aliases**, with the administrator password.
- **Menu and icon:** the app shows up in the applications menu with its own icon.
- **Clean uninstall:** removing the package also undoes the DNS and firewall setup for aliases.

**Fixes**

- The app no longer freezes when the controller is down or DNS fails; it shows the error message instead (all platforms).

## What it does

- Sign in with the **password** or **OIDC** (single sign-on) methods of your Boundary cluster
- Browse and search **targets**, with their **IDs** and **aliases**, filtered by type, scope and active sessions
- For targets with **host sources**, see the IDs of **all hosts** behind them
- Connect to **SSH, TCP and RDP** targets with one click, and disconnect when you are done. Each time, choose to open your preferred client or only start the session and see the local port
- Pick the program that opens each protocol (RDP, SSH, and TCP in a browser), or set your own command. See [Preferred clients](#preferred-clients)
- Right-click a target ID → **Copy command** to get the matching `boundary connect` command for the terminal
- See and end **your own sessions**, with times in Brasília time (GMT-3), UTC or your system time zone
- Watch the **Boundary Client Agent** from the top bar: status, time left on your token, pause and resume
- Get a banner when your **token expires** and sign in again in one click
- **Windows:** detects when alias names are not reaching the Client Agent and fixes the DNS rule with one button
- **Linux:** reach targets by alias name; the app resolves the names and opens the session on first connection
- English and Brazilian Portuguese, light and dark themes

## Prerequisites

| You need                                                                                                               | Why                                              |
| ---------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------ |
| A **HashiCorp Boundary** cluster you can sign in to                                                                    | The app is a client; it has no server of its own |
| **Windows/macOS:** the **Boundary Installer** ([download](https://developer.hashicorp.com/boundary/install#installer)) | The app runs it to open sessions                 |
| **Linux:** the **Boundary CLI** (`boundary`), see [Linux requirements](#linux-requirements)                            | The app runs it to open sessions                 |
| **Windows 10/11 (64-bit)**, **macOS on Apple Silicon** or **Linux x86_64**                                             | Supported platforms                              |

The app looks for the Boundary CLI in the usual install locations (`/opt/homebrew/bin`, `/usr/local/bin`, `/usr/bin`, or `C:\Program Files\HashiCorp\Boundary\boundary.exe` on Windows), then in the `PATH`. On Windows it only accepts `boundary.exe`.

### Linux requirements

**System**

- 64-bit Linux (x86_64) with a desktop environment (GNOME, KDE, XFCE…)
- Debian 12+, Ubuntu 22.04+, CentOS Stream / RHEL / Rocky / Alma 9+

**Boundary CLI** (required): the app uses `boundary` to open sessions. Install it from the HashiCorp repository.

Debian/Ubuntu:

```sh
wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install boundary
```

CentOS/RHEL/Rocky/Alma:

```sh
sudo dnf install -y dnf-plugins-core
sudo dnf config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
sudo dnf install boundary
```

**To use aliases** (optional):

- `systemd-resolved` running
- `nftables`
- `pkexec` (polkit), to ask for the administrator password during setup

Debian/Ubuntu usually ship with these. On CentOS/RHEL/Rocky/Alma, `systemd-resolved` is not active by default; enable it with:

```sh
sudo dnf install systemd-resolved nftables polkit
sudo systemctl enable --now systemd-resolved
sudo ln -sf /run/systemd/resolve/stub-resolv.conf /etc/resolv.conf
```

## Install

**Windows:** download the `.msi` from the latest release and run it. The app appears in the Start menu.
The signed build is also available on the Microsoft Store _(link to be added)_.

**macOS:** download the `.dmg`, open it and drag the app to Applications.

**Linux:** download the package from the latest release and install it.

Debian/Ubuntu:

```sh
sudo apt install ./gatehouse-boundary_1.0.2-1_amd64.deb
```

CentOS/RHEL/Rocky/Alma:

```sh
sudo dnf install ./gatehouse-boundary-1.0.2-1.x86_64.rpm
```

Use `apt`/`dnf` (not `dpkg -i`/`rpm -i`) so dependencies are installed automatically. Then open **Gatehouse for Boundary** from the applications menu or run `gatehouse-boundary` in a terminal.

To remove: `sudo apt remove gatehouse-boundary` or `sudo dnf remove gatehouse-boundary`.

## First run

1. Enter the URL of your Boundary cluster (for example `https://boundary.example.com:9200`) and press **Continue**.
2. Choose the sign-in method and sign in.
3. Pick a target and press **Connect**.

Your sign-in is also shared with the Boundary CLI and Client Agent on your computer, so `boundary` commands in a terminal use the same account.

## Preferred clients

**Settings → Preferred Clients** sets the program that **Connect** opens for each target type. It points the program at the local port that `boundary connect` opens.

| Protocol      | macOS                                                          | Windows                                                | Linux                                                         |
| ------------- | -------------------------------------------------------------- | ------------------------------------------------------ | ------------------------------------------------------------- |
| Windows RDP   | System default, Windows App, Microsoft Remote Desktop          | Remote Desktop Connection (`mstsc`)                    | System default (`xdg-open`), Remmina, FreeRDP, FreeRDP 3      |
| SSH           | System default (`ssh://`), Terminal, iTerm                     | Console window, Windows Terminal, PowerShell, PuTTY    | `x-terminal-emulator`, GNOME Terminal, Konsole, xterm         |
| TCP (browser) | System default, Safari, Google Chrome, Firefox, Microsoft Edge | System default, Google Chrome, Microsoft Edge, Firefox | System default (`xdg-open`), Firefox, Google Chrome, Chromium |

Built-in SSH options run `ssh -p <port> -o NoHostAuthenticationForLocalhost=yes 127.0.0.1` with your local user name. Credentials that Boundary injects are not used, unlike `boundary connect ssh`. TCP opens `http://127.0.0.1:<port>`.

**Custom command…** runs any program. Put quotes around arguments that contain spaces, and use these placeholders:

| Placeholder | Replaced with                                                  |
| ----------- | -------------------------------------------------------------- |
| `{host}`    | Local address of the session (usually `127.0.0.1`)             |
| `{port}`    | Local port of the session                                      |
| `{url}`     | `http://{host}:{port}`                                         |
| `{file}`    | Path to a temporary `.rdp` file that points at `{host}:{port}` |

Examples:

```text
open -a "Royal TSX" {file}
ssh -p {port} admin@{host}
firefox https://{host}:{port}
```

## Known limitations

- **Linux:** while the app is closed, names inside alias domains do not resolve. Avoid creating aliases in the same domain as the controller (for example alias `*.company.com` with controller `boundary.company.com`); prefer internal domains such as `.internal` or `.local`.

## Privacy

The app talks only to the Boundary server you enter. It has no analytics, no ads and no accounts of its own, and nothing is sent to the developer. Read the [privacy policy](docs/privacy/index.html).

---

# Português (Brasil)

Um cliente desktop independente para o [HashiCorp Boundary](https://www.boundaryproject.io). Entre, encontre seus alvos e abra sessões em uma janela simples, sem precisar ficar no terminal.

> O Gatehouse for Boundary é um projeto independente. Não é afiliado, endossado nem patrocinado pela HashiCorp ou pela IBM. "HashiCorp" e "Boundary" são marcas de seus respectivos proprietários.

**[Baixar a última versão](../../releases/latest)** · [Política de privacidade](https://carlosdoki.github.io/gatehouse-for-boundary/privacy/) · [Reportar um problema](../../issues)

_Última atualização: 10 de outubro de 2026 (versão 1.0.3)_

## Novidades da 1.0.3

- **Clientes preferidos para todos os protocolos.** **Settings → Preferred Clients** agora tem linhas para RDP, SSH e TCP (navegador), cada uma com opções prontas por plataforma e um comando personalizado. Veja [Clientes preferidos](#clientes-preferidos).
- **Escolha como conectar.** O **Conectar** pergunta se você quer abrir o cliente preferido ou só iniciar a sessão e ver a porta local (com um botão para copiá-la).
- **Copiar comando.** Clique com o botão direito no ID de um alvo para copiar `boundary connect [ssh|rdp] -target-id <id> -addr <cluster>`. Alvos TCP não têm subcomando.
- **Correção (Windows):** a busca pela CLI do Boundary aceita só `boundary.exe`, então um arquivo `boundary` sem extensão perdido no `PATH` não quebra mais o login com o erro "%1 não é um aplicativo Win32 válido".

## 🐧 Novidades da 1.0.2: Gatehouse for Boundary agora no Linux

Esta versão traz o Gatehouse for Boundary para Linux, com pacotes **.deb** (Debian/Ubuntu) e **.rpm** (CentOS Stream/RHEL/Rocky/Alma).

**Novidades**

- **Suporte a Linux:** login (senha e OIDC), lista de targets, sessões e conexão, como no macOS e no Windows.
- **Aliases no Linux:** acesse os targets pelo nome do alias (ex.: `https://site.domain.local`). O app responde os nomes dos aliases e abre a sessão do Boundary sozinho na primeira conexão. A configuração do sistema é feita uma vez, em **Settings → Aliases**, com a senha de administrador.
- **Menu e ícone:** o app aparece no menu de aplicativos com o ícone próprio.
- **Desinstalação limpa:** remover o pacote também desfaz a configuração de DNS e firewall dos aliases.

**Correções**

- O app não trava mais a tela quando o controller está fora do ar ou o DNS falha; mostra a mensagem de erro no lugar (todas as plataformas).

## O que faz

- Entra com os métodos de **senha** ou **OIDC** (login único) do seu cluster Boundary
- Navega e busca **alvos**, com seus **IDs** e **aliases**, filtrando por tipo, escopo e sessões ativas
- Para alvos com **host sources**, mostra os IDs de **todos os hosts** vinculados
- Conecta em alvos **SSH, TCP e RDP** com um clique e desconecta quando terminar. A cada vez, escolha abrir o seu cliente preferido ou só iniciar a sessão e ver a porta local
- Escolhe o programa que abre cada protocolo (RDP, SSH e TCP no navegador), ou um comando seu. Veja [Clientes preferidos](#clientes-preferidos)
- Clique com o botão direito no ID de um alvo → **Copiar comando** para pegar o comando `boundary connect` correspondente para o terminal
- Mostra e encerra **as suas sessões**, com horários em Brasília (GMT-3), UTC ou no fuso do sistema
- Acompanha o **Boundary Client Agent** na barra superior: status, tempo restante do token, pausar e retomar
- Avisa quando o **token expira** e permite entrar de novo com um clique
- **Windows:** detecta quando os nomes dos aliases não chegam ao Client Agent e corrige a regra de DNS com um botão
- **Linux:** acessa os alvos pelo nome do alias; o app resolve os nomes e abre a sessão na primeira conexão
- Português do Brasil e inglês, temas claro e escuro

## Pré-requisitos

| Você precisa de                                                                                                      | Para quê                                     |
| -------------------------------------------------------------------------------------------------------------------- | -------------------------------------------- |
| Um cluster **HashiCorp Boundary** em que você consiga entrar                                                         | O app é um cliente; não tem servidor próprio |
| **Windows/macOS:** a **Boundary Installer** ([download](https://developer.hashicorp.com/boundary/install#installer)) | O app a executa para abrir as sessões        |
| **Linux:** a **CLI do Boundary** (`boundary`), veja [Requisitos (Linux)](#requisitos-linux)                          | O app a executa para abrir as sessões        |
| **Windows 10/11 (64 bits)**, **macOS em Apple Silicon** ou **Linux x86_64**                                          | Plataformas suportadas                       |

O app procura a CLI do Boundary nos locais de instalação usuais (`/opt/homebrew/bin`, `/usr/local/bin`, `/usr/bin`, ou `C:\Program Files\HashiCorp\Boundary\boundary.exe` no Windows) e depois no `PATH`. No Windows ele aceita só `boundary.exe`.

### Requisitos (Linux)

**Sistema**

- Linux 64-bit (x86_64) com ambiente gráfico (GNOME, KDE, XFCE…)
- Debian 12+, Ubuntu 22.04+, CentOS Stream / RHEL / Rocky / Alma 9+

**Boundary CLI** (obrigatório): o app usa o `boundary` para abrir as sessões. Ele vem do repositório da HashiCorp.

Debian/Ubuntu:

```sh
wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install boundary
```

CentOS/RHEL/Rocky/Alma:

```sh
sudo dnf install -y dnf-plugins-core
sudo dnf config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
sudo dnf install boundary
```

**Para usar aliases** (opcional):

- `systemd-resolved` ativo
- `nftables`
- `pkexec` (polkit), para pedir a senha de administrador na configuração

No Debian/Ubuntu normalmente já vêm instalados. No CentOS/RHEL/Rocky/Alma o `systemd-resolved` não vem ativo; ative com:

```sh
sudo dnf install systemd-resolved nftables polkit
sudo systemctl enable --now systemd-resolved
sudo ln -sf /run/systemd/resolve/stub-resolv.conf /etc/resolv.conf
```

## Instalação

**Windows:** baixe o `.msi` da última versão e execute. O app aparece no menu Iniciar.
A versão assinada também estará na Microsoft Store _(link a ser adicionado)_.

**macOS:** baixe o `.dmg`, abra e arraste o app para Aplicativos.

**Linux:** baixe o pacote da última versão e instale.

Debian/Ubuntu:

```sh
sudo apt install ./gatehouse-boundary_1.0.2-1_amd64.deb
```

CentOS/RHEL/Rocky/Alma:

```sh
sudo dnf install ./gatehouse-boundary-1.0.2-1.x86_64.rpm
```

Use `apt`/`dnf` (e não `dpkg -i`/`rpm -i`) para instalar as dependências automaticamente. Depois abra **Gatehouse for Boundary** pelo menu de aplicativos ou rode `gatehouse-boundary` no terminal.

Para remover: `sudo apt remove gatehouse-boundary` ou `sudo dnf remove gatehouse-boundary`.

## Primeiro uso

1. Informe a URL do seu cluster Boundary (por exemplo `https://boundary.exemplo.com:9200`) e clique em **Continuar**.
2. Escolha o método de login e entre.
3. Escolha um alvo e clique em **Conectar**.

O seu login também é compartilhado com a CLI do Boundary e o Client Agent do computador, então comandos `boundary` no terminal usam a mesma conta.

## Clientes preferidos

**Settings → Preferred Clients** define o programa que o **Conectar** abre para cada tipo de alvo. O programa é apontado para a porta local que o `boundary connect` abre.

| Protocolo         | macOS                                                       | Windows                                                | Linux                                                       |
| ----------------- | ----------------------------------------------------------- | ------------------------------------------------------ | ----------------------------------------------------------- |
| Windows RDP       | Padrão do sistema, Windows App, Microsoft Remote Desktop    | Conexão de Área de Trabalho Remota (`mstsc`)           | Padrão do sistema (`xdg-open`), Remmina, FreeRDP, FreeRDP 3 |
| SSH               | Padrão do sistema (`ssh://`), Terminal, iTerm               | Janela de console, Windows Terminal, PowerShell, PuTTY | `x-terminal-emulator`, GNOME Terminal, Konsole, xterm       |
| TCP (navegador)   | Padrão do sistema, Safari, Google Chrome, Firefox, Microsoft Edge | Padrão do sistema, Google Chrome, Microsoft Edge, Firefox | Padrão do sistema (`xdg-open`), Firefox, Google Chrome, Chromium |

As opções prontas de SSH rodam `ssh -p <porta> -o NoHostAuthenticationForLocalhost=yes 127.0.0.1` com o seu usuário local. As credenciais que o Boundary injeta não são usadas, ao contrário do `boundary connect ssh`. O TCP abre `http://127.0.0.1:<porta>`.

**Custom command…** roda qualquer programa. Coloque aspas nos argumentos que têm espaços e use estes marcadores:

| Marcador | Substituído por                                                    |
| -------- | ------------------------------------------------------------------ |
| `{host}` | Endereço local da sessão (normalmente `127.0.0.1`)                 |
| `{port}` | Porta local da sessão                                              |
| `{url}`  | `http://{host}:{port}`                                             |
| `{file}` | Caminho de um arquivo `.rdp` temporário que aponta para `{host}:{port}` |

Exemplos:

```text
open -a "Royal TSX" {file}
ssh -p {port} admin@{host}
firefox https://{host}:{port}
```

## Limitações conhecidas

- **Linux:** com o app fechado, os nomes dentro dos domínios dos aliases não resolvem. Evite criar aliases no mesmo domínio do controller (ex.: alias `*.empresa.com.br` com controller `boundary.empresa.com.br`); prefira domínios internos como `.internal` ou `.local`.

## Privacidade

O app se comunica apenas com o servidor Boundary que você informar. Não tem análise de uso, anúncios nem contas próprias, e nada é enviado ao desenvolvedor. Leia a [política de privacidade](docs/privacy/index.html).
