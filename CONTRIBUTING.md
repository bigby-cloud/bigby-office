# Contributing to Bigby.office

Thanks for taking the time to contribute. Bigby.office is built on
Collabora Online, and most of the code here is theirs. Where a change
should go depends on what it touches.

## Reporting issues

For problems with Bigby.office or Bigby.cloud, use our
[report form][report]. Please search the existing issues on this
repository first, in case someone has already reported it.

If you've found a security problem, tell us privately through the
same form rather than opening a public issue.

[report]: https://bigby.cloud/report-an-issue/

## Contributing source code

### Changes to Bigby.office

Open a pull request against `main` on this repository. Keep each
commit small and about one thing, and read [MAINTAINING.md][maintaining]
first. It explains why we prefer configuration and the `branding/`
folder to editing Collabora's files.

[maintaining]: MAINTAINING.md

### Changes that would help everyone

If a fix isn't specific to Bigby.office, it's better sent to
Collabora. Everyone using Collabora Online gets it, and we don't have
to carry it in our fork. Collabora take patches through their Gerrit
server at https://gerrit.collaboraoffice.com, not through GitHub.
Their [contributing guide][upstream] explains how.

[upstream]: https://github.com/CollaboraOnline/online.mirror/blob/main/CONTRIBUTING.md

### Sign your work

We use the Developer Certificate of Origin (DCO) as an additional
safeguard for the Bigby.office project. This is a well
established and widely used mechanism to assure contributors have
confirmed their right to license their contribution under the
project's license.

By making a contribution to this project, I certify that:

(a) The contribution was created in whole or in part by me and I
    have the right to submit it under the appropriate version of
    the Mozilla Public License v2, or (for artwork) the Creative
    Commons Zero (CC0) license; or

(b) The contribution is based upon previous work that, to the best
    of my knowledge, is covered under an appropriate open source
    license and I have the right under that license to submit that
    work with modifications, whether created in whole or in part by
    me, under the aforementioned licenses, in the appropriate
    version; or

(c) The contribution was provided directly to me by some other
    person who certified (a) or (b) and I have not modified it.

(d) I understand and agree that this project and the contribution
    are public and that a record of the contribution (including all
    metadata and personal information I submit with it, including
    my sign-off) is maintained indefinitely and may be redistributed
    consistent with this project or the open source license(s)
    involved.

(e) I am granting this work to this project under the terms of both
    the Mozilla Public License v2 and the GNU Lesser General Public
    License version 3 as published by the Free Software Foundation.

(f) I agree that my contributions are adequately acknowledged by
    the "Copyright the Collabora Online contributors." header, and
    the public records of contribution.

If you can certify the above, add this line to every git commit
message:

```
Signed-off-by: Random J Developer <random@developer.example.org>
```

Use your real name and your real email address (sorry, no pseudonyms
or anonymous contributions). If you set your `user.name` and
`user.email` git configs, you can sign your commit automatically
with `git commit -s`.

### Commit messages

When reviewing a patch, we look for the following information in the
commit message:

- **Title**: a single line, short and to the point summary of what
  the patch does.
  - Rationale: to be able to do `git log --pretty=oneline` and have
    a usable result.

- **Intro**: observation of the current state.
  - Rationale: the problem to be solved is obvious to you, but not
    to the reviewer. It is good to have a list of steps to reproduce
    the problem.

- **Problem description**: pros and cons of the current state.
  - Rationale: when some feature does not work the way expected,
    frequently there is some other use case that motivated the
    current behavior. It is easier to not break the old use case
    with your change if you are aware of it.
  - If there was an old use case and you found it by research,
    please document it so the person reading the commit message
    finds it easily.

- **Solution**: give orders to the codebase.
  - A short description of how you introduce new behavior while not
    breaking old behavior is useful, because it may not be too
    obvious just by looking at what you changed.

An alternative is to have much of this information in a (public)
issue, refer to that issue, and have a short commit message. That
works better e.g. when using images to demonstrate the problem.

### Coding style

There is not really any serious rationale why the code ended up
being written in the style it is, but unless you plan to change some
style detail completely and consistently all over, please keep to
the style of the existing code when editing.

The style is roughly as follows, in rough order of importance:

- Source code files should have unix line terminators (LF).

#### C++

- As in the engine, no hard TABs in source files. Only spaces.
  Indentation step is four columns.
- As in the engine, the braces `{` and `}` of the block of `if`,
  `switch`, and `while` statements go on separate lines.
- Following Poco conventions, non-static member variables are
  prefixed with an underscore. Static members have a CamelCase name.
- Do use C++20. When writing new code, prefer the C++ Standard
  Library over Poco classes and functions.
- Always prefer the C++ wrapped version of a C library API. I.e.
  include `<cstring>` instead of `<string.h>`, use `std::memcpy()`
  instead of `memcpy()`, etc.
- Use `std::` prefix for all std API; do not ever do `using std;`.
  It is OK to use `using Poco::Foo;` all over (maybe not a good
  idea?), but please no `using` in headers.
- Member functions use `camelCaseWithInitialLowerCase` instead of
  `CamelCaseWithInitialUpperCase`.
- No kind of Hungarian prefixes.
- `return` is not a function but a statement; it does not need
  extra parentheses.
- Use `auto` in the following cases only:
  - iterators,
  - range-based for loops,
  - the type is spelled out in the same line already (e.g.
    initializing from a cast or a function that has a single type
    parameter).

  In other cases it makes the code more readable to still spell out
  the type explicitly.

#### JavaScript

- Indent code with TABs.
- For new code, use TypeScript.

### Security credential related changes

- Instead of the usual one, two reviews are needed.
- Instead of just approving the pull request, please add your
  explicit sign-off to the commit message when you review.

### AI policy

We consider AI tools that help programming just one more tool that,
if used judiciously, can help developers learn to code. Volunteers
using such AI tools are permitted to get good ideas, but you are
responsible for the resulting code: you should be able to explain
each hunk of the diff you have written based on the generated
samples. Submitting patches without understanding their content,
copying AI generated code verbatim, or not testing the result before
submitting would create risk and waste reviewer time, and is
forbidden.

## Translations

Translations come from Collabora. Please submit them through their
[Weblate project][weblate].

[weblate]: https://hosted.weblate.org/projects/collabora-online
