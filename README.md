# satysfi-csv

A CSV reader and writer for SATySFi.


# install


You can install csv package with [Satyrographos](https://github.com/na4zagin3/satyrographos).

```
opam pin add "git+https://github.com/puripuri2100/satysfi-csv.git"

opam install satysfi-csv

satyrographos install
```


After installation, you can import this package by writing the code in preamble.

```
@require: csv/csv
```


# usage

Use `CSV.parser` function to read the CSV.

`CSV.parser` function returns `((string list) list) csv-parser-error result` type.

`csv-parser-error` type is defined as follows:

```
type csv-parser-erro =
  | CSVParserErrorUnExpectedChar of int * string
  | CSVParserErrorEOI
```

`'ok 'err result` type is defined in [satysfi-base/base.satyg](https://github.com/nyuichi/satysfi-base/blob/master/src/base.satyg#L49). Use the functions provided by [satysfi-base/result.satyg](https://github.com/nyuichi/satysfi-base/blob/master/src/result.satyg) when operating the `result` type.


```
let csv = `1,2,3
a,"b
c",""d",e",f""`

let csv-data = CSV.parser csv
% =>
%Ok([
%  [`1`; `2`; `3`];
%  [`a`; `b
%c`; `d",e",f`]
%])
```

You can change the delimiter of CSV:

```

let csv = `1,000:2,000:3,000
a:"b
c":""d":e",f""`

let csv-data = CSV.parser ?:(`:`) csv
% =>
%Ok([
%  [`1,000`; `2,000`; `3,000`];
%  [`a`; `b
%c`; `d",e",f`]
%])
```


Use `CSV.printer` function to write the CSV data.

`CSV.printer` is of type `string ?-> (string list) list -> string`

```
let data = [
  [`1`; `2`; `3`];
  [`a"b`; `a"
c"d`; `e
f`];
]

let csv = CSV.printer data
% ==>
%`1,2,3
%""a"b"",""a"
%c"d"","e
%f"`

% You can change the delimiter of CSV:
let csv2 = CSV.printer ?:(`:`) data
% ==>
%`1:2:3
%""a"b"":""a"
%c"d"":"e
%f"`
```

---

This software released under the [MIT license](https://github.com/puripuri2100/satysfi-csv/blob/master/LICENSE).

Copyright (c) 2021 Naoki Kaneko (a.k.a. "puripuri2100")

