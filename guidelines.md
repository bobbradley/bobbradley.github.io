# Software Guidelines

[Broken Windows Theory]: https://en.wikipedia.org/wiki/Broken_windows_theory

The following are guidelines for software development in C, C++, Objective-C, and Swift. Note that these are just guidelines and not absolute rules. If you feel strongly against a particular guideline, email me about it. It may be that the guideline is wrong or stale and needs to be changed. It may be a special case where an exception is warranted. Or we may just disagree about the guideline.

## 100 Style ##

A good coding style and its consistent use can make a big difference in the quality of a codebase. It helps avoid the [Broken Windows Theory]. When people see that a codebase is meticulously maintained, they tend to be a little more careful when they make changes. It can also help avoid bugs, such as lack of a curly brace causing incorrect code execution.

### 100.1 Maintain existing style

Maintain the style of the file even if it conflicts with these guidelines. If the code uses kernel-style bracing then don't add code using Allman-style bracing. It's better to keep the file consistent. Don't reformat the entire file without a good reason either. Sometimes an external file is imported into the repository with the desire to minimize differences. This allows syncing with an external repository more easily.

However, if you're making major changes to the file and the existing style is difficult to maintain, talk to the existing owner to see if it's okay to reformat the entire file. If you are becoming the new owner of the file then it's up to you whether to reformat, but prefer to follow the style guidelines of this document rather than introduce your own.

### 100.1.1 File Format Conventions

-	Text files should use UTF-8. Don't use UTF-16, MacRoman, Windows Latin, or other encodings.
	UTF-8 is supported by all modern editors and allows any Unicode code point to be encoded.
-	Prefer not to include a BOM since it's not needed with UTF-8 and can confuse some parsers.
-	Use LF line endings on non-Windows systems. This is what all standard Mac and Unix editors use.
-	Use CF-LF line endings on Windows systems. Most Windows tools expect this and it's not worth trying to fight them. 
	Note that all popular version control systems (git, svn, etc.) will automatically convert between CF-LF and LF when
	checking in/out. If you're using git, make sure `core.autocrlf` is on with `git config --global core.autocrlf true`.
-	Use LF line endings when storing in the version control system. Most version control systems will do this for you 
	automatically.
-	Don't uses spaces in file or folder names. Spaces require quoting whenever they are used in 
	command line and many build environments, which makes them error prone.
-	Plists should be stored as XML when checked in and converted to binary during the build process (e.g. by Xcode's
	"Convert Copied Files" option). XML allows easier diffing and viewing. Even if tools like git are configured to 
	convert to text on-the-fly, other tools, such as commit email generators may not support it so it's better to use text.
-	Strings files should be stored as UTF-8, not UTF-16, when checked in and converted to binary during the build process.
	UTF-16 has the same problem as binary files for diffing and viewing.

## References

Author: Bob Bradley <bradley@me.com>  
Date: 2020/02/07. Permanent ID of this document: 8fa2dbd66648de01a531e391bba7ce7e.  
Repo: <https://github.com/bobbradley/bobbradley.github.io>

- Document IDs: <http://cr.yp.to/bib/documentid.html>
- Allman Brace Style: <https://en.wikipedia.org/wiki/Indent_style#Allman_style>
- AsciiDoc text-based document generation: <http://asciidoc.org/>
- Broken windows theory: <http://en.wikipedia.org/wiki/Broken_windows_theory>
- Crash-only Software: <https://en.wikipedia.org/wiki/Crash-only_software>
- Overcommit: What is Overcommit?: <http://www.etalabs.net/overcommit.html>
- Principle of Least Surprise?: <https://en.wikipedia.org/wiki/Principle_of_least_astonishment>
