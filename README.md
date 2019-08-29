# Info
This repo serves as an example for reproducing bug/weird behaviour I've noticed in Gotenberg:5.

# Prerequisites
Download a docker image for gotenberg:5 and run it locally

# Steps to reproduce
1. Run Gotenberg at <GOTENBERG_URL>
1. call convert/html with the `index.html` and `header.html` e.g., which **is successful**:
    ```bash
    curl --request POST \                                                                    
        --url http://localhost:3000/convert/html \
        --header 'Content-Type: multipart/form-data' \
        --form files=@index.html \
        --form files=@header.html \
        -o result.pdf
    ```
1. Add any character to the header file, and call `curl` again - **now it fails** with error message `printing page to PDF: cdp.Page: PrintToPDF: rpcc: message too large (increase write buffer size or enable compression)`
1. Leave that additional character in the header.html, but now attach additionally footer.html, and it **will pass**:
    ```bash
    curl --request POST \                                                                    
        --url http://localhost:3000/convert/html \
        --header 'Content-Type: multipart/form-data' \
        --form files=@index.html \
        --form files=@header.html \
        --form files=@footer.html \
        -o result.pdf
    ```
1. add any character to the `footer.html` and call thte `curl` again - **it fails** with the same error message
