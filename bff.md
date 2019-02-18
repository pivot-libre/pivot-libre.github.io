---
layout: page
title: Ballot File Format (BFF)
permalink: /bff/
---

The Ballot File Format aims to concisely represent ranked ballots and ranked election results.

## Contributing
The format is a working draft. You can suggest improvements and hold discussions [here](https://github.com/pivot-libre/pivot-libre.github.io/issues). If you are comfortable with GitHub, you are welcome to submit pull requests to [the source file for this web page](https://github.com/pivot-libre/pivot-libre.github.io/blob/master/bff.md).

## BFF Examples
```
A > B > C
A > B = C
A = C > B
A = B = C
```

## Basics
We want to represent a ranking of options. An option could be almost anything, for example: `apple`, or `banana`. Options are ranked via the `>` and `=` symbols. For example, if `apple` is preferred over `banana`, that would be `apple > banana` in BFF. If `apple` and `banana` are tied, that would be `apple = banana`. To rank more options, add more options separated by comparisons. For example, if `apple` is in 1st place, `banana` and `cherry` are tied for 2nd place, and `date` is in 3rd place, that would be `apple > banana = cherry > date`. This format can be used to encode both ranked ballots and ranked election results.

## Restrictions
To keep things simple, options can only be compared via a single `>` or `=` character. The `<` character is forbidden everywhere. It is invalid to combine the comparison characters (`>=`). It is invalid to combine multiple comparison characters (`>>`, `==`). All of the following are invalid.
```
A < B
A >= B
A >> B
A == B
```

The character `*` is forbidden, except in specific situations detailed in [Multipliers](#multipliers).

## Whitespace
If there is horizontal whitespace (spaces, tabs) between a comparison and an option, it is ignored. All of the following are valid and equivalent.
```
A>B
A >B
A> B
A > B
```

Spaces within options are valid and significant. All of the following are valid and distinct.
```
Apple Pie > Banana Cream Pie
ApplePie > BananaCreamPie
ApplePie > Banana CreamPie
```

## Ballot-Specific Rules
### One Ballot Per Line
Each ballot should be represented on its own line.

### Multipliers
For ballots only (not election results), a ballot can be replicated by prefixing it with a multiplier. A multiplier consists of an integer and the `*` character. This permits something repetitive like this...

```
A > B > C
A > B > C
A > B > C
```
...to be represented more concisely as...
```
3 * A > B > C
```

There is no obligation to use multipliers. It is valid to use a mix of multipliers and non-multipliers. All of the following examples are valid and equivalent.

```
A > B > C
A > B > C
A > B > C
C > A > B
```

```
A > B > C
1 * A > B > C
A > B > C
C > A > B
```

```
2 * A > B > C
A > B > C
C > A > B
```

```
C > A > B
3 * A > B > C
```

Horizontal whitespace between the integer and the `*` is permitted and insignificant. Horizontal whitespace between the `*` and the rest of the ballot is permitted and insignificant. All of the following examples are valid and equivalent.

```
3*A > B > C
3 *A > B > C
3* A > B > C
3 * A > B > C
```

## Result-Specific Rules
Since there is only one result for an election, multipliers (`*` characters) are forbidden in results.
