
Got to: `~/Library/KeyBindings`.\
If the folder `KeyBindings` doesn't exist, create one.

Add a file `DefaultKeyBinding.dict` with the following content:
```
{
  "\UF729"  = moveToBeginningOfParagraph:; // home
  "\UF72B"  = moveToEndOfParagraph:; // end
  "$\UF729" = moveToBeginningOfParagraphAndModifySelection:; // shift-home
  "$\UF72B" = moveToEndOfParagraphAndModifySelection:; // shift-end
  "^\UF729" = moveToBeginningOfDocument:; // ctrl-home
  "^\UF72B" = moveToEndOfDocument:; // ctrl-end
  "^$\UF729" = moveToBeginningOfDocumentAndModifySelection:; // ctrl-shift-home
  "^$\UF72B" = moveToEndOfDocumentAndModifySelection:; // ctrl-shift-end
}
```

This remapping does the following in most Mac apps - including Chrome (some apps manage their key handling directly):
* `Home` and `End` will go to start and end of line
* `Shift Home` and `Shift End` will select to start and end of line
* `Ctrl Home` and `Ctrl End` will go to start and end of document
* `Shift Ctrl Home` and `Shift Ctrl End` will select to start and end of document

Note that you need to reboot after creating this file for it to take effect. Also make sure your editor does not append TXT to the end of it!
