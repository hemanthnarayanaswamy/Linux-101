# The `passwd` command

In Linux, `passwd` command changes the password of user accounts. A normal user may only change the password for their own account, but a superuser may change the password for any account.
`passwd` also changes the account or associated password validity period.

## Change Your Own Password
1. Open a terminal on your Linux system.
2. Type `passwd` and press Enter.
3. Enter your current password when prompted.
4. Type your new password and press Enter.
5. Retype the new password to confirm.

## Change Another User’s Password (Admin)
1. Open a terminal.
2. Type `sudo passwd username` and press Enter.
3. Enter your admin password if prompted.
4. Type the new password for the user and press Enter.
5. Retype the new password to confirm.

## Force a User to Change Password at Next Login

Type `sudo passwd -e username` and press Enter.

## Set Password Expiration

Type `sudo passwd -x days username` and press Enter.

## Set Minimum Days Between Password Changes

Type `sudo passwd -n days username` and press Enter.

## Set Password Expiry Warning Days

Type `sudo passwd -w days username` and press Enter.

## Delete a User’s Password (Passwordless Login)

Type `sudo passwd -d username` and press Enter.