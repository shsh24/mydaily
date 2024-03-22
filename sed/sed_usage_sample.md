# Usage sample of sed utility
### Add/Delete a line based on lineno

* general rule
```
sed '<lineno>[i|a|d]LINE-CONTENT' <file>
```
lineno: start from 1, $ means the last line.  
i: add before lineno, a: add after lineno (append), d: delete line

Examples:
```
sed '1i<content>' # add to the first line
sed '$a<content>' # append to the ene of file
sed '1d'              # delete the first line
sed '$d'              # delete the last line
```

### Match and add multiple lines
```
/<match-express>/{
a\
line-1-content\
line-2-content\
line-3-content
}
```

### Delete matched line
```
'/<match-express>/d'        # delete matched line
'/<match-express>/,$d'      # delete matched line to end of file
'/<match-express>/,+2d'     # delete matched line and next 2 lines
'1,/<match-express>/d       # delete matched line and all previous lines
'/^$/d'                     # delete empty line
```


### Update based on lineno
```
'2s/<from>/<to>/'           # replace <from> to <to> in line 2
'2!s/<from>/<to>/'          # replace <from> to <to> all lines but line 2
'2,3s/<from>/<to</'         # replace <from> to <to> in line 2 and line 3
'2,$s/<from>/<to</'         # replace <from> to <to> from line 2 and later
```

### Update based on content
```
'/<match-express>/s/<from>/<to>/'                     # replace <from> to <to> for all matched lines
'/<match-express1>/,/<match-express2>/s/<from>/<to>/' # replace <from> to <to> for all lines between matched express1 line and matched express2 line
```
```
