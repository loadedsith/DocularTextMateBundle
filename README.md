DocularTextMateBundle
=====================

Textmate 2 bundle for Docular is very much a work in progress: Feel free to create pull requests for new features.

#Install

    cd /Applications/TextMate.app/Contents/SharedSupport/Bundles (Textmate 1.5.10 & 2)
    git clone git://github.com/loadedsith/DocularTextMateBundle Docular.tmbundle

#Use

`/**` + [TAB] 

Command + Option + Control + [\\] 

You can also command + control + [T] to 'Select Bundle Item'. You'd be looking for a command (bundle item) called "Block Comment Documentation" so you could probably type `comdoc` or something similar, then press enter.

#Result

I've done my best to implement the doc type systems with their properties.

Simply invoking the snippet gives you the following, but there be more magic here than that!
  
	  /**
	   * @doc function
	   * @name <myModule>.moduleSection:<myFuction>
	   * 
	   * 
	   * @returns <type> <description>
	   * @param <type> <description>
	   * 
	   * @description <description>
	   *
	   */

Perhaps the best way to understand what yo can get out of the snippet would be to dissect it:


	/**
	 * @${1|doc,ngdoc|} ${2|function,module,overview,interface,object,directive,filter,service,method|}
	 * ${3:@name ${4:<myModule>}.moduleSection:${5:<myFuction>}}${3/$|(.+)/(?1::@id <id>)/}
	 * 
	 * ${7:${2/function|module|method$|(.+)/(?1::@returns <type> <description>\n * @param <type> <description>)/}${2/interface|object$|(.+)/(?1::@property <type> <description>)/}${2/directive$|(.+)/(?1::@element <element>\n * @example <multiline>)/}}
	 * 
	 * @description ${6:<description>}
	 */



    ${#|[args]}

This is a tab stop, # is its tab number. If `[args]` is a string, that string will be default text. If `[args]` looks like this: `|x,y|` TextMate will offer a dropdown with `x` and `y` as options.

	${1|doc,ngdoc|}:

The first tab stop, offers a selection box of `doc` or `ngdoc`.

	${2|function,module,overview,interface,object,directive,filter,service,method|}

The second tab, offers a selection of both `doc` and `ngdoc` types.

	${3:@name ${4:<myModule>}.moduleSection:${5:<myFuction>}}${3/$|(.+)/(?1::@id <id>)/}

The third selects the @name row

	${4:<myModule>}
	
The fourth affords a module name insertion

	${5:<myFuction>}
	
The fifth affords a function name insertion

	${3/$|(.+)/(?1::@id <id>)/}

This is not a tab stop, instead its a replacement function which says: When the third tab stop (the `@name` row), is empty, replace it with `@id <id>`. Ideally this would offer tab stops too, but TextMate doesn't seem to support this level of snippet recursion.
	
	${7:${2/function|module|method$|(.+)/(?1::\n * @returns <type> <description>\n * @param <type> <description>)/}${2/interface|object$|(.+)/(?1::\n * @property <type> <description>)/}${2/directive$|(.+)/(?1::\n * @element <element>\n * @example <multiline>)/}}


Tab stop seven, more info below;

	${2/function|module|method$|(.+)/(?1::\n * @returns <type> <description>\n * @param <type> <description>)/}

This is not a tab stop either, it says: If tab stop 2 (our `docType`) is `function`, `module`, or `method`, append a new line, ` * @returns <type> <description>`, another line and ` * @param <type> <description>`. `method` is just a different name for `function`, and these are the optional parameters for `function` and `module`.

	${2/interface|object$|(.+)/(?1::\n * @property <type> <description>)/}

Still on the same line of the snippet, this checks tab stop 2 for the `docType`s `interface` and `object`, appending `@property <type> <description>` to the comment block if one is found. `Object` and `interface` are just different names for the same `docType`.

	${2/directive$|(.+)/(?1::\n * @element <element>\n * @example <multiline>)/}

After taking care of `interface` and `object`, check the tab stop 2 again, if its `directive`, appends the `element` and `example` properties.

	 @description ${6:<description>}

Last but not least, affords a quick description insertion.
