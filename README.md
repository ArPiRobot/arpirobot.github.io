# ArPiRobot Docs

*Uses mkdocs with mkdocs-material theme and mike for versioning of docs.*

```sh
python3 -m pip install -r requirements.txt -U
mike serve
```

## Deploying

```sh
# Eg mike deploy --push --update-aliases 1.0 latest
# Change version as needed
# Add latest tag to latest docs (only for new version of docs, not updating existing version's docs)
mike deploy --push --update-aliases [version] [latest]
```

## Updating Doxygen

- Clone corelib repo and run the following in it
    ```sh
    python3 generate_docs.pygit s
    ```
- Docs are in the `doxygen-docs` folder. Copy it to `docs/` in the `arpirobot.github.io` repo.