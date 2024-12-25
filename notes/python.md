Learning from Dr Fred Baptiste's Python Fundamentals course on [Udemy](https://www.udemy.com/course/python3-fundamentals/).

- Created by **Guido van Rossum** in 1989.
- Was named after the British comedy group **Monty Python**.
- Still actively developed today
	- Python 2.0 released in 2000 → last release was 2.7 (end of April 2020).
- The reference implementation of python is **CPython** otherwise known as **Canonical Python**. This is the standard, official python.

## Virtual Environments
Virtual Environments `venv` in Python are used to make a copy of Python installation to enable you switch between Python versions and Python dependency versions or differences for your Python project.

Within Python virtual environments (or venv) are scripts to "activate" / "deactivate" the environment.

### Creating virtual environments
```bash
<python version/path> -m venv <your_env_name>
```

For example, if using python3.9 and you'd like to name your virtual environment 'py39_course':
```bash
python3.9 -m venv py39_course
```

> Where do you store the `venv` file? I usually store mine in the same directory as the project I'm working on to keep things simple and less complicated.

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

> Switch to another venv when a venv is activated by running:

```bash
source <path_to_venv>/bin/activate
```
## Python package installer `pip`
`pip` lets you install python packages to your `venv`.
- activate the virtual environment first (sets your PATH)
- `pip install package_name`
	- install versions
	- `pip install package_name==1.3.2`
	- `pip install package_name<=1.2`
	- `pip install package_name>2.0`
- use `requirements.txt` to keep track of required packages and versions.
	- `pip install -r requirements.txt` to install packages from `requirements.txt`

## Running python
### Python REPL (read-eval-print-loop).
The Python REPL is the interactive mode of reading, compiling, running your code. This is also known as **interactive mode**.
![python repl](assets/Pasted%20image%2020241225112258.png)
### Python Script mode.
This is different from Python REPL mode. Here, in Script mode, you write all your code first then call Python and it runs your code and outputs it to the system.
![python script mode](assets/Pasted%20image%2020241225112435.png)
### Jupyter Notebooks
Jupyter Notebooks is software that has been developed on top of Python that wraps the REPL into a nicer interface. This is not the only one software available that does this. There are others are well.  
Jupyter Notebooks is a REPL, but browser-based.

![jupyter notebook](assets/Pasted%20image%2020241225115903.png)

The files you create and save into your repo locally from Jupyter Notebooks are called '**a notebook**' and they usually use the `.ipynb` extension.

> Jupyter Notebooks is also known as **interactive python**.

#### Jupyter Notebook commands
- `CTRL + Enter` runs code in selected cell
- `ESC` exists edit mode to view mode
- `Enter` to edit a cell
- In view mode, `dd` deletes the selected cell
- `y` transforms a cell to code
- `m` transforms a cell to markdown
- `r` transforms a cell to raw.
## Python basics
### Integers
- The `int` type
	- used to represent literal numbers: 0, 1, 100, -100, etc.
	- integers can be of any magnitude (as long as you have memory!)
	- integer numbers can be created from a literal in the python code
		- 100
		- -100
		- 10_500_000 (note how you can use underscores for readability)
	- or, as the result of some calculation (expression)
		- 1 + 1
#### Integer representations
- computers only know two numbers
	- 0 and 1 → binary system, aka base 2
- any number in a computer is represented using powers of 2
### Floats
- the `float` type
- used to represent real numbers (floating point): 3.14, -1.3
- can use python literals to define a `float`
	- 3.14
	- -1.3
	- 1_234.567_876
- the decimal point differentiates a `float` from an `int` when using literals
	- 1 → `int`
	- 1.0 → `float`
#### Float binary representations
 - `floats` work in the same way as [Integer representations](#Integer%20representations)
 - `floats` are represented by using powers of 2 and fractions of powers of 2

$$\begin{align}
\frac{1}{2^2}+\frac{0}{2^2}+\frac{1}{2^3}&=\frac{1}{2}+\frac{0}{4}+\frac{1}{8} \\
&= 0.5 + 0 + 0.125 = 0.625
\end{align}$$ 
- certain numbers do not have a finite decimal representation $(\frac{1}{3})$
- same happens with binary representations!

$$0.1 = \frac{0}{2} + \underbrace{\underbrace{\frac{0}{4} + \frac{0}{8} + \frac{1}{16} + \frac{1}{32}}_{0.09375} + \frac{0}{64} + \frac{0}{128} + \frac{0}{256} + \frac{0}{512}}_{.099609375} + ...$$



