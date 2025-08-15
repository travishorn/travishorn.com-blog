---
title: "Converting TSV to CSV with DuckDB"
datePublished: Fri Aug 15 2025 11:00:44 GMT+0000 (Coordinated Universal Time)
cuid: cmecpw3j0000w02jy6r831ixz
slug: converting-tsv-to-csv-with-duckdb
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1755187807186/b37ebdc9-d8f3-4990-be2d-580899e17525.png
tags: data-science, bash, cli, csv, transformation

---

DuckDB is an incredibly versatile, in-process database. Its powerful SQL engine and ability to read various file formats make it an excellent tool for data transformation tasks right from your command line. In this post, you'll learn how to use DuckDB to convert one or more TSV files into CSV files.

You can download DuckDB from [https://duckdb.org/#quickinstall](https://duckdb.org/#quickinstall) and then run it from your terminal.

It can do all kinds of things, like…

* Read and write CSV, Parquet, and JSON
    
* Perform SQL operations including window functions and CTEs
    
* Query other databases like MySQL and PostgreSQL
    
* Guarantee ACID transactions
    

It’s great for querying CSV files:

```sql
SELECT * FROM read_csv('input.csv');
```

It can also write to CSV:

```sql
COPY my_data TO 'output.csv';
```

In fact, you can combine these statements to read in from and write out to a file:

```sql
COPY (SELECT * FROM read_csv('input.csv')) TO 'output.csv';
```

Reading in data just to write it back out is a little pointless. It’s like doing `cp input.csv output.csv` with more steps.

The real power comes in when you have differing file formats. For example, you can convert a tab-separated file to a comma-separated file.

```sql
COPY (SELECT * FROM read_csv('input.tsv', delim = '\t')) TO 'output.csv';
```

From the terminal, DuckDB allows you to execute a command and immediately exit, using the `-c` option:

```bash
duckdb -c "SELECT 1;"
```

So you can perform the conversion without entering into interactive mode:

```bash
duckdb -c "COPY (SELECT * FROM read_csv('input.tsv', delim = '\t')) TO 'output.csv';"
```

If you have multiple tab-separated files in the same directory, you can use a Bash script to loop over them:

```bash
#!/bin/bash

# Define script parameters
# Default to current directory if no argument provided
directory_path="${1:-$(pwd)}"

echo "Searching for .tsv files in: $directory_path"

# Check if directory exists
if [ ! -d "$directory_path" ]; then
    echo "Error: Directory '$directory_path' does not exist."
    exit 1
fi

# Get all .tsv files in the specified directory
tsv_files=( "$directory_path"/*.tsv )

# Check if any .tsv files were found
if [ ! -e "${tsv_files[0]}" ]; then
    echo "No .tsv files found in the directory: $directory_path"
    exit 0
fi

echo "Found ${#tsv_files[@]} .tsv files. Starting conversion..."

# Loop through each .tsv file found
for file in "${tsv_files[@]}"; do
    # Get the base name and directory
    filename=$(basename "$file")
    basename="${filename%.*}"
    dirname=$(dirname "$file")
    
    # Construct the output filename
    output_file="$dirname/$basename.csv"
    
    echo "Processing $filename -> $basename.csv..."
    
    # Execute the DuckDB command
    # Escape single quotes in paths by doubling them
    input_path=$(echo "$file" | sed "s/'/''/g")
    output_path=$(echo "$output_file" | sed "s/'/''/g")
    
    sql_command="COPY (SELECT * FROM read_csv('$input_path', delim = '\t')) TO '$output_path';"
    
    duckdb -c "$sql_command"
    
    if [ $? -eq 0 ]; then
        echo "Conversion complete for $filename."
    else
        echo "Error: Failed to convert $filename."
    fi
done

echo "All files have been converted."
```

Or, if you’re on Windows, a PowerShell script can do the same thing:

```powershell
# Define script parameters.
# The `[Parameter(Mandatory=$false)]` attribute makes the parameter optional.
# The default value is set to the current working directory's path.
param (
    [string]$directoryPath = $(Get-Location).Path
)

# Use the provided or default directory path.
Write-Host "Searching for .tsv files in: $directoryPath"

# Get all files with a .tsv extension in the specified directory.
# The -File parameter ensures that we only get files and not subdirectories.
$tsvFiles = Get-ChildItem -Path $directoryPath -Filter "*.tsv" -File

# Check if any .tsv files were found.
if ($null -eq $tsvFiles) {
    Write-Host "No .tsv files found in the directory: $directoryPath"
}
else {
    Write-Host "Found $($tsvFiles.Count) .txt files. Starting conversion..."

    # Loop through each .tsv file found.
    foreach ($file in $tsvFiles) {
        # Construct the output filename by changing the extension from .tsv to .csv.
        $outputFile = Join-Path -Path $file.DirectoryName -ChildPath "$($file.BaseName).csv"

        # The DuckDB SQL command needs to have paths correctly escaped.
        # This approach ensures the entire SQL command is a single, properly quoted string.
        # It also replaces any single quotes in the path with two single quotes (escaped).
        $inputPath = $($file.FullName).Replace("'", "''")
        $outputPath = $($outputFile).Replace("'", "''")

        $sqlCommand = "COPY (SELECT * FROM read_csv('$inputPath', delim = '`t')) TO '$outputPath';"

        # Write a message to the console to show which file is being processed.
        Write-Host "Processing $($file.Name) -> $($file.BaseName).csv..."

        # Execute the DuckDB command using the PowerShell call operator (&).
        # This is the preferred method for running external executables and avoids quoting issues.
        & duckdb -c $sqlCommand

        Write-Host "Conversion complete for $($file.Name)."
    }

    Write-Host "All files have been converted."
}
```

That's all there is to it. By combining DuckDB's file handling capabilities with a simple loop in a script, you can automate the conversion of multiple files at once. This method is fast and scalable, allowing you to handle a large number of files with a single command-line tool.

Photo by [Daniel Seßler](https://unsplash.com/@danielsessler?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/duck-standing-on-brown-rock-2qguco4_iaE?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)