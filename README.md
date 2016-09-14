# move-literals - Move string literals

Maybe you're writing a reverse engineering challenge, and you want to obfuscate your code. Maybe you're stuck with a huge legacy codebase in dire need of refactoring. You want to perform a set of operations on source code, but doing it manually is iterative, time consuming and boring. Your IDE isn't as helpful as you would want it to be. Maybe you try using awk or sed and regular expressions, but after a while you realize you need to take a step up in the Chomsky hierarchy.

Enter Parsing Expression Grammars and LPeg.

It's fast, it's elegant, it makes you never want to think about regular expressions ever again.

This repo contains an example of how PEGs can be used. The example finds all string literals in a context where they would be compiled, and replaces them with preprocessor definitions. Why? Well, sometimes you just want to move all your strings around. You don't have to move them, you could just as easy replace them with a rot13, base64 encoded version and force push it to your company's master branch. Try it out!

# dependencies

Lua (probably/maybe > 5.1)
LPeg

# example

````
$ cat example.c
#include <stdio.h>

#ifdef __FOO_PLATFORM
#error \
  "unsupported platform"
#endif

int main(int argc, char *argv[]) {
  // char *x = "foobar";
  char *x = "foobarbaz";
  printf("%s\n", x);
  return 42;
}


$ ./move-literals.lua example.c > result.c
$ cat result.c
#define STRSYM_FOOBARBAZ \
   "foobarbaz"
#define STRSYM__S_N \
   "%s\n"
#include <stdio.h>

#ifdef __FOO_PLATFORM
#error \
  "unsupported platform"
#endif

int main(int argc, char *argv[]) {
  // char *x = "foobar";
  char *x = STRSYM_FOOBARBAZ;
  printf(STRSYM__S_N, x);
  return 42;
}
````


