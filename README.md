# LocalTiddlyServer
Local Tiddlywiki server on Go-Rye with implementation of Tiddlywiki principles

1. Project for studying and experimenting with Go, Rye, ASON format and Tiddlywiki.
2. The first phase of the [PARSEQ](https://github.com/Serj-Aleks/PARSEQ) project
3. An application, by clicking on the icon on your desktop, that launches a local host with a TW5 interface with a special core or plugin for organizing tiddlywiki files with a search for all tiddlers in them.

---

**LPTWS** - local personal server for organizing Tiddlywiki files and searching through all tiddlers in them, as well as with the maximum implementation of all Tiddlywiki principles in the project. A project for studying and experimenting with technologies - Go, Rye, ASON and Tiddlywiki.

**Specification**

1. Local server means that all our Tiddlywiki files are stored in certain controlled places of the OS file system on the computer. Optimally, in one folder - TiddlyWikiFiles. Since I use ChromeOS Flex and the LPTWS project folder should be stored in the "Linux Files" folder, it is natural that "Linux Files" -> LPTWS -> TiddlyWikiFiles . This is the first aspect of the project configuration.
2. To search through all tiddlers, it is also natural to select all tiddlers that will be searched. "All tiddlers" means "tiddlers of the user or author of the TiddlyWiki file", respectively without shadow or system tiddlers, plugins and special types such as images and so on. Again, it would be possible to put all such selected tiddlers in one folder, but then an additional tag would have to be entered for each tiddler to demonstrate its belonging to a certain TiddlyWiki file. An alternative option is folders for each wiki file, i.e. - tiddlers -> TWF ...
3. Search line of a separate tiddler type with tiddler transclusion so that some links to TiddlyWiki files can be taken out like the default browser interface. The search line is also a Rye console. Links to the taken out files in the search line are presented as ShortCut or file favicons with a signature (title and subtitle). On the right side of the default server file page, there is a list of all TiddlyWiki files (file names as links that open these files in a separate browser window). Naturally, the <div> of the tiddler with the search line should be expanded to the full width of the browser window and collapsed by the corresponding "button".
4. Tiddlers are stored in ASON format. Their structure, accordingly, is only that which is necessary for searching and indexing - title, subtitle, tags, text, transclusions and custom fields.
5. TiddlyWiki file parsing, searching and indexing are performed using Rye. Add functionality for converting blogs to tiddlywiki file - wordpress, blogger, lifejournal and telegram.
6. Naturally, it should be possible to delete TiddlyWiki files and download them by URL, including empty TiddlyWiki files with the latest version on tiddlywiki.com
7. The server as an application should be like a browser extension, when the local host with the server application is automatically launched by the icon. It should be possible to automatically upgrade the core of the TiddlyWiki file to the latest version by clicking the appropriate button (without manually dragging it to https://tiddlywiki.com/upgrade.html and then saving it).

Bots will do the coding. I don't know how useful it will be to point to projects as prototypes:

* https://github.com/tiddlyhost/tiddlyhost-com
* https://github.com/cs8425/tiddlywikid
* https://github.com/K4rian/twserver-go
* https://github.com/rsc/tiddly

There is a suspicion that this may be confusing.

I will use ten bots and describe the results for each one separately in retrospect, but before posting the code as in the blog, I will describe the interaction process for each bot separately in the [wiki](https://github.com/Serj-Aleks/LocalTiddlyServer/wiki/LocalTiddlyServer-%D1%81-%D1%87%D0%B5%D1%82%D0%B2%D0%B5%D1%80%D0%BA%D0%BE%D0%B9-%D0%B1%D0%BE%D1%82%D0%BE%D0%B2)
. The wiki is in Russian, because it is easier for me, so that the process goes faster. If suddenly someone is really interested, it is not a problem to translate now.

In the process of compiling a single promt for all bots and experimenting with them, the idea was born to formulate a promt using bots. Below is the promt that I fed to everyone. Uniform and with an indication of the project configuration. Only Mistral balked, the main ones all gave something without questions. The results of each bot can be seen in the wiki. And below is the promt itself.

---

Your Role: You are an elite system architect and full-stack developer specializing in Go, Ryelang, and building high-performance native web applications. Your task is to create and deliver a complete, production-ready project from scratch based on the following comprehensive technical specifications. You must generate all the code, file structure, installation scripts, and documentation in one go, without asking any follow-up questions.

End Goal: Provide a non-programmer with a complete TiddlyWiki Hub application that installs with a single script and is immediately ready to use. It should be a powerful yet intuitive tool for managing a personal knowledge base.

Architecture and Tech Stack

Backend: Pure Go (Golang). Libraries used: only the standard library, gorilla/websocket for web sockets, and chromedp for headless interaction with the browser (updating the kernel, creating screenshots).

Scripts and data logic: Ryelang.

Frontend: Pure HTML5, CSS3, and JavaScript (ES6+). No external frameworks (React, Vue, Node.js, etc.) allowed.

Data format: TiddlyWiki .html for storage, .ason for search index.

Go <-> Ryelang interaction: Go calls Ryelang scripts as external processes (exec.Command). Ryelang returns JSON results for data exchange.

Backend <-> Frontend interaction: REST API for data and one WebSocket connection for lifecycle tracking.

Project Structure (Must follow)

```
/TiddlyLauncher/
|-- tiddlyhub # Compiled Go binary
|-- main.go # Server source code in Go
|-- go.mod # Go dependencies file
|-- /frontend/
| |-- index.html
| |-- style.css
| |-- app.js
|-- /rye_scripts/
| |-- indexer.rye # Indexes a single TiddlyWiki HTML -> ASON
| |-- search.rye # Searches all ASON files
| |-- query.rye # Executes direct Ryelang commands
|-- /wikis/ # Folder for storing .html and .ason files
| |-- MyNotes.html
| |-- MyNotes.ason
|-- /data/
| |-- metadata.json # Metadata for files (tags, contexts)
| |-- cache/
| | |-- favicons/ # Cached favicons
| | |-- screenshots/ # Cached preview screenshots
|-- install.sh # Installation script for Linux/macOS/ChromeOS
|-- install.ps1 # Installation script for Windows
|-- README.md # Full documentation
```

Detailed functional requirements
Application lifecycle:

Startup: The application is started as a single local process via a shortcut. The go server starts on localhost:8080 and immediately opens this page in the browser.
Automatic termination: Implement a reliable shutdown mechanism:

Primary: Establish a WebSocket connection (/ws). When it is broken (tab is closed), the server waits 2 seconds and exits (os.Exit(0)).

Backup: JS logic window.onunload sends navigator.sendBeacon('/api/exit', '') to notify the server about closing.

Web Interface (Two-Column Layout):

Layout: The interface should be two-column.

- Left column (Main area): This is the "story " where open tiddlers from the selected TiddlyWiki file are displayed.
- Right Column (Menu/Sidebar): This is the control panel. It contains:

- A list of all available TiddlyWiki files.
- A search bar/Ryelang console.
- A URL import bar.
- A search results area.

Tiddler interactivity: Implement an "expand" mechanism. Each tiddler in the left column should have a button icon (e.g. '⤢') in its header. When clicked, the div of that tiddler should smoothly expand to 100% of the window width, visually covering the right column. Clicking it again returns the tiddler to its original width, showing the right column again.

Search bar/Ryelang console: A single input line in the right column. Plain text starts a search. Text with the rye> prefix is executed as a Ryelang command.

Search results: Search does more than just filter files. It displays a list of found results in the right column, under the search bar. Each result should contain:

- The name of the source file.
- The title of the found tiddler.

Snippet: A fragment of text with the found word highlighted.

Backend API and Logic in Go:

GET /api/wikis: Returns a JSON array with deep meta information about each file:

- filename: File name.
- title, subtitle: from the <title> tag.
- core_version: Core version extracted from the tiddler $:/core.
- tiddler_count: Total number of tiddlers.
- top_tags: An array of the 5 most popular tags inside the file.
- favicon_path: Local path to the cached favicon.

GET /api/search?q={query}: Accepts the query. Calls search.rye. Should be able to handle complex searches (e.g. in:work tag:project term). Returns JSON with structured results for displaying snippets.

POST /api/upgrade/{filename}: Key function. Uses chromedp to automate the upgrade process. Logic: download a new empty.html, run it in a headless browser, programmatically import all tiddlers from the user's old file into it, and save the result.

POST /api/import: Accepts a URL for import.

GET /api/screenshot/{filename}: (Stub function) Uses chromedp to create a screenshot preview of the html file and saves it to data/cache/screenshots/.

POST /api/publish/{filename}: (Stub function) Placeholder for future integration with Tiddlyhost API.

Ryelang logic:

- indexer.rye: Accepts a path to an .html file. Parses it, extracts all tiddlers, their tags, determines the kernel version and saves all this information to the corresponding .ason file.
- search.rye: Accepts a search query (including in:, tag: tags). Searches all .ason files in the directory, forms JSON with results ready for generating snippets.

Installer and documentation requirements

Installation scripts (install.sh and install.ps1):

- Do not require root/administrator rights.

Automate everything:

- Check for Go and Ryelang. If they are not there, notify the user.
- Create a ~/TiddlyLauncher directory.
- Copy all project files.
- Compile Go code (go build).
- Create a launcher shortcut (.desktop for Linux/ChromeOS Flex).
- Adaptation to x86/ARM architecture must be taken into account (e.g. via compilation flags).

Documentation (README.md):

- For the user: Step-by-step installation instructions, detailed description of each interface function, examples of complex search queries and Ryelang commands.
- For the developer: Brief description of the architecture and all API endpoints.

Final requirement: Give the answer as a single block containing all the above files with their full contents. Use Markdown to separate the files. Don't ask questions, generate the entire project in its most complete and advanced version.

---
Practice immediately showed that there is little fundamental difference between my purely promt and the promt made with bots, to understand that "turnkey" is promised by everyone, but only Google and "professionals" like the Singaporean and Claude do it. Specifying the project configuration is useful, but for such a banal conclusion, which I made with the first promt, it is not important. That is, at the stage of these two promts, thirteen systems participated in the experiment - [GAIStudio](https://github.com/Serj-Aleks/LocalTiddlyServer/wiki/GAIStudio), [[Gemini]], [[Manus]], [[Claude]], Copilot, MetaAI, Grok, Mistral, ChatGPT, Perlexity, Kimi, DeepSeec and Qwen. Only the first four can be taken into real development and continuation of the experiment. The rest are lying that they can and at most they can be used as a backup for small hints. I will put Mistral above ChatGPT in this list because the Frenchman does not lie, and ChatGPT is above the others because it maintains context in all chats.
