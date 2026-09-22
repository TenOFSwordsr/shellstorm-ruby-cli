# ssww.rb - shell-storm.org Shellcode Archive CLI (Ruby)

A terminal client for the shell-storm.org exploit/shellcode database: search the archive and print
matching entries as a table, or fetch a stored shellcode listing by its ID. It is a Ruby rework of
Hood3dRob1n's Shell-Storm Ruby API, saved from a pastebin link with a personal header; the archive
lookups are the entire feature set. Read-only, no exploit code of its own.

**Suggested repo name:** `shellstorm-ruby-cli`
**Stack:** Ruby, `net/http`, `optparse`, `rubygems`, `text-table` gem
**Status:** archived
**Last modified:** 2019-12-04

## What it does

- `--search TERM` (also `-S`) - GETs `http://shell-storm.org/api/?s=<term>` with the query
  lowercased (the comment notes results fail otherwise), splits the response on `::::` and renders
  Author / Platform / Description / ID rows via `text-table`.
- `--display ID` (`-D`) - GETs `http://shell-storm.org/shellcode/files/shellcode-<id>.php` and prints
  the body between line offsets `7..-13`, with `&quot;`, `&gt;`, `&lt;` and `&amp;` un-escaped so the
  listing reads as source.
- Hand-rolled ANSI palette (`HC`, `FRED`, `FGRN`, `FWHT`, `FCYN`) instead of a colour gem, a `cls`
  helper that branches on `RUBY_PLATFORM =~ /win32/`, and a `SIGINT` trap that prints a warning and
  exits 666.
- OptionParser validation requires `:method` to be set; missing options, invalid flags and missing
  arguments each reprint the banner and usage. With no arguments it defaults `ARGV[0]` to `-h`.

## Layout

```
ssww.rb   the whole script (~136 lines)
```

## Running it

```
gem install text-table
ruby ssww.rb --search arm
ruby ssww.rb --display 660
```

## Notes

- Attribution is contradictory and should be fixed before publishing: the comment block signs "By:
  <author>" and cites `pastebin.com/y4wNMjr8`, while the printed banner still says "By: Hood3dRob1n".
  Keep the upstream credit.
- The archive it queries is a catalogue of shellcode payloads. The script itself neither builds nor
  delivers anything, but the combination reads as dual-use tooling and the file is 20 years of web
  assumptions deep.
- Brittle by design: the `7..-13` slice depends on the exact HTML layout of shell-storm's pages, the
  transport is plain HTTP, and there is no error handling around network failures.
- Filename does not match the internal name (`shell-storm-api.rb`) or the banner.
