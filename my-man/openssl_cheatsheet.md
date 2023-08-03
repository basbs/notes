1. Extract ssl certificate
    ```
    openssl s_client -connect google.com:443 < /dev/null | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > public.crt
    ```
2. Extract user certificate from the p12 file
   ```
    openssl pkcs12 -clcerts -nokeys  -in usercert.p12 -out usercert.pem
   ```
3. Extract private key from the p12 file.
   ```
   openssl pkcs12 -nocerts -in usercert.p12 -out userkey.pem
   ```
4. View certificate information
   ```
   openssl x509 -subject -issuer -dates -noout -in usercert.pem
   ```
5. 

