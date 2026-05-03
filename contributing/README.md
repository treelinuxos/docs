# Contributing to Treelinux

## Development Setup

### Prerequisites

- Python 3.8+
- QEMU (for testing)
- Git

### Clone Repos

```bash
git clone https://github.com/treelinuxos/treelinux.git
git clone https://github.com/treelinuxos/sprout.git
```

### Sprout Development

```bash
cd sprout
pip install -e .
```

Run tests:
```bash
python3 -m unittest discover -s tests -v
```

## Project Structure

### treelinux (this repo)
- `sprout/` - sprout package manager source
- `docs/` - documentation
- `treelinux-base.qcow2` - base VM image

### sprout repo
- `sprout/` - Python package
- `modules/` - .smp module examples
- `configs/` - example .prc configs
- `tests/` - unit tests

## Code Style

- No comments unless asked
- Concise, direct code
- Follow existing patterns

## Submitting Changes

1. Fork the repo
2. Create a feature branch
3. Make your changes
4. Run tests
5. Submit pull request

## Internals

### .prc Parser

Located in `sprout/parser.py`. Parses `.prc` files into structured dicts.

### Diff Engine

Located in `sprout/diff.py`. Compares desired config vs actual system state.

### Applier

Located in `sprout/applier.py`. Applies config to live system.

### Module Runner

Located in `sprout/runner.py`. Executes `.smp` modules with `sprout_lib` available.

## License

GPLv3 - see LICENSE file.
