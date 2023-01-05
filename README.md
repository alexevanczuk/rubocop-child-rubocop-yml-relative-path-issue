This reproduces an issue related to how `inherit_from` converts paths.

This is related to `https://github.com/rubocop/rubocop/pull/11260`

To reproduce the issue:

1) Run `bundle exec rubocop` and observe no issues
2) Comment out the `Include` for `Layout/LineLength` in `.rubocop.yml` and rerun `bundle exec rubocop` and observe the issue.
