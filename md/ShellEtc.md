#Lecture Template



-
-
#First Slide

[4em](https://www.w3schools.com/CSSref/css_units.asp) [checksum](https://en.wikipedia.org/wiki/Checksum) [Clojure](https://clojure.org/) [gwt](http://www.gwtproject.org/) [emmet](http://emmet.io/)
_



pwd - "Print Working Directory" prints out your current location (known as your current working directory or 'cwd')
ls - lists contents of the cwd
cd - change directory. This is how you move around the file system. You can specify the destination as an absolute or relative path.
echo - prints text to the terminal
cat - concatenates zero or more files. Often used to print the contents of a file to the terminal. Often misused
less/more - Display contents of a file one page at a time ...just remember that less is more (more or less) and you'll be fine.
grep - search for the specified text or pattern
Other commands worth knowing
head - display the first lines of a file
tail - display the last lines of a file



# perform the same initial search, but filter out the lines containing "baz"
grep "^foo.*bar$" file.txt | grep -v "baz"

# if you literally want to search for the string,
# and not the regex, use fgrep (or grep -F)
fgrep "foobar" file.txt


W
_

```


-
# New sub-slide using `-\n`

Some content  
`backticks.surround("sample code");`


-
-
# New slide using `-\n-\n`

More content and examples coming soon...

```Java
for(Larger code : samples){
  use ? triple : backticks;
}
```
