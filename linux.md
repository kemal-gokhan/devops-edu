# Linux Fundemental Commands

man (manual)
--help (command option to find out options or help)

"This is my personal study while learning Linux environment. 
I'm gonna share my experience for other people and to follow my notes."


file called always abckg.txt (you should change if you want to use it)

# Fundamental knowledge
Input - Output - Error
0		1			2

**yourinput > addedyouroutput**
**yourinput >> addedyouroutput** (appended)
ls >  abckg.txt | cp abckg.txt abckgnew.txt && rm abckg.txt
'>' redirects output to a file, overwriting the file.
'>>' redirects output to a file appending the redirected output at the end.

### Piping:
Command-1 | Command-2 | …| Command-N
find *.txt | less (common usage with less in order to read easily.)

pwd (show working directory)
ls (show everything)
cd (move directory)	
which cal
mkdir (make directory - WHAT)
rmdir (remove directory - WHAT )
touch (create file - WHAT )
rm (remove files or directory - WHAT)
mv (move or rename - WHAT WHERE)
cp (copy)
> (redirect)
wc (count)
	ls | wc -w
| (pipe)
find (find files in directory)
history (show command history)
ps (report current process)
top (windows task manager :) )
kill (terminate to process)
su (command with chosen user - WHO) #su kemal 
