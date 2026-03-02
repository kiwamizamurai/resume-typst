# typst-resume

Typstで履歴書/職務経歴書を生成する

## Output PDF

- [resume/resume.pdf](resume/resume.pdf)
- [cv/cv.pdf](cv/cv.pdf)

## Requirements

- [typst](https://typst.app/) (managed via [mise](https://mise.jdx.dev/))
- [HackGen font](https://github.com/yuru7/HackGen)

```console
$ brew install --cask font-hackgen font-hackgen-nerd
```

## Usage

```console
$ mise run cv      # Build cv PDF
$ mise run resume  # Build resume PDF
$ mise run build   # Build all PDFs
```

## Thanks

- [takeokunn/typst-resume-template](https://github.com/takeokunn/typst-resume-template)
