# su-dung-sed-awk
Sử dụng SED, AWK
# vim animals.txt
dog is animals
doOr
my cat is in fighting with the dog
^  :matches the character(s) at the beginning of a line

# sed -ne '/^dog/p' animals.txt
dog is animals
$  :matches the character(s) at the end of a line

# sed -ne '/dog$/p' animals.txt
my cat is in fighting with the dog
Tasks: Match line which contains only 'dog'

# sed -ne '/^dog$/p' animals.txt
# sed -ne '/^dog$/p'    ;- reads from STDIN, Press Enter after each line, Terminate with CTRL-D
# cat animals.txt | sed -ne '/^dog$/p' 
# cat animals.txt | sed -ne '/^dog$/Ip'   ;- Prints matches case-insensitively
.   :matches any character (typically except new line)

# sed -ne '/^d...$/Ip' animals.txt
doOr
# sed -ne '/^d.../Ip' animals.txt    ;- Prints matches case-insensitively
dog is animals
doOr
2. Regex quantifiers

*   :0 or more matches of the previous character

# sed -ne '/^do*/Ip' animals.txt  ;Started by 'd' and '0' or more "o" character
dog is animals
doOr
duck is very small
+  :1 or more matches of the previous character

# sed -ne '/^d.\+/Ip' animals.txt  ;RegExes using the escape character '\'
dog is animals
doOr
?  :0 or 1 of the previous character

3. Characters classes

Allow to search for a range of characters
 a. [0-9]
 b. [a-z][A-Z]

# sed -ne '/^d.\+[0-9]/Ip' animals.txt
di9 girl
4. Print Specific Lines of a file

;Note: '-e' is optional if there is only 1 instruction to execute
# sed -ne '1p' animals.txt - prints first line of file
# sed -ne '2p' animals.txt - prints second line of file
# sed -ne '$p' animals.txt - prints last printable line of file
# sed -ne '2,4p' animals.txt  ;- prints lines 2-4 from file
# sed -ne '1!p' animals.txt  ;- prints ALL EXCEPT line #1
# sed -ne '1,4!p' animals.txt  ;- prints ALL EXCEPT line 1 - 4
# sed -ne '/dog/p' animals.txt ;- prints ALL line scontaining 'dog' - case-sensitive
# sed -ne '/dog/Ip' animals.txt  ;- prints ALL line scontaining 'dog' - case-insensitive
# sed -ne '/[0-9]/p' animals.txt  ;- prints ALL lines with AT LEAST 1 numeric
# sed -ne '/cat/,/deer/p' animals.txt  ;- prints ALL lines beginning with 'cat', ending with 'deer'
# sed -ne '/deer/,+2p' animals.txt  ;- prints the line with 'deer' plus 2 extra lines
5. Delete Lines using Sed Addresses

# sed -e '/^$/d' animals.txt   ;- deletes blank lines from file
# sed -e '1d' animals.txt   ;- deletes the first line form animals.txt
# sed -e '1,4d' animals.txt    ;- deletes lines 1-4 form animals.txt
# sed -e '1~2d' animals.txt    ;- deletes every 2nd line beginning with line 2 => 1, 3, 5...
6. Saves Sed's Changes using Output Redirection

# sed -e '/^$/d' animals.txt > animals2.txt   ;- deletes blank lines from file and creates new output file 'animals2.txt
7. Search & Replace using Sed

# sed -e 's/find/replace/g' animals.txt   ;- replaces 'find' with 'replace'
;Note: Left Hand Side (LHS) supports literals and RegExes
;Note: Right Hand Side (RHS) supports literals and back references
- Ví dụ

# sed -e 's/LinuxCBT/UnixCBT/'    ;- replaces 'LinuxCBT' with 'UnixCBT' on STDIN to STDOUT
# sed -e 's/LinuxCBT/UnixCBT/I'    ;- replaces 'LinuxCBT' with 'UnixCBT' on STDIN to STDOUT (Case-Insensitives)
Note: Replacements occur on the FIRST match, unless 'g' is appended to the s/find/replace/g sequence

# sed -e 's/LinuxCBT/UnixCBT/Ig'   ;- replaces 'LinuxCBT' with 'UnixCBT' on STDIN to STDOUT (Case-Insensitives)
- Task

Remove ALL blank lines
Substitute 'cat', regardless of case, with 'Tiger'
Note: Whenever using '-n' option, you MUST specify the print modifier 'p'

$ sed -ne '/^$/d' -e 's/cat/Tiger/Ig' animals.txt   ;removes blank lines & substitutes 'cat' with 'Tiger'
$ sed -e '/^$/d; s/cat/Tiger/Igp' animals.txt   ;does the same as above, simply separate multiple commands with semicolons
8. Update Source File - Backup Source File

# sed -i.bak -e '/^$/d; s/Cat/Tiger/Igp' animals.txt  ;performs as above, but ALSO replaces the source file and backs it up
9. Search & Replace (Text Substitution) Continued

# sed -e '/address/s/find/replace/g/' file
# sed -e '/Tiger/s/dog/mutt/g' animals.txt  ;substitutes 'dog' with 'mutt' where line contains 'Tiger' and print all
# sed -ne '/Tiger/s/dog/mutt/gp' animals.txt  ;substitutes 'dog' with 'mutt' where line contains 'Tiger' and print only line 'Tiger'
# sed -e '/Tiger/s/dog/mutt/gI' animals.txt   ;Updates lines that with 'Tiger', replace dog with mutt
# sed -e '/^Tiger/s/dog/mutt/gI' animals.txt  ;Updates lines that begin with 'Tiger' , replace dog with mutt
# sed -e '/^Tiger/Is/dog/mutt/gI' animals.txt  ;Updates lines that begin with 'Tiger' (Case-Insensitive)
- Note: & = The full value of the Left Hand Side (Pattern Matched) OR the values in the pattern space

- Task: Intersperse each line with the word 'Animal '

# sed -ne 's/.*/&/p' animals.txt  ;replace the matched pattern with the matched pattern
# sed -ne 's/.*/Animal &/p' animals.txt  ;Thêm 'Animal' on each line
# sed -ne 's/.*/Animal: &/p' animals.txt   ;Thêm 'Animal:' on each line
# sed -ne 's/.*[0-9]$/&/p' animals.txt  ;returns animals with at least 1 numeric at the end of line
duck is very small1234
# sed -ne 's/.*[0-9]\{1\}/&/p' animals.txt  ;returns animals with  at least 1 numeric of the line
duck is very small1234
d95s girl
# sed -ne 's/[a-z][0-9]\{4\}$/&/pI' animals.txt  ;returns animal(s) with 4 numeric values at the end of the line
duck is very small1234
# sed -ne 's/[a-z][0-9]\{1,4\}$/&/pI' animals.txt  ;returns animal(s) with at leaset 1, up to 4 numeric values at the end of the line
duck is very small1234
Rat is small123
10. Grouping & Backreferences

- Note: Segement matches into back references using escaped parenthesis: \(RegEx\)

# sed -ne 's/\(.*\)\([0-9]\)$/&/p' animals.txt  ;This creates 2 variables: \1 & \2
duck is very small1234
Rat is small123
# sed -ne 's/\(.*\)\([0-9]\)$/\1/p' animals.txt  ;This creates 2 variables: \1 & \2 but references \1
duck is very small123
Rat is small12
# sed -ne 's/\(.*\)\([0-9]\)$/\2/p' animals.txt  ;This creates 2 variables: \1 & \2 but references \2
4
3
# sed -ne 's/\(.*\)\([0-9]\)$/\1 \2/p' animals.txt  ;This creates 2 variables: \1 & \2 but references \1 and \2
duck is very small123 4
Rat is small12 3
11. Apply Changes to Multiple Files

- Note:  Sed Supports Globbing: *, ?

# sed -ne 's/\(.*\)\([0-9]\)$/\1 \2/p' animals*.txt  ;This creates 2 variables: \1 & \2 but references \1 and \2
11. Sed Scripts

- Note: Sed supports scripting, which means, the ability to dump 1 or more instructions into 1 file

- Sed Scripting Rules:

 Sed applies ALL rules to each line
 Sed applies ALL changes dynamically to the pattern space
 Sed ALWAYS works with the current line
# sed -f script_file_name text_file

# vim animals.sed    ;Bỏ phần comment khi chạy 
/^$/d  ;removes blank lines
s/dog/frog/Ig  ;substitute globally 'dog' with 'frog' - (case-insensitive)
s/tiger/lion/Ig  ;substitute globally 'tiger' with 'lion' - (case-insensitive)
s/.*/Animals: &/  ;Interspersed 'Animals:'
s/animals/mammals/iG  ;replaced 'Animals' with mammals'
s/\([a-z]*\)\([0-9]*\)/\1/Ip  ;Strips trailing numeric values from alphas

# sed -f animals.sed animals.txt
