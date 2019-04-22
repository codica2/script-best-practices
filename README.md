# Best script practices

![Bash](https://cdn-images-1.medium.com/max/256/1*FEE98iWinlZBYkxBAG8MvA.png)

**Bash** is an ideal systems language for UNIX or command line tasks. It can cover almost all of your problems, you just need to know to use use it.

* **Basic Rules**

  * The principles of Clean Code apply to Bash

  * All code blocks should be commented, it it makes script easier to read and understand

  ```bash
  # Function description
  func(){
      ....
  }
  ```

  * Always double quote variables, including subshells. No naked `$` signs

* **Variables**
  
  * For variables, use names that reflect the values stored in them, but try to make it more briefly

  * Always use local when setting variables, unless there is reason to use declare

    * Exception being rare cases when you are intentionally setting a variable in an outer scope.

  ```bash
  func(){
      cd $PWD
  }
  ```

* **Functions**

  * Declare variables with a meaningful name for positional parameters of functions

  * Use UNIX-like approach: a function does one thing.

  ```bash
  # Function description
  func(){
      printf("Hello world!")
  }
  ```

* **I/O**

  * Your script should always react to abnormal input

  ```bash
  func(){
      echo -e "Unexpected argument!"
  }
  ```

  * It should always return 0 on success or different value in other case, it would be great if you can handle those

* **Security**
  
  * Don't store your credentials in shell scripts, people can have access to your script. Use `ENV` variables or vaults instead.

* **Useful Tips**
  
  * Prefer absolute paths `/home/user/file` instead of relative `~/file`

  * Use `.sh` or `.bash` extension if file is meant to be included/sourced. Never on executable script.

  * Never use deprecated style.

    * Define functions as `func() { }`

    * Always use `[[...]]` instead of `[]`

    * Never use backticks, use `$( ... )`

  * You should always remember that your script can be executed on other machines or in container, so you need to use environment variables i.e.
  `cd $HOME` instead of `cd /home/user`