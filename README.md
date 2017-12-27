# directory-crypt
A bash script used to easily mount/unmount/create ecryptfs encrypted directories.

## Installation
1. Copy the file `./bash_scripts/d-crypt` to a directory in your path (e.g. `/usr/local/bin/`).
1. Make sure the file `d-crypt` is executable (e.g. `sudo chmod +x /usr/local/bin/d-crypt`).
1. Install ecryptfs-utils (e.g. `sudo apt install ecryptfs-utils`).
1. Create your password file (e.g. `touch ~/.ssh/mypassword.txt`).
1. Add your password to your password file in this format: `passphrase_passwd=<your_long_difficult_password>`
1. Encrypt your password file: `openssl enc -aes-256-cbc -in mypassword.txt -out mypassword.txt.enc`
1. In the d-crypt script, point UNLOCK_KEY to your password file: `UNLOCK_KEY="/home/${USER}/.ssh/mypassword.txt"`

That's it, d-crypt should now work from your terminal.

## Usage

### Create a new Encrypted Directory
The following example creates a new secure directory called "Secure" in the /tmp directory:
```
$ d-crypt -m /tmp/Secure
```
You will be prompted for the OpenSSL password to unlock your password file, and your sudo password. If successful, this will create `/tmp/Secure` - an ecryptfs mount. Any files you create/copy in to `/tmp/Secure` will only be accessible when `/tmp/.Secure` is mounted (see below on mounting existing encrypted directories).

### Mount an Encrypted Directory
If you have an existing unmounted encrypted directory, you can mount it using:
```
$ d-crypt -m /tmp/Secure
```
This will make any files you stored in /tmp/Secure accessible.

### Unmount an Encrypted Directory
Encrypted directories will automatically be unmounted when you log out. However, it's always a good idea to unmount them manually. Any files sotred within an unmounted encrypted directory will be unreadbale. To manually unmount an encrypted directory:
```
$ d-crypt -u /tmp/Secure
```

### View the Status of a Directory
You can see if a directory is mounted or not using:
```
$ d-crypt -s /tmp/Secure
```

## Notes
- Directories are created in pairs: <.DirectoryName> and <DirectoryName> (one hidden).
- The hidden directory is the ecryptfs directory (i.e. encrypted directory).
- You cannot create a nested ecryptfs directory (for example, in an encrypted home directory).
- For better security, it's a good idea to keep your password file on a USB pen drive.
- This script is a very rough prototype (helper script).
- I will be writing a C++ version in the not too distant future.
