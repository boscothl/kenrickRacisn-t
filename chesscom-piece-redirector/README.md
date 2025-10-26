# Chess.com Piece Redirector (Chrome extension)

This small Chrome extension redirects chess.com piece image requests (served from images.chesscomfiles.com) to your own server so you can use custom piece images.

How it works
- The extension uses the Manifest V3 declarativeNetRequest API and a regex-based redirect rule.
- Any request matching:
  - https://images.chesscomfiles.com/chess-themes/pieces/.../<filename>.png
  - will be redirected to: http://swhl.ddns.net:8080/<filename>.png

Files
- `manifest.json` — extension manifest (MV3) with declarative rule resource.
- `rules.json` — declarativeNetRequest rules (redirect rule).

Important notes
- Mixed content: chess.com is served over HTTPS. Redirecting image requests from HTTPS to plain HTTP may be blocked by modern browsers as mixed (insecure) content. To avoid this, host your images over HTTPS (for example: https://swhl.ddns.net:8080/ or use a valid TLS-enabled domain/port) and update `rules.json` substitution accordingly.
- The rule preserves the original filename (e.g., `bq.png`, `wq.png`) and redirects only image resource types.

How to install (Chrome / Edge)
1. Open Chrome and go to chrome://extensions.
2. Enable "Developer mode" (top-right).
3. Click "Load unpacked" and select the `chesscom-piece-redirector` folder.
4. The extension should appear in the list and be enabled.

Testing
1. With the extension enabled, open a new tab and paste a known chess.com piece URL, e.g.:
   https://images.chesscomfiles.com/chess-themes/pieces/glass/150/bq.png
2. With the extension active you should be redirected to:
   http://swhl.ddns.net:8080/bq.png
3. If the browser blocks the request (mixed content), serve your images over HTTPS and update `rules.json` accordingly.

Customizing
- To change the server domain or path, edit the `regexSubstitution` in `rules.json`.
- To broaden or narrow the regex match, edit the `regexFilter` in `rules.json`.

Alternative approaches
- If declarativeNetRequest doesn't cover your use case (e.g., dynamic complex logic), you can implement a background service worker using webRequest (note: Chrome MV3 restricts blocking webRequest; this approach works better in Firefox). Another alternative is to host images under the same domain via a proxy/reverse-proxy or ensure HTTPS on your image host.

Security & privacy
- This extension redirects only specific image requests. Only enable it when you trust the target server. Serving images from a third-party server means the server owner can observe requests.

If you'd like, I can:
- Add a second ruleset to rewrite to HTTPS automatically.
- Provide a fallback background script for Firefox or environments that allow blocking webRequest.
