# java-ls
# What is the Language Server Protocol?
Implementing support for features like autocomplete, goto definition, or documentation on hover for a programming language is a significant effort. Traditionally this work must be repeated for each development tool, as each provides different APIs for implementing the same features.

The idea behind a Language Server is to provide the language-specific smarts inside a server that can communicate with development tooling over a protocol that enables inter-process communication.

The idea behind the Language Server Protocol (LSP) is to standardize the protocol for how tools and servers communicate, so a single Language Server can be re-used in multiple development tools, and tools can support languages with minimal effort.

LSP is a win for both language providers and tooling vendors!

# How it works

A language server runs as a separate process and development tools communicate with the server using the language protocol over JSON-RPC. Below is an example for how a tool and a language server communicate during a routine editing session:



 1)   The user opens a file (referred to as a document) in the tool: The tool notifies the language server that a document is open (‘textDocument/didOpen’). From now on, the truth about the contents of the document is no longer on the file system but kept by the tool in memory. The contents now has to be synchronized between the tool and the language server.

 2)  The user makes edits: The tool notifies the server about the document change (‘textDocument/didChange’) and the language representation of the document is updated by the language server. As this happens, the language server analyses this information and notifies the tool with the detected errors and warnings (‘textDocument/publishDiagnostics’).

 3)   The user executes “Go to Definition” on a symbol of an open document: The tool sends a ‘textDocument/definition’ request with two parameters: (1) the document URI and (2) the text position from where the ‘Go to Definition’ request was initiated to the server. The server responds with the document URI and the position of the symbol’s definition inside the document.

4)    The user closes the document (file): A ‘textDocument/didClose’ notification is sent from the tool informing the language server that the document is now no longer in memory. The current contents is now up to date on the file system.

This example illustrates how the protocol communicates with the language server at the level of document references (URIs) and document positions. These data types are programming language neutral and apply to all programming languages. The data types are not at the level of a programming language domain model which would usually provide abstract syntax trees and compiler symbols (for example, resolved types, namespaces, …). The fact, that the data types are simple and programming language neutral simplifies the protocol significantly. It is much simpler to standardize a text document URI or a cursor position compared with standardizing an abstract syntax tree and compiler symbols across different programming languages.


