## LaTeX + Obsidian
To write mathematical numbers or text representations in Obsidian, you can utilize LaTeX syntax, which allows for the formatting of equations and symbols effectively. Here are the steps and tips for doing so:
### Writing Mathematical Equations

1. **Using Dollar Signs**:
	- To include **inline** mathematical expressions, enclose your LaTeX code within single dollar signs:
	  ```text
	  $your_latex_code$
		```        
    - For **block-level** equations, use double dollar signs:
      ```text
      $$your_latex_code$$
		```
2. **Common LaTeX commands**:
	- Fractions: `\frac{numerator}{denominator}` (e.g., $\frac{1}{2}$)
	- Exponents: `x^2` for $x^2$ and `x^3` for $x^3$
	- Square roots: `\sqrt{x}` for $\sqrt{x}$​
	- Summation: `\sum_{i=1}^{n} i` for $\sum_{i=1}^{n} i$
	- Integrals: `\int_{a}^{b} f(x) \, dx` for $\int_{a}^{b} f(x) \, dx$

Learn more:
- [link 1](https://www.perplexity.ai/search/in-obsidian-how-do-i-write-mat-Qnz48iPxRDeJrK_CUrZfbw)
- [link 2](https://www.perplexity.ai/search/how-can-i-write-fractions-in-l-_7xXMTPJTouhy33Fzl8rDg)
### Line breaks and multi-line environments
>To effectively use line breaks, you need to wrap your equations in an appropriate environment like `align`, `aligned`, or `multline`. For example:

```tex
$$
\begin{align}
1 &= 1 \\
2 &= 2
\end{align}
$$
```
[Learn more](https://www.perplexity.ai/search/in-obsidian-how-do-i-write-mat-Qnz48iPxRDeJrK_CUrZfbw) (scroll to the 4th prompt).

### Underbrace
Using the `\underbrace{}_{}` tag, this allows you to create horizontal annotations. [See this example](python/python.md#^latex-underbrace) to learn more.
```tex
$$
\underbrace{<content>}_{<annotated-tag>}
$$
```

### Encapsulate fractions in brackets
```tex
\left(\dfrac{numerator}{denominator}\right)
```
[Learn more](https://www.perplexity.ai/search/how-do-i-specify-brackets-or-p-1efrdbUsQ5aWFOWs6MUgZA)

---
