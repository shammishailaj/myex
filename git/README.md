# How to count the number of lines in a git repository

```
git ls-files | xargs wc -l
```

See: https://gist.github.com/mandiwise/dc53cb9da00856d7cdbb


- If getting "too long error", do the following:

```
git ls-files > listOfFiles
cat listOfFiles | xargs wc -l
```

- Ignore certain file-types while counting:

```
git ls-files | grep -v ".js" | xargs wc -l
```
