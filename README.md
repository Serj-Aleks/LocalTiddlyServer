# LocalTiddlyServer
Local Tiddlywiki server on Go-Rye with implementation of Tiddlywiki principles

1. Project for studying and experimenting with Go, Rye, ASON format and Tiddlywiki.
2. The first phase of the [PARSEQ](https://github.com/Serj-Aleks/PARSEQ) project
3. An application, by clicking on the icon on your desktop, that launches a local host with a TW5 interface with a special core or plugin for organizing tiddlywiki files with a search for all tiddlers in them.

**LPTWS** - local personal server for organizing Tiddlywiki files and searching through all tiddlers in them, as well as with the maximum implementation of all Tiddlywiki principles in the project. A project for studying and experimenting with technologies - Go, Rye, ASON and Tiddlywiki.

**Спецификация**

1. Local server means that all our Tiddlywiki files are stored in certain controlled places of the OS file system on the computer. Optimally, in one folder - TiddlyWikiFiles. Since I use ChromeOS Flex and the LPTWS project folder should be stored in the "Linux Files" folder, it is natural that "Linux Files" -> LPTWS -> TiddlyWikiFiles . This is the first aspect of the project configuration.
2. To search through all tiddlers, it is also natural to select all tiddlers that will be searched. "All tiddlers" means "tiddlers of the user or author of the TiddlyWiki file", respectively without shadow or system tiddlers, plugins and special types such as images and so on. Again, it would be possible to put all such selected tiddlers in one folder, but then an additional tag would have to be entered for each tiddler to demonstrate its belonging to a certain TiddlyWiki file. An alternative option is folders for each wiki file, i.e. - tiddlers -> TWF ...
3. Search line of a separate tiddler type with tiddler transclusion so that some links to TiddlyWiki files can be taken out like the default browser interface. The search line is also a Rye console. Links to the taken out files in the search line are presented as ShortCut or file favicons with a signature (title and subtitle). On the right side of the default server file page, there is a list of all TiddlyWiki files (file names as links that open these files in a separate browser window). Naturally, the <div> of the tiddler with the search line should be expanded to the full width of the browser window and collapsed by the corresponding "button".
4. Tiddlers are stored in ASON format. Their structure, accordingly, is only that which is necessary for searching and indexing - title, subtitle, tags, text, transclusions and custom fields.
5. TiddlyWiki file parsing, searching and indexing are performed using Rye.
6. Naturally, it should be possible to delete TiddlyWiki files and download them by URL, including empty TiddlyWiki files with the latest version on tiddlywiki.com
7. The server as an application should be like a browser extension, when the local host with the server application is automatically launched by the icon. It should be possible to automatically upgrade the core of the TiddlyWiki file to the latest version by clicking the appropriate button (without manually dragging it to https://tiddlywiki.com/upgrade.html and then saving it).
