Envelope encryption sounds complex. It's actually pretty elegant.

How AWS KMS works under the hood:

→ KMS generates a data key for your file
→ That key encrypts the file locally
→ KMS encrypts the data key and stores it alongside the file

Your data never travels to KMS. Only the key does.

Small detail, explains a lot about AWS security design.

#AWS #DevOps #CloudSecurity
