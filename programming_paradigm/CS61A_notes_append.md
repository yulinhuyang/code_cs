### introduction

draw[::-1] 全部反转

text.count('the')

words = set(text) 

{w for w in words if w == w[::-1] and len(w) > 4}

max({w for w in words if w[::-1] in words},key = len)

max(words,key = lambda x: sum([1 for w in x if w in 'aeiou'])
