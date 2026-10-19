# Gatehouse for Boundary

An independent desktop client for [HashiCorp Boundary](https://www.boundaryproject.io). Sign in, find your targets and open sessions from a clean window, without living in the terminal.

> Gatehouse for Boundary is an independent project. It is not affiliated with, endorsed by or sponsored by HashiCorp or IBM. "HashiCorp" and "Boundary" are trademarks of their respective owners.

**[Download the latest release](../../releases/latest)** · [Privacy policy](https://carlosdoki.github.io/gatehouse-for-boundary/privacy/) · [Report a problem](../../issues)

_Last updated: October 19, 2026 (version 1.0.5)_

Português (Brasil): [veja abaixo](#português-brasil)

## What's new in 1.0.5

- **Port in the target list.** Each target shows its default port next to the address (e.g. `db.internal:5432`), or `port 5432` when its hosts come from host sources. Search also matches the port.
- **Database and Kubernetes clients for TCP targets.** **Connect** on a TCP target has a **Client** field: the preferred client as before, or Cassandra, Kubernetes, MongoDB, MySQL, PostgreSQL or Redis. The app picks one from the target's port when it can. See [Database and Kubernetes clients](#database-and-kubernetes-clients).
- **Status menu.** Pause, resume or start the Client Agent without opening the window: right-click the Dock icon or click the shield icon in the menu bar on macOS, or use the tray icon on Windows. On Linux the tray shows alias status and setup. See [Status menu](#status-menu).
- **Fix (Windows): broken Client Agent config.** Some installs leave a second `log_file` line with a macOS path at the end of `boundary-client-agent.hcl`. HCL does not accept a repeated setting, so the agent will not start. The config editor (**Settings → Client Agent**) now warns about repeated settings and macOS paths, and **Remove** deletes those lines before you save.
- **Linux:** the `.deb` and `.rpm` packages now depend on AppIndicator, used by the tray icon.

## What's new in 1.0.4

- **Font size.** **Settings → Application → Font size** scales the whole app from 80% to 150%, in 10% steps. **Ctrl/Cmd +**, **Ctrl/Cmd −** and **Ctrl/Cmd 0** (reset to 100%) work anywhere in the app.
- **Copy command for a host.** In a target's host list, right-click a host ID → **Copy command** to get `boundary connect … -target-id <id> -host-id <host> -addr <cluster>`.
- **Warp for SSH (macOS).** Warp is now a built-in SSH option in [Preferred clients](#preferred-clients).
- **Aliases without the `list` grant.** Aliases now come from the aliases your user can resolve (as the Client Agent does), so users without permission to list the global aliases still see them. The global listing remains as a fallback.
- **Fix (macOS):** the app no longer makes macOS ask twice for Keychain access.
- **Clearer connection errors.** TLS, DNS and connection-refused failures show their cause instead of a generic "network error".

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
- Browse and search **targets**, with their **IDs**, **address and port** and **aliases**, filtered by type, scope and active sessions
- For targets with **host sources**, see the IDs of **all hosts** behind them
- Connect to **SSH, TCP and RDP** targets with one click, and disconnect when you are done. Each time, choose to open your preferred client or only start the session and see the local port
- Open TCP targets in a **database or Kubernetes client** (PostgreSQL, MySQL, MongoDB, Redis, Cassandra, Kubernetes) through `boundary connect <client>`. See [Database and Kubernetes clients](#database-and-kubernetes-clients)
- Pick the program that opens each protocol (RDP, SSH, and TCP in a browser), or set your own command. See [Preferred clients](#preferred-clients)
- Right-click a target or host ID → **Copy command** to get the matching `boundary connect` command for the terminal
- See and end **your own sessions**, with times in Brasília time (GMT-3), UTC or your system time zone
- Watch the **Boundary Client Agent** from the top bar: status, time left on your token, pause and resume
- A **status menu** outside the window: the Dock and menu bar on macOS, the system tray on Windows and Linux. See [Status menu](#status-menu)
- Get a banner when your **token expires** and sign in again in one click
- **Windows:** detects when alias names are not reaching the Client Agent and fixes the DNS rule with one button
- **Linux:** reach targets by alias name; the app resolves the names and opens the session on first connection
- English and Brazilian Portuguese, light and dark themes, adjustable font size

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
| SSH           | System default (`ssh://`), Terminal, iTerm, Warp               | Console window, Windows Terminal, PowerShell, PuTTY    | `x-terminal-emulator`, GNOME Terminal, Konsole, xterm         |
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

## Database and Kubernetes clients

On a TCP target, **Connect → Client** can also run one of the Boundary CLI's client subcommands in a terminal window:

| Client     | Command                     | Fields                               | Picked by default for port |
| ---------- | --------------------------- | ------------------------------------ | -------------------------- |
| Cassandra  | `boundary connect cassandra` | Username, Keyspace                   | 9042                       |
| Kubernetes | `boundary connect kube`     | Extra arguments (e.g. `get pods -A`) | 6443                       |
| MongoDB    | `boundary connect mongo`    | Username, Database                   | 27017                      |
| MySQL      | `boundary connect mysql`    | Username, Database                   | 3306                       |
| PostgreSQL | `boundary connect postgres` | Username, Database                   | 5432                       |
| Redis      | `boundary connect redis`    | Username                             | 6379                       |

All fields are optional. **Extra client arguments** are passed to the client after `--`.

- The CLI starts the session and the client, so credentials that Boundary brokers for the target are used.
- The client program (`cqlsh`, `kubectl`, `mongosh`, `mysql`, `psql`, `redis-cli`) must be installed and in the `PATH`.
- The terminal follows your SSH choice in Preferred Clients: Terminal or iTerm on macOS; the console window or Windows Terminal on Windows; GNOME Terminal, Konsole, xterm or `x-terminal-emulator` on Linux.
- The token goes to the CLI through a temporary script that only your user can read, never on the command line. On macOS and Linux the script deletes itself as soon as it runs. On Windows it deletes itself when the client exits.
- These sessions are not shown with **Disconnect** on the target. Close the client or the terminal to end them, or cancel them on the Sessions page.

## Status menu

While you are signed in, the app shows a small menu outside its window:

| Platform | Where                                                                | Contents                                                                                                        |
| -------- | -------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| macOS    | Right-click the Dock icon, or click the shield icon in the menu bar | Client Agent status; Pause / Resume / Start agent; the menu bar icon also has an entry to bring the window back |
| Windows  | System tray icon: left click opens the window, right click the menu | Client Agent status; Pause / Resume / Start agent; Open Gatehouse                                               |
| Linux    | System tray icon                                                     | Alias status; Set up aliases (when needed); Open Gatehouse                                                      |

On Linux the tray icon needs AppIndicator support. On plain GNOME, install the "AppIndicator and KStatusNotifierItem Support" extension. Without it the app works as usual, without the icon. Closing the window still quits the app, and the icon goes with it.

## Known limitations

- **Linux:** while the app is closed, names inside alias domains do not resolve. Avoid creating aliases in the same domain as the controller (for example alias `*.company.com` with controller `boundary.company.com`); prefer internal domains such as `.internal` or `.local`.

## Privacy

The app talks only to the Boundary server you enter. It has no analytics, no ads and no accounts of its own, and nothing is sent to the developer. Read the [privacy policy](docs/privacy/index.html).

---

# Português (Brasil)

Um cliente desktop independente para o [HashiCorp Boundary](https://www.boundaryproject.io). Entre, encontre seus alvos e abra sessões em uma janela simples, sem precisar ficar no terminal.

> O Gatehouse for Boundary é um projeto independente. Não é afiliado, endossado nem patrocinado pela HashiCorp ou pela IBM. "HashiCorp" e "Boundary" são marcas de seus respectivos proprietários.

**[Baixar a última versão](../../releases/latest)** · [Política de privacidade](https://carlosdoki.github.io/gatehouse-for-boundary/privacy/) · [Reportar um problema](../../issues)

_Última atualização: 19 de outubro de 2026 (versão 1.0.5)_

## Novidades da 1.0.5

- **Porta na lista de alvos.** Cada alvo mostra a porta padrão ao lado do endereço (ex.: `db.internal:5432`), ou `port 5432` quando os hosts vêm de host sources. A busca também encontra pela porta.
- **Clientes de banco de dados e Kubernetes para alvos TCP.** O **Conectar** de um alvo TCP tem um campo **Client**: o cliente preferido, como antes, ou Cassandra, Kubernetes, MongoDB, MySQL, PostgreSQL ou Redis. O app escolhe um pela porta do alvo quando consegue. Veja [Clientes de banco de dados e Kubernetes](#clientes-de-banco-de-dados-e-kubernetes).
- **Menu de status.** Pause, retome ou inicie o Client Agent sem abrir a janela: clique com o botão direito no ícone do Dock ou clique no ícone de escudo na barra de menus no macOS, ou use o ícone da bandeja no Windows. No Linux a bandeja mostra o status e a configuração dos aliases. Veja [Menu de status](#menu-de-status).
- **Correção (Windows): config do Client Agent quebrada.** Algumas instalações deixam uma segunda linha `log_file` com um caminho do macOS no fim do `boundary-client-agent.hcl`. O HCL não aceita configuração repetida, então o agent não inicia. O editor de configuração (**Settings → Client Agent**) agora avisa sobre configurações repetidas e caminhos do macOS, e o **Remove** apaga essas linhas antes de salvar.
- **Linux:** os pacotes `.deb` e `.rpm` agora dependem do AppIndicator, usado pelo ícone da bandeja.

## Novidades da 1.0.4

- **Tamanho da fonte.** **Settings → Application → Font size** ajusta o app inteiro de 80% a 150%, de 10% em 10%. Os atalhos **Ctrl/Cmd +**, **Ctrl/Cmd −** e **Ctrl/Cmd 0** (volta para 100%) funcionam em qualquer tela.
- **Copiar comando para um host.** Na lista de hosts de um alvo, clique com o botão direito no ID do host → **Copiar comando** para pegar `boundary connect … -target-id <id> -host-id <host> -addr <cluster>`.
- **Warp no SSH (macOS).** O Warp agora é uma opção pronta de SSH em [Clientes preferidos](#clientes-preferidos).
- **Aliases sem a permissão de `list`.** Os aliases agora vêm dos aliases que o seu usuário consegue resolver (como faz o Client Agent), então quem não tem permissão para listar os aliases globais também os vê. A listagem global continua como alternativa.
- **Correção (macOS):** o macOS não pede mais duas vezes o acesso ao Keychain.
- **Erros de conexão mais claros.** Falhas de TLS, DNS e conexão recusada mostram a causa em vez de um "erro de rede" genérico.

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
- Navega e busca **alvos**, com seus **IDs**, **endereço e porta** e **aliases**, filtrando por tipo, escopo e sessões ativas
- Para alvos com **host sources**, mostra os IDs de **todos os hosts** vinculados
- Conecta em alvos **SSH, TCP e RDP** com um clique e desconecta quando terminar. A cada vez, escolha abrir o seu cliente preferido ou só iniciar a sessão e ver a porta local
- Abre alvos TCP em um **cliente de banco de dados ou Kubernetes** (PostgreSQL, MySQL, MongoDB, Redis, Cassandra, Kubernetes) via `boundary connect <cliente>`. Veja [Clientes de banco de dados e Kubernetes](#clientes-de-banco-de-dados-e-kubernetes)
- Escolhe o programa que abre cada protocolo (RDP, SSH e TCP no navegador), ou um comando seu. Veja [Clientes preferidos](#clientes-preferidos)
- Clique com o botão direito no ID de um alvo ou host → **Copiar comando** para pegar o comando `boundary connect` correspondente para o terminal
- Mostra e encerra **as suas sessões**, com horários em Brasília (GMT-3), UTC ou no fuso do sistema
- Acompanha o **Boundary Client Agent** na barra superior: status, tempo restante do token, pausar e retomar
- Um **menu de status** fora da janela: no Dock e na barra de menus no macOS, na bandeja do sistema no Windows e no Linux. Veja [Menu de status](#menu-de-status)
- Avisa quando o **token expira** e permite entrar de novo com um clique
- **Windows:** detecta quando os nomes dos aliases não chegam ao Client Agent e corrige a regra de DNS com um botão
- **Linux:** acessa os alvos pelo nome do alias; o app resolve os nomes e abre a sessão na primeira conexão
- Português do Brasil e inglês, temas claro e escuro, tamanho da fonte ajustável

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
| SSH               | Padrão do sistema (`ssh://`), Terminal, iTerm, Warp         | Janela de console, Windows Terminal, PowerShell, PuTTY | `x-terminal-emulator`, GNOME Terminal, Konsole, xterm       |
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

## Clientes de banco de dados e Kubernetes

Em um alvo TCP, **Conectar → Client** também pode rodar um dos subcomandos de cliente da CLI do Boundary numa janela de terminal:

| Cliente    | Comando                      | Campos                                  | Escolhido por padrão na porta |
| ---------- | ---------------------------- | --------------------------------------- | ----------------------------- |
| Cassandra  | `boundary connect cassandra` | Usuário, Keyspace                       | 9042                          |
| Kubernetes | `boundary connect kube`      | Argumentos extras (ex.: `get pods -A`)  | 6443                          |
| MongoDB    | `boundary connect mongo`     | Usuário, Banco de dados                 | 27017                         |
| MySQL      | `boundary connect mysql`     | Usuário, Banco de dados                 | 3306                          |
| PostgreSQL | `boundary connect postgres`  | Usuário, Banco de dados                 | 5432                          |
| Redis      | `boundary connect redis`     | Usuário                                 | 6379                          |

Todos os campos são opcionais. Os **argumentos extras do cliente** são passados ao cliente depois de `--`.

- A CLI inicia a sessão e o cliente, então as credenciais que o Boundary fornece para o alvo são usadas.
- O programa cliente (`cqlsh`, `kubectl`, `mongosh`, `mysql`, `psql`, `redis-cli`) precisa estar instalado e no `PATH`.
- O terminal segue a sua escolha de SSH em Clientes preferidos: Terminal ou iTerm no macOS; janela de console ou Windows Terminal no Windows; GNOME Terminal, Konsole, xterm ou `x-terminal-emulator` no Linux.
- O token vai para a CLI por um script temporário que só o seu usuário consegue ler, nunca pela linha de comando. No macOS e no Linux o script se apaga assim que roda. No Windows ele se apaga quando o cliente fecha.
- Essas sessões não aparecem com **Desconectar** no alvo. Feche o cliente ou o terminal para encerrá-las, ou cancele na página de Sessões.

## Menu de status

Enquanto você está logado, o app mostra um pequeno menu fora da janela:

| Plataforma | Onde                                                                               | Conteúdo                                                                                                   |
| ---------- | ---------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| macOS      | Botão direito no ícone do Dock, ou clique no ícone de escudo na barra de menus     | Status do Client Agent; Pausar / Retomar / Iniciar o agent; o ícone da barra de menus também reabre a janela |
| Windows    | Ícone na bandeja do sistema: clique esquerdo abre a janela, clique direito o menu | Status do Client Agent; Pausar / Retomar / Iniciar o agent; Abrir o Gatehouse                              |
| Linux      | Ícone na bandeja do sistema                                                        | Status dos aliases; Configurar aliases (quando necessário); Abrir o Gatehouse                              |

No Linux o ícone da bandeja precisa de suporte a AppIndicator. No GNOME puro, instale a extensão "AppIndicator and KStatusNotifierItem Support". Sem ela o app funciona normalmente, só sem o ícone. Fechar a janela continua encerrando o app, e o ícone some junto.

## Limitações conhecidas

- **Linux:** com o app fechado, os nomes dentro dos domínios dos aliases não resolvem. Evite criar aliases no mesmo domínio do controller (ex.: alias `*.empresa.com.br` com controller `boundary.empresa.com.br`); prefira domínios internos como `.internal` ou `.local`.

## Privacidade

O app se comunica apenas com o servidor Boundary que você informar. Não tem análise de uso, anúncios nem contas próprias, e nada é enviado ao desenvolvedor. Leia a [política de privacidade](docs/privacy/index.html).
