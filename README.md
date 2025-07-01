# Python UV Setup

![Latest Release](https://img.shields.io/github/v/release/p6m-actions/python-uv-setup?style=flat-square&label=Latest%20Release&color=purple)

## Description

A GitHub Action that automates the setup of UV (ultra-fast Python package installer and resolver) with built-in caching support for both UV installation and virtual environments. This action handles the installation of UV and configures caching to improve workflow execution times.

The action automatically handles:

- UV installation caching
- Virtual environment caching
- Version management
- Python version setup (optional)

## Usage

Add the following step to your GitHub Actions workflow:

```yaml
- uses: p6m-actions/python-uv-setup@v1
  with:
    version: "0.4.29" # Optional: Specify UV version
    python-version: "3.11" # Optional: Specify Python version
```

## Inputs

| Input          | Required | Default | Description                                          |
| -------------- | -------- | ------- | ---------------------------------------------------- |
| version        | false    | latest  | Specific version of UV to install (e.g. '0.4.29')   |
| python-version | false    |         | Version of Python to install (e.g. '3.11', '3.12') |

## Outputs

| Output         | Description                                     |
| -------------- | ----------------------------------------------- |
| version        | The actual version of UV that was installed    |
| python-version | The actual version of Python that was installed |

## Examples

Basic usage with latest UV version:

```yaml
steps:
  - uses: actions/checkout@v4
  - uses: p6m-actions/python-uv-setup@v1
  - run: uv sync
```

Specify UV and Python versions:

```yaml
steps:
  - uses: actions/checkout@v4
  - uses: p6m-actions/python-uv-setup@v1
    with:
      version: "0.4.29"
      python-version: "3.11"
  - run: uv sync
```

Complete workflow example with testing:

```yaml
name: Test Python Project
on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: ["3.9", "3.10", "3.11", "3.12"]
    
    steps:
      - uses: actions/checkout@v4
      
      - uses: p6m-actions/python-uv-setup@v1
        with:
          python-version: ${{ matrix.python-version }}
      
      - name: Install dependencies
        run: uv sync
      
      - name: Run tests
        run: uv run pytest
      
      - name: Run linting
        run: uv run ruff check
```

Using with caching for faster builds:

```yaml
steps:
  - uses: actions/checkout@v4
  
  - uses: p6m-actions/python-uv-setup@v1
    with:
      version: "0.4.29"
      python-version: "3.11"
  
  # Cache is automatically handled by the action
  - name: Install dependencies
    run: uv sync
  
  - name: Build package
    run: uv build
```