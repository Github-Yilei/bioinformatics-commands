# Short note to tr

The tr command in Linux is used to perform simple but useful translations of one set of characters into another. Learn some practical examples of the tr command.

## Lower and upper case Converter
    
    cat sample.txt | tr 'a-z' 'A-Z'
    
## Replacing character(s) 

**tr** can replace one set of characters with another set of characters. 

    echo AATTGGCC | tr 'ATGC' 'tacg'
