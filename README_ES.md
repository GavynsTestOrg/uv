# uv

[![uv](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/astral-sh/uv/main/assets/badge/v0.json)](https://github.com/astral-sh/uv)
[![image](https://img.shields.io/pypi/v/uv.svg)](https://pypi.python.org/pypi/uv)
[![image](https://img.shields.io/pypi/l/uv.svg)](https://pypi.python.org/pypi/uv)
[![image](https://img.shields.io/pypi/pyversions/uv.svg)](https://pypi.python.org/pypi/uv)
[![Actions status](https://github.com/astral-sh/uv/actions/workflows/ci.yml/badge.svg)](https://github.com/astral-sh/uv/actions)
[![Discord](https://img.shields.io/badge/Discord-%235865F2.svg?logo=discord&logoColor=white)](https://discord.gg/astral-sh)

Un administrador de paquetes y proyectos de Python extremadamente rápido, escrito en Rust.

<p align="center">
  <picture align="center">
    <source media="(prefers-color-scheme: dark)" srcset="https://github.com/astral-sh/uv/assets/1309177/03aa9163-1c79-4a87-a31d-7a9311ed9310">
    <source media="(prefers-color-scheme: light)" srcset="https://github.com/astral-sh/uv/assets/1309177/629e59c0-9c6e-4013-9ad4-adb2bcf5080d">
    <img alt="Muestra un gráfico de barras con resultados de referencia." src="https://github.com/astral-sh/uv/assets/1309177/629e59c0-9c6e-4013-9ad4-adb2bcf5080d">
  </picture>
</p>

<p align="center">
  <i>Instalación de las dependencias de <a href="https://trio.readthedocs.io/">Trio</a> con una caché precalentada.</i>
</p>

## Aspectos destacados

- Una sola herramienta para reemplazar `pip`, `pip-tools`, `pipx`, `poetry`, `pyenv`, `twine`, `virtualenv` y
  más.
- [10-100x más rápido](https://github.com/astral-sh/uv/blob/main/BENCHMARKS.md) que `pip`.
- Proporciona [administración integral de proyectos](#projects), con un
  [archivo de bloqueo universal](https://docs.astral.sh/uv/concepts/projects/layout#the-lockfile).
- [Ejecuta scripts](#scripts), con soporte para
  [metadatos de dependencias en línea](https://docs.astral.sh/uv/guides/scripts#declaring-script-dependencies).
- [Instala y administra](#python-versions) versiones de Python.
- [Ejecuta e instala](#tools) herramientas publicadas como paquetes de Python.
- Incluye una [interfaz compatible con pip](#the-pip-interface) para obtener una mejora de rendimiento con una
  CLI conocida.
- Admite [workspaces](https://docs.astral.sh/uv/concepts/projects/workspaces) al estilo de Cargo para
  proyectos escalables.
- Uso eficiente del espacio en disco, con una [caché global](https://docs.astral.sh/uv/concepts/cache) para
  la deduplicación de dependencias.
- Se puede instalar sin Rust ni Python mediante `curl` o `pip`.
- Es compatible con macOS, Linux y Windows.

uv cuenta con el respaldo de [Astral](https://astral.sh), los creadores de
[Ruff](https://github.com/astral-sh/ruff) y [ty](https://github.com/astral-sh/ty).

## Instalación

Instale uv con los instaladores independientes:

```bash
# En macOS y Linux.
curl -LsSf https://astral.sh/uv/install.sh | sh
```

```bash
# En Windows.
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

O bien, desde [PyPI](https://pypi.org/project/uv/):

```bash
# Con pip.
pip install uv
```

```bash
# O con pipx.
pipx install uv
```

Si se instala mediante el instalador independiente, uv puede actualizarse a la versión más reciente:

```bash
uv self update
```

Consulte la [documentación de instalación](https://docs.astral.sh/uv/getting-started/installation/) para
obtener detalles y métodos de instalación alternativos.

## Documentación

La documentación de uv está disponible en [docs.astral.sh/uv](https://docs.astral.sh/uv).

Además, la documentación de referencia de la línea de comandos se puede consultar con `uv help`.

## Funcionalidades

### Proyectos

uv administra las dependencias y los entornos de los proyectos, con soporte para archivos de bloqueo, workspaces y más,
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

Consulte la [documentación de proyectos](https://docs.astral.sh/uv/guides/projects/) para comenzar.

uv también admite la compilación y publicación de proyectos, incluso si no están administrados con uv. Consulte la
[guía de publicación](https://docs.astral.sh/uv/guides/publish/) para obtener más información.

### Scripts

uv administra dependencias y entornos para scripts de un solo archivo.

Cree un script nuevo y agregue metadatos en línea que declaren sus dependencias:

```console
$ echo 'import requests; print(requests.get("https://astral.sh"))' > example.py

$ uv add --script example.py requests
Updated `example.py`
```

Luego, ejecute el script en un entorno virtual aislado:

```console
$ uv run example.py
Reading inline script metadata from: example.py
Installed 5 packages in 12ms
<Response [200]>
```

Consulte la [documentación de scripts](https://docs.astral.sh/uv/guides/scripts/) para comenzar.

### Herramientas

uv ejecuta e instala herramientas de línea de comandos proporcionadas por paquetes de Python, de forma similar a `pipx`.

Ejecute una herramienta en un entorno efímero con `uvx` (un alias de `uv tool run`):

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

Instale una herramienta con `uv tool install`:

```console
$ uv tool install ruff
Resolved 1 package in 6ms
Installed 1 package in 2ms
 + ruff==0.5.0
Installed 1 executable: ruff

$ ruff --version
ruff 0.5.0
```

Consulte la [documentación de herramientas](https://docs.astral.sh/uv/guides/tools/) para comenzar.

### Versiones de Python

uv instala Python y permite cambiar rápidamente entre versiones.

Instale varias versiones de Python:

```console
$ uv python install 3.12 3.13 3.14
Installed 3 versions in 972ms
 + cpython-3.12.12-macos-aarch64-none (python3.12)
 + cpython-3.13.9-macos-aarch64-none (python3.13)
 + cpython-3.14.0-macos-aarch64-none (python3.14)

```

Descargue versiones de Python según sea necesario:

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

Use una versión específica de Python en el directorio actual:

```console
$ uv python pin 3.11
Pinned `.python-version` to `3.11`
```

Consulte la [documentación de instalación de Python](https://docs.astral.sh/uv/guides/install-python/) para
comenzar.

### La interfaz de pip

uv proporciona un reemplazo directo para comandos comunes de `pip`, `pip-tools` y `virtualenv`.

uv amplía sus interfaces con funciones avanzadas, como reemplazos de versiones de dependencias,
resoluciones independientes de la plataforma, resoluciones reproducibles, estrategias de resolución alternativas y
más.

Migre a uv sin cambiar los flujos de trabajo existentes y experimente una aceleración de 10-100x con la
interfaz `uv pip`.

Compile requisitos en un archivo de requisitos independiente de la plataforma:

```console
$ uv pip compile requirements.in \
   --universal \
   --output-file requirements.txt
Resolved 43 packages in 12ms
```

Cree un entorno virtual:

```console
$ uv venv
Using Python 3.12.3
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
```

Instale los requisitos bloqueados:

```console
$ uv pip sync requirements.txt
Resolved 43 packages in 11ms
Installed 43 packages in 208ms
 + babel==2.15.0
 + black==24.4.2
 + certifi==2024.7.4
 ...
```

Consulte la [documentación de la interfaz pip](https://docs.astral.sh/uv/pip/index/) para comenzar.

## Contribuir

El proyecto apoya a colaboradores de todos los niveles de experiencia y será excelente ver más participación en el
proyecto. Consulte la
[guía de contribución](https://github.com/astral-sh/uv?tab=contributing-ov-file#contributing) para comenzar.

## Preguntas frecuentes

#### ¿Cómo se pronuncia uv?

Se pronuncia como "you - vee" ([`/juː viː/`](https://en.wikipedia.org/wiki/Help:IPA/English#Key))

#### ¿Cómo debe escribirse uv?

Simplemente "uv", por favor. Consulte la [guía de estilo](./STYLE.md#styling-uv) para obtener más detalles.

#### ¿Qué plataformas admite uv?

Consulte el documento de [soporte de plataformas](https://docs.astral.sh/uv/reference/platforms/) de uv.

#### ¿Está uv listo para producción?

Sí, uv es estable y se usa ampliamente en producción. Consulte el documento de
[política de versiones](https://docs.astral.sh/uv/reference/versioning/) de uv para obtener más detalles.

## Agradecimientos

El solucionador de dependencias de uv usa [PubGrub](https://github.com/pubgrub-rs/pubgrub) internamente. El equipo agradece a
los mantenedores de PubGrub, en especial [Jacob Finkelman](https://github.com/Eh2406), por
su apoyo.

La implementación de Git de uv se basa en [Cargo](https://github.com/rust-lang/cargo).

Algunas de las optimizaciones de uv se inspiran en el gran trabajo visto en [pnpm](https://pnpm.io/),
[Orogene](https://github.com/orogene/orogene) y [Bun](https://github.com/oven-sh/bun). También se aprendió mucho
del [Posy](https://github.com/njsmith/posy) de Nathaniel J. Smith y se adaptó su
[trampoline](https://github.com/njsmith/posy/tree/main/src/trampolines/windows-trampolines/posy-trampoline)
para el soporte de Windows.

## Licencia

uv se licencia bajo una de las siguientes opciones:

- Apache License, Version 2.0, ([LICENSE-APACHE](LICENSE-APACHE) o
  <https://www.apache.org/licenses/LICENSE-2.0>)
- licencia MIT ([LICENSE-MIT](LICENSE-MIT) o <https://opensource.org/licenses/MIT>)

a elección de cada persona.

Salvo que se indique explícitamente lo contrario, cualquier contribución enviada intencionalmente para su inclusión en uv
por cualquier persona, según la definición de la licencia Apache-2.0, tendrá doble licencia como se indicó antes, sin
términos ni condiciones adicionales.

<div align="center">
  <a target="_blank" href="https://astral.sh" style="background:none">
    <img src="https://raw.githubusercontent.com/astral-sh/uv/main/assets/svg/Astral.svg" alt="Made by Astral">
  </a>
</div>