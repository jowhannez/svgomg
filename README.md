# svgomg
Minimize svg and print data URI. Requires node package manager (npm).

## Usage
Save the code below to a file named *svgomg*. Make sure the script location is in your $PATH

```bash
$ $PATH
bash: /c/Users/johannes/bin:/mingw64/bin:/usr/local/bin:/usr/bin:/bin:/mingw64/bin:/usr/bin
```
I put the script in my ~/bin directory.

Call the script by using the following command anywhere:
```bash
svgomg filename.svg
```

## Results
The script generates a file with the same name, in the same directory, but with the file ending *.min.svg.* It also outputs the data URI of the generated image.


## Code
```bash
#!/bin/sh

# Add .npm-global if it doesn't exist
#global_folder=$HOME"/.npm-global"
#if !(test -d $global_folder); then
#  mkdir $global_folder
#fi

# Add SVGO if it doesn't exist
echo "Checking SVGO version:"
if svgo -v ; then
  printf "\nSVGO is installed\n\n"
elif sudo; then
  printf "Installing svgo globally\n" && sudo npm install -g svgo
else
  printf "Installing svgo globally\n" && npm install -g svgo
fi

# Add datauri if it doesn't exist
echo "Checking datauri version:"
if datauri ; then
  printf "\nDatauri is installed\n"
elif sudo; then
  printf "Installing datauri globally\n" && sudo npm install -g datauri-cli
else
  printf "Installing datauri globally\n" && npm install -g datauri-cli
fi

# Optimize the svg and print datauri
old_name=$1
find=".svg"
replace=".min.svg"
new_name=${old_name//$find/$replace}
svgo $old_name -o $new_name && printf "\nData URI:\n\n" && datauri $new_name

# Message out to user
if test -f $new_name; then
  printf "\nNew file created: $new_name"
else
  printf "\nSomething went wrong. Maybe you have some spaces in the filename?"
fi
```
