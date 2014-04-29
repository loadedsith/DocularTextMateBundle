DocularTextMateBundle
=====================

Textmate 2 bundle for Docular is very much a work in progress: Feel free to create pull requests for new features.

#Install

    cd /Applications/TextMate.app/Contents/SharedSupport/Bundles (Textmate 1.5.10 & 2)
    git clone git://github.com/loadedsith/DocularTextMateBundle Docular.tmbundle

#Use

`/**` + [TAB] 

Command + Option + Control + [\\] 

You can also command + control + [T] ('Select Bundle Item'), you'd be looking for a command called "block comment documentation" so you could probably type `comdoc` or something similar.

#Result
  
    /**
      * @doc overview
      * @name myModule.moduleSection:myFuction
      *
      * @description This function rules!
      *
      */

There are various tab stops here, at first you'll have a choice of `function` or `overview`, stop 2 is myModule and stop 3 is myFunction. Stop 4, the final stop, is the description.
