Ritsec 2023 Stego
Bitmap writeup - Some of these squares are not like the others.

I first opened the bitmap.bmp with a hex editor and notes the phrase XOR BY ‘FF’, so bear that in mind for later:
 
![image](https://github.com/hacklaugh/CTF-write-ups/assets/126184849/68b1e767-9c80-479e-aa41-8aea7e656d6a)

Next, use Python’s PIL library and Counter to analyse the pixels in the bitmap:

![image](https://github.com/hacklaugh/CTF-write-ups/assets/126184849/e85f9f86-4fe0-46e2-929e-3a01920b02d4)

This showed lots of pixels with R=G=B such as (159,159,159) in large numbers, but interestingly also found a number of pixels which were unique and didn’t follow this pattern. I focused on the unique pixels and extracted the pixels where R,G,B weren’t the same. I then converted these to hex with:

![image](https://github.com/hacklaugh/CTF-write-ups/assets/126184849/a91f9e6d-99c8-4ab8-9f35-cb3ec44d8c92)

…and got the following:	
b3bdffb9a8b0b7acb6bab4a0d8c2a6b6ada0baacabd8a0bca9b6dfbdd8c2a8b0b3acb6b9dfd8b7bbb0b2bcc2badfbcbdc2b1b6a7bab7aab0dfb7c2abdfa7baafb6bcadbab7acaad7b6dfbab7dfb1d6a7ba9bd8c2f5a4bb404ed427655ad809ca63b29e1d4c2e047bac522ff3d368b6ed5ce5ffd8ff58000020524f205942464627000027

At this point I tried the XOR by ‘FF’ hint found earlier using CyberChef:

![image](https://github.com/hacklaugh/CTF-write-ups/assets/126184849/557dbc0e-a080-4546-be6b-ebd51dcc3d2e)

![image](https://github.com/hacklaugh/CTF-write-ups/assets/126184849/caf39f8c-96e1-4bdf-a81f-7379ad59e788)

This gave me something, I was on the right track, but didn’t quite make sense, and stumped me for ages, the letters weren’t quite in the right order. But then finally B’=WOLSIF made me think of ‘blowfish’ 

bdd8c2 a8b0b3 acb6b9 when XORed is: B'=WOLSIF

bdd8c2 a8b0b3 b9b6ac when XORed is B'=WOLFIS, so cb6b9 should be b9b6ac, basically each pixel when converted to hex then also needed converting from little endian to big endian.

I cheated a bit here, and instead just swapped (r, g, b) with (b, g, r) in rgb_to_hex:

![image](https://github.com/hacklaugh/CTF-write-ups/assets/126184849/5597ff6d-5177-45b5-b4be-a335ca034e3a)

Which gives:
ffbdb3b0a8b9b6acb7a0b4baa6c2d8a0adb6abacbabca0d8dfb6a9c2d8bdb3b0a8b9b6acb7d8dfb2b0bbbac2bcbdbcdfb6b1c2b7baa7dfb0aaabc2b7baa7dfbcb6afb7baadd7aaacbadfb6b1dfb7baa7d6c2d89bbba4f5d44e405a6527ca09d89eb2632e4c1dac7b04f32f52b668d3e55cedffd8ff0000584f5220425920274646270000
and in CyberChef now get:

![image](https://github.com/hacklaugh/CTF-write-ups/assets/126184849/a274f1d2-93f1-41da-9e87-825c79d9dd2f)

which is another hint that we need to Xor by FF, which gives:

![image](https://github.com/hacklaugh/CTF-write-ups/assets/126184849/c16d5ba1-4e40-4b7f-b10b-e83eec3f9659)

This now gives us the info we need to Xor and decrypt the non-ASCII values between the two quotes starting dD[

![image](https://github.com/hacklaugh/CTF-write-ups/assets/126184849/57df10f3-1787-49d5-9e6b-997e0b2349b6)

![image](https://github.com/hacklaugh/CTF-write-ups/assets/126184849/5008d57d-58b8-42d4-8541-4ae351241db1)

![image](https://github.com/hacklaugh/CTF-write-ups/assets/126184849/b841b9dd-2f12-40de-8173-386a503ece4b)
