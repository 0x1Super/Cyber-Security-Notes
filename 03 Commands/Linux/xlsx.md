Open excel files linux

```
libreoffice FILE.xlsx
```

# bypassing sheet protection

```bash
unzip file.xlsx
find . | grep sheet # find protected sheet 
# Delete sheetProtection algorithmName Line
zip file.xlsx -r .

```