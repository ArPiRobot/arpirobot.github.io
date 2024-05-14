
*Bug fixes, minor additions to specific tools, libraries, etc that do not significantly alter the overall use / design of the framework should be published as new "builds" of a single component (third version number incremented). Changes across various components should be a new minor release (second version number). Significant redesign to the approach of the framework should be a new major release.*

## New build of specific component (not a new major or minor version)  

- Publish release on github
- Update download link on download page
- Generate doxygen docs and update docs repo (if needed)
- Other docs updates if needed
- Update docs using existing version tag (using mike)


## New Major / minor version (all components) 

- Handle milestone stuff on GitHub repos
- Publish new releases for all components on each github repo (version number change if nothing else; every component's major and minor versions should match)
- Change docs
    - Generate doxygen docs and update docs repo
    - Download links
    - Links to new github releases for each component
    - Any functional changes that need documentation
    - Publish updated docs with new version tag using mike
