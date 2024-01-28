
*Note that some components that are bundled with the OS Images (CameraStreaming and Tools) may not have releases on GitHub, but just tags.*

## New build of specific component (not a new major or minor version)  

- Publish release on github
- Update download link on download page


## New Major / minor version (all components) 

- Handle milestone stuff on GitHub repos
- Publish new releases for all components on each github repo (version number change if nothing else; every component's major and minor versions should match)
- Change docs
    - Generate doxygen docs and update docs repo
    - Download links
    - Links to new github releases for each component
    - Any functional changes that need documentation
    - Publish updated docs with new version tag using mike
