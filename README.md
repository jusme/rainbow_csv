## Overview
Rainbow CSV: minimalistic but powerful vim plugin for viewing csv/tsv files and executing SQL "select" queries.
* The plugin highlights csv columns in different rainbow colors. 
* Rainbow csv also allows user to run simple "select" queries in SQL-like RBQL language e.g. `select c1, int(c2) + int(c3) * 10 where c4 != 'car' order by c1 desc`

To enter a "select" query, press `F5`. To execute the query press `F5` again. If you want to replace the source table with select results, press `F5` again.

There are 2 ways to enable csv columns highlighting:
1. CSV autodetection based on file content. File extension doesn't have to be .csv or .tsv
2. Manual CSV delimiter selection with `:RainbowDelim` command (So you can use it even for non-csv files, e.g. to highlight function arguments in different colors)


![screenshot tsv](https://raw.githubusercontent.com/mechatroner/rainbow_csv/master/screenshot.png)


## RBQL Description
minimalistic SQL-like language that supports "select" queries with python expressions.

### Main Features
* Use python expressions inside "select", "where" and "order by" statements
* Use "c1", "c2", ... , "cN" as column names to write select queries
* Output entries appear in the same order as in input unless "ORDER BY" is provided.
* "lnum" variable holds entry line number
* Input csv/tsv table may contain varying number of entries (but select query must be written in a way that prevents output of missing values)

### Supported SQL Keywords (Keywords are case insensitive)
* select 
* where 
* order by
* desc/asc
* distinct

### Special variables
* `c1`, `c2`, ... , `cN` - column names
* `*` - whole line/entry
* `lnum` - line number (1-based)
* `flen` - number of columns in current line/entry

### Query examples

* `select * where lnum <= 10` - this is an equivalent of bash command "head -n 10", lnum is 1-based')
* `select c1, c4` - this is an equivalent of bash command "cut -f 1,4"
* `select * order by int(c2) desc` - this is an equivalent of bash command "sort -k2,2 -r -n"
* `select * order by random.random()` - random sort, this is an equivalent of bash command "sort -R"
* `select lnum, *` - enumerate lines, lnum is 1-based
* `select * where re.match(".*ab.*", c1) is not None` - select entries where first column has "ab" pattern


## Mappings

|Key           | Action                                                      |
|--------------|-------------------------------------------------------------|
|`<leader>d`   | Print info about current column (under the cursor)          |
|`F5`          | Start "select" query editing for the current csv file       |
|`F5`          | Execute currently edited "select" query                     |
|`F5`          | Replace the original csv file content with "select" results |


## Commands

#### :RainbowDelim

Mark current file as csv and highlight columns in rainbow colors, character
under the cursor will be used as delimiter. Selection will be recorded in the
config file for future vim sessions.

#### :NoRainbowDelim

This command will disable rainbow columns highlighting for the current file.
Usefull when autodection mechanism has failed and marked non csv file as csv
this command also has an alias `:NoRainbowDelim`


## Configuration

#### g:rcsv_delimiters
*Default: [	,]*
By default plugin checks only TAB and comma characters during autodetection stage.
You can override this variable to autodetect tables with other separators. e.g. `let g:rcsv_delimiters = [	;:,]`

#### g:disable_rainbow_csv_autodetect
You can disable csv files autodetection mechanism by setting this variable value to 1.
You will still be able to use manual csv delimiter selection.

#### g:rcsv_max_columns
*Default: 30*
Autodetection will fail if buffer has more than `g:rcsv_max_columns` columns.
You can rise or lower this limit.


## Optional "Header" file feature
Rainbow csv allows you to create a special "header" file for your table files. It should have the same name as the table file but with ".header" suffix (e.g. for "input.tsv" the header file is "input.tsv.header"). The only purpose of header file is to provide csv column names for `:RbGetColumn` command.


## Installation

Install with your favorite plugin manager.


## Requirements
vim compiled with "python" or "python3".
