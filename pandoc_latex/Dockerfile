FROM alpine:latest

# Install only what is strictly necessary for Markdown -> PDF
RUN apk add --no-cache \
    pandoc \
    texlive-xetex \
    texmf-dist-latexextra \
    # Standard fonts are required for the PDF to actually render text
    ttf-liberation \
    fontconfig

# Refresh font cache so XeLaTeX sees the liberation fonts
RUN fc-cache -fv

WORKDIR /data
ENTRYPOINT ["pandoc"]
