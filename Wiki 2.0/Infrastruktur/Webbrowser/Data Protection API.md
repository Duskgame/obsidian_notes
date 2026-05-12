---
aliases:
  - DPAPI
---
Data Protection Application Programming Interface (DPAPI) is a simple cryptographic application programming interface available as a built-in component in Windows 2000 and later versions of Microsoft Windows operating systems. In theory, the Data Protection API can enable symmetric encryption of any kind of data; in practice, its primary use in the Windows operating system is to perform symmetric encryption of asymmetric private keys, using a user or system secret as a significant contribution of entropy. 

For nearly all cryptosystems, one of the most difficult challenges is "key management" – in part, how to securely store the decryption key. If the key is stored in plain text, then any user that can access the key can access the encrypted data. If the key is to be encrypted, another key is needed, and so on. DPAPI allows developers to encrypt keys using a symmetric key derived from the user's logon secrets, or in the case of system encryption, using the system's domain authentication secrets.

The DPAPI keys used for encrypting the user's RSA keys are stored under %APPDATA%\Microsoft\Protect\{SID} directory, where {SID} is the Security Identifier of that user. The DPAPI key is stored in the same file as the master key that protects the users private keys. It usually is 64 bytes of random data.
