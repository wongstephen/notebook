``` bash
for file in *
do
filename="${file%.*}"
cwebp -q 100 "$file" -o "${filename}.webp"
done
```
`