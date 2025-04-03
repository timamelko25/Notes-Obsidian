```table-of-contents
title: 
style: nestedList # TOC style (nestedList|nestedOrderedList|inlineFirstLevel)
minLevel: 0 # Include headings from the specified level
maxLevel: 0 # Include headings up to the specified level
includeLinks: true # Make headings clickable
hideWhenEmpty: false # Hide TOC if no headings are found
debugInConsole: false # Print debug info in Obsidian console
```
# script.sh
## Shebang line - строка шебанга. состоит из #!. Строка шебанга имеет следующий формат:
#!_interpreter_ [_optional-arg_]
_interpreter_ должен быть абсолютным путём к исполняемому файлу программы (если интерпретатором является скрипт, он тоже должен начинаться с шебанга). Необязательный _optional‑arg_ должен иметь формат единственного аргумента (по причинам переносимости он не должен содержать пробелы). Пробел после #! является опциональным
```bash
#! /bin/bash
```

## Variables
```bash
username = "hello"
filename = $3
```

## User input

```bash
read -p "Enter username: " user
echo "Username: $user" 
```

## Conditional ```if``` statement

```bash
if ["$EUID" -ne 0]; then
	echo "You are not runnig this script as root user"
else 
	echo "You are runnig this script as root user"
fi
```

## Loops

```bash
echo "Counting to 5:"
for i in {1..5}; do
	echo "$i"
done
```

## Functions

```bash
function meet() {
	echo "Hello, $1"
}
meet("Error")
```

## Conditional Case Statement

```bash
read num
case $num in
	1) echo "1: " ;;
	2) echo "2: " ;;
	*) echo "Invalid choice" ;;
esac
```

## File operations

```bash
if [ -e "$filename" ] && [ -d "$filename" ]; then
	echo "File exists in directory"
else 
	echo " File does not exist or is not a directory"
```

## Command line Arguments

```bash
echo "First argument: $1"
echo "Second argument: $2"
```

## Exit status code

```bash
cat nonexist.txt 2> /dev/null
echo "Exit status: $?"
```

## Index arrays

```bash
fruits = ("Apple" "Orange" "Banana")
echo "Fruits: ${fruits[0]}"
```

## Associative Arrays

```bash
declare -A capitals
capitals[USA] = "Washington"
capitals[France] = "Paris"
echo "Capital of France: ${capitals[France]}"
``` 

## Command Substituition

```bash
current_date = $(date)
echo "Today is: $current_date"
```
## Arithmetic Operations

```bash
res = $( expr 15 - 2)
echo $res
```

## Parameter Expansion

```bash
SRC = "path/to/foo.cpp"
BASEPATH = ${SRC##*/}
echo $BASEPATH
```

## Process Signal Handing

```bash
trap 'echo "received SIGTEM signal. Cleaning up..."; exit' SIGTERM
```

циклы, ключи запуска, if