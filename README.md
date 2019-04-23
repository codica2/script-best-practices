# Best script practices

![Bash](https://cdn-images-1.medium.com/max/256/1*FEE98iWinlZBYkxBAG8MvA.png)

[**Bash**](https://www.gnu.org/software/bash/) is an ideal systems language for UNIX or command line tasks. Of course in some cases it's better to use a systems language like C or Go. But Bash can cover almost all of your problems, you just need to use it.

* **Basic Rules**

  * The principles of Clean Code apply to Bash

  * All code blocks should be commented, it it makes script easier to read and understand

  * Always double quote variables, including subshells. No naked `$` signs

     ```bash
    echo "Names without double quotes"
    echo
    names="Codica Shell Practises"
    for name in $names; do
        echo "$name"
    done
    echo

    echo "Names with double quotes"
    echo
    for name in "$names"; do
        echo "$name"
    done
    ```

* **Variables**
  
  * For variables, use names that reflect the values stored in them, but try to make it more briefly
    * Make static variables readonly

    ```bash
    # Password file variable
    readonly passwd_file='/etc/passwd'
    ```

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

  * You should preffer `printf` instead of `echo`

    * `printf` gives more control over the output, it’s more portable and its behaviour is defined better.

  * Your script should always react to abnormal input

  ```bash
  func(){
      echo -e "Unexpected argument!"
  }
  ```

  * It should always return 0 on success or different value in other case, it would be great if you can handle those

  * If your script require user input, use prompts
  
    ```bash
    printf "Input email: "
    read email
    printf "$email"
    ```

    ![prompt](https://media.giphy.com/media/S9Ps0mDRJhTbT9hYxw/giphy.gif)

* **Security**
  
  * Don't store your credentials in shell scripts, people can have access to your script. Use `ENV` variables or [vaults](https://www.vaultproject.io/) instead.

  * You should always remember that your script can be executed on other machines or in container, so you need to use environment variables i.e.
  
  ```bash
  # Copying from remote machine with ENV path
  scp 192.168.0.1:$HOME $HOME
  ```

  * Instead of

  ```bash
  # Copying from remote machine with with absolute path
  scp 192.168.0.1:/home/myuser /home/otheruser
  ```

  * Because there may not be those directories you want to copy

  * Sometimes script will be executed even when a command fails, it will affect the rest of the script. Use `set -o errexit` exit a script when a command fails
    * `nounset` flag to exit when your script tries to use undeclared variables
    * `xtrace` to trace what gets executed. Useful for debug

  ```bash
   #let script exit if a command fails
   set -o errexit
   set -o nounset
   set -o xtrace
  ```

* **Useful Tips**
  
  * Prefer absolute paths `/home/user/file` instead of relative `~/file`

  * Use `.sh` or `.bash` extension if file is meant to be included/sourced. Never on executable script.

  * Never use deprecated style.

    * Define functions as `func() { }`

    * Always use `[[...]]` instead of `[]`

    * Never use backticks, use `$( ... )`

## License

ubuntu-laptop-script is Copyright © 2015-2019 Codica. It is released under the [MIT License](https://opensource.org/licenses/MIT).

## About Codica

[![Codica logo](https://www.codica.com/assets/images/logo/logo.svg)](https://www.codica.com)

We love open source software! See [our other projects](https://github.com/codica2) or [hire us](https://www.codica.com/) to design, develop, and grow your product.