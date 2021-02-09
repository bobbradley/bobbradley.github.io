# Software Guidelines

[Allman Brace Style]:		https://en.wikipedia.org/wiki/Indent_style#Allman_style
[Broken Windows Theory]:	https://en.wikipedia.org/wiki/Broken_windows_theory

The following are guidelines for software development in C, C++, Objective-C, and Swift. Note that these are just guidelines and not absolute rules. If you feel strongly against a particular guideline, email me about it. It may be that the guideline is wrong or stale and needs to be changed. It may be a special case where an exception is warranted. Or we may just disagree about the guideline.

[//]: # (===================================================================================================================)
## 100 Style ##

A good coding style and its consistent use can make a big difference in the quality of a codebase. It helps avoid the [Broken Windows Theory]. When people see that a codebase is meticulously maintained, they tend to be a little more careful when they make changes. It can also help avoid bugs, such as lack of a curly brace causing incorrect code execution.

[//]: # (-------------------------------------------------------------------------------------------------------------------)
### 100.1 Maintain existing style

Maintain the style of the file even if it conflicts with these guidelines. If the code uses kernel-style bracing then don't add code using Allman-style bracing. It's better to keep the file consistent. Don't reformat the entire file without a good reason either. Sometimes an external file is imported into the repository with the desire to minimize differences. This allows syncing with an external repository more easily.

However, if you're making major changes to the file and the existing style is difficult to maintain, talk to the existing owner to see if it's okay to reformat the entire file. If you are becoming the new owner of the file then it's up to you whether to reformat, but prefer to follow the style guidelines of this document rather than introduce your own.

[//]: # (-------------------------------------------------------------------------------------------------------------------)
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

[//]: # (-------------------------------------------------------------------------------------------------------------------)
### 100.2 Use Allman brace style

The [Allman Brace Style] puts the brace associated with a control statement on the next line, indented to the same level as the control statement. Statements within the braces are indented to the next level. Apply these rules in all cases where curly braces are used: classes, control flow, enums, functions, structs, etc. This style also extends to other brace-like constructs, such as open array, dictionary, and string literals.

Brace style is mostly a personal preference. People often feel strongly about it. Here's the reasoning for using Allman:

-	Consistency of having the opening and closing brace at the same level.
-	Symmetry of beginning and ending a block with a brace vs beginning with a keyword and ending with a brace.
-	Allows selecting an entire block to move it from an if to and else if, etc. without having to manually select or edit 
	the opening brace if it's at the end of the line.
-	Inconsistency of kernel style putting the opening brace for a function on the next line, but other braces on the right 
	edge.
-	Adds a little vertical whitespace between the control statement and body to make it easier to read.
-	Whitesmiths style can be confusing by indenting the braces.

Prefer to use braces around the body of all control statements. Exceptions can be made for cases when the condition and body fit on a single line and are trivially related, such as calling a function/block if non-null.

**Brace style example**

```
enum
{
	MyEnum1 = 1, 
	MyEnum2 = 2, 
};

void MyFunction( void )
{
	if( someCondition )
	{
		printf( "Test 1\n" );
	}
	else
	{
		printf( "Test 2\n" );
	};
}

NSDictionary *dict = 
@{
	@"key" : @"value", 
	@"key2" : 
	@[
		@"value2"
	]
};
```

[//]: # (-------------------------------------------------------------------------------------------------------------------)
### 100.3 Use tabs instead of spaces

Use tabs instead of spaces and a tab width of 4. Tabs are supported by all editors and provide the same behavior. Some editors support emulating tabs with spaces, but it's not universal or complete and often results in a confusing mix of spaces and tabs. Using spaces also makes it difficult to copy and paste snippets correctly due to the selection precision required, which results in small spacing mistakes throughout the codebase (e.g. 3 spaces here, 5 spaces there). Editors differ in their default tab width, but it's easier to change that via a preference than finding and fixing the spacing errors introduced when using spaces.

Tabs are conceptually the correct character for indenting to the next column (and is why tabs were invented in the first place). The only issue is that people disagree on the width of a column. So fix the problem and specify the width of a tab instead of trying to treat the symptom by emulating tabs with spaces. These guidelines specify a tab width of 4 as a balance between making a clear visual indicator of indentation vs being so wide that it requires excessive wrapping.

EditorConfig <http://EditorConfig.org> is a standard for specifying the editor configuration for a project. The following should be put into a file named ".editorconfig" at the root of the repository for the project:

```
# EditorConfig: http://EditorConfig.org

# Top-most EditorConfig file.
root = true

# Default for all files.
[*]
charset = utf-8
indent_size = 4
indent_style = tab
```

[//]: # (-------------------------------------------------------------------------------------------------------------------)
### 100.4 C-style language Whitespace guidelines

Most of these are personal preference. For parenthesis, touching the keyword/function name more tightly associates it visually. For spaces before/after parenthesis, it makes it easier to read things like the condition of an if statement. It also causes the body of the if statement to line up horizontally with the condition.

#### No space before open parenthesis. Space after opening parenthesis and before closing parenthesis.
```
if( x )
```
Note: This rule also apply to other control structures: while, do, for, else if, return, sizeof, function calls, etc.

#### No space before parenthesis for function calls.
```
printf( "a" );
```

#### No space after return and always use parenthesis.
```
return( x + 1 );
```
People sometimes say "return isn't a function, don't treat it like one", but parenthesis isn't what marks something as a function or not. switch, if, while, sizeof, etc. aren't functions and those use parenthesis. Every other C keyword that takes an expression uses parenthesis so this guideline is consistent with those by using parenthesis.

#### No space after sizeof and always use parenthesis.
```
sizeof( x );
```

#### do/while loops: blank line before ending curly brace. Single tab before "while".
```
do
{
	DoWork();

}	while( KeepGoing() );
```
This keeps gives some vertical separation between the body and its condition and horizontally aligns them to improve readability.

#### Normal for loops: use a single space after each semicolon.
```
for( i = 0; i < n; ++i )
```

#### Infinite for loops: don't use spaces between semicolons.
```
for( ;; )
```

#### Empty loops: use "{}" for the body on the same line so it's clear that it's intentional.
```
while( Work() ) {}
```
This is perferred over a semicolon because those can trigger warnings are can look like a mistake.

#### switch statements
- Indent case line 1 level from the switch line.
- Indent case body by 1 level from the case line.
- Separate case statements with a blank line and indent blank lines to indent level of cases.
- Switch statement case bodies that define variables
	- Opening brace on line following case line.
	- Align opening brace to beginning of case keyword.
	- Put the closing brace on the line after the break statement.
	- Align closing brace the same as the opening brace.

```
switch( x )
{
	case 1:
		printf( "First\n" );
		break;
	
	case 2:
	{
		int		x;
		
		x = CalculateValue();
		printf( "Second %d\n", x );
		break;
	}
	
	default:
		printf( "Other\n" );
		break;
}
```

#### Variable types with multiple words: use a single space between each word.
```
unsigned long long x;
```

#### Pointer variable types: use a single space between the base type and `*`.
```
void *ptr;
```

#### Pointer variable types with multiple `*`: no space between `*`.
```
void ***ptr;
```

#### Multiple variables: align variable names to 2 tabs from widest type.
```
unsigned long long		x;
void *					y;
void ***				z;
```

#### Casts: no space between parenthesis. Space after the parenthesis.
```
x = (int) y;
```

#### structs
- Blank line before the closing brace for visual separation between the fields and the name.
- Single tab after the brace so the name lines up with the body.

```
typedef struct
{
	uint8_t		a;
	uint8_t		b;
	
}	MyType;
```

#### Objective-C methods: no space after open brackets and no space before close brackets.
```
[obj doit];
```

#### Objective-C method parameters: no space after colon (except for lining up to previous line).
```
[obj setValue:x];
```

#### Objective-C auto-boxing: no space between parenthesis.
```
NSNumber *num = @(123);
```

#### Files end with a newline, but no blank lines.
Lack of a newline can trigger warnings (and looks wrong). Blank lines are unnecessary.

#### Objective-C and C++ access control specifiers (@public/public, @private/private, etc.).
- Indent one level (same as instance variables and methods).
- Blank line between the lines below them.
- Lines below should line up horizontally with the access control specifier.
- Order them public then protected then private.

```
@interface MyClass
{
	@public
	
	int		_someInt;
	
	@private
	
	int		_somePrivateInt;
}
@end
```

#### C++ namespaces: don't indent their contents, add trailing comment if more than 1/2 page.
```
namespace MyNamespace
{

class MyClass
{
	...
};

}

namespace MyNamespace
{

... more than 1/2 page of code.

} // MyNamespace
```
These often wrap large chunks of code so indenting would force all the code to be shifted, which seems excessive.

#### Whitespace example showing nearly every situation
```
struct
{
	int		a;
	long	b;
	
}	MyTypedefExample;

OSStatus MyFunction( int inParam1, int inParam2 )
{
	OSStatus		err;
	int				i, x;
	void *			ptr;
	void **			ptrPtr;
	
	(void) inParam2;
	
	require_action( ( inParam >= 0 ) && ( inParam <= 5 ), exit, err = kRangeErr );
	
	if( inParam1 != 0 )
	{
		printf( "%ld, %@\n", (long) a, @(123) );
	}
	else if( inParam > 2 )
	{
		
	}
	
	for( i = 0; i < 5; ++i )
	{
		printf( "Test %d\n", i );
	}
	
	do
	{
		DoWork( &x );
		
	}	while( x > 0 );
	
	switch( x )
	{
		case 1:
			break;
		
		case 2:
		{
			int y = DoOtherWork();
			printf( "%d\n", y );
			break;
		}
		
		default:
			break;
	}
	
	ptr = malloc( 123 );
	require_action( ptr, exit, err = kNoMemoryErr );
	
	ptrPtr = &ptr;
	DoPtrWork( ptrPtr );
	free( ptr );
	
	[SomeObjectiveCObject doWork];
	[SomeObjectiveCObject doOtherWork:y];
	err = kNoErr;
	
exit:
	return( err );
}

@interface MyClass
{
	@public
	
	int		_someInt;
	
	@private
	
	int		_somePrivateInt;
}
@end

namespace MyNamespace
{
	class MyCPlusPlusClass
	{
		public:
		
		int		_someInt;
		
		private:
		
		int		_somePrivateInt;
	};
}
```

[//]: # (===================================================================================================================)
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
