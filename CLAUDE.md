# CLAUDE.md

## Project Overview

ElectricPy is a Python library providing electrical engineering functions and constants for research, development, education, and exploration. It includes support for phasor analysis, power systems, fault calculations, unit conversions, Bode plots, LaTeX formula generation, and visualization.

## Build & Install

```bash
pip install .                    # Standard install
pip install .[full]              # Install with all optional deps (arcflash, numdifftools)
pip install .[fault]             # Install with arc flash support
```

Build system: **flit** (PEP 517). Version is in `electricpy/version.py`.

## Testing

```bash
pip install -r test/requirements.txt   # Install test deps (pytest, xdoctest)
pytest --xdoctest                       # Run all tests + doctests
```

- Tests live in `test/` directory, one file per module (e.g., `test_phasor.py`, `test_passive.py`)
- CI runs on Python 3.8, 3.9, 3.10, 3.11 (ubuntu-latest)
- Doctests in source are executed via `--xdoctest` flag

## Linting & Style

```bash
pip install pydocstyle
cd electricpy && pydocstyle --convention=numpy
```

- **pydocstyle** enforces NumPyDoc docstring format (CI runs on Python 3.11)
- No autoformatter (black/ruff) is configured; follow existing code style

## Code Conventions

### Imports

Internal modules alias imports with underscore prefix to avoid namespace pollution:

```python
import numpy as _np
import cmath as _c
```

### Naming

- Constants: `UPPER_CASE` (e.g., `WATTS_PER_HP`, `NAN`)
- Functions: `snake_case` (e.g., `tcycle`, `phasor_impedance`)
- Private helpers: underscore prefix (e.g., `_rad`, `_as_float`)
- Use descriptive parameter names (`Vgenerator` not `V1`) when multiple similar quantities are in one function

### Docstrings (NumPyDoc format - required)

```python
def func(param):
    r"""Brief one-line description.

    Extended description.

    .. math:: V = I \cdot R

    Parameters
    ----------
    param : type
        Description.

    Returns
    -------
    result : type
        Description.

    Examples
    --------
    >>> import electricpy as ep
    >>> ep.func(42)
    result
    """
```

- First line must be a brief one-line summary
- Use `r"""` (raw) when docstrings contain LaTeX
- Include `.. math::` blocks for any non-trivial formulas
- Include runnable doctest examples (validated by `--xdoctest`)

### Code Style

- Build on existing ElectricPy functions when possible
- Add comments to clarify non-obvious operations
- Follow patterns in existing modules for consistency

## Project Structure

```
electricpy/          # Main package
  __init__.py        # Core functions (largest module)
  phasors.py         # Phasor generation and manipulation
  constants.py       # Electrical constants and matrices
  conversions.py     # Unit conversions
  passive.py         # RLC / passive component calculations
  machines.py        # Motor/generator calculations
  fault.py           # Fault analysis (optional: arcflash)
  bode.py            # Bode plots / frequency response
  latex.py           # LaTeX formula generation
  visu.py            # Visualization (phasor plots, power triangles)
  sim.py             # Simulation utilities
  thermal.py         # Thermal calculations
  compute.py         # Computational utilities
  math.py            # Math helpers
  geometry/          # Coordinate geometry (Point, Line, Circle, Triangle)
  active/            # Active electronics submodule
  version.py         # Package name and version
test/                # pytest test suite
docsource/           # Sphinx documentation source
docs/                # Generated documentation output
```

## CI/CD

- **pytest**: Runs tests on push/PR for `.py` changes (Python 3.8-3.11)
- **pydocstyle**: Validates NumPyDoc docstring format on all push/PR
- **sphinx**: Builds docs, generates coverage badge, deploys to GitHub Pages
- **pip-audit**: Scans dependencies for security vulnerabilities
- **release**: Auto-publishes to PyPI when `electricpy/version.py` changes on master
