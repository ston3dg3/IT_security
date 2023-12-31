# This is a file with help instructions on gpg signing

### important note:
make sure that your timezone is properly set and your current time
corresponds to the one displayed on your system. Otherwise, gpg keys
will return warnings

1. Generate a new gpg key pair (public and private)
```gpg --gen-key```
As username put any identifier, same for e-mail
You can view if the key was correctly generated by typing
```gpg --list-keys```
In the uid line, you can read the user ID and their e-mail.
The level of trust is displayed in square brackets

2. Export the public key to a separate file
From the list keys command, you can read the key fingerprint
that looks like: C8033056371AAAAA69CD34A64F6B8DDDDD9B838D
Copy it and use it to export the public key to a file public_key.asc
You can name the file whatever you want.
```gpg --armor --export <key_fingerprint> > <public_key.asc>```

3. Send the file through E-mail with the public key as an attachment
You can change the settings in your E-mail service provider app so that
your E-mails are always signed, encrypted, or both with a gpg key.
For that you will need to insert the fingerprint of the key into the E-mail app.

4. Let your friend save the file on their computer locally and add it to their keyring using
```gpg --import <public_key.asc>```
They can check if the key was succesfully added using
```gpg --list-keys```

5. Tell your friend to sign the key they received with their signature using
```gpg --sign-key --ask-cert-level <key_fingerprint>```
And set the minimum cert level to 2 if you are 100% sure the message came from
your friend.

6. Now your friend should export the public key again and send it back after they signed with
their private key. They can export the key with:
```gpg --armor --export <key_fingerprint> > <public_key.asc>```
Then they can send the key back to you by attaching the file to an E-Mail

7. You will need to update your local gpg key with the newly signed key from your friend by integrating it into your keyring. For that, use the gpg insert statement with the user ID that you specified in Step 1:
```gpg -insert <user-id>```

8. Now you can check if the person who signed the key was really your friend
by typing:
```gpg --list-sigs <user-id>```
If the key lists 2 signatures, one from you, and one from your friend, then you did everything correctly and should have a working gpg key which is trusted by 2 people.



### Collection of useful gpg commands

- sign a gpg key with
```gpg --sign-key <keyid>```

- list all users who signed a particular public key.
User-id can be anything that appears in the uid row
when the command gpg --list-keys is used
```gpg --list-sigs <user-id>```

- list all the public keys in the keyring
```gpg --list-keys```

- list all the private keys in keyring
```gpg --list-secret-keys```

- export the public key to a separate file
```gpg --armor --export <key_fingerprint> > <public_key.asc>```

- edit an already generated gpg key
```gpg edit <key_fingerprint>```
this will open a gpg console which accepts different commands. 
`trust` will allow you to change the trust level of the chosen key
