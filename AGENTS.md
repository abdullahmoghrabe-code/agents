# AGENTS.md

Instructions for AI coding agents working on TF-Agents.

## Project Overview

TF-Agents is a TensorFlow library for reinforcement learning and contextual bandits. The main library code is in `tf_agents/`.

## Directory Structure

- `tf_agents/` - Core library (agents, environments, policies, networks, etc.)
- `docs/tutorials/` - Jupyter notebook tutorials
- `tools/` - Development and testing utilities
- `.continue/checks/` - Continue CI check configurations

## Build & Install

```bash
# Install with test dependencies
pip install -e .[tests]

# Install with Reverb (replay buffer, Linux only)
pip install tf-agents[reverb]

# TensorFlow must be installed separately
pip install tensorflow  # or tf-nightly for development
```

## Environment Variables

- `TF_USE_LEGACY_KERAS=1` - Required; uses keras-2 (tf-keras) instead of keras-3

## Running Tests

```bash
# Run all tests
python setup.py test

# Run release tests (nightly or stable)
./tests_release.sh --type nightly
./tests_release.sh --type stable
```

Tests requiring isolation are listed in `test_individually.txt`. Known broken tests are in `broken_tests.txt` (CPU) and `broken_tests_gpu.txt` (GPU).

## Building Packages

```bash
# Build wheel (requires PYTHON_VERSION env var)
export PYTHON_VERSION=python3.9
./pip_pkg.sh /path/to/output [--release]
```

## Code Style

Follow `STYLE_GUIDE.md`:
- Use TensorFlow style guide
- Use `name_scope` at start of every function
- Run tensor args through `tf.convert_to_tensor` after name_scope
- Do not create tensors inside `@property` methods
- Define `__all__` in every module
- Use `utils.common.function` instead of `tf.function` in library code
- Avoid `tf.control_dependencies`; use `utils.common.function_in_tf1` for control flow ops

## Key Dependencies

- `tensorflow` / `tf-nightly`
- `tf-keras` / `tf-keras-nightly`
- `tensorflow-probability` / `tfp-nightly`
- `dm-reverb` / `dm-reverb-nightly` (Linux only)
- `gymnasium`, `gym`
- `numpy >= 1.26.2`

## Notes

- Reverb (replay buffer) only works on Linux
- Python 3.11+ requires pygame 2.1.3+
- Use `typing-extensions == 4.5.0` to avoid TF 2.15.0 compatibility issues
