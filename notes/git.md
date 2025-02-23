> To read the formatted colors of `git st` (or `git status`) output when piping through the `less` command in WSL (Ubuntu), ensure git outputs color. [Learn more here!](https://www.perplexity.ai/search/say-i-m-reading-git-st-output-pXGHhPZQSYaPBiX4bftRew).

Ensure git outputs color: You need to configure Git to always output color codes, even when the output is piped. You can do this by setting the following configuration:
```bash
git config --global color.ui always
```

Check out this blog for default settings to improve Git: https://blog.gitbutler.com/how-git-core-devs-configure-git/

