# Sobelow

[![Module Version](https://img.shields.io/hexpm/v/sobelow.svg)](https://hex.pm/packages/sobelow)
[![Hex Docs](https://img.shields.io/badge/hex-docs-lightgreen.svg)](https://hexdocs.pm/sobelow/)
[![Total Download](https://img.shields.io/hexpm/dt/sobelow.svg)](https://hex.pm/packages/sobelow)
[![License](https://img.shields.io/hexpm/l/sobelow.svg)](https://hex.pm/packages/sobelow)
[![Last Updated](https://img.shields.io/github/last-commit/nccgroup/sobelow.svg)](https://github.com/nccgroup/sobelow/commits/master)

Sobelow is a security-focused static analysis tool for the
Phoenix framework. For security researchers, it is a useful
tool for getting a quick view of points-of-interest. For
project maintainers, it can be used to prevent the introduction
of a number of common vulnerabilities.

Currently Sobelow detects some types of the following
security issues:

* Insecure configuration
* Known-vulnerable Dependencies
* Cross-Site Scripting
* SQL injection
* Command injection
* Code execution
* Denial of Service
* Directory traversal
* Unsafe serialization

Potential vulnerabilities are flagged in different colors
according to confidence in their insecurity. High confidence is
red, medium confidence is yellow, and low confidence is green.

A finding is typically marked "low confidence" if it looks
like a function could be used insecurely, but it cannot
reliably be determined if the function accepts user-supplied
input. That is to say, if a finding is marked green, it may be
critically insecure, but it will require greater manual
validation.

**Note:** This project is in constant development, and
additional vulnerabilities will be flagged as time goes on.
If you encounter a bug, or would like to request additional
features or security checks, please open an issue!

---

## 💻 System Requirements / Requisitos de Sistema

### 🇺🇸 English — Minimum & Recommended Configuration

**Sobelow** runs on the **Elixir / Erlang (BEAM)** runtime to perform static security analysis (SAST) on Phoenix applications, supporting three hardware profiles based on project scale and CI/CD usage:

| Resource | 🟢 Minimum (Small/Medium Phoenix App — Local Scan) | 🟡 Intermediate (Large Phoenix Umbrella / CI Pipeline) | 🔵 Recommended (Multi-App Monorepo + Continuous SAST) |
| :--- | :--- | :--- | :--- |
| **CPU** | **1 to 2 vCPUs** (`x86_64` / `arm64`) | **2 to 4 vCPUs** (`x86_64` / `arm64`) | **4+ vCPUs** (`x86_64` / `arm64`) |
| **RAM** | **512 MB to 1 GB RAM** | **2 GB to 4 GB RAM** | **4 GB to 8 GB RAM** |
| **Storage (SSD)** | **300 MB free** (Erlang/OTP + Elixir + Sobelow escript) | **1 GB free** (Mix `_build/` cache + Hex packages) | **2 GB+ SSD** (fast Mix compilation & AST parsing) |
| **GPU (Optional)** | Not required | Not required | Not required |
| **Operating System** | Linux (Ubuntu/Debian/Alpine), macOS 12+, or Windows 10/11 | Linux (CI runner / Docker `elixir` image), macOS, or Windows | Ubuntu 24.04 LTS, macOS 13+, or Official Elixir Docker Image |

#### 📦 Resource Usage by Architecture Component
- **Erlang BEAM VM & Elixir Runtime**: ~80 MB to 200 MB base RAM footprint.
- **Sobelow Static AST Analyzer (`mix sobelow` / standalone `escript` + `Jason` JSON output)**: ~50 MB to 300 MB RAM depending on the number of Phoenix controllers, `.eex` templates, routers, and configuration modules inspected.
- **Mix Compilation & Dependency Check (`mix.exs` / `mix.lock`)**: ~200 MB to 1 GB RAM during initial target project compilation when executed as a Mix `:dev` dependency.

#### 🛠️ Software Dependencies
- **Core Runtime**: `Elixir >= 1.4` (recommended `1.14+`) and compatible `Erlang/OTP` (`BEAM` VM with `:logger`, `:eex`, `:inets`)
- **Build & Package Tooling**: `Mix` and `Hex` (`mix local.hex`)
- **Library Dependencies (`mix.exs`)**: `jason ~> 1.0` (JSON output formatting), `ex_doc ~> 0.20` (development documentation only)

### 🇧🇷 Português (BR) — Configuração Mínima & Recomendada

O **Sobelow** executa sobre o runtime **Elixir / Erlang (BEAM)** para realizar análise estática de segurança (SAST) em aplicações Phoenix, operando em três perfis de hardware conforme o porte do projeto e o uso em pipelines CI/CD:

| Recurso | 🟢 Mínimo (App Phoenix Pequeno/Médio — Scan Local) | 🟡 Intermediário (Projeto Umbrella Grande / Pipeline CI) | 🔵 Recomendado (Monorepo Multi-App + SAST Contínuo) |
| :--- | :--- | :--- | :--- |
| **CPU** | **1 a 2 vCPUs** (`x86_64` / `arm64`) | **2 a 4 vCPUs** (`x86_64` / `arm64`) | **4+ vCPUs** (`x86_64` / `arm64`) |
| **Memória RAM** | **512 MB a 1 GB RAM** | **2 GB a 4 GB RAM** | **4 GB a 8 GB RAM** |
| **Armazenamento (SSD)** | **300 MB livres** (Erlang/OTP + Elixir + escript Sobelow) | **1 GB livre** (cache Mix `_build/` + pacotes Hex) | **2 GB+ SSD** (compilação Mix rápida e análise AST) |
| **GPU (Opcional)** | Não necessária | Não necessária | Não necessária |
| **Sistema Operacional** | Linux (Ubuntu/Debian/Alpine), macOS 12+ ou Windows 10/11 | Linux (Runner CI / imagem Docker `elixir`), macOS ou Windows | Ubuntu 24.04 LTS, macOS 13+ ou Imagem Docker Oficial Elixir |

#### 📦 Consumo por Componente da Arquitetura
- **Erlang BEAM VM & Runtime Elixir**: ~80 MB a 200 MB de RAM base.
- **Analisador Estático AST Sobelow (`mix sobelow` / `escript` standalone + saída JSON via `Jason`)**: ~50 MB a 300 MB de RAM conforme a quantidade de controllers Phoenix, templates `.eex`, routers e arquivos de configuração analisados.
- **Compilação Mix & Inspeção de Dependências (`mix.exs` / `mix.lock`)**: ~200 MB a 1 GB de RAM durante a compilação inicial do projeto alvo ao rodar como dependência `:dev` do Mix.

#### 🛠️ Dependências de Software
- **Runtime Core**: `Elixir >= 1.4` (recomendado `1.14+`) e `Erlang/OTP` compatível (`BEAM` VM com `:logger`, `:eex`, `:inets`)
- **Ferramentas de Build & Pacotes**: `Mix` e `Hex` (`mix local.hex`)
- **Dependências do Projeto (`mix.exs`)**: `jason ~> 1.0` (formatação de saída JSON), `ex_doc ~> 0.20` (apenas documentação em desenvolvimento)

---

## Installation

To use Sobelow, you can add it to your application's dependencies.

```elixir
def deps do
  [
    {:sobelow, "~> 0.8", only: :dev}
  ]
end
```

You can also install Sobelow globally by executing the following
from the command line:

    $ mix escript.install hex sobelow

To install from the master branch, rather than the latest release,
the following command can be used:

    $ mix escript.install github nccgroup/sobelow

## Use

The simplest way to scan a Phoenix project is to run the
following from the project root:

    $ mix sobelow

## Options

**Note:** Any path arguments should be absolute paths, or
relative to the application root.

  * `--root` or `-r` - Specify the application root directory.
  Accepts a path argument, e.g. `../my_project`.

  * `--verbose` or `-v` - Print code snippets and additional
  finding details.

  * `--ignore` or `-i` - Ignore given finding types. Accepts a
  comma-separated list of module names, e.g. `XSS.Raw,Traversal`.

  * `--ignore-files` - Ignore files. Accepts a comma-separated
  list of file names, e.g. `config/prod.exs`.

  * `--details` or `-d` - Get finding-type details. Accepts a
  single module name, e.g. `Config.CSRF`.

  * `--all-details` - Get details of all finding-types.

  * `--private` - Skip update checks.

  * `--router` - Specify router location. This only needs to be
  used if the router location is non-standard. Accepts a path
  argument, e.g. `my/strange/router.ex`.

  * `--exit` - Return non-zero exit status at or above a confidence
  threshold of `low`, `medium`, or `high`. Defaults to `false` which returns a zero exit status

  * `--threshold` - Return findings at or above a confidence level
  of `low` (default), `medium`, or `high`.

  * `--format` or `-f` - Specify findings output format. Accepts a format,
  e.g. `txt` or `json`.

      Note that options such as `--verbose` will not work with the `json` format.
      All `json` formatted findings contain a `type`, `file`, and `line` key.
      Other keys may vary.

  * `--quiet` - Return a single line indicating number of findings.
  Otherwise, return no output if there are no findings.

  * `--compact` - Minimal, single-line findings with output colorised
    according to confidence.

  * `--flycheck` - Minimal, single-line findings that are compatible
    with flycheck-based tooling.

  * `--save-config` - Generates a configuration file based on command
  line options. See [Configuration Files](#configuration-files) for more
  information.

  * `--config` - Run Sobelow with configuration file. See [Configuration Files](#configuration-files)
  for more information.

  * `--mark-skip-all` - Mark all displayed findings as skippable.

  * `--clear-skip` - Clear configuration created by `--mark-skip-all`.

  * `--skip` - Ignore findings that have been marked for skipping. See [False Positives](#false-positives)
  for more information.

## Configuration Files
Sobelow allows users to save frequently used options in a
configuration file. For example, if you find yourself constantly
running:

    $ mix sobelow -i XSS.Raw,Traversal --verbose --exit Low

You can use the `--save-config` flag to create your `.sobelow-conf`
config file:

    $ mix sobelow -i XSS.Raw,Traversal --verbose --exit Low --save-config

This command will create the `.sobelow-conf` file at the root
of your application. You can edit this file directly to make
changes.

You can also run the command without any options:

    $ mix sobelow --save-config

when you first start out using this package - the generated configuration file
will be populated with the default values for each option. (This helps in
quickly incorporating this package into a pre-existing codebase.)

Now if you want to run Sobelow with the saved configuration,
you can run Sobelow with the `--config` flag.

    $ mix sobelow --config

## False Positives
Sobelow favors over-reporting versus under-reporting. As such,
you may find a number of false positives in a typical scan.
These findings may be individually ignored by adding a
`# sobelow_skip` comment, along with a list of modules, before
the function definition.

```elixir
# sobelow_skip ["Traversal"]
def vuln_func(...) do
  ...
end
```

When integrating Sobelow into a new project, there can be a
large number of false positives. To mark all printed findings
as false positives, run sobelow with the `--mark-skip-all` flag.

Once you have tagged the appropriate findings, run
Sobelow with the `--skip` flag.

    $ mix sobelow --skip

While `# sobelow_skip` comments can only mark function-level
findings (and so cannot be used to skip configuration issues),
the `--mark-skip-all` flag can be used to skip any finding
type.

## Modules
Findings categories are broken up into modules. These modules
can then be used to either ignore classes of findings (via the
`ignore` and `skip` options) or to get vulnerability details (via the
`details` option).

This list, and other helpful information, can be found on the
command line:

    $ mix help sobelow

## Updates
When scanning a project, Sobelow will occasionally check for
updates, and will print an alert if a new version is available.
Sobelow keeps track of the last update-check by creating a
`.sobelow` file in the root of the scanned project.

If this functionality is not desired, the `--private` flag can
be used with the scan.
