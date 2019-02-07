[//]: # (tags: openssl)
### specify protocol and algorithm to use with openssl client for testing

For TLSv1.1:

`openssl s_client -connect HOSTNAME:PORT -tls1_1 -cipher CAMELLIA128-SHA`

For TLSv1.2:

`openssl s_client -connect HOSTNAME:PORT -tls1_2 -cipher ANOTHERSSLALGO`
