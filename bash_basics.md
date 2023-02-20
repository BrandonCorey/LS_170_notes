## Basics ##
Bash is a command language used for interracting with a system enviroment
- `echo` - Can be used to output string values
  - `echo 'Hello world'`
- `read` - Reads input and stores in a specified variable. Can also pass multiple variables to read
  -  `read last_name first_name`
- `$` - used to reference a variables's value. Strings can also be interpolated using double quotes `"` with dollar sign
  - `echo $last_anme` 
  - `echo "hello $first_name"`

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
- Brackets can be omitted if strictly evaluating a boolean instead of a condition
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
```bash
#!/usr/bin/bash

number=10

if [[ $number -lt 20 ]]
then
  echo 'Number is less than 20'
elif [[ $number -gt 20 ]]
then
  echo 'Number is greater than 20!'
else
  echo 'Number is equal to 20'
fi
```

### loops ###
```bash
#!/usr/bin/bash
# while loops
counter=0
max=100

while [[ $counter -le $max ]]
do
  echo $counter
  ((counter++))
done
```
```bash
#!/usr/bin/bash
# until loops

counter=0
max=100

until [[ $counter -gt $max ]]
do
  echo $counter
  ((counter++))
done
```
```bash
#!/usr/bin/bash
# for loops
numbers='1 2 3 4 5 6 7 8 9 10'

for number in $numbers
do
  echo $number
done
```
![image](https://user-images.githubusercontent.com/93304067/220205742-cede10c1-f0ab-45c9-8401-0087318b463b.png)


### functions ###
Function definitions refer to passed arugments based on their positions
```bash
#!/usr/bin/bash

greet () {
  echo "Hi $1"
}

greet 'brandon'
```
