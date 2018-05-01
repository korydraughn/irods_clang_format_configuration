# irods_clang_format_configuration
### .clang-format
```
---
Language:        Cpp
# BasedOnStyle:  Mozilla
AccessModifierOffset: -4
AlignAfterOpenBracket: Align
AlignConsecutiveAssignments: false
AlignConsecutiveDeclarations: false
AlignEscapedNewlinesLeft: false
AlignOperands:   true
AlignTrailingComments: true
AllowAllParametersOfDeclarationOnNextLine: false
AllowShortBlocksOnASingleLine: false
AllowShortCaseLabelsOnASingleLine: false
AllowShortFunctionsOnASingleLine: false
AllowShortIfStatementsOnASingleLine: false
AllowShortLoopsOnASingleLine: false
AlwaysBreakAfterDefinitionReturnType: None
AlwaysBreakAfterReturnType: None
AlwaysBreakBeforeMultilineStrings: false
AlwaysBreakTemplateDeclarations: true
BinPackArguments: false
BinPackParameters: false
BraceWrapping:
  AfterClass:      true
  #AfterControlStatement: true
  AfterControlStatement: false
  AfterEnum:       true
  AfterFunction:   true
  AfterNamespace:  true
  AfterStruct:     true
  AfterUnion:      true
  BeforeCatch:     true
  BeforeElse:      true
  IndentBraces:    false
BreakBeforeBinaryOperators: None
BreakBeforeBraces: Custom
BreakBeforeTernaryOperators: true
BreakConstructorInitializersBeforeComma: true
#ColumnLimit:     80
ColumnLimit:     120
CommentPragmas:  '^ IWYU pragma:'
ConstructorInitializerAllOnOneLineOrOnePerLine: false
ConstructorInitializerIndentWidth: 4
ContinuationIndentWidth: 4
Cpp11BracedListStyle: true
DerivePointerAlignment: false
DisableFormat:   false
ExperimentalAutoDetectBinPacking: false
ForEachMacros:   [ foreach, Q_FOREACH, BOOST_FOREACH ]
IncludeCategories:
  - Regex:           '^"(llvm|llvm-c|clang|clang-c)/'
    Priority:        2
  - Regex:           '^(<|"(gtest|isl|json)/)'
    Priority:        3
  - Regex:           '.*'
    Priority:        1
IndentCaseLabels: true
IndentWidth:     4
IndentWrappedFunctionNames: false
KeepEmptyLinesAtTheStartOfBlocks: false
MacroBlockBegin: ''
MacroBlockEnd:   ''
MaxEmptyLinesToKeep: 1
NamespaceIndentation: All
PenaltyBreakBeforeFirstCallParameter: 19
PenaltyBreakComment: 300
PenaltyBreakFirstLessLess: 120
PenaltyBreakString: 1000
PenaltyExcessCharacter: 1000000
PenaltyReturnTypeOnItsOwnLine: 200
PointerAlignment: Left
ReflowComments:  true
SortIncludes:    true
SpaceAfterCStyleCast: true
SpaceBeforeAssignmentOperators: true
SpaceBeforeParens: ControlStatements
SpaceInEmptyParentheses: false
SpacesBeforeTrailingComments: 1
SpacesInAngles:  false
SpacesInContainerLiterals: true
SpacesInCStyleCastParentheses: false
SpacesInParentheses: false
SpacesInSquareBrackets: false
Standard:        Cpp11
TabWidth:        8
UseTab:          Never
...
```
### Updates
- Curly braces follow control statements instead of a line-break.
- Column limit changed from 80 to 120 characters.

### Notes
- If you change the configuration, make sure to undo any formatting previously done.  Not doing so could result in unexpected results.
- Use `git clang-format` instead of `clang-format` so that only the modified files will be formatted.

### Git Pre-Commit Hook
Do the following:
```
$ cd root/of/your/repo
$ cd .git/hooks
$ touch pre-commit
$ chmod +x pre-commit
```
Open `pre-commit` in your favorite editor and type the following.
This example uses python 2, but you can use any language you want as long as the same operations can be accomplished.
This script does not automatically format code.  Instead, it checks to see if the new code has been processed by clang-format.
```python
#! /usr/bin/python

from __future__ import print_function
import subprocess

# Git Clang-Format will read properties from your git configuration.  You can set default properties
# in your git configuration or pass them to the command directly.  This script assumes you'll set options
# in your git configuration.  Git Clang-Format configuration options include:
#   clangformat.binary
#   clangformat.style
#   clangformat.extensions
#
# You'd set them by running the following commands:
#   git config --global clangformat.binary clang-format
#   git config --global clangformat.style file
#   git config --global clangformat.extensions 'h,c,hpp,cpp'
#
# You can pass them directly by using the line below (if you do this, you'll need to pass them everywhere):
#   output = subprocess.check_output(['git', 'clang-format', '-v', '--extensions', 'h,c,hpp,cpp', '--style', 'file', '--diff'])
output = subprocess.check_output(['git', 'clang-format', '--diff'])

messages = [
    'no modified files to format\n',
    'clang-format did not modify any files\n'
]

# If you receive this message and you have unstaged changes, you may need to run the following to
# allow changes to those files as well.
#   git clang-format -f [OPTIONS ...]
if output not in messages:
    print('Run git clang-format [-f], then commit.\n')
    exit(1)

exit(0)
```

### Official Clang-Format v3.8 Documentation
- [Home](http://releases.llvm.org/3.8.0/tools/clang/docs/ClangFormat.html)
- [Style Options](http://releases.llvm.org/3.8.0/tools/clang/docs/ClangFormatStyleOptions.html)
