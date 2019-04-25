# Best script practices

![Bash](https://cdn-images-1.medium.com/max/256/1*FEE98iWinlZBYkxBAG8MvA.png)

[**Bash**](https://www.gnu.org/software/bash/) is an ideal systems language for UNIX or command line tasks. Of course, in some cases it's better to use systems languages like C or Go. However, Bash can solve almost all of your problems, you just need to use it.

## Basic Rules

* It’s recommended to apply Clean Code principles while working with Bash.
  
* All code blocks should be commented. Thus, it makes the script easier to read and understand.

  * Always double quote variables, including subshells. No naked `$` signs.

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

## Variables

* Variables is used to store your values. You should operate them carefully, because sometimes Bash sometimes can use undeclared variables.
  
* Concerning variables, use names that reflect the values stored in them, but try to make it more briefly.

* Make static variables read-only.

    ```bash
    # Password file variable
    readonly passwd_file='/etc/passwd'
    ```

  * Always use local when setting variables, unless there is reason to use declare.

    * An exception to this are cases when you are intentionally setting a variable in an outer scope.

    ```bash
    func(){
        cd $PWD
    }
     ```

## Functions

* Functions are created to reduce the amount of code. Try to write brief functions, that do only one task if it is possible.

* Declare variables with a meaningful name for positional parameters of functions.

  * Use UNIX-like approach: a function does one thing.

   ```bash
    # Function description
   func(){
       printf("Hello world!")
   }
   ```

## I/O

* Your script will sometimes require input from user. Try to make a clear message of your script requirements and don't forget to handle all correctly.

* You should prefer `printf` to `echo`.

  * `printf` gives more control over the output, it’s more portable and its behaviour is defined better.

  * Your script should always react to abnormal input.

  ```bash
  func(){
      echo -e "Unexpected argument!"
  }
  ```

  * It should always return 0 in case of a success and another value otherwise. It would be great if you can handle those.

  * If your script requires user input, use prompts.
  
    ```bash
    printf "Input email: "
    read email
    printf "$email"
    ```

    ![prompt](https://media.giphy.com/media/S9Ps0mDRJhTbT9hYxw/giphy.gif)

## Security

* Security is very important thing. When writing a script be prepared for that your script will be executed by a less experienced person, so you should make your script fool-proof.
  
* Don't store your credentials in shell scripts, people can have access to your script. Use `ENV` variables or [vaults](https://www.vaultproject.io/) instead.

  * You should always remember that your script can be executed on other machines or in container, so you need to use environment variables i.e.
  
  ```bash
  # Copying from remote machine with ENV path
  scp 192.168.0.1:$HOME $HOME
  ```

  * Instead of

  ```bash
  # Copying from remote machine with an absolute path
  scp 192.168.0.1:/home/myuser /home/otheruser
  ```

  * The matter is that there may not be those directories you want to copy.

  * Sometimes the script will be executed even when a command fails. Thus, will affect the rest of the script. Use `set -o errexit` exit a script when a command fails.

    * `nounset` flag to exit when your script tries to use undeclared variables.
    * `xtrace` helps to trace what gets executed (useful for debugging)

  ```bash
   #let the script exit if a command fails
   set -o errexit
   set -o nounset
   set -o xtrace
  ```

## Useful Tips
  
* You should prefer absolute paths `/home/user/file` instead to relative `~/file`.

  * Use `.sh` or `.bash` extension if the file is meant to be included/sourced. Don’t use these extensions on executable script.

  * Do not use deprecated style.

    * Define functions as `func() { }`.

    * Always use `[[...]]` instead of `[]`.

    * Do not use backticks, use `$( ... )`.

## License

Best script practices is Copyright © 2015-2019 Codica. It is released under the [MIT License](https://opensource.org/licenses/MIT).

## About Codica

[![Codica logo](https://www.codica.com/assets/images/logo/logo.svg)](https://www.codica.com)

We love open source software! See [our other projects](https://github.com/codica2) or [hire us](https://www.codica.com/) to design, develop, and grow your product.