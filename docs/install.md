# Install

## Preview package expectation

This public preview surface is designed as a landing zone plus safe examples.

The runnable preview should be distributed as a curated release package, such as:

- a wheel
- or a packaged preview bundle with install instructions

Cloning the public preview repository alone is not the primary install path. Start from the release bundle when you want to evaluate the runnable CLI quickly.

## Supported Windows preview path once the package is downloaded

```powershell
py -3 -m venv .venv
.\.venv\Scripts\python.exe -m pip install .\dist\signalforge_preview-0.1.1-py3-none-any.whl
cmd /c ".venv\Scripts\activate.bat && signalforge --help"
```

Why this path:

- it is the most reliable first-run path in current preview validation
- it avoids common PowerShell execution-policy friction
- it gets you to report generation fast

## Alternative invocation

Direct venv executable invocation also works:

```powershell
.\.venv\Scripts\signalforge.exe --help
```

## Notes

- Python 3.10+ is recommended
- installation may still need normal package download access for Python dependencies
- the generated HTML preview itself is intended to be viewable offline after generation
- the current supported preview path is Windows-first
- this staging folder does not include the private runtime source tree
