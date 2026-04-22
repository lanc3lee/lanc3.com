In most Unix-based text processing tools like **awk**,

see "columns" as strings of characters separated by **whitespace** (spaces or tabs).

### Quick Breakdown

If you have a line of text like: `apple orange banana`

- `print $1` → `apple`

- **`print $2` → `orange`**

- `print $3` → `banana`

- `print $0` → `apple orange banana` (the entire line)


---

### Common Examples

- **Using awk:** If you want to extract the second column from a file: