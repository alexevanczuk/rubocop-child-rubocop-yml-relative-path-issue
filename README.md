This reproduces a potential bug where when determining if a `file_name_matches_any?` in `lib/rubocop/cop/base.rb`, we ask for `config.path_relative_to_config(file)`, even when the cop is defined in an inherited `.rubocop.yml` file. That is, I expected that include paths would be relative to *where they are defined* rather than where the `.rubocop.yml` is that included the rule (in this case, via inheritance).

This repo minimally reproduces the problem. `bin/rubocop` produces different results with these two commands:

```
~/workspace/rubocop-child-rubocop-yml-relative-path-issue - (main !x?) $ bin/rubocop packs/my_pack_without_issue/
Inspecting 1 file
C

Offenses:

packs/my_pack_without_issue/spec/path/to/my_spec.rb:3:1: C: [Correctable] Style/StringLiterals: Prefer single-quoted strings when you don't need string interpolation or special symbols.
""
^^

1 file inspected, 1 offense detected, 1 offense auto-correctable
```

As expected, an issue from `Style/StringLiterals` is found.

```
~/workspace/rubocop-child-rubocop-yml-relative-path-issue - (main !x?) $ bin/rubocop packs/my_pack_with_issue/
Inspecting 1 file
.

1 file inspected, no offenses detected
```

This is unexpected because the pack-level `.rubocop.yml` just points to the parent one:
```
inherit_from: 
  - ../../.rubocop.yml
```

Which uses the `Style/StringLiterals` cop.
