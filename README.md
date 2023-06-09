# Lab-5_20200148
Lab 5 completed


Reference: [Click Here](https://github.com/mikepound/feistel/blob/master/feistel.py)


## submitted by Jeet Narodia

### code used

``` python

    # !/usr/bin/env python3


    import hashlib
    import os
    import argparse
    from pkcs import PKCS7
    from feistel import FeistelNetwork
    from modes import ECB, CBC, CTR
    from iterators import file_block_iterator, eof_signal_iterator


    """
    The Mike Encryption Standard


    Never use this in production code!
    """




    def main():


        # Command line arguments
        parser = argparse.ArgumentParser()
        group = parser.add_mutually_exclusive_group(required=True)
        group.add_argument('-e', '--encrypt', action="store_true")
        group.add_argument('-d', '--decrypt', action="store_true")
        parser.add_argument('-m', '--mode', type=str, default='ECB')
        parser.add_argument('-i', '--iv', type=str,
                            help='Initialization Vector, used for CBC mode')
        parser.add_argument('-n', '--nonce', type=str,
                            help='Nonce to use for CTR mode')
        parser.add_argument("input_file")
        parser.add_argument("output_file")
        args = parser.parse_args()


        key = input("Enter {} key: ".format(
            "encryption" if args.encrypt else "decryption"))
        key = key.encode("UTF-8")


        cipher = FeistelNetwork(key)


        # Select mode of operation
        if args.mode == "ECB":
            padding_scheme = PKCS7(cipher.block_size)
            mode = ECB(cipher, padding_scheme)
        elif args.mode == "CBC":
            # if no IV was informed on command line, generate one (pseudo)randomly
            iv = args.iv.encode('utf-8') if args.iv else\
                os.urandom(cipher.block_size)
            # test for a valid IV length
            if len(iv) != cipher.block_size:
                raise ValueError("Invalid IV length")
            padding_scheme = PKCS7(cipher.block_size)
            mode = CBC(cipher, iv, padding_scheme)


        elif args.mode == "CTR":


            if args.nonce is None:
                parser.error("CTR mode requires a value for --nonce")


            nonce_size = cipher.block_size // 2
            try:
                nonce = int(args.nonce)
                nonce = nonce.to_bytes(nonce_size, "big")
            except ValueError:
                nonce = args.nonce.encode("utf-8")
                if len(nonce) > nonce_size:
                    nonce = nonce[:nonce_size]
                else:
                    nonce = nonce.rjust(nonce_size, b'\0')


            mode = CTR(cipher, nonce):
        else:
            raise ValueError(
                "Mo![image](https://user-images.githubusercontent.com/77286167/227494045-03b0066f-9d29-4b0b-beb1-389d4bb817a0.png)
de of operation {} is not recognised".format(args.mode))


        # Encrypt or decrypt
        if args.encrypt:
            file_blocks = file_block_iterator(args.input_file, cipher.block_size)
            with open(args.output_file, 'wb') as f:
                for ciphertext in mode.encrypt(file_blocks):
                    f.write(ciphertext)
        else:
            file_blocks = file_block_iterator(args.input_file, cipher.block_size)
            with open(args.output_file, 'wb') as f:
                for plaintext in mode.decrypt(file_blocks):
                    f.write(plaintext)




    if __name__ == "__main__":
        main()




    app = frontApp()
    app = run()
    
   
```

Errors caught by mypy:



![image](https://user-images.githubusercontent.com/77286167/227495565-c6b4d429-f441-48dd-babb-dacc0f04b92e.png)


Reducing errors(removing name-defined errors and correcting syntax):

![image](https://user-images.githubusercontent.com/77286167/227496507-bc7ac9de-09a9-4b18-8f2e-b78a3bf06644.png)

After installing all the non present modules, the error  was due to dependent files used in this particular code, they were resolved by placing the dependent files at proper location (same as the original file)

Final result after resolving all the errors:

![image](https://user-images.githubusercontent.com/77286167/227496939-fd261d40-b8fc-4577-8e07-8a5c07142cb7.png)





Corrected code after resolving all the errors:

``` python
    # !/usr/bin/env python3

    import hashlib
    import os
    import argparse
    from pkcs import PKCS7
    from feistel import FeistelNetwork
    from modes import ECB, CBC, CTR
    from iterators import file_block_iterator, eof_signal_iterator

    """
    The Mike Encryption Standard

    Never use this in production code!
    """


    def main():

        # Command line arguments
        parser = argparse.ArgumentParser()
        group = parser.add_mutually_exclusive_group(required=True)
        group.add_argument('-e', '--encrypt', action="store_true")
        group.add_argument('-d', '--decrypt', action="store_true")
        parser.add_argument('-m', '--mode', type=str, default='ECB')
        parser.add_argument('-i', '--iv', type=str,
                            help='Initialization Vector, used for CBC mode')
        parser.add_argument('-n', '--nonce', type=str,
                            help='Nonce to use for CTR mode')
        parser.add_argument("input_file")
        parser.add_argument("output_file")
        args = parser.parse_args()

        key = input("Enter {} key: ".format(
            "encryption" if args.encrypt else "decryption"))
        key = key.encode("UTF-8")

        cipher = FeistelNetwork(key)

        # Select mode of operation
        if args.mode == "ECB":
            padding_scheme = PKCS7(cipher.block_size)
            mode = ECB(cipher, padding_scheme)
        elif args.mode == "CBC":
            # if no IV was informed on command line, generate one (pseudo)randomly
            iv = args.iv.encode('utf-8') if args.iv else\
                os.urandom(cipher.block_size)
            # test for a valid IV length
            if len(iv) != cipher.block_size:
                raise ValueError("Invalid IV length")
            padding_scheme = PKCS7(cipher.block_size)
            mode = CBC(cipher, iv, padding_scheme)

        elif args.mode == "CTR":

            if args.nonce is None:
                parser.error("CTR mode requires a value for --nonce")

            nonce_size = cipher.block_size // 2
            try:
                nonce = int(args.nonce)
                nonce = nonce.to_bytes(nonce_size, "big")
            except ValueError:
                nonce = args.nonce.encode("utf-8")
                if len(nonce) > nonce_size:
                    nonce = nonce[:nonce_size]
                else:
                    nonce = nonce.rjust(nonce_size, b'\0')

            mode = CTR(cipher, nonce)
        else:
            raise ValueError(
                "Mode of operation {} is not recognised".format(args.mode))

        # Encrypt or decrypt
        if args.encrypt:
            file_blocks = file_block_iterator(args.input_file, cipher.block_size)
            with open(args.output_file, 'wb') as f:
                for ciphertext in mode.encrypt(file_blocks):
                    f.write(ciphertext)
        else:
            file_blocks = file_block_iterator(args.input_file, cipher.block_size)
            with open(args.output_file, 'wb') as f:
                for plaintext in mode.decrypt(file_blocks):
                    f.write(plaintext)



    if __name__ == "__main__":
        main()

```
