# ArPiRobot Docs

```sh
python3 -m pip install -r requirements.txt -U
mkdocs serve
```

## Deploying

```sh
mkdocs gh-deploy
```

## Updating Doxygen

- Clone corelib repo and run the following in it
    ```sh
    python3 generate_docs.py
    ```
- Copy doxygen-docs folder to this repo on the gh-pages branch
- Commit and push gh-pages branch