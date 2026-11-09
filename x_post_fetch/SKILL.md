# Read X Posts and Threads

Public pilot adaptation: personal and environment-specific details removed; substance unchanged.

## Choose the route

- Reach for the `x_thread` helper first whenever the user shares an X or Twitter status URL, or asks what a post says, what a thread says, or whether the full text was retrieved. It needs no browser, no login and no API key.

- Run it from the shell. Give it the URL exactly as the user shared it, because it also accepts mirror and oembed URLs.

```text
python3 <path-to-your-x_thread-helper>/x_thread.py "<url>"
```

- Use `--mode post` only when the user wants the single post and speed matters more than completeness. Say that long-form text may be truncated in that mode.

- Fall back to your browser tool or web search tool only when the helper reports the post unavailable, or when the user needs something this route cannot give, such as media viewing or a logged-in view. Name which route actually produced the text when you report back.

## Read the result

- Treat the digest as the post content. The header line gives the author, the post count, the root ID and the start ID, so check that the start ID matches the URL the user shared before summarizing.

- Keep the sections apart when you summarize. The numbered body is the author's own thread, quoted posts are other people's words quoted by the thread, the start path is other authors between the shared post and the root, and replies by others are reactions rather than part of the author's claim.

- Present an article post's body as the article. Its heading lines and any fenced code block are part of the article, so do not drop a code block when the user asked for the full text.

- Watch the completeness signals. A thread count of 1 for a post the user described as a thread, a note that stayed short, or replies that look sparse can mean the public page exposed only part of the conversation. Say what was actually retrieved, including the reply count, instead of claiming the whole conversation.

## Handle failures

- On exit code 3, check the ID once against the user's URL for a copy mistake. Then report the post as unavailable through this route and offer the browser fallback.

- Never retry the helper in a loop and never widen it into bulk collection. One shared link is one build.

## Available connectors

- [X Thread helper documentation](<path-to-your-x_thread-helper-documentation>) for the helper's commands, output format, error codes and limits.

- Use the connectors and skills available in your environment to find related tools.

- If nothing here fits, look on the web for a maintained API or MCP server and set it up with your environment's connector setup guide, or build a custom connector.
