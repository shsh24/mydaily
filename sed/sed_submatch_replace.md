# Usage samples of sed submatch replace

1. Delete all after 'AAAA'
```
$ echo "XXXAAAAYYY" | sed -e 's/\(AAAA\).*/\1/g'
```
2. Replace all after 'AAAA' with 'BBBB'
```
$ echo "XXXAAAAYYY" | sed -e 's/\(AAAA\).*/\1BBBB/g'
```
3. Delete all before 'AAAA'
```
$ echo "XXXAAAAYYY" | sed -e 's/.*\(AAAA\)/\1/g'
```
4. Replace all before 'AAAA' with 'BBBB'
```
$ echo "XXXAAAAYYY" | sed -e 's/.*\(AAAA\)/BBBB\1/g' 
```
5. Delete 'AAAA'
```
$ echo "XXXAAAAYYY" | sed -e 's/\(.*\)\(AAAA\)\(.*\)/\1\3/g'
```
6. Delete all but 'AAAA'
```
$ echo "XXXAAAAYYY" | sed -e 's/.*\(AAAA\).*/\1/g'
```
7. Reorder two words separared by whitespace
```
$ echo "hello world" | sed 's/\([a-zA-Z0-9_]\+\) \([a-zA-Z0-9_]\+\)/\2 \1/g'
```
8. Replace substring start from 4th char and length is 2 with 'XXX'
```
$ echo "12345678" | sed 's/\(.\{3\}\)\(.\{2\}\)\(.*\)/\1XXX\3/'
```
9. Replace keyword "required" in "auth required pam_rhosts.so" with "sufficient"
```
echo "..." | sed "s/^\(auth[ \t]\+\)\(required\)\([ \t]\+\)\(pam_rhosts.so.*\)/\1sufficient\3\4/"
```

10. General rules
    * expression within a parentheses is a submatch.
    * the submatch is started from 1.
