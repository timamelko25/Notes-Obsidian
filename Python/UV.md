uv - package and virtual environments manager 

```table-of-contents
title: 
style: nestedList # TOC style (nestedList|nestedOrderedList|inlineFirstLevel)
minLevel: 0 # Include headings from the specified level
maxLevel: 0 # Include headings up to the specified level
includeLinks: true # Make headings clickable
hideWhenEmpty: false # Hide TOC if no headings are found
debugInConsole: false # Print debug info in Obsidian console
```
# Install

## Windows

```
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
```
## Linux

```
curl -LsSf https://astral.sh/uv/install.sh | sh
```
# Update uv

```
uv self update
```

# Create project

## Create new project

```
uv init __name__
cd __name__
```
## Create project in existing directory

```
mkdir __name__
cd __name__
uv init
```

After creating project you can see new files:
- .gitignore
- .python-version - contains info about python version by default. if verion not installed `uv` automaticly will install correct version. To change version in file `uv python pin __ver__`
- main.py
- pyproject.toml - contains info about installed dependencies
```
[project]name = "venv"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = []
```

# Run project

```
uv run main.py
```

Automaticly create and activate/deactivate venv. Also create uv.lock file 

Run file with no creating venv folder in system

```
uv run --with requests main.py 
```

Also with run command without flag `--with` you can create comment script in top of the file

```
# /// script
# requires-python = ">=3.13"
# dependencies = [
#     "requests"
# ]

# ///
def main():
	...
```

Also run file without `uv run` you can create shebang line

```
#!/usr/bin/env -S uv run --script
```
# Install package

```
uv add requests
```

with specific version 

```
uv add requests==2.31.0
```

**Install from requirements.txt**

```
uv add -r requirements.txt
```

You can write packages in dependencies and install from there with command

```
uv sync
```

# Remove package

```
uv remove requests yt-dlp
```
# Packages list

```
uv tree
```

# Install new python version 

```
uv python install 3.10
```

see all version (only installed with flag --only-installed)

```
uv python list
```

# Pip

Uv have backward compatibility with pip. You can run commands with 

```
uv pip install -r requirements.txt
```
# UVX
(alias for `uv tool run`)
Using for tools (ruff)

## Ruff 
linter and code formatter

### Run 

```
uvx ruff check .
```

fix found warnings

```
uvx ruff check . --fix
```


dev????

???
```
nano pyrightconfig.json
```

```
{
	"venvPath": ".",
	"venv": ".venv"
}
```