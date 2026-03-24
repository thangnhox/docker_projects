```Bash
docker build -t pandox-latex .

# Run the conversion
docker run --rm -v $(pwd):/data pandox-latex \
  input.md \
  -o output.pdf \
  --pdf-engine=xelatex \
  --variable mainfont="Liberation Serif"
```
