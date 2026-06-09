# uv

[![uv](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/astral-sh/uv/main/assets/badge/v0.json)](https://github.com/astral-sh/uv)
[![image](https://img.shields.io/pypi/v/uv.svg)](https://pypi.python.org/pypi/uv)
[![image](https://img.shields.io/pypi/l/uv.svg)](https://pypi.python.org/pypi/uv)
[![image](https://img.shields.io/pypi/pyversions/uv.svg)](https://pypi.python.org/pypi/uv)
[![Actions status](https://github.com/astral-sh/uv/actions/workflows/ci.yml/badge.svg)](https://github.com/astral-sh/uv/actions)
[![Discord](https://img.shields.io/badge/Discord-%235865F2.svg?logo=discord&logoColor=white)](https://discord.gg/astral-sh)

Un gestore di pacchetti e progetti Python estremamente veloce, scritto in Rust.

<p align="center">
  <picture align="center">
    <source media="(prefers-color-scheme: dark)" srcset="https://github.com/astral-sh/uv/assets/1309177/03aa9163-1c79-4a87-a31d-7a9311ed9310">
    <source media="(prefers-color-scheme: light)" srcset="https://github.com/astral-sh/uv/assets/1309177/629e59c0-9c6e-4013-9ad4-adb2bcf5080d">
    <img alt="Mostra un grafico a barre con i risultati dei benchmark." src="https://github.com/astral-sh/uv/assets/1309177/629e59c0-9c6e-4013-9ad4-adb2bcf5080d">
  </picture>
</p>

<p align="center">
  <i>Installazione delle dipendenze di <a href="https://trio.readthedocs.io/">Trio</a> con una cache già pronta.</i>
</p>

## Punti salienti

- Un singolo strumento per sostituire `pip`, `pip-tools`, `pipx`, `poetry`, `pyenv`, `twine`, `virtualenv` e altro.
- [10-100x più veloce](https://github.com/astral-sh/uv/blob/main/BENCHMARKS.md) di `pip`.
- Offre una [gestione completa dei progetti](#progetti), con un [lockfile universale](https://docs.astral.sh/uv/concepts/projects/layout#the-lockfile).
- [Esegue script](#script), con supporto per [metadati inline delle dipendenze](https://docs.astral.sh/uv/guides/scripts#declaring-script-dependencies).
- [Installa e gestisce](#versioni-di-python) versioni di Python.
- [Esegue e installa](#strumenti) strumenti pubblicati come pacchetti Python.
- Include un'[interfaccia compatibile con pip](#linterfaccia-pip-1) per un aumento delle prestazioni con una CLI familiare.
- Supporta [workspaces](https://docs.astral.sh/uv/concepts/projects/workspaces) in stile Cargo per progetti scalabili.
- Efficiente nello spazio su disco, con una [cache globale](https://docs.astral.sh/uv/concepts/cache) per la deduplicazione delle dipendenze.
- Installabile senza Rust o Python tramite `curl` o `pip`.
- Supporta macOS, Linux e Windows.

uv è supportato da [Astral](https://astral.sh), i creatori di [Ruff](https://github.com/astral-sh/ruff) e [ty](https://github.com/astral-sh/ty).

## Installazione

Installa uv con i programmi di installazione autonomi:

```bash
# On macOS and Linux.
curl -LsSf https://astral.sh/uv/install.sh | sh
```

```bash
# On Windows.
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

Oppure, da [PyPI](https://pypi.org/project/uv/):

```bash
# With pip.
pip install uv
```

```bash
# Or pipx.
pipx install uv
```

Se è stato installato tramite l'installer standalone, uv può aggiornarsi all'ultima versione:

```bash
uv self update
```

Per dettagli e metodi di installazione alternativi, vedere la [documentazione di installazione](https://docs.astral.sh/uv/getting-started/installation/).

## Documentazione

La documentazione di uv è consultabile su [docs.astral.sh/uv](https://docs.astral.sh/uv).

Inoltre, `uv help` mostra la documentazione di riferimento della riga di comando.

## Funzionalità

### Progetti

uv gestisce le dipendenze e gli ambienti di progetto, con supporto per lockfile, workspace e altro, in modo simile a `rye` o `poetry`:

```console
$ uv init example
Initialized project `example` at `/home/user/example`

$ cd example

$ uv add ruff
Creating virtual environment at: .venv
Resolved 2 packages in 170ms
   Built example @ file:///home/user/example
Prepared 2 packages in 627ms
Installed 2 packages in 1ms
 + example==0.1.0 (from file:///home/user/example)
 + ruff==0.5.0

$ uv run ruff check
All checks passed!

$ uv lock
Resolved 2 packages in 0.33ms

$ uv sync
Resolved 2 packages in 0.70ms
Checked 1 package in 0.02ms
```

Per iniziare, vedere la [documentazione sui progetti](https://docs.astral.sh/uv/guides/projects/).

uv supporta anche la creazione e la pubblicazione di progetti, anche se non sono gestiti con uv. Per saperne di più, vedere la [guida alla pubblicazione](https://docs.astral.sh/uv/guides/publish/).

### Script

uv gestisce dipendenze e ambienti per script composti da un singolo file.

Crea un nuovo script e aggiungi metadati inline che dichiarano le dipendenze:

```console
$ echo 'import requests; print(requests.get("https://astral.sh"))' > example.py

$ uv add --script example.py requests
Updated `example.py`
```

Quindi, esegui lo script in un ambiente virtuale isolato:

```console
$ uv run example.py
Reading inline script metadata from: example.py
Installed 5 packages in 12ms
<Response [200]>
```

Per iniziare, vedere la [documentazione sugli script](https://docs.astral.sh/uv/guides/scripts/).

### Strumenti

uv esegue e installa strumenti da riga di comando forniti da pacchetti Python, in modo simile a `pipx`.

Esegui uno strumento in un ambiente effimero usando `uvx` (un alias per `uv tool run`):

```console
$ uvx pycowsay 'hello world!'
Resolved 1 package in 167ms
Installed 1 package in 9ms
 + pycowsay==0.0.0.2
  """

  ------------
< hello world! >
  ------------
   \   ^__^
    \  (oo)\_______
       (__)\       )\/\\
           ||----w |
           ||     ||
```

Installa uno strumento con `uv tool install`:

```console
$ uv tool install ruff
Resolved 1 package in 6ms
Installed 1 package in 2ms
 + ruff==0.5.0
Installed 1 executable: ruff

$ ruff --version
ruff 0.5.0
```

Per iniziare, vedere la [documentazione sugli strumenti](https://docs.astral.sh/uv/guides/tools/).

### Versioni di Python

uv installa Python e consente di passare rapidamente da una versione all'altra.

Installa più versioni di Python:

```console
$ uv python install 3.12 3.13 3.14
Installed 3 versions in 972ms
 + cpython-3.12.12-macos-aarch64-none (python3.12)
 + cpython-3.13.9-macos-aarch64-none (python3.13)
 + cpython-3.14.0-macos-aarch64-none (python3.14)

```

Scarica le versioni di Python quando necessario:

```console
$ uv venv --python 3.12.0
Using Python 3.12.0
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate

$ uv run --python pypy@3.8 -- python --version
Python 3.8.16 (a9dbdca6fc3286b0addd2240f11d97d8e8de187a, Dec 29 2022, 11:45:30)
[PyPy 7.3.11 with GCC Apple LLVM 13.1.6 (clang-1316.0.21.2.5)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>>
```

Usa una versione specifica di Python nella directory corrente:

```console
$ uv python pin 3.11
Pinned `.python-version` to `3.11`
```

Per iniziare, vedere la [documentazione sull'installazione di Python](https://docs.astral.sh/uv/guides/install-python/).

### L'interfaccia pip

uv fornisce un sostituto diretto dei comuni comandi `pip`, `pip-tools` e `virtualenv`.

uv estende le relative interfacce con funzionalità avanzate, come la possibilità di sostituire le versioni delle dipendenze, risoluzioni indipendenti dalla piattaforma, risoluzioni riproducibili, strategie di risoluzione alternative e altro.

Migra a uv senza cambiare i flussi di lavoro esistenti e sperimenta un'accelerazione di 10-100x con l'interfaccia `uv pip`.

Compila i requisiti in un file requirements indipendente dalla piattaforma:

```console
$ uv pip compile requirements.in \
   --universal \
   --output-file requirements.txt
Resolved 43 packages in 12ms
```

Crea un ambiente virtuale:

```console
$ uv venv
Using Python 3.12.3
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
```

Installa i requisiti bloccati:

```console
$ uv pip sync requirements.txt
Resolved 43 packages in 11ms
Installed 43 packages in 208ms
 + babel==2.15.0
 + black==24.4.2
 + certifi==2024.7.4
 ...
```

Per iniziare, vedere la [documentazione sull'interfaccia pip](https://docs.astral.sh/uv/pip/index/).

## Contributi

Il progetto sostiene contributori di ogni livello di esperienza e accoglie con favore la partecipazione. Per iniziare, vedere la [guida ai contributi](https://github.com/astral-sh/uv?tab=contributing-ov-file#contributing).

## FAQ

#### Come si pronuncia uv?

Si pronuncia "you - vee" ([`/juː viː/`](https://en.wikipedia.org/wiki/Help:IPA/English#Key))

#### Come dovrebbe essere stilizzato uv?

Semplicemente "uv", per favore. Per dettagli, vedere la [guida di stile](./STYLE.md#styling-uv).

#### Quali piattaforme supporta uv?

Vedere il documento sul [supporto delle piattaforme](https://docs.astral.sh/uv/reference/platforms/) di uv.

#### uv è pronto per la produzione?

Sì, uv è stabile ed è ampiamente usato in produzione. Per dettagli, vedere il documento sulla [policy di versionamento](https://docs.astral.sh/uv/reference/versioning/) di uv.

## Ringraziamenti

Il sistema di risoluzione delle dipendenze di uv usa internamente [PubGrub](https://github.com/pubgrub-rs/pubgrub). Grande riconoscenza va ai responsabili di PubGrub, in particolare a [Jacob Finkelman](https://github.com/Eh2406), per il supporto.

L'implementazione Git di uv si basa su [Cargo](https://github.com/rust-lang/cargo).

Alcune ottimizzazioni di uv traggono ispirazione dall'ottimo lavoro visto in [pnpm](https://pnpm.io/), [Orogene](https://github.com/orogene/orogene) e [Bun](https://github.com/oven-sh/bun). Molto è stato appreso anche da [Posy](https://github.com/njsmith/posy) di Nathaniel J. Smith e il suo [trampoline](https://github.com/njsmith/posy/tree/main/src/trampolines/windows-trampolines/posy-trampoline) è stato adattato per il supporto a Windows.

## Licenza

uv usa una delle seguenti licenze:

- Apache License, Version 2.0, ([LICENSE-APACHE](LICENSE-APACHE) oppure <https://www.apache.org/licenses/LICENSE-2.0>)
- licenza MIT ([LICENSE-MIT](LICENSE-MIT) oppure <https://opensource.org/licenses/MIT>)

a scelta.

Salvo dichiarazione esplicita contraria, qualsiasi contributo inviato intenzionalmente per l'inclusione in uv, come definito nella licenza Apache-2.0, ricade nella doppia licenza sopra indicata, senza termini o condizioni aggiuntivi.

<div align="center">
  <a target="_blank" href="https://astral.sh" style="background:none">
    <img src="https://raw.githubusercontent.com/astral-sh/uv/main/assets/svg/Astral.svg" alt="Creato da Astral">
  </a>
</div>