Learning from Dr Fred Baptiste's Python Fundamentals course on [Udemy](https://www.udemy.com/course/python3-fundamentals/).

- Created by **Guido van Rossum** in 1989.
- Was named after the British comedy group **Monty Python**.
- Still actively developed today
	- Python 2.0 released in 2000 → last release was 2.7 (end of April 2020).
- The reference implementation of python is **CPython** otherwise known as **Canonical Python**. This is the standard, official python.

## Virtual Environments
Virtual Environments in Python are used to make a copy of Python installation to enable you switch between Python versions and Python dependency versions or differences for your Python project.

Within Python virtual environments (or venv) are scripts to "activate" / "deactivate" the environment.

### Creating virtual environments
```bash
<python version/path> -m venv <your_env_name>
```

For example, if using python3.9 and you'd like to name your virtual environment 'py39_course':
```bash
python3.9 -m venv py39_course
```

> If using zsh on wsl:ubuntu and you encounter a problem of not seeing the indication of the activated virtual environment via the text in parenthesis next to your user prompt in your terminal (as shown in the image below). Do the following:

![venv indication in python](assets/Pasted%20image%2020241224235005.png)

1. Open `~/.zshrc` file and check that you have included `virtualenv` in your plugins array, like so:
   ```bash
   plugins=(git virtualenv python)
   ```
2. If plugin included and venv indication is not working, disable prompt suppression:
   ```bash
   # export VIRTUAL_ENV_DISABLE_PROMPT=1
   ```
3. Check common configuration files for the env variable `VIRTUAL_ENV_DISABLE_PROMPT`:
   ```bash
   grep -r "VIRTUAL_ENV_DISABLE_PROMPT" ~/.zshrc ~/.zshenv ~/.oh-my-zsh/*
     ```
4. If after commenting out the env variable `export` command, it's still set. Unset the environment variable:
   ```bash
   unset VIRTUAL_ENV_DISABLE_PROMPT
     ```
5. Reload `~/.zshrc`:
   ```bash
   source ~/.zshrc
	 ```

