Using % as modulus operator <br/> 
using p = prime number = 19807040628566084398385987581 <br/>

We solved the question mainly using the modular arithmetic and one brute force method to calculate the value of g and then password.

## Given the pairs (a, b) of the form b = password * g ^ a .
	11226815350263531814963336315    = password * g ^ 324    -eqn 1
	9190548667900274300830391220  = password * g ^ 2345   -eqn 2
	4138652629655613570819000497   = password * g ^ 9513   -eqn 3

## Dividing one equation with other 
    (g^A1 / g^A2 ) = (B1 / B2) %p 
    g^(A1-A2)   = (B1 * B2^-1) %p
    g^(A12)     = (B21) %p   
    So the equation we got were
	  g^2021  = 7021284369301638640577066679 %p         -eqn 21) Dividing 2 by 1
 	  g^7168  = 6339248851737327508924059257 %p	  -eqn 32) Dividing 3 by 2
 	  g^9189  = 3426347385144995225825016781 %p	  -eqn 31) Dividing 3 by 1
 
## "BRUTE FORCE/ HIT AND TRIAL FUNCTION" -- 
	Assuming that if by dividing or multiplying in some order the above three expression, if the exponent on g becomes 1 we can get value of g directly.  So i tried to find possible set of values(i, j, k) such that          +- a31 * i +- a32 * j +- a21 * k == 1  using a hardcoded program.
	And we found many sets of such values.
	We picked one set (139,0,632) as it had a 0 for a32 which made calculation easier
	-9189*139 + 632*2021 = 1

## Therefore after appropriate multiplication and division, we got 2 new expressions
	From 31
	 g ^ (a31 * 139) = (g ^ a31)^ 139 = b31 ^ 139 %p
      => g ^ 1277271  = 3426347385144995225825016781^139 %p = 17064457453994872811494067145 

	From 21
	 g ^ (a21 * 632) = (g ^ a21)^ 632 = b21 ^ 632 %p
      => g ^ 1277272 = 7021284369301638640577066679^632%p = 9145714735161140899390199931   
	
    Dividing above two equations
        g^ (1277272 - 1277271) = (9145714735161140899390199931 * ( 17064457453994872811494067145 ^-1)%p )%p
     => g = 192847283928500239481729

     *** Here we matched this g with the given g = 1___4_2______0___94____9 with missing entries and it matched.

## Finally we computed password using the eqn 1 by substituting value of g
	password = ( b1 * (g^a1)^-1 ) % p
	password = 3608528850368400786036725

- NOTE: I may have missed %p in some places. Please forgive for missing %p in those places for the mathematical equations.
