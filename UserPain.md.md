[TOC]

# Zgłaszanie bugów

Zgłaszane bugi powinny być oznaczone priorytetem wyliczonym jako User Pain.
http://www.lostgarden.com/2008/05/improving-bug-triage-with-user-pain.html

## Opis kryteriów

**Type** (What type of bug is this?)

    7: Crash: Bug causes crash or data loss. Asserts in the Debug release.
    6: Major usability: Impairs usability in key scenarios.
    5: Minor usability: Impairs usability in secondary scenarios.
    4: Balancing: Enables degenerate usage strategies that harm the experience.
    3: Visual and Sound Polish: Aesthetic issues
    2: Localization:
    1: Documentation: A documentation issue

**Priority** (How will those affected feel about the bug?)

    5: Blocking further progress on the daily build.
    4: A User would return the product. Cannot RTM. The Team would hold the release for this bug.
    3: A User would likely not purchase the product. Will show up in review. Clearly a noticeable issue.
    2: A Pain – users won’t like this once they notice it. A moderate number of users won’t buy.
    1: Nuisance – not a big deal but noticeable. Extremely unlikely to affect sales.

**Likelihood** (Who will be affected by this bug)

    5: Will affect all users.
    4: Will affect most users.
    3: Will affect average number of users.
    2: Will only affect a few users.
    1: Will affect almost no one.

## Wyliczanie User Pain

User pain wylicza się jako iloczyn liczb wynikajcych z tych czterech kryteriów. Następnie skaluje się go do przedziału [0;1].