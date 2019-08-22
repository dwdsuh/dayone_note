# Bash Export Command Example

```bash
export variablename=value
export -f functionname # exports a function in the current shell
```

- export a variable or function to the environment of all the child processes running in the current shell. 

  ```bash
  #check the variable is well-exported by those commands
  env
  export -p
  ```



 