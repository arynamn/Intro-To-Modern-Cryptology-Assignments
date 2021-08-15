It was mentioned in the clue that encryption used is either 4, 6 or 10 round DES and two characters were one byte. Also we were allowed to do chosen plaintext attack.
We started decrypting assuming it to be 6-round DES. 

-  After entering many combination of ASCII character, we analyzed that ciphertext contains only 16 letters i.e. from f to u. Consequently, input could also consists of these 16 letters only.

-  We used two 3 round characteristic equations with probability 1/16 to break this 6 round DES.
ch1<4008 0000 0400 0000> and ch2<0020 0008 0000 0400>.
We reversed the effect of IP for ch1 and ch2 and got ip1(0000801000004000) and ip2(0000080100100000).
We randomly generated 300 , 64 bit strings, computed its xor with ip1 and ip2.
Then we mapped the 64 bit strings to  16 character string ranging between f and u. We stored random generated and xored with ip1 string in pt1.txt and randomly generated and xored with ip2 string in pt2.txt. 
We obtained corresponding cipher text using :
ssh -i key student@65.0.124.36 < pt1.txt > out1.txt
ssh -i key student@65.0.124.36 < pt2.txt > out2.txt
We clean all these 4 text files and named it pt1_clean.txt, pt2_clean.txt, ct1_clean.txt, ct2_clean.txt.

-  Given,
a) ch1 : Sboxes (2,5,6,7,8) have 000000 input xor and thus 00000 output xor  and xL5 = (0400 0000).
b) ch2 : Sboxes (1,2,4,5,6) have 000000 input xor  and thus 0000 output xor and xL5 = (0400 0000).

We applied the steps below:
1. Invert the effect of Final Permutation using RFP_INV on ciphertext pairs to get L6R6 and L6'R6'.
2. Compute value post S boxes using    postS = P_{INV} ( xL5 \oplus R5 \oplus R5') postS=P 
INV

 (xL5⊕R5⊕R5 
′
 )
3. Computed preKey and preKey' using L6 = R5 and L6' = R5' pair by applying expansion operation.
Here postS is 8 x 4bits , prekey and prekey' is 8 x 6bits.
4. For each Sbox  S_i S 
i

  that have 0000 output xor:
	- Initialize a dictionary for all possible 64 combination of keys with count 0.
	- For all 300 ciphertext pairs, Increase the count for key k in dictionary if:
		 S_i ( prekey \oplus k) \oplus S_i ( prekey'  \oplus  k) == postS  S 
i

 (prekey⊕k)⊕S 
i

 (prekey 
′
 ⊕k)==postS
	- Return the key k with max count.

Applying above process for ct1_clean.txt and ct2_clean.text.
We got the correct keys corresponding to S(1,2,4,5,6,7,8) boxes.
6th round key = '111101110011xxxxxx000111011011110110010101110100'

- Then we calculated the mapping of 56 bit key to the 6th round key in a dictionary. We reverse mapped the 6th round key to the 56 bit key using the dictionary.

Main Key = 'x11xx1xx01011x100xx11x11001x1010011x11011010x10x0101x011'

Now we have 42 bits out of 56 bits key. 
->Then, we applied brute force on the rest 14 bits of the key using a plaintext and its equivalent cipher text and obtained final key as:
Key = '01101110010111100111101100101010011011011010010001010011'

- Finally we take DES Encrypted password = "fpfhqoljnnnhfpkhrshthpmppoglhoup".
  -  Decrypt one 16 character block at a time and concatenated both result.
      DES Decrypted password = 'lumkmjmhljlkljlklrmlifififififif'
  -  Converted it to binary and then to ASCII and finally got 'outrdedelv000000'


- Code Details:
  - "Input_Generator_And_Output_Cleaner.py"  : Generate Input pairs and clean text files.
  - "DES_Parameters.py" : Keep all arrays, tables for S boxes , Expansion, Permutation, PC1, dictionary for bitmapping etc.
  - "DES_Break.py" : 
      1 Compute Sboxes with 0 output xor and xL5 for each characteristic equation.
		  2 Computing keys for 6th round.
		  3 Calculate key mapping from 56 bit to 6th round key
		  4 Brute Force DES bits
		  5 Decrypt password


External References:  https://medium.com/lotus-fruit/breaking-des-using-differential-cryptanalysis-958e8118ff41
