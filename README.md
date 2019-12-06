# otp.py
Command-line OTP calculator, with automatic password creation/storage.

# Apache Specific
The Apache Software Foundation (ASF) uses the
[Orthrus](https://github.com/gmcdonald/orthrus)
PAM module on its machines/VMs. Orthrus implements
[RFC 2289](https://tools.ietf.org/html/rfc2289) to offer One Time
Password (OTP) challenges, in order to `sudo`. That package emits MD5-based
challenges (`otp-md5`), so this script only constructs MD5-based responses.

# Usage
```
[remote]$ sudo bash
otp-md5 440 someseed ext
Password: 

[local]$ ./otp.py
Challenge? otp-md5 440 someseed ext
Response: MEAT SAD JERK STUN ARGO ITS
NOTE: copied to clipboard
```
Note that you must copy/paste the challenge string from the remote host to the
prompt on the local host. The response can then be pasted into the remote host's
`Password:` prompt.

If the local host has the `xclip` program, then the reponse will be automatically
copied to the clipboard. If that is not present, or does not exit with success,
then the response string can be manually copied, then pasted to the remote.

## Testing
```
$ ./otp.py --test
```
This will run a few internal tests. Any problems will raise an `AssertionError`

# Seeds and Passwords
If new seed is seen (ie. by running `ortpasswd`), then `otp.py` will construct
a new password and store the seed and password into `$HOME/.otp`. The password
will be used the next time the seed is seen.

# Possible TODO Items
* If `xclip` fails, try using `pbcopy` to support macOS
* Use the `keyring` python package to support keyrings instead of a plaintext file
* Handle MD4 and SHA1 challenges
* Track RFC 2289 more closely in "acceptable" inputs

# License
Licensed under the [Apache License, v2.0](https://www.apache.org/licenses/LICENSE-2.0)
