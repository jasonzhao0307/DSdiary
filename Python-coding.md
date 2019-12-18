1. how to read a file and count how many characters are upper case? Do it in one line of code. 

`
count = sum(1 for line in open('path_to_file') for char in line if char.isupper())
`

1.1 What if the file is very large?
`
with open('path_to_file') as file:
    count = sum(1 for line in file for char in line if char.isupper())
`