# Trigger the bug

Run these commands:

    git clone https://github.com/unixpickle/dart_mixin_bug.git
    cd dart_mixin_bug
    dartanalyzer lib/mixin_bug.dart

I get an exception:

    NoSuchMethodError: method not found: 'substitute2'
    Receiver: null
    Arguments: [_List len:0, _List len:0]
    #0      Object.noSuchMethod (dart:core-patch/object_patch.dart:45)
    #1      TypeResolverVisitor._createImplicitContructor (package:analyzer/src/generated/resolver.dart:23556)
    #2      TypeResolverVisitor.visitClassTypeAlias (package:analyzer/src/generated/resolver.dart:23035)
    #3      ClassTypeAlias.accept (package:analyzer/src/generated/ast.dart:3798)
    #4      NodeList.accept (package:analyzer/src/generated/ast.dart:19469)
    #5      CompilationUnit.visitChildren (package:analyzer/src/generated/ast.dart:4272)
    ...

So, yeah, it screws up the Dart Editor and makes me sad.

# The underlying bug

If I have a main library file like this:

    library mylib;
    
    part 'crash_analyzer.dart';
    
    class ClassA {
      ClassA(_) {}
    }

And a secondary file `crash_analyzer.dart`:

    part of mylib;
    class ClassB = ClassA;

Then the analyzer will fail. The analyzer doesn't crash if `crash_analyzer.dart` is inlined instead of being included with the `part` directive.

# Workaround

For now, you'll just have to do your mixin declarations and other `class ... = ...` stuff in the main file for your library.
