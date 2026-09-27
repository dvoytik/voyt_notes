
### If I forgot a key binding - search through definitions and descriptions

```
<leader>sk
type_keywords_of_what_you_want_to_recall
```
or
```
:Telescope keymaps
```

### Make current file executable and execute it
```
:!chmod +x %
:!./%
```

### Execute lua and show result:
```
:lua =vim.uv.cwd()
```
