# Yubikey-4-Series-Smarcard-OpenPGP
Using OpenPGP in a linux environment. Creating a key with write permissons and key for signing key's. Key's will have a RSA 2048 Encryption Key and Master key wiht backup and expiration date of a year. 

Reference  Yubico's Developer

Note: If you haven't set a User PIN or an Admin PIN for OpenPGP, the default values are 123456 and 12345678, respectively. If the User PIN and/or Admin PIN have been changed and are not known, the OpenPGP Applet can be reset by following this article.

These instructions will show you how to set up your YubiKey with OpenPGP. Before you begin, decide if you want to generate the private key on the YubiKey device, or if you want to generate the private key off of the YubiKey and then move the subkeys to the YubiKey. To allow for your PGP keys to be backed up, we recommend you generate them externally, not directly on the YubiKey. Once keys have been moved to/generated on the device, we also recommend that you personalize the YubiKey by changing the PIN, setting the admin PIN, and so on. Changing the PINs can be done by running the command gpg --change-pin.

Requirements
A compatible YubiKey.
A current version of the GnuPG software installed.
Windows: GPG4Win
macOS: GPG Tools
Linux: Pre-installed on all common distributions.
 
Generating Keys externally from the YubiKey (Recommended)
Note: It is strongly recommended that you to generate keys on an offline system, such as a live Linux distribution like Ubuntu. Note that with live Linux, certain packages (like scdaemon) may need to be installed manually.

Insert the YubiKey into the USB port if it is not already plugged in.
Enter the GPG command: 

<gpg --expert --full-gen-key>

## When prompted to specify the key type, enter 1 (for "RSA and RSA (Default)") and press Enter.
Specify the size of key you want to generate. Do one of the following:
## For a YubiKey NEO, enter 2048 and press Enter.
The Series 4 only supports up tp 2048
## For a YubiKey 4 or 5, enter 4096 and press Enter.
Specify the expiration date of the key, and press Enter. Verify the expiration date when prompted.
Now you will enter your user information. Enter your Real Name and press Enter. Be sure to enter both your first and last name.
Enter your Email Address and press Enter.
If desired, enter a Comment about this key, and press Enter. (To leave the comment blank, just press Enter.)
Review the information you entered, make any changes if necessary. If all information is correct, enter O (for Okay) and press Enter.
A dialog box is displayed so you can enter the passphrase for your key. While the key is being generated, move your mouse around or type on the keyboard to gain enough entrophy. When the key has been generated, you will see several messages displayed. Make a note of the key ID, that is displayed in the message such as "gpg: key 1234ABC marked as ultimately trusted". The key ID in this case is 1234ABC and you will need this key ID to perform other operations.
To add an authentication key:

Note: Recent release of GnuPG may have the default allowed actions to be both sign and encrypt. Please be sure to check the default allowed action before proceeding with adding the authentication key. 

Insert the YubiKey into the USB port if it is not already plugged in.
## Enter the GPG command: gpg --expert --edit-key 1234ABC (where 1234ABC is the key ID of your key)
## Enter the command: addkey
## Enter the passphrase for the key. Note that this is the passphrase, and not the PIN or admin PIN.
You are prompted to specify the type of key. Enter 8 for RSA.
Initial default will be Sign and Encrypt. To select authentication key toggle S to disable sign, E to disable encrypt, A to enable authentication.
Once you can confirm that authentication is the current allowed actions select Q to Finish the selection.
Specify the key size.
Specify the expiration of the authentication key (this should be the same expiration as the key).
When prompted to save your changes, enter y (yes).
To add a signing key:

Note: Recent release of GnuPG may have the default allowed actions to be both sign and encrypt. Please be sure to check the default allowed action before proceeding with adding the signing key. 

Enter the GPG command: gpg 
<--expert --edit-key 1234ABC (where 1234ABC is the key ID of your key) if you are not in edit mode already.>
Enter the command: 
<addkey>
Enter the passphrase for the key. Note that this is the passphrase, and not the PIN or admin PIN.
You are prompted to specify the type of key. Enter 8 for RSA.
Initial default will be Sign and Encrypt. You can either select E to also toggle Encryption as an allowed actions or continue with sign being allowed for the subkey. 
Once you can confirm that authentication is the current allowed actions select Q to Finish the selection.
Specify the key size.
Specify the expiration of the authentication key (this should be the same expiration as the key).
When prompted to save your changes, enter y (yes).
To create a backup of your key:

Insert the YubiKey into the USB port if it is not already plugged in.
Enter the GPG command: 
<gpg --export-secret-key --armor> 1234ABC (where 1234ABC is the key ID of your key)
Store the text output from the command in a safe place ( e.g. Print the text, save the text in password managers, save the text on a USB storage device).
To import the key on your YubiKey:

Insert the YubiKey into the USB port if it is not already plugged in.
Enter the GPG command: 
<gpg --edit-key 1234ABC (where 1234ABC is the key ID of your key)>
Enter the command: 
<keytocard>
When prompted if you really want to move your primary key, enter y (yes).
When prompted where to store the key, select 1. This will move the signature subkey to the PGP signature slot of the YubiKey.
Enter the command: 
 <key 1>
Enter the command: 
  <keytocard>
When prompted where to store the key, select 2. This will move the encryption subkey to the YubiKey.
Enter the command: 
   <key 1>
Enter the command: 
    <key 2>
Enter the command: 
     <keytocard>
When prompted where to store the key, select 3. This will move the authentication subkey to the YubiKey.
Enter the command: 
      <quit>
When prompted to save your changes, enter y (yes). You have now saved your keyring to your YubiKey.
Generating Your PGP Key directly on Your YubiKey
Warning: Generating the PGP on the YubiKey ensures that malware can never steal your PGP private key, but it means that the key can not be backed up so if your YubiKey is lost or damaged the PGP key is irrecoverable. 

Insert the YubiKey into the USB port if it is not already plugged in.
Open Command Prompt (Windows) or Terminal (macOS / Linux).
Enter the GPG command: 
<gpg --card-edit>
At the gpg/card> prompt, enter the command: admin
If you want to use keys larger than 2048 bits, run: key-attr
Enter the command: generate
When prompted, specify if you want to make an off-card backup of your encryption key. 
Note: This is a shim backup of the private key, not a full backup, and cannot be used to restore to a new YubiKey.
Specify how long the key should be valid for (specify the number in days, weeks, months, or years).

Confirm the expiration day.
When prompted, enter your name.
Enter your email address.
If needed, enter a comment.
Review the name and email, and accept or make changes.
Enter the default admin PIN again. The green light on the YubiKey will flash while the keys are being written.

Using your YubiKey's OpenPGP function on multiple computers
In order to use the OpenPGP private keys stored on your YubiKey on computers apart from the one where they were generated, it is necessary to import the corresponding public keys. There are a number of ways to accomplish this; please see this guide for more information.

 

Further Reading
For more advanced usage of the YubiKey's OpenPGP application with GPG, please refer to https://github.com/drduh/YubiKey-Guide.






Erin. 2020, May 12. Using Your Yubikey with OpenPGP
https://support.yubico.com/hc/en-us/articles/360013790259-Using-Your-YubiKey-with-OpenPGP
