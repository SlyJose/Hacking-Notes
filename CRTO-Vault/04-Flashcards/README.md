# Flashcards

One file per domain, formatted for the [Obsidian Spaced Repetition](https://github.com/st3v3nmw/obsidian-spaced-repetition) plugin.

## Syntax

Use `#flashcards` tag in each file. Cards use `::` for single-line and `?` for multi-line:

**Single-line:**
```
What tool performs Kerberoasting from a Beacon?::Rubeus (`execute-assembly Rubeus.exe kerberoast`)
```

**Multi-line:**
```
What are the prerequisites for Kerberoasting?
?
- Valid domain user credentials
- Network access to a Domain Controller
- Target accounts must have SPNs registered
```

## Tips

- Write cards that test recall of commands, prerequisites, OPSEC considerations, and detection signatures
- Link back to the technique note for context: `See [[Kerberoasting]]`
- Review daily for best retention
