- [script.sh](#scriptsh)
	- [Shebang line - строка шебанга. состоит из #!. Строка шебанга имеет следующий формат:](#shebang-line---%D1%81%D1%82%D1%80%D0%BE%D0%BA%D0%B0-%D1%88%D0%B5%D0%B1%D0%B0%D0%BD%D0%B3%D0%B0-%D1%81%D0%BE%D1%81%D1%82%D0%BE%D0%B8%D1%82-%D0%B8%D0%B7--%D0%A1%D1%82%D1%80%D0%BE%D0%BA%D0%B0-%D1%88%D0%B5%D0%B1%D0%B0%D0%BD%D0%B3%D0%B0-%D0%B8%D0%BC%D0%B5%D0%B5%D1%82-%D1%81%D0%BB%D0%B5%D0%B4%D1%83%D1%8E%D1%89%D0%B8%D0%B9-%D1%84%D0%BE%D1%80%D0%BC%D0%B0%D1%82)
	- [Variables](#variables)
	- [User input](#user-input)
	- [Conditional ```if``` statement](#conditional-if-statement)
	- [Loops](#loops)
	- [Functions](#functions)
	- [Conditional Case Statement](#conditional-case-statement)
	- [File operations](#file-operations)
	- [Command line Arguments](#command-line-arguments)
	- [Exit status code](#exit-status-code)
	- [Index arrays](#index-arrays)
	- [Associative Arrays](#associative-arrays)
	- [Command Substituition](#command-substituition)
	- [Arithmetic Operations](#arithmetic-operations)
	- [Parameter Expansion](#parameter-expansion)
	- [Process Signal Handing](#process-signal-handing)

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