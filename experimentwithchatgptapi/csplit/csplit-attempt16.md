# Attempt 16 - final code with finesse

![image](https://user-images.githubusercontent.com/129967941/230116064-9e776501-cc8c-4f9b-a5c8-1f88b68a4c28.png)


![image](https://user-images.githubusercontent.com/129967941/230116276-e98dca66-722d-4ecc-8192-9762f6dbb233.png)


# Finally the code looked like for sqlsplit.sh

```
#!/bin/bash

if [ -z "$1" ]; then
  echo "Usage: $0 large_file.sql"
  exit 1
fi

# Preprocess the input file to remove non-matching lines
sed -n '/DROP TABLE IF EXISTS/,/UNLOCK TABLES;/p' "$1" > tmp.sql

# Split the preprocessed file using csplit, redirecting stderr to a file
csplit --quiet -f output_file_ -z tmp.sql '/DROP TABLE IF EXISTS/' '{*}' '/UNLOCK TABLES/' --suffix-format='%04d.sql' 2> csplit_error.log

# Print the error messages to the console, except the file size-related error message
cat csplit_error.log | grep -v "csplit: output_file_[0-9]*\.sql: file size greater than size limit" >&2

# Clean up the temporary files
rm tmp.sql csplit_error.log
```

# Execution of code looked like this:

```
chmod +x sqlsplit.sh
./sqlsplit.sh dumpfile.sql
```
