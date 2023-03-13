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
    python3 generate_docs.pygit s
    ```
- Docs are in the `doxygen-docs` folder. Copy it to `docs/` in the `arpirobot.github.io` repo.