# uv

[![uv](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/astral-sh/uv/main/assets/badge/v0.json)](https://github.com/astral-sh/uv)
[![image](https://img.shields.io/pypi/v/uv.svg)](https://pypi.python.org/pypi/uv)
[![image](https://img.shields.io/pypi/l/uv.svg)](https://pypi.python.org/pypi/uv)
[![image](https://img.shields.io/pypi/pyversions/uv.svg)](https://pypi.python.org/pypi/uv)
[![Actions status](https://github.com/astral-sh/uv/actions/workflows/ci.yml/badge.svg)](https://github.com/astral-sh/uv/actions)
[![Discord](https://img.shields.io/badge/Discord-%235865F2.svg?logo=discord&logoColor=white)](https://discord.gg/astral-sh)

Un gestor de paquetes y proyectos de Python extremadamente rápido, escrito en Rust.

<p align="center">
  <picture align="center">
    <source media="(prefers-color-scheme: dark)" srcset="https://github.com/astral-sh/uv/assets/1309177/03aa9163-1c79-4a87-a31d-7a9311ed9310">
    <source media="(prefers-color-scheme: light)" srcset="https://github.com/astral-sh/uv/assets/1309177/629e59c0-9c6e-4013-9ad4-adb2bcf5080d">
    <img alt="Shows a bar chart with benchmark results." src="https://github.com/astral-sh/uv/assets/1309177/629e59c0-9c6e-4013-9ad4-adb2bcf5080d">
  </picture>
</p>

<p align="center">
  <i>Instalación de las dependencias de <a href="https://trio.readthedocs.io/">Trio</a> con una caché caliente.</i>
</p>

## Aspectos destacados

- Una sola herramienta para reemplazar `pip`, `pip-tools`, `pipx`, `poetry`, `pyenv`, `twine`, `virtualenv` y
  más.
- [10-100x más rápido](https://github.com/astral-sh/uv/blob/main/BENCHMARKS.md) que `pip`.
- Proporciona [gestión integral de proyectos](#projects), con un
  [archivo de bloqueo universal](https://docs.astral.sh/uv/concepts/projects/layout#the-lockfile).
- [Ejecuta scripts](#scripts), con soporte para
  [metadatos de dependencias en línea](https://docs.astral.sh/uv/guides/scripts#declaring-script-dependencies).
- [Instala y gestiona](#python-versions) versiones de Python.
- [Ejecuta e instala](#tools) herramientas publicadas como paquetes de Python.
- Incluye una [interfaz compatible con pip](#the-pip-interface) para obtener una mejora de rendimiento con una
  CLI familiar.
- Soporta [workspaces](https://docs.astral.sh/uv/concepts/projects/workspaces) al estilo Cargo para
  proyectos escalables.
- Eficiente en espacio de disco, con una [caché global](https://docs.astral.sh/uv/concepts/cache) para
  la deduplicación de dependencias.
- Se puede instalar sin Rust ni Python mediante `curl` o `pip`.
- Soporta macOS, Linux y Windows.

[Astral](https://astral.sh) respalda uv y creó
[Ruff](https://github.com/astral-sh/ruff) y [ty](https://github.com/astral-sh/ty).

## Instalación

Instala uv con los instaladores independientes:

```bash
# On macOS and Linux.
curl -LsSf https://astral.sh/uv/install.sh | sh
```

```bash
# On Windows.
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

O bien, desde [PyPI](https://pypi.org/project/uv/):

```bash
# With pip.
pip install uv
```

```bash
# Or pipx.
pipx install uv
```

Si se instala mediante el instalador independiente, uv puede actualizarse a la versión más reciente:

```bash
uv self update
```

La [documentación de instalación](https://docs.astral.sh/uv/getting-started/installation/) detalla métodos alternativos de instalación.

## Documentación

La documentación de uv está en [docs.astral.sh/uv](https://docs.astral.sh/uv).

Además, `uv help` muestra la documentación de referencia de la línea de comandos.

## Funciones

### Proyectos

uv gestiona dependencias y entornos de proyectos, con soporte para archivos de bloqueo, workspaces y más,
de forma similar a `rye` o `poetry`:

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

Consulta la [documentación de proyectos](https://docs.astral.sh/uv/guides/projects/) para empezar.

uv también soporta compilar y publicar proyectos, incluso si uv no los gestiona. Consulta la
[guía de publicación](https://docs.astral.sh/uv/guides/publish/) para obtener más información.

### Scripts

uv gestiona dependencias y entornos para scripts de un solo archivo.

Crea un script nuevo y añade metadatos en línea que declaren sus dependencias:

```console
$ echo 'import requests; print(requests.get("https://astral.sh"))' > example.py

$ uv add --script example.py requests
Updated `example.py`
```

Después, ejecuta el script en un entorno virtual aislado:

```console
$ uv run example.py
Reading inline script metadata from: example.py
Installed 5 packages in 12ms
<Response [200]>
```

Consulta la [documentación de scripts](https://docs.astral.sh/uv/guides/scripts/) para empezar.

### Herramientas

uv ejecuta e instala herramientas de línea de comandos proporcionadas por paquetes de Python, de forma similar a `pipx`.

Ejecuta una herramienta en un entorno efímero con `uvx` (un alias de `uv tool run`):

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

Instala una herramienta con `uv tool install`:

```console
$ uv tool install ruff
Resolved 1 package in 6ms
Installed 1 package in 2ms
 + ruff==0.5.0
Installed 1 executable: ruff

$ ruff --version
ruff 0.5.0
```

Consulta la [documentación de herramientas](https://docs.astral.sh/uv/guides/tools/) para empezar.

### Versiones de Python

uv instala Python y permite cambiar rápidamente entre versiones.

Instala varias versiones de Python:

```console
$ uv python install 3.12 3.13 3.14
Installed 3 versions in 972ms
 + cpython-3.12.12-macos-aarch64-none (python3.12)
 + cpython-3.13.9-macos-aarch64-none (python3.13)
 + cpython-3.14.0-macos-aarch64-none (python3.14)

```

Descarga versiones de Python según sea necesario:

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

Usa una versión concreta de Python en el directorio actual:

```console
$ uv python pin 3.11
Pinned `.python-version` to `3.11`
```

Consulta la [documentación de instalación de Python](https://docs.astral.sh/uv/guides/install-python/) para
empezar.

### La interfaz de pip

uv ofrece un reemplazo directo para los comandos habituales de `pip`, `pip-tools` y `virtualenv`.

uv amplía sus interfaces con funciones avanzadas, como anulaciones de versión de dependencias,
resoluciones independientes de la plataforma, resoluciones reproducibles, estrategias alternativas de resolución y
más.

Migra a uv sin cambiar los flujos de trabajo existentes y experimenta una aceleración de 10-100x con la
interfaz `uv pip`.

Compila los requisitos en un archivo de requisitos independiente de la plataforma:

```console
$ uv pip compile requirements.in \
   --universal \
   --output-file requirements.txt
Resolved 43 packages in 12ms
```

Crea un entorno virtual:

```console
$ uv venv
Using Python 3.12.3
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
```

Instala los requisitos bloqueados:

```console
$ uv pip sync requirements.txt
Resolved 43 packages in 11ms
Installed 43 packages in 208ms
 + babel==2.15.0
 + black==24.4.2
 + certifi==2024.7.4
 ...
```

Consulta la [documentación de la interfaz pip](https://docs.astral.sh/uv/pip/index/) para empezar.

## Contribuir

Existe una gran pasión por apoyar a personas colaboradoras con todos los niveles de experiencia y sería estupendo ver
su participación en el proyecto. Consulta la
[guía de contribución](https://github.com/astral-sh/uv?tab=contributing-ov-file#contributing) para empezar.

## Preguntas frecuentes

#### ¿Cómo se pronuncia uv?

Se pronuncia como "you - vee" ([`/juː viː/`](https://en.wikipedia.org/wiki/Help:IPA/English#Key))

#### ¿Cómo debe escribirse uv?

Simplemente "uv", por favor. Consulta la [guía de estilo](./STYLE.md#styling-uv) para ver los detalles.

#### ¿Qué plataformas soporta uv?

Consulta el documento de [compatibilidad de plataformas](https://docs.astral.sh/uv/reference/platforms/) de uv.

#### ¿Está uv listo para producción?

Sí, uv es estable y se usa ampliamente en producción. Consulta el documento de
[política de versiones](https://docs.astral.sh/uv/reference/versioning/) de uv para ver los detalles.

## Agradecimientos

El resolvedor de dependencias de uv usa [PubGrub](https://github.com/pubgrub-rs/pubgrub) internamente. uv agradece a las
personas mantenedoras de PubGrub, en especial a [Jacob Finkelman](https://github.com/Eh2406), su apoyo.

uv basa su implementación de Git en [Cargo](https://github.com/rust-lang/cargo).

Algunas de las optimizaciones de uv se inspiran en el gran trabajo de [pnpm](https://pnpm.io/),
[Orogene](https://github.com/orogene/orogene) y [Bun](https://github.com/oven-sh/bun). uv también aprendió mucho de [Posy](https://github.com/njsmith/posy) de Nathaniel J. Smith y adaptó su
[trampoline](https://github.com/njsmith/posy/tree/main/src/trampolines/windows-trampolines/posy-trampoline)
para ofrecer soporte para Windows.

## Licencia

uv ofrece estas opciones de licencia

- Apache License, Version 2.0, ([LICENSE-APACHE](LICENSE-APACHE) o
  <https://www.apache.org/licenses/LICENSE-2.0>)
- licencia MIT ([LICENSE-MIT](LICENSE-MIT) o <https://opensource.org/licenses/MIT>)

a elección del usuario.

Salvo que se indique explícitamente lo contrario, cualquier contribución enviada intencionadamente para incluirse en uv
por parte de quien contribuye, tal como se define en la licencia Apache-2.0, tendrá doble licencia como se indica arriba, sin
términos ni condiciones adicionales.

<div align="center">
  <a target="_blank" href="https://astral.sh" style="background:none">
    <img src="https://raw.githubusercontent.com/astral-sh/uv/main/assets/svg/Astral.svg" alt="Made by Astral">
  </a>
</div>