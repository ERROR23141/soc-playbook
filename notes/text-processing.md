# Text Processing Cheatsheet

## Finding unique lines
- Show only lines that appear **once** in a file:
  `sort <file> | uniq -u`
- Show how many times each line appears then filters the unique:
  `sort <file> | uniq -c | grep " 1 "`

## Frequency of lines (who appears most?)
- Show each unique line with a count, sorted from least to most frequent:
  `sort <file> | uniq -c | sort -n`

## Using `awk` to find unique lines
- Count each line and print only lines that appear once:
  `awk '{count[$0]++} END {for (line in count) if (count[line] == 1) print line}' <file>`
