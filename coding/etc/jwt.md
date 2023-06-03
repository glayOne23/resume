# JWT
- Structure :
1. Header
2. Payload
3. Signature

- Base64
    - Each Base64 digit represents exactly 6 bits of data. Three 8-bit bytes (i.e., a total of 24 bits) can therefore be represented by four 6-bit Base64 digits.
    - Common to all binary-to-text encoding schemes, Base64 is designed to carry data stored in binary formats across channels that only reliably support text content. Base64 is particularly prevalent on the World Wide Web[1] where its uses include the ability to embed image files or other binary assets inside textual assets such as HTML and CSS files.
    - https://en.wikipedia.org/wiki/Base64

## Code
- pip install pyjwt
1. HS256 (HMAC + SHA)
    - Encode
        ```python
        >>> import jwt
        >>> payload = {'id':1, 'name':'hammam'}
        >>> secret = 'fsafs342vmsa[23-423rjwjfsdfsdfsafa,'
        >>> token = jwt.encode(payload, secret).decode('utf-8')
        >>> token
        'eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpZCI6MSwibmFtZSI6ImhhbW1hbSJ9.sGeYc3GJnuKOkyCCFWO4FnFdaRBGaLUXPIuYTBUj8Ss'
        ```
    - Decode
        ```python
        jwt.decode(token, secret)
        ```
2. RS256 (RSA + SHA)
- `pip install cryptography`
- to make public and private key : `ssh-keygen -t rsa -b 4096`
    - Encode
        ```python
        >>> import jwt
        >>> payload = {'id':1, 'name':'hammam'}
        >>> private_key = open('jwt-test-key').read()
        >>> token = jwt.encode(payload, private_key, algorithm='RS256').decode('utf-8')
        >>> token
        'eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJpZCI6MSwibmFtZSI6ImhhbW1hbSJ9.csPHyiYsVoVGiZMgVsRL369rAXIxKk-0kPoucnddplwHSqh93icWc6f-5w5MyMW5axcav3GE43pEFePahBiZZAoXW5JbVK8VW_Bc1aiw-gEZz_3cywDedFZdkaOZTZHI30taXk_IZrA3gKOvx4Y9VOinVro68H-neQ-eCeMzcQNsgPBwyrokLVKs1ILVFIdtB-LuPrF9agjUulBccfAi0WS5bLQlTig7XQYvyBrA89XPH1ID-YJWUP1mCNwq86qwLr0AUHEwAQZdkW6SEGB6JZTjRRKDE9a3g_lOLTUwFqh5XAu-Q4L5TyMYSY6Sj6BUhapMskwZmvePHQwqcCA8VcKYYpmGQPzIuPX4D5t_1Illa6_2Y537ygRt6MzDIaJ_IAhbIDba9AmPy0bjl6MbqI3S1IqI_omhE3_GNWeSw-9ocmy53wp5dr_UkquaTTHN48zNsdshaTcFWWMGBSgZpodrTeMVyjrPzN5xwUwavSU1ad62Fy9TJP52YOw0L2DIV3s8YNdb5DdQaObaQklFks0BRZO4gAY5XCrkR0AlZQUVjzr2EDH1lKUXdih9FepXvZHprpLlJUnUDe1bnjMNaEwxbggJHj47dWRXUBhDlJEO4Zbf2-k4-hEsRugJNQHWJDBpZlVjx-lEEs0tCyEu8PpUfTTSWGsGM0cuB809zK8'
        ```
    - Decode
        ```python
        >>> public_key = open('jwt-test-key.pub').read()
        >>> jwt.decode(token, public_key, algorithm=['RS256'])
        {'id': 1, 'name': 'hammam'}
        ])
        ```
