# Gatehouse for Boundary

An independent desktop client for [HashiCorp Boundary](https://www.boundaryproject.io). Sign in, find your targets and open sessions from a clean window, without living in the terminal.

> Gatehouse for Boundary is an independent project. It is not affiliated with, endorsed by or sponsored by HashiCorp or IBM. "HashiCorp" and "Boundary" are trademarks of their respective owners.

**[Download the latest release](../../releases/latest)** · [Privacy policy](docs/privacy/index.html) · [Report a problem](../../issues)

Português (Brasil): [veja abaixo](#português-brasil)

## What it does

- Sign in with the **password** or **OIDC** (single sign-on) methods of your Boundary cluster
- Browse and search **targets**, with their **aliases**, filtered by type, scope and active sessions
- Connect to **SSH, TCP and RDP** targets with one click, and disconnect when you are done
- See and end **your own sessions**, with times in Brasília time (GMT-3), UTC or your system time zone
- Watch the **Boundary Client Agent** from the top bar: status, time left on your token, pause and resume
- Get a banner when your **token expires** and sign in again in one click
- **Windows:** detects when alias names are not reaching the Client Agent and fixes the DNS rule with one button
- English and Brazilian Portuguese, light and dark themes

## Prerequisites

| You need                                                                                                      | Why                                              |
| ------------------------------------------------------------------------------------------------------------- | ------------------------------------------------ |
| A **HashiCorp Boundary** cluster you can sign in to                                                           | The app is a client; it has no server of its own |
| The **Boundary Installer** installed ([download](https://developer.hashicorp.com/boundary/install#installer)) | The app runs it to open sessions                 |
| **Windows 10/11 (64-bit)** or **macOS on Apple Silicon**                                                      | Supported platforms                              |

## Install

**Windows:** download the `.msi` from the latest release and run it. The app appears in the Start menu.
The signed build is also available on the Microsoft Store _(link to be added)_.

**macOS:** download the `.dmg`, open it and drag the app to Applications.

## First run

1. Enter the URL of your Boundary cluster (for example `https://boundary.example.com:9200`) and press **Continue**.
2. Choose the sign-in method and sign in.
3. Pick a target and press **Connect**.

Your sign-in is also shared with the Boundary CLI and Client Agent on your computer, so `boundary` commands in a terminal use the same account.

## Privacy

The app talks only to the Boundary server you enter. It has no analytics, no ads and no accounts of its own, and nothing is sent to the developer. Read the [privacy policy](docs/privacy/index.html).

---

# Português (Brasil)

Um cliente desktop independente para o [HashiCorp Boundary](https://www.boundaryproject.io). Entre, encontre seus alvos e abra sessões em uma janela simples, sem precisar ficar no terminal.

> O Gatehouse for Boundary é um projeto independente. Não é afiliado, endossado nem patrocinado pela HashiCorp ou pela IBM. "HashiCorp" e "Boundary" são marcas de seus respectivos proprietários.

**[Baixar a última versão](../../releases/latest)** · [Política de privacidade](docs/privacy/index.html) · [Reportar um problema](../../issues)

## O que faz

- Entra com os métodos de **senha** ou **OIDC** (login único) do seu cluster Boundary
- Navega e busca **alvos**, com seus **aliases**, filtrando por tipo, escopo e sessões ativas
- Conecta em alvos **SSH, TCP e RDP** com um clique e desconecta quando terminar
- Mostra e encerra **as suas sessões**, com horários em Brasília (GMT-3), UTC ou no fuso do sistema
- Acompanha o **Boundary Client Agent** na barra superior: status, tempo restante do token, pausar e retomar
- Avisa quando o **token expira** e permite entrar de novo com um clique
- **Windows:** detecta quando os nomes dos aliases não chegam ao Client Agent e corrige a regra de DNS com um botão
- Português do Brasil e inglês, temas claro e escuro

## Pré-requisitos

| Você precisa de                                                                                             | Para quê                                     |
| ----------------------------------------------------------------------------------------------------------- | -------------------------------------------- |
| Um cluster **HashiCorp Boundary** em que você consiga entrar                                                | O app é um cliente; não tem servidor próprio |
| A **Boundary Installer** instalada ([download](https://developer.hashicorp.com/boundary/install#installer)) | O app a executa para abrir as sessões        |
| **Windows 10/11 (64 bits)** ou **macOS em Apple Silicon**                                                   | Plataformas suportadas                       |

## Instalação

**Windows:** baixe o `.msi` da última versão e execute. O app aparece no menu Iniciar.
A versão assinada também estará na Microsoft Store _(link a ser adicionado)_.

**macOS:** baixe o `.dmg`, abra e arraste o app para Aplicativos.

## Primeiro uso

1. Informe a URL do seu cluster Boundary (por exemplo `https://boundary.exemplo.com:9200`) e clique em **Continuar**.
2. Escolha o método de login e entre.
3. Escolha um alvo e clique em **Conectar**.

O seu login também é compartilhado com a CLI do Boundary e o Client Agent do computador, então comandos `boundary` no terminal usam a mesma conta.

## Privacidade

O app se comunica apenas com o servidor Boundary que você informar. Não tem análise de uso, anúncios nem contas próprias, e nada é enviado ao desenvolvedor. Leia a [política de privacidade](docs/privacy/index.html).
