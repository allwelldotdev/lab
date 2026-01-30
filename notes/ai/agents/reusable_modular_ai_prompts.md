These prompts work well with the following LLMs and Agentic IDEs:
- Antigravity (Agentic IDE)

### For reading and/or updating docs

> To ensure you have the right, comprehensive, and up-to-date knowledge of the codebase and instructions, read the docs in *[placeholder: e.g. `./.agent` directory]*. After reading, test yourself to confirm to yourself that you have read and do understand all the relevant, up-to-date information on the code. Then, we may proceed.

### For short, straight-to-the-point, responses

Great for lower token consumption.

> Don't respond verbose.

> Respond with relevant detail without being verbose.

### For testing and validation

For Antigravity `browser_subagent` website testing:

> After making changes, open the browser and test till success (task requirements are met).

### For git commit or MR messages

Gemini 3 Flash works well here.
> See diff between index files and last commit and suggest a git commit title and message, then MR request title and message. Don't respond verbose.