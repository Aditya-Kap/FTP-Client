# 4700 FTP

This is an ftp client that is written in Python 3, which implements that FTP protocol directly.
The 4700ftp file contains the doccumented source code for the client wit comments doccumenting the code above every function starting with a '#'. The Code dose not use any inbuild libraries,modules,etc that implement FTP.

## High level approach:

This program handles the apprach in three layers:

1.  **Command Line**: this part is responsible for the parsing of the operations and  it's working parameters. It checks if the correct number and kind of arguments were given (for example URL vs Local path). It also fills default such as usesr, ports etc.

2.  **Session Setup**: This parts handles the session setup procedures such as opening the channels, reading the server's 'hello' message etc. It logs in with username and password provided (default if not provided) and sets the pre operations procedures.

3. **OPerations**: This is the part whiich handles actual operations such as making a folder, editing/deleating a file. The client sends one coommand on the main connection and checks for the reply. Listing a folder, downloading and uploading require a second connection for the actual data. For each of these the client follows the basic steps:
    - asking the server for data connection.
    - Reading the ip adresses/port from the server's reply and connecting to it
    - sending a command and waiting for the server to be ready
    - sending/recieving the data and closing the data connection
    - waits for the server to confirm that the transfer is finished.

Every seassion ends with quit and errors that are encountered are printed out.

## Challanges:

- Getting the right order of operations on the data channel because one out of order operation can crash the system. For Example if we don't wait for the hello message from the server and immidetly start our operations not knowing that the server is not connected.
- Handeling Multiple line replies.
- Making sure that the client closes a data connection after uploading/downloading so that the server knows the file is complete.
- While testing verbose mode found a bug where directory mode was crashing the program.


## Testing:

Tested on the khoury linux machine provided. these are the following tests ran:

- **Login and Listning**: ran 'ls' with '--verbose' to see the messages between the server and client, checking if the client reads the 'hello' message first and performs the tasks in the right order as menctioned in the session setup part of the approach.
- **Downloading**: Downloaded a .png and cross checked it's size to confirm the binary mode works.
- **Upload and Moving**: Made a test folder, moved the image to it and checked if it arrived in full.
- **Deleating** Used 'rm' to delete the file and 'rmdr' to remove the test folder to return the folder to it's original state.
- **Errors**: asked for a file which did not exist, and as expected an error was returned.


## Refrences: 

- Basic information on the FTP Client coding:
        https://docs.python.org/3/library/ftplib.html
- Official FTP commands:
        https://www.rfc-editor.org/info/rfc959/
        https://en.wikipedia.org/wiki/List_of_FTP_commands
- FTP Server return Codes:
        https://en.wikipedia.org/wiki/List_of_FTP_server_return_codes
        
