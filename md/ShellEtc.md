#Shell, etc.



-
-
#Useful Commands
<p class="fragment fade-up">**w** -</p>
<p class="fragment fade-up">**pwd** - "Print Working Directory" prints out your current location (known as your current working directory or 'cwd')</p>
<p class="fragment fade-up">**ls** - lists contents of the cwd</p>
<p class="fragment fade-up">**cd** - change directory. This is how you move around the file system. You can specify the destination as an absolute or relative path.</p>
<p class="fragment fade-up">**echo** - prints text to the terminal</p>
<p class="fragment fade-up">**cat** - concatenates zero or more files. Often used to print the contents of a file to the terminal. Often misused</p>


_
_
#Useful Commands (contin.)
<p class="fragment fade-up">**less/more** - Display contents of a file one page at a time ...just remember that less is more (more or less) and you'll be fine.</p>
<p class="fragment fade-up">**grep** - search for the specified text or pattern</p>
<p class="fragment fade-up">**touch**</p>
<p class="fragment fade-up">**mkdir**</p>
<p class="fragment fade-up">**rmdir**</p>
<p class="fragment fade-up">**rm**</p>
<p class="fragment fade-up">**cp**</p>
<p class="fragment fade-up">**mv**</p>

-
-
# Other commands worth knowing
<p class="fragment fade-up">head - display the first lines of a file</p>
<p class="fragment fade-up">tail - display the last lines of a file</p>

# perform the same initial search, but filter out the lines containing "baz"
grep "^foo.*bar$" file.txt | grep -v "baz"

# if you literally want to search for the string,
# and not the regex, use fgrep (or grep -F)
fgrep "foobar" file.txt


W
_
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
