# go-fuzz-list
Lists go fuzz targets

It is intended to be used in conjunction with the `Go fuzz` action, see <https://github.com/Andrew-Morozko/go-fuzz-example>.

* Searches for the fuzz targets matching the given regexp in the given package path.
* Filters out the fuzz targets that are mentioned in the open issues with the title starting with "Fuzzer `FuzzFunctionName` failed" and the body containing the package name.
* Returns a list of json objects:
  ```json
  [
    {
      "package": "github.com/Andrew-Morozko/go-fuzz-example/divide",
      "func": "FuzzDivide",
      "packageRelative": "divide"
    },
    {
      "package": "github.com/Andrew-Morozko/go-fuzz-example",
      "func": "FuzzSum",
      "packageRelative": ""
    }
  ]
  ```
* Expects the project to be checked out and to have golang installed.
* Requires the `issues: read` permission to fetch the open issues.
* Tested on `ubuntu-latest` runner.

## Example
```yml
jobs:
  list:
    name: List fuzz targets
    runs-on: ubuntu-latest
    permissions:
      issues: read
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-go@v5
        with:
          go-version: "stable"
      - uses: ./list
        id: list-targets
        with:
          # Default: "^Fuzz", matches all fuzz funcs
          func-regexp: "^FuzzDivide"
          # Default: "./...", matches all packages
          package-path: github.com/Andrew-Morozko/go-fuzz-example/divide
          # Default: ""
          tags: "foo,bar"
    outputs:
      targets: ${{steps.list-targets.outputs.targets}}
```


