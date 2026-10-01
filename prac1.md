# Практическая работа №1

## Задача 1

```bash
grep -o '^[^:]*' /etc/passwd | sort
```

## Задача 2

```bash
grep -E '^[[:alnum:]_.-]+[[:space:]]+[0-9]+' /etc/protocols | awk '{print $2, $1}' | sort -nr | head -n 5
```

## Задача 3

```bash
#!/bin/bash

text="$*"
len=${#text}
border=$(printf '%*s' $((len + 2)) '' | tr ' ' '-')

echo "+$border+"
echo "| $text |"
echo "+$border+"
```

## Задача 4

```bash
#!/bin/bash

grep -oE '[A-Za-z_][A-Za-z0-9_]*' "$1" | sort -u | tr '\n' ' '
echo
```

## Задача 5

```bash
#!/bin/bash

if [ -z "$1" ]; then
    echo "Usage: ./reg <file>"
    exit 1
fi

if [ ! -f "$1" ]; then
    echo "Error: file $1 not found"
    exit 1
fi

sudo cp "$1" /usr/local/bin/
sudo chmod 755 "/usr/local/bin/$(basename "$1")"

echo "Command $(basename "$1") registered"
```

## Задача 6

```bash
#!/bin/bash

if [ $# -eq 0 ]; then
    set -- *.c *.js *.py
fi

for file in "$@"; do
    [ -f "$file" ] || continue

    first=$(head -n 1 "$file")

    case "$file" in
        *.c|*.js) pattern='^[[:space:]]*(//|/\*)' ;;
        *.py)     pattern='^[[:space:]]*#' ;;
        *)        echo "$file: unsupported extension"; continue ;;
    esac

    if echo "$first" | grep -qE "$pattern"; then
        echo "$file: comment found"
    else
        echo "$file: no comment"
    fi
done
```

## Задача 7

```bash
#!/bin/bash
find "${1:-.}" -type f -exec sha256sum {} + | sort | uniq -w64 --all-repeated=separate | cut -c67-
```

## Задача 8

```bash
#!/bin/bash
find "${2:-.}" -type f -name "*.$1" -print0 | tar -cvf "${1}_files.tar" --null -T -
```

## Задача 9

```bash
#!/bin/bash
sed 's/    /\t/g' "$1" > "$2"
```

## Задача 10

```bash
#!/bin/bash
find "$1" -maxdepth 1 -type f -name "*.txt" -empty -printf "%f\n"
```
