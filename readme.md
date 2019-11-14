# TLS protocol
The goal of this lab is to get familiar with underlying principals of SSL/TLS cryptographic protocol.

## Task 1: Diffie–Hellman key exchange
Implement the [vanilla](https://en.wikipedia.org/wiki/Diffie–Hellman_key_exchange#Cryptographic_explanation) DH algorithm.
Try it with ``p=37`` and `g=5`. Can you make it working with recommended values 
``p=0xFFFFFFFFFFFFFFFFC90FDAA22168C234C4C6628B80DC1CD129024E088A67CC74020BBEA63B139B22514A08798E3404DDEF9519B3CD3A431B302B0A6DF25F14374FE1356D6D51C245E485B576625E7EC6F44C42E9A637ED6B0BFF5CB6F406B7EDEE386BFB5A899FA5AE9F24117C4B1FE649286651ECE45B3DC2007CB8A163BF0598DA48361C55D39A69163FA8FD24CF5F83655D23DCA3AD961C62F356208552BB9ED529077096966D670C354E4ABC9804F1746C08CA18217C32905E462E36CE3BE39E772C180E86039B2783A2EC07A28FB5C55DF06F4C52C9DE2BCBF6955817183995497CEA956AE515D2261898FA051015728E5A8AACAA68FFFFFFFFFFFFFFFF``
and ``g=2`` ?

## Task 2: Diffie–Hellman key 
Turn a DH secret into a key. Use ``sha1`` to generate `BLOCK_SIZE = 16` long key material.

## Task 3: Bulk cipher
Ensure you have working implementation of AES in CBC mode with PKCS&#35;7 padding. It is recommended to use  `BLOCK_SIZE = 16`
You will need ``encrypt(key, iv, message)`` and `decrypt(key, iv, encrypted_message)` functions. 
You can check your implementation with ``bulk_cipher.py`` example.

## Task 4: Implement simple SSL/TLS setup
It's time to have some fun now. Checkout `tls_101.py` example. Implement `Agent()` class such that this code executes with no errors.
You might want to use DH keys to seed AES_CBC bulk cipher you have implemented before
The interface for the ``Agent()`` class should support:
* sending/receiving public data (`p` and `g`)
* sending/receiving public key
* sending/receiving messages

Please, use recommended values for `p` and `g` for DH key exchange protocol.

## Task 5: Man-in-the-middle
Oh, no! Looks like something is wrong here! Who the hell is Mallory? 
Implement `MITM()` class such that `itls_101.py` runs with no errors.
The interface should support:
* sending/receiving public data (`p` and `g`)
* sending/receiving public key
* intercept_message

## Task 6: RSA
RSA algorithm is the most used asymmetric encryption algorithm in the world. It is based on the principal that it is easy to multiply large numbers, but factoring large numbers is very hard.
Within the TLS context it is used for both key exchange and generate signatures for security certificates. Let us implement this algorithm.
Here are few hints:
* Please use `p = 13604067676942311473880378997445560402287533018336255431768131877166265134668090936142489291434933287603794968158158703560092550835351613469384724860663783`, `q = 20711176938531842977036011179660439609300527493811127966259264079533873844612186164429520631818559067891139294434808806132282696875534951083307822997248459` and `e=3` for the key generation procedure.
* You might want to implement your `invmod` function. Test it with values `a=19` and `m=1212393831`. You should get `701912218`.
Your function should also correctly handles the case  when `a=13` and `m=91`
* You might want to implement functions `encrypt(bytes_, ...)/decrypt(bytes_,...)` and separately `encrypt_int(int_, ...)/decrypt_int(int_,...)`
* Please use [big endian](https://en.wikipedia.org/wiki/Endianness#Big-endian) notation when transforming bytes to integer

## Task 7:  RSA broadcast attack
It's time to check now that despite quite complex math involved in RSA algorithm it is still might be vulnerable to a number of attacks.
In this exercise we will implement the RSA broadcast attack (a.k.a simplest form of [Håstad's broadcast attack](https://en.wikipedia.org/wiki/Coppersmith's_attack#Håstad's_broadcast_attack)) 
Assume yourself an attacker who was lucky enough to capture any 3 of the ciphertexts and their corresponding public keys.
Check out `message_captured`. Here are a few hints for this exercise:
* The data is encrypted using `encrypt_int(public, bytes2int(message.encode()))`. 
* Please note, that in all 3 case public keys _are different_

Can you read the message? How Chinese remainder theorem is helping you here?
