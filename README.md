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

### Notes
- If you change the configuration, make sure to undo any formatting previously done.&nbsp; Not doing so could result in unexpected results.
- Use `git clang-format` instead of `clang-format` so that only the modified files will be formatted.

### Adding a Style Configuration to your Project
To add the style configuration to one or more projects, do the following:
1. Create a file called `.clang-format`.
2. Copy and paste the options above into the file.
3. Place the file in a parent directory of the source tree.&nbsp; Clang-Format will check the current directory and then parent directories until it finds a style configuration file.

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
This script does not automatically format code.&nbsp; Instead, it checks to see if the new code has been processed by clang-format.
```python
#! /usr/bin/python

from __future__ import print_function
from subprocess import check_output

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
#   cmd = ['git', 'clang-format', '--extensions', 'h,c,hpp,cpp', '--style', 'file', '--diff']
#   output = check_output(cmd)
output = check_output(['git', 'clang-format', '--diff'])

messages = [
    'no modified files to format\n',
    'clang-format did not modify any files\n'
]

# If you receive this message and you have unstaged changes, you may need to run the following to
# allow changes to those files as well.
#   git clang-format -f
if output not in messages:
    print('Run git clang-format [-f], then commit.\n')
    exit(1)

exit(0)
```
To test, modify a C/C++ file and commit the changes.&nbsp; You should receive a message stating that you need to run
`git clang-format`.&nbsp; Do just that and you should be good to go.&nbsp; The tool will only format the modified code.

### Official Clang-Format v3.8 Documentation
- [Home](http://releases.llvm.org/3.8.0/tools/clang/docs/ClangFormat.html)
- [Style Options](http://releases.llvm.org/3.8.0/tools/clang/docs/ClangFormatStyleOptions.html)
