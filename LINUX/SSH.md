# SSH

Linux has standard folders/files for SSH:

    The SSH files are stored in "~/.ssh"
    The tilde ~ is an alias for the user home folder, e.g., /home/<your username>

    The public key filename is the private key filename with .pub as the extension.

    Stored (known) server fingerprints are written to known_hosts
    This is used to detect "man in the middle" attacks. If the host fingerprint changes, SSH will report an error.

    The file authorized_keys is used to store public keys
    This is used to allow the user to maintain a collection of identity keys in one place (easier to backup and restore). The authorized_keys file is a collection of public keys, created by simply echoing out (cat) the contents of a public key, appending it to the bottom of the existing authorized_keys file.

    SSH keys must have 600 or more restrictive permissions in place
    If permissions are too open, SSH will report an error and refuse to run until you correct the security problem.


-------------------------------------------------------------------------------------------------------------------
**what-is-the-difference-between-authorized-keys-and-known-hosts-file-for-ssh**
__________________________________________________________________________________________

**Server authentication**

One of the first things that happens when the SSH connection is being established is that the server sends its public key to the client, 
and proves (thanks to public-key cryptography) to the client that it knows the associated private key. This authenticates the server: 
if this part of the protocol is successful, the client knows that the server is who it claims it is.

The client may check that the server is a known one, and not some rogue server trying to pass off as the right one. 
SSH provides only a simple mechanism to verify the server's legitimacy: it remembers servers you've already connected to, 
in the ~/.ssh/known_hosts file on the client machine (there's also a system-wide file /etc/ssh/known_hosts). 
The first time you connect to a server, you need to check by some other means that the public key presented by the server is really the public key of the server you wanted to connect to. 
If you have the public key of the server you're about to connect to, you can add it to ~/.ssh/known_hosts on the client manually.

By the way, known_hosts can contain any type of public key supported by the SSH implementation, not just DSA (also RSA and ECDSA).

Authenticating the server has to be done before you send any confidential data to it. In particular, if the user authentication involves a password, the password must not be sent to an unauthenticated server.

**User authentication**

The server only lets a remote user log in if that user can prove that they have the right to access that account.
 Depending on the server's configuration and the user's choice, the user may present one of several forms of credentials (the list below is not exhaustive).


-------------------------------------------------------------------------------------------------------------------
**what-is-the-difference-between-authorized-keys-and-known-hosts-file-for-ssh**
__________________________________________________________________________________________

**Authorized Keys**

By default SSH uses user accounts and passwords that are managed by the host OS. (Well, actually managed by PAM but that distinction probably isn't too useful here.) 
What this means is that when you attempt to connect to SSH with the username 'bob' and some password the SSH server program will ask the 
OS "I got this guy named 'bob' who's telling me his password is 'wonka'. Can I let him in?" If the answer is yes, then SSH allows you to authenticate and you go on your merry way.

In addition to passwords SSH will also let you use what's called public-key cryptography to identify you. The specific encryption algorithm can vary, but is usually RSA or DSA, 
or more recently ECDSA. In any case when you set up your keys, using the ssh-keygen program, you create two files. One that is your private key and one that is your public key.
 The names are fairly self-explanatory. By design the public key can be strewn about like dandelion seeds in the wind without compromising you. 
 The private key should always be kept in the strictest of confidence.

So what you do is place your public key in the authorized_keys file. Then when you attempt to connect to SSH with username 'bob' and your private key it will ask the 
OS "I got this guy name 'bob', can be be here?" If the answer is yes then SSH will inspect your private key and verify if the public key in the authorized_keys file is its pair. 
If both answers are yes, then you are allowed in.

**Known Hosts**

Much like how the authorized_keys file is used to authenticate users the known_hosts file is used to authenticate servers. Whenever SSH is configured on a new server it always 
generates a public and private key for the server, just like you did for your user. Every time you connect to an SSH server, it shows you its public key, together with a proof that it 
possesses the corresponding private key. If you do not have its public key, then your computer will ask for it and add it into the known_hosts file. If you have the key, and it matches, 
then you connect straight away. If the keys do not match, then you get a big nasty warning. This is where things get interesting. The 3 situations that a key mismatch typically happens are:



