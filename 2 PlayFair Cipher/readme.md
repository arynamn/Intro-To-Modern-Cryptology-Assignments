We mapped Morse code to  their corresponding alphabets via manual inspection.

- Play-fair Decryption Algorithm:
1. Key <br/>
The algorithm needs a 5*5 key grid in which first the key (“SECURITY”)  and then other remaining alphabets expect J is filled in 5x5 matrix in row order. 

2. Text Decryption : <br/>
Remove all characters like spaces,full stop except alphabets.
Taking two characters at a time ,one of the three substitution mentioned below is taken:
- If both the letters appear on the same row, replace them with the letters to their immediate left respectively (wrapping around to the left if required). 
- If both the letters appear on the same column, replace the letter above each one (wrapping around to the bottom if letter is at the top). 
- If both the letters are in different rows and columns, form a rectangle with these two letters and replace the letters on the horizontal opposite corner of the rectangle.
