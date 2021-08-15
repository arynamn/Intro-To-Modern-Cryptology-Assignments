Tools used : Frequency Analysis, Pyhton, Pyenchant Dictionary
- Since there are more than 2 single-letter word. So we removed spaces since only ‘i’ and ‘a’ are valid.
- After substituting y->m and e->t, we counted all triagram, found mey so replaced ‘e’->h.
- During deciphering, we found some words were shifted to the end.
- Since the digits were shifted by 8, to know actual digit in original text, we tried all no in -9to9 and where shifting felt logical. We found shifting by -4 made passcode 03->69 which got accepted.
- Case of the password was also preserved.
- When stuck, we calculated frequency of n-letter combinations and tried all possible word which existed using pyenchant-dictionary.
