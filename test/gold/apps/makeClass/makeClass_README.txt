makeClass argc: 2 [1:-th] 
makeClass -th 
#### Usage

`Usage:`    makeClass [-e|-f|-l|-n|-s|-r|-a|-v|-h] className  
`Purpose:`  Creates a C++ class based on definitions found in ./classDefinition/<className>.
             
`Where:`  

| Option | Full Command | Description |
| -------: | :------- | :------- |
|  | className | name of C++ class(es) to process |
| -e | --edit | edit <className(s)> definition file in local folder |
| -f | --filter | process only files with :filter in their names |
| -l | --list | list all definition files found in the local folder |
| -n | --namespace | surround the code with a --namespace:ns |
| -s | --silent | on error (no classDefinition file) exit silently |
| -r | --run | run the class maker on associated <className(s)> |
| -a | --all | clear, edit, list, or run all class definitions found in the local folder |
| -v | --verbose | set verbose level. Follow with a number to set level (-v0 for silent) |
| -h | --help | display this help screen |

#### Other Options

Enter `--version` to display the current version of the tool.  
Enter `--nocolors` to turn off colored display.  
Enter `--wei` (default), `--ether`, or `--dollars` to alter the way value is displayed.  

All `quickBlocks` command-line tools support the `--file:filename` option. Place valid commands, on separate lines, in a file and include the above option. In some cases, this option may significantly improve performance. Place a semi-colon at the start of a line to make it a comment.

#### Powered by QuickBlocks&reg;
