- After entering the sequence of commands: exit1->exit2->exit4->exit3->exit1->exit4->exit4->exit2->exit2->exit1->read, we get to know that this door has RSA encryption with exponent 5. 

-  RSA encryption:  <br/>
>    Encryption : C = M<sup>e</sup> mod n <br/>
>    Decryption : M = C<sup>d</sup> mod n <br/>

- The value of n, e, C is known to us. To decrypt the cipher text we need the value of d(private key). We can compute d from below equation: <br/>
	&emsp; de = 1 mod ϕ(n)ϕ(n)   <br/>
  But we don't know ϕ(n) because n is large and finding \phi(n)ϕ(n) requires factoring n which is hard to compute. So we can't compute the value of d. <br/>

- We notice that the message is encrypted using exponent 5 which is small. It is a special case of RSA encryption that can be decrypted if there is no proper use of Random Padding.

- We crawled the web and get to know that there exists an algorithm for attacking RSA when the public exponent e is small named as Coppersmith Theorem. Coppersmith's attack describes a class of cryptographic attacks on the public-key cryptosystem RSA.

- Coppersmith Theorem:
Let N be an integer and f ∈ Z[x]  be a monic polynomial of degree d over the integers. Given N and f, we can recover all x<sub>0</sub> < X such that f( x<sub>0</sub> ) = 0modN in polynomial time where X = N<sup> 1/d − ϵ </sup> for 1/d > ϵ > 0. So the problem can be formulated as: <br/>
&emsp; f(M) = (p + M)<sup>e</sup> mod N

- The basic idea here is to find a polynomials whose roots contain the solution and whose coefficient are small so that its roots are calculable. So now our aim is to convert our problem into polynomial form to apply this algorithm. 

- We first need to find out whether padding is used or not. We get to know this information by checking whether C<sup>1/e</sup> is integer or not. So in this case padding was used.

- Now expressing into polynomial form: <br/>
	&emsp; (p + M)<sup>e</sup> = C mod n <br/>
  &emsp; or  (p + M)<sup>e</sup> - C = 0 mod n <br/>
  &emsp; or (p + M)<sup>e</sup> - C = 0 mod n<sup>k</sup> <br/>

- Here e, n, C is known but we don't know p and M. In order to find out padding bits we tried string given on the screen --
	"Kerberos: This door has RSA encryption with exponent 5 and the password is:" 
and it didn't work. We also tried few variations in the string by placing some extra space and characters but still we didn't get roots of the equation.

- Later we tried the hex code that we get while entering the sequence of commands:
	&emsp; 59 6f 75 20 73 65 65 20 <br/>
	&emsp; 61 20 47 6f 6c 64 2d 42 <br/>
	&emsp; 75 67 20 69 6e 20 6f 6e <br/>
	&emsp; 65 20 63 6f 72 6e 65 72 <br/>
	&emsp; 2e 20 49 74 20 69 73 20 <br/>
	&emsp; 74 68 65 20 6b 65 79 20 <br/>
	&emsp; 74 6f 20 61 20 74 72 65 <br/>
	&emsp; 61 73 75 72 65 20 66 6f <br/>
	&emsp; 75 6e 64 20 62 79   <br/>
	
- After converting above sequence of hex code into its ASCII-value, we get the string:      
"You see a Gold-Bug in one corner. It is the key to a treasure found by"

- We converted this string into binary form to get the padding bits(p).

- The variable M is unknown but we can figure out the upper bound on length of M from the Coppersmith theorem since it is given in theorem that the root(x<sup>0</sup>) < N<sup>1/e</sup>. So we randomly assumed of 100 bits length and we found the root.

- Now the final equation becomes: <br/>
	&emsp; &emsp; ((binary\_pad << length<sup>M</sup>) + M)<sup>e</sup> - C

- We form lattice with the coefficients of polynomial and if we find the short vectors of the matrix we will be able to find vectors less than N^{k} and therefore the answer we are seeking because these vectors will represent tractable polynomials with roots that are possible solutions to the problem. The LLL algorithm is able to do this and we found the root.

- We found root of the equation as: 
1000000100001001000000011010000111010101100010010000010110110000100001

- After converting this binary into ASCII, we get decrypted text: "B@hubAl!"


References:
  - https://www.cryptologie.net/article/222/implementation-of-coppersmith-attack-rsa-attack-using-lattice-reductions/
  - CopperSmith Sage  https://github.com/mimoo/RSA-and-LLL-attacks
  - https://en.wikipedia.org/wiki/Coppersmith%27s_attack
  - Jason Dyer, Lattice Reduction on Low-Exponent RSA, 2002
