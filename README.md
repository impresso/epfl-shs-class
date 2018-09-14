# epfl-shs-class
Set of instructions for using data in the frame of EPFL SHS class

### Ho to get the data


### How to transform it

`for f in *.bz2; do bzcat $f | jq -c '{id: .id, type: .tp, date: .d, title: .t, fulltext: .ft}' > "${f%.jsonl.bz2}-reduced.jsonl.bz2"`

what does the command do:
- iterate over the files having the suffix `bz2` (each file lies in the variable `$f`)
- open the archive (`bzcat`) and produce a stream of json
- send (pipe `|`) this stream into `jq` 
- apply some filtering on the json content
- send the output to a file which name is composed of the input file, completed with `-reduced`
