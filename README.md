# Sosman1992-Reverse_Engineering_Week-4_Lab_report_Assignment-


##
After downloading the file from the website, I extracted the downloaded zip file to my Desktop and provided with a file named `keyg3nme` I run the file `keyg3nme` in my linux terminal using `file command` to ascertain the type of file; from the output it showed that `keyg3nme` is 64-Bit file. In addition also run `strings` command on this executable to see the printable strings contained within this binary and I found out some interesting  strings including but not limited: `Enter your key :`, `Good job mate, now go keygen me`, `nope`.

I then proceeded to Ghidra to see how best I can decompile and analyze this executable file. First and foremost after launching Ghidra, from the `Symbol Tree` pane I search for the actual main function which is basically the entry point of the program and double click it to launch. This will bring both the listing entity into the main function. Looking at this in the Decompiler window pane it can be seen that the decompiler try to infer the signature of the main function, nontheless the C language standard defines exactly how `main` function signature looks like, I went on to copy how the main function and right click it the function signature and hit edit function and pasted in the signature from the C standard and the the types and the names differ from `undefined8 main(void)`. The statement `iVar1 = validate_key((ulong)local_14), I deduce that local_14 is an `integer` variable that is used for holding entered key by a user. The entered key is then casted to a `long` data type with a  function named (ulong) and then passed as a function to the function called `validate_key` and then stored in a variable called `iVar1`. I then double click the function `validate_key` to have a look at the actions it performs whenever it is called , upon opening it, I saw that it has a data type of `bool` and takes an integer value as its signature namely `iParam1` equivalent of `iVar1` in main, it then return a true or false to be stored in iVar. However in C language syntax  `1` is used to represent `true` and `0` is used to represent `false` which is assigned as the value of iVar variable. The value is achieved with the statement `return (ulong) (iParm1 % 0x4c7 == 0)` in the validate_key function. Since I know that iParm1 is equivalent to entered value by the user, but   0x4c7  is  a hexadecimal number literal, hence converting it to decimal implies 1223 as its equivalence. In summary the logic that is being carried by the key_validate function is `(if number_entered % 1223 == 0)` then return 1 if the executed condition is true 0 for false and then stored in the iVar variable. The next statement in the Decompiler pane goes to compare the value in iVar against 1 and if they are equal and equivalent then it prints  (“Good job mate”)`  else it prints (`nope`). After it returns 0 to the operating system to signal for the successful operation of the program.

In conclusion  Key = 0,  or Key = 1223, or any multiple of 1223 is the password for `keyg3nme`, because they are the values that will always make the logic statement `(if number_entered % 1223 == 0)` true whenever the validate_key function is called. 


