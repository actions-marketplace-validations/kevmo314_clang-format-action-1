# clang-format action

runs clang-format on the diff of the most recent commit

Fixed version of purduesigbots/clang-format-action

# usage

```yml
name: "format code"

# run on pull requests to develop
on:
  pull_request:
    branches:
      - develop

jobs:
  format:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
        with:
          # check out HEAD on the branch
          ref: ${{ github.head_ref }}
          # make sure the parent commit is grabbed as well, because
          # that's what will get formatted (i.e. the most recent commit)
          fetch-depth: 2
      # format the latest commit
      - uses: purduesigbots/clang-format-action@1
        # use one of clang-format's supported styles or leave this out to use the style in your .clang-format file
        with:
          style: file
      # commit the changes (if there are any)
      - name: Commit changes
        uses: stefanzweifel/git-auto-commit-action@v4.1.2
        with:
          commit_message: 🎨 apply clang-format changes
          branch: ${{ github.head_ref }}
```

## why

there are already a couple versions of this action floating around ([cvra/clang-format-action](https://github.com/cvra/clang-format-action) and [IvanLudvig/clang-format-action](https://github.com/IvanLudvig/clang-format-action)) but these either

- aren't on the actions marketplace,
- aren't recently updated,
- assume a particular default style for your code (with no way to change it),
- or have some sketchy docker files

so for maximum ease of use we are providing a more robust action here
