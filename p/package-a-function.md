---
layout: default
---

# Package a Function

## Table of contents

- [Intro](#intro)

- [to Index](/index)

## Intro

The Function has to be packaged before uploading to Dispatcher:   
- Function is an external file (like .py, .jar, and so on)    
- Function has to be signed so Dispatcher can verify legibility of Function's source 

An application 'package-function' is used for packaging purpose.

Default steps for packaging Functions are:   
- create temporary folder   
- in that temp folder, create file functions.yaml and configure desired parameters. 
  For more details about functions.yaml see [Function](/p/function)    
- from this temp folder, run the following command  

java -jar /mh-root/git/apps/package-function/target/package-function.jar function.zip \[path to private key file\]   
> the path '/mh-root/git' is related to directory structure as described on [Full installation](/p/full-installation) page. 

- first parameter is the name of resulted .zip archive (in this example it's function.zip)   
- second parameter is optional and is used only when the Function has to be signed.   
\[path to private key file\] must point to a valid private key, i.e. /mh-root/git/private-key.txt
> the path '/mh-root/git' is related to directory structure as described on [Full installation](/p/full-installation) page. 


For details how to generate public and private keys pair, see [Gen keys](gen-keys) page.   

