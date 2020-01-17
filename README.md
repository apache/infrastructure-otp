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
The challenge presented by Orthrus can be copy/pasted onto the command line,
or (absent command line args) into the `Challenge?` prompt.

```
[remote]$ sudo bash
otp-md5 440 someseed ext
Password: 

[local]$ ./otp.py
Challenge? otp-md5 440 someseed ext
Response: MEAT SAD JERK STUN ARGO ITS
NOTE: copied to clipboard
```
or
`[local]$ ./otp.py otp-md5 440 someseed ext`

Note that you must copy/paste the challenge string from the remote host to the
prompt on the local host. The response can then be pasted into the remote host's
`Password:` prompt.

If the local host has the `xclip` or `pbcopy` program, then the reponse will be automatically
copied to the clipboard. If that is not present, or does not exit with success,
then the response string can be manually copied, then pasted to the remote.

The algorithm ("otp-md5") is the default, and the only supported algorithm at
the moment. On both the command line and the `Challenge?` prompt, that may be
omitted. For example:
```
[local]$ ./otp.py
Challenge? 440 someseed ext
Response: MEAT SAD JERK STUN ARGO ITS
NOTE: copied to clipboard
```
or
```
[local]$ ./otp.py 440 someseed ext
Response: MEAT SAD JERK STUN ARGO ITS
NOTE: copied to clipboard
```

Note that _some_ Orthus implementations produce extra words (eg. `ext`). These
will be ignored. The important parts are the algorithm (optional), the sequence,
and the seed values.

The algorithm associated with a seed will be stored into the `.otp` file,
and will override any future specification of an algorithm. Or, it shall
provide the algorithm when it is not explicit on the command line or
challenge prompt. In all cases, `otp-md5` is the current default.

## Alternative Usage
The script examines the program name used to invoke it (`sys.argv[0]`), and
if that name matches a defined algorithm (`otp-*`), then it will use the command
line arguments to select the sequence and seed.

For example:
```
[local]$ ln -s $somewhere/otp.py otp-md5
[local]$ otp-md5 440 someseed ext
Response: MEAT SAD JERK STUN ARGO ITS
NOTE: copied to clipboard
```

Note that the challenge string from [remote] is directly pasted to the
shell prompt.

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
* Use the `keyring` python package to support keyrings instead of a plaintext file
* Handle MD4 and SHA1 challenges
* Track RFC 2289 more closely in "acceptable" inputs

# License
Licensed under the [Apache License, v2.0](https://www.apache.org/licenses/LICENSE-2.0)
