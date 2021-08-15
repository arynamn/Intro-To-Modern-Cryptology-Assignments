After entering many combination of ASCII character, we analyzed that ciphertext contains only 16 letters i.e. from f to u. Consequently, input could also consists of these 16 letters only as 16 letters can be mapped from 0 to 15.

- We assumed two characters might be of one byte as it was the case in the previous level of game.

- We observed that the MSB of each byte in the output is 0 supporting the fact that each byte belongs to F<sub>128</sub>. So the byte range becomes ff to mu which means 00 to 7f in hex base.

- We noticed that on giving inputs "if" or "jf" we get the same output as of inputs "i" or "j" which implied second character is used for padding and had no effect.

- We generated input plain text of 8x128 in which only one byte is non-zero at a time. We store the generated input in "inputs.txt" and cleaned input in "input_clean.txt" so that we can get corresponding cipher text easily using ssh. The generated cipher text is stored in "output.txt" and its 8x128 representation in "output_clean.txt". 

- We aim to find an invertible 8x8 key matrix A with elements from F<sub>128</sub> and E vector of 8x1 dimension so that we can apply EAEAE transformation on input block.

- By changing one or two character in the plain text we examine the change in corresponding cipher text. We  observed that if we change the i<sup>th</sup>
  pair bit of plain text, the cipher text after i<sup>th</sup> pair bit starts changing. We therefore assumed that A can be lower triangular matrix.
          
          Plaintext : ghijklmfghiklfij
          Ciphertext: hsfghsklfrmllgjh

          Plaintext : ghijklmfghiklfik
          Ciphertext: hsfghsklfrmllggp

          Plaintext : ghijklmfghikjfik
          Ciphertext: hsfghsklfrmlhkmg


- Consider an input where i<sup>th</sup> non-zero byte is x. The corresponding output should be: <br/>
      ( a <sub>i,i</sub> x  x <sup>e<sub>i</sub></sup> ) <sup>e<sub>i</sub></sup> ) <sup>e<sub>i</sub></sup>
          
- We know that elements of A are from  F<sub>128</sub> and elements of E are number between 1 to 126 as given in the clue. So we iterate over all the plaintext-ciphertext pairs for each value of a<sub>i,i</sub> and e<sub>i</sub> , compare them  and store the  possible values of a<sub>i,i</sub>  and e<sub>i</sub>

- Now we need to find the exact values out of these possible values and non-diagonal elements of A as well. We again iterate over these pairs and use plaintext-ciphertext pairs to get the exact value out of the above possibilities satisfying the EAEAE condition. We will also get non-diagonal elements a<sub>i,j</sub>  by iterating over a<sub>i,i</sub> and a<sub>j,j</sub>
  
- We get the exact elements of  A matrix  and E vector.

- Now we take passsword ="gsgqljjkfgglmufrifkfhjjtmikjflkr" and break it into two halves p1="gsgqljjkfgglmufr" and p2="ifkfhjjtmikjflkr" and decrypted it. We got the ASCII value of decrypted text as "swqramnlad000000".
