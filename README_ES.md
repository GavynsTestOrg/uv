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
  <i>Instalando las dependencias de <a href="https://trio.readthedocs.io/">Trio</a> con una caché caliente.</i>
</p>

## Aspectos destacados

- Una sola herramienta para reemplazar `pip`, `pip-tools`, `pipx`, `poetry`, `pyenv`, `twine`, `virtualenv` y más.
- [10-100x más rápido](https://github.com/astral-sh/uv/blob/main/BENCHMARKS.md) que `pip`.
- Proporciona [gestión integral de proyectos](#proyectos), con un [archivo de bloqueo universal](https://docs.astral.sh/uv/concepts/projects/layout#the-lockfile).
- [Ejecuta scripts](#scripts), con soporte para [metadatos de dependencias en línea](https://docs.astral.sh/uv/guides/scripts#declaring-script-dependencies).
- [Instala y gestiona](#versiones-de-python) versiones de Python.
- [Ejecuta e instala](#herramientas) herramientas publicadas como paquetes de Python.
- Incluye una [interfaz compatible con pip](#la-interfaz-de-pip) para obtener una mejora de rendimiento con una CLI familiar.
- Admite [workspaces](https://docs.astral.sh/uv/concepts/projects/workspaces) al estilo de Cargo para proyectos escalables.
- Uso eficiente del espacio en disco, con una [caché global](https://docs.astral.sh/uv/concepts/cache) para la deduplicación de dependencias.
- Se puede instalar sin Rust ni Python mediante `curl` o `pip`.
- Compatible con macOS, Linux y Windows.

Astral respalda uv. Astral también crea [Ruff](https://github.com/astral-sh/ruff) y [ty](https://github.com/astral-sh/ty).

## Instalación

Instala uv con los instaladores independientes:

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

El instalador independiente permite que uv se actualice a la versión más reciente:

```bash
uv self update
```

Consulta la [documentación de instalación](https://docs.astral.sh/uv/getting-started/installation/) para obtener detalles y métodos de instalación alternativos.

## Documentación

La documentación de uv está en [docs.astral.sh/uv](https://docs.astral.sh/uv).

Además, `uv help` muestra la documentación de referencia de la línea de comandos.

## Funciones

### Proyectos

uv gestiona dependencias y entornos de proyectos, con soporte para archivos de bloqueo, workspaces y más, de forma similar a `rye` o `poetry`:

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

uv también admite compilar y publicar proyectos, incluso si uv no los gestiona. Consulta la [guía de publicación](https://docs.astral.sh/uv/guides/publish/) para obtener más información.

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
       (__)\       )\/\
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

Usa una versión específica de Python en el directorio actual:

```console
$ uv python pin 3.11
Pinned `.python-version` to `3.11`
```

Consulta la [documentación de instalación de Python](https://docs.astral.sh/uv/guides/install-python/) para empezar.

### La interfaz de pip

uv proporciona un reemplazo inmediato para comandos comunes de `pip`, `pip-tools` y `virtualenv`.

uv amplía sus interfaces con funciones avanzadas, como anulaciones de versiones de dependencias, resoluciones independientes de la plataforma, resoluciones reproducibles, estrategias de resolución alternativas y más.

Migra a uv sin cambiar los flujos de trabajo existentes y consigue una aceleración de 10-100x con la interfaz `uv pip`.

Compila requisitos en un archivo de requisitos independiente de la plataforma:

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

El proyecto apoya a quienes contribuyen en todos los niveles de experiencia y agradece una mayor participación. Consulta la [guía de contribución](https://github.com/astral-sh/uv?tab=contributing-ov-file#contributing) para empezar.

## Preguntas frecuentes

#### ¿Cómo se pronuncia uv?

Se pronuncia como "you - vee" ([`/juː viː/`](https://en.wikipedia.org/wiki/Help:IPA/English#Key))

#### ¿Cómo debe escribirse uv?

Simplemente "uv", por favor. Consulta la [guía de estilo](./STYLE.md#styling-uv) para obtener más detalles.

#### ¿Qué plataformas admite uv?

Consulta el documento de [compatibilidad de plataformas](https://docs.astral.sh/uv/reference/platforms/) de uv.

#### ¿Está uv listo para producción?

Sí, uv es estable y se usa ampliamente en producción. Consulta el documento de [política de versionado](https://docs.astral.sh/uv/reference/versioning/) de uv para obtener más detalles.

## Agradecimientos

El resolvedor de dependencias de uv usa [PubGrub](https://github.com/pubgrub-rs/pubgrub) internamente. uv agradece el apoyo del equipo responsable de mantener PubGrub, en especial de [Jacob Finkelman](https://github.com/Eh2406).

La implementación de Git de uv se basa en [Cargo](https://github.com/rust-lang/cargo).

Algunas de las optimizaciones de uv se inspiran en el gran trabajo de [pnpm](https://pnpm.io/), [Orogene](https://github.com/orogene/orogene) y [Bun](https://github.com/oven-sh/bun). uv también aprendió mucho de [Posy](https://github.com/njsmith/posy) de Nathaniel J. Smith y adaptó su [trampoline](https://github.com/njsmith/posy/tree/main/src/trampolines/windows-trampolines/posy-trampoline) para la compatibilidad con Windows.

## Licencia

uv ofrece una de las siguientes licencias:

- Apache License, Version 2.0, ([LICENSE-APACHE](LICENSE-APACHE) o <https://www.apache.org/licenses/LICENSE-2.0>)
- MIT license ([LICENSE-MIT](LICENSE-MIT) o <https://opensource.org/licenses/MIT>)

a elección de quien lo use.

A menos que se indique explícitamente lo contrario, cualquier contribución enviada intencionadamente para su inclusión en uv, según se define en la licencia Apache-2.0, tendrá doble licencia como se indica arriba, sin términos ni condiciones adicionales.

<div align="center">
  <a target="_blank" href="https://astral.sh" style="background:none">
    <img src="https://raw.githubusercontent.com/astral-sh/uv/main/assets/svg/Astral.svg" alt="Made by Astral">
  </a>
</div>