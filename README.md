# How to be Angry

This is a collection of documentation for angr. By reading this, you'll become and angr pro and will be able to fold binaries to your whim.

# What is Angr?

Angr is a multi-architecture binary analysis platform, with the capability to perform dynamic symbolic execution (like Mayhem, KLEE, etc) and various static analyses on binaries. Several challenges must be overcome to do this. They are, roughly:

- Loading a binary into the analysis program.
- Translating a binary into an intermediate representation (IR).
- Translating that IR into a semantic representation (i.e., what it *does*, not just what it *is*).
- Performing the actual analysis. This could be:
 - A full-program static analysis (i.e., type inference, program slicing).
 - A symbolic exploration of the program's state space (i.e., "Can we execute it until we find an overflow?").
 - Some combination of the above (i.e., "Let's execute only program slices that lead to a memory write, to find an overflow.")

Angr has components that meet all of these challenges. This document will explain how each one works, and how they can all be used to accomplish your evil goals.

# Getting started

- [Installing angr](https://git.seclab.cs.ucsb.edu/gitlab/angr/angr_docker/blob/master/README.md)
- [Loading a binary](./loading.md)
- [Intermediate representation (IR)](./ir.md)
- [Semantic meaning](./semantic_meaning.md)
- [Running analyses](./analyses.md)
- [FAQ](./faq.md)


# Coding rules
We try to get as close as the [PEP8 code convention](http://legacy.python.org/dev/peps/pep-0008/) as possible.
If you use Vim, the [python-mode](https://github.com/klen/python-mode) plugin does all you need. You can also [manually configure](https://wiki.python.org/moin/Vim) vim to adopt this behavior.

Most importantly, please consider the following when writing code as part of Angr:
- Try to use attribute access (see the `@property` decorator) instead of getters and setters wherever you can. This isn't Java, and attributes enable tab completion in iPython.
- DO NOT, under ANY circumstances, `raise Exception`. **Use the right exception type**. If there isn't a correct exception type, subclass the core exception of the module that you're working in (i.e. AngrError in Angr, SimError in SimuVEX, etc) and raise that. Note that the `assert` statement falls under this as well. We catch, and properly handle, the right types of errors in the right places, but AssertionError and Exception are not handled anywhere and force-terminate analyses.
- Avoid tabs, use space indentation instead. Even though it's wrong, the de-facto standard is 4 spaces. It is a good idea to adopt this from the beginning, as merging code that mixes both tab and space indentation is awful.
- Avoid super long lines. PEP8 recommends 80 character long lines. It's okay to have longer lines, but keep in mind that long lines are harder to read and should be avoided.
- Avoid too long functions, it is often better to break them up into smaller functions.
- Prefer _ to __ for private members (so that we can access them anyway when debugging).
- **Document** your code. Every *class definition* and *public function definition* should have some description of:
 - What it does.
 - What are the type and the meaning of the parameters.
 - What it returns.

