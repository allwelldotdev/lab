These prompts work well with the following LLMs and Agentic IDEs:
- Antigravity (Agentic IDE)

### For reading and/or updating docs

> To ensure you have the right, comprehensive, and up-to-date knowledge of the codebase and instructions, read the docs in *[placeholder: e.g. `./.agent` directory]*. After reading, test yourself to confirm to yourself that you have read and do understand all the relevant, up-to-date information on the code. Then, we may proceed.

> Start by reading the docs in *[placeholder: e.g. `./.agent` directory]* to get yourself abreast of all functionality and metadata on the codebase. Then accurately perform the following task: 
> ...

 > Make sure you get it right. If you're unsure of steps to take reread the docs for direction, or ask questions before you proceed. If you fail stupendously, you WILL get fired from your job.
### For short, straight-to-the-point, responses

Great for lower token consumption.

> Don't respond verbose.

> Respond with relevant detail without being verbose.

### For testing and validation

For Antigravity `browser_subagent` website testing:

> After making changes, open the browser and test till success (task requirements are met).

### For git commit or MR messages

Gemini 3 Flash works well here.
> Check diff between current working directory (including untracked files, if any) and the last commit and suggest a git commit title and message, then MR request title and message. Present the result in a markdown code block. Do not be verbose with the titles and messages - comprehensive simplicity is what matters. Do not add any links.

> Check diff between the current working directory files (including untracked files, if any) and the last commit and suggest a git commit title and message. Do not add any links. Do not be verbose in your commit title or message responses. Present the result in a markdown code block.

> Check git diff from last commit to this commit `8b2de56` and suggest a comprehensive MR request title and message. Present the result in a markdown code block. Do not add any links. Do not be verbose with the titles and messages - comprehensive simplicity is what matters.