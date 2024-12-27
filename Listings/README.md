The various stages of the Taipan listing reformatting

`Taipan_TRS80-orig.txt` is the code verbatim from [Taipan listing](https://taipangame.com/BASIC.txt)
The other files are: 
- `Taipan_TRS80-orig-missspace.txt`
  - The original file with the missing second spaces *after* line number having been added (manually)
- `Taipan_TRS80-orig-missspace-indent.txt`
  - The five space indentation is removed
- `Taipan_TRS80-orig-missspace-indent-1line.txt`
  - The newlines have all been removed
- `Taipan_TRS80-orig-missspace-indent-1line-fixed.txt`
  - Newlines, and spacing around line numbers, are fixed
- `Taipan_TRS80-orig-missspace-indent-1line-fixed2.txt`
  - Lines 10-98 are fixed (manually)
  - Long lines are fixed (manually)

From [Script_Issues.txt](https://github.com/greenonline/TRS80Taipan/blob/main/Listings/TRS80%20Taipan%20script%20issues.txt)
```none
# Final script
# Starts on a file with correct double space after line number
# Remove indent
sed 's/^ \{5\}//' Taipan_TRS80-orig-missspace.txt > Taipan_TRS80-orig-missspace-indent.txt 
# Remove newlines
cat Taipan_TRS80-orig-missspace-indent.txt | tr -d '\n'> Taipan_TRS80-orig-missspace-indent-1line.txt 
# Add newline and remove spaces either side of line number
perl -p -e 's/\s(\d{3,5})\s\s/\n$1 /g' Taipan_TRS80-orig-missspace-indent-1line.txt > Taipan_TRS80-orig-missspace-indent-1line-fixed.txt 

# Add newline to end of file?
echo '\n' >> Taipan_TRS80-orig-missspace-indent-1line-fixed.txt
# Tidy the 2 digit line numbers - remove second space, and preceding space 
# and add newline preceding line number
# Filename: Taipan_TRS80-orig-missspace-indent-1line-fixed2.txt
```

## See also

- [Taipan for TRS-80](https://gr33nonline.wordpress.com/2024/12/23/taipan-for-trs-80/)
