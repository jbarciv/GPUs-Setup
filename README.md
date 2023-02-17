# new-setup
Commonly used instruction to setting up again your "workstation"

## Login in the server
For avoiding the need of writting the password every time you need to login in the server you can use this [link](https://urldefense.com/v3/__https://www.thegeekstuff.com/2008/11/3-steps-to-perform-ssh-login-without-password-using-ssh-keygen-ssh-copy-id/__;!!KwNVnqRv!A_x86MYnut7a45t1fPXr1g2zn_sYDNoHeCdK5_ysde-LDEh9dtsR7aG_QtV1xpQLKKOdqENFuXXv4tv5lTwVETFGEIA$)

## Creation of virtual environment -> Python development
First be sure that *python3* is already installed. If not, you can install it this way: `sudo apt install python3`.

Then we are going to install these packages:
`sudo apt install python3-pip`,
`sudo apt-get install python3-venv` or `sudo pip install virtualenv` if *pip* does not work use this:
`python3 -m pip install <paquete>`. Where paquete is *virtualenv*.

Finally, we can create the virtual environment:

`python3 -m venv ~/.python_venv`

feel free to change *the path* for something more suitable for you.
