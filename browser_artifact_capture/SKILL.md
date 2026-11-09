# Browser Artifact Capture

Public pilot adaptation: personal and environment-specific details removed; substance unchanged.

## Save the file before you leave the page

- Download the generated or returned file to a named local path as soon as it appears, and do not close or reload the page before the download finishes.

- Keep the generating page open until the saved file is checked, because a preview link can disappear when the page reloads or its history clears.

- Save a screenshot only as a labelled preview fallback when the original file cannot be downloaded, and never present that screenshot as the original file.

## Check the saved file before you report it

- Open the saved file from disk and confirm that it exists, has a nonzero size, and opens in the expected format before you report delivery.

- Report a preview, a thumbnail, or a cropped screenshot as a preview only, and state clearly that the original file is still unobtained when that is the case.

- Count the verified files against the requested count, and report completed, pending, failed, and not started results separately when any file is missing.

- Check a video by playing the saved file and confirming that it moves beyond the first frame, and check its duration or frame count when the task depends on motion or timing.

## Handle a failed upload or a misleading preview

- Try a browser file upload dialog at most twice, and stop trying when the dialog fails to open in the same way on the second attempt.

- Ask the user to upload the file directly when the dialog keeps failing, and do not claim that an external service saw the file until the upload is confirmed.

- Treat a GIF shown in a chat or preview pane as a first frame only unless the saved file itself plays as animation, and request the video file itself for any evaluation that depends on motion.

- Treat a repeated upload or download failure as a browser limit for that session, not as proof that the file was sent, received, or evaluated.

## Available connectors

- Use the connectors and skills available in your environment to find related tools.

- If nothing here fits, look on the web for a maintained API or MCP server and set it up, or build a custom connector.

## Alternatives for better results

- Ask the source to provide the original file in a durable format when only a preview link is offered, and save that original before any evaluation begins.

- Ask for a video file rather than an animated preview when the evaluation depends on movement, camera work, or timing.

- Search for the archive method when a verified file needs long term saving with its source and verification record kept separate.
