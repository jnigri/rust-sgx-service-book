# Formatting and Linting

If you are using [Visual Studio Code](https://code.visualstudio.com/), you can create a folder `.vscode` at the root of the project and add the following files:

`extensions.json`
```
{
    "recommendations": [
        "matklad.rust-analyzer",
    ]
}
```

`settings.json`
```
{
    "editor.formatOnSave": true,
}
```