# Build and Release PROMOS XChart Fork

## Build

To build the XChart core library with PROMOS customizations just run maven in __xchart__ directory:
```
mvn clean install
```

## Release

To create and publish a release please use this process:
1. Determine the next development version
    - The version should begin with the upstream version the master branch is currently based on (e.g. _3.8.0_).
    - The development version should always end in _-SNAPSHOT_.
    - The custom version for PROMOS contains a qualifier string _PROMOS.<custom_version>_ (e.g. _PROMOS.1_)
      where _custom_version_ should be incremented for every release based on the same upstream version.
    - So if you release version _3.8.0-PROMOS.1_ the next development version should be _3.8.0-PROMOS.2-SNAPSHOT_
2. Replace version strings, push version to git and deploy built artifacts to maven repository by running maven
   on branch __master__ in __xchart__ directory:
```
mvn -B gitflow:release -DdevelopmentVersion=3.8.0-PROMOS.2-SNAPSHOT
```

## Update upstream version

In order to update to a current upstream release...
1. Merge a release tag from the upstream repo into this repo's master branch.
2. Update the version of the XChart core project in xchart/pom.xml to reflect the new upstream version.
    - For example, if your custom version is _3.8.0-PROMOS.2-SNAPSHOT_ and you update to the upstream
      version _3.9.0_, your new version will be _3.9.0-PROMOS.1-SNAPSHOT_.
3. Commit and push changes to github.
