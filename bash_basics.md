## Basics ##
Bash is a command language used for interracting with a system enviroment
- `echo` - Can be used to output string values
  - `echo 'Hello world'`
- `read` - Reads input and stores in a specified variable. Can also pass multiple variables to read
  -  `read last_name first_name`
- `$` - used to reference a variables's value
  - `echo $last_anme` 

### Files ###
Use the `.sh` extension to denote bash files
- Can put basic commands inside them e.g `echo hello world!`

**Execute** with bash command followed by file name as argument `bash <bashfile>`

**Alernative Execution**

Add `#!/usr/bin/bash` to first line of file
```bash
#!/usr/bin/bash

echo 'Hello world!'
```
- `#!` - shebang is used to say we want to use the program at the following file path to run the code in the bash file
  - `/usr/bin/bash` is where `bash` is installed on my flavor of linux
- `chmod +x <bashfile>` - Used to add executable permissions to the bash file so that we can run it
- `./` - Used to run a file with executable permissions

So all together, we can do
```bash
chmod +x hello_world.sh
./hello_world.sh # 'Hello world!'
```
### Conditionals ###
We can use `if` and `case` statements in Bash to work with conditional logic. Syntax:
```
if [[ <condition> ]]
then
  <command>
fi
```
```bash
if true
then
  echo 'True'
fi
```
