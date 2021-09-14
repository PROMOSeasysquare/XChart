# PROMOS XChart Fork

This repository is a fork of the official XChart repo with customizations for PROMOS Consult.

## Versioning

The actual customizations are kept in the branch _promos/patch_. For each upstream
version there is a specific version branch _promos/<upstream_version>_ (e.g. _promos/3.8.0_).

## Add customizations

To add new customizations check out and commit on branch _promos/patch_.

In order to create and publish these changes switch to
current version branch and merge patch branch.
E.g. for version 3.8.0:
```
git switch promos/3.8.0
git merge promos/patch
```

Then push to origin
```
git push origin promos/3.8.0
```

## Update upstream version

To update the custom version to the latest upstream version please follow this process:

Fetch upstream
```
git remote add upstream https://github.com/knowm/XChart.git
git fetch upstream
```

Rebase patch branch on new upstream version (e.g. 3.8.1)
```
git switch promos/patch
git rebase xchart-3.8.1
```

Create new version branch
```
git branch promos/3.8.1 promos/patch
git switch promos/3.8.1
```

Update maven version. In `xchart/pom.xml` replace this
```
<name>XChart</name>
<description>The core XChart library</description>
```
with
```
<version>3.8.1-PROMOS.1-SNAPSHOT</version>
<name>XChart Promos</name>
<description>The core XChart library customized for PROMOS</description>
```
Attention: Make sure to adapt the version string to match the upstream version used above!

Finally commit and push your changes to origin:
```
git add xchart/pom.xml
git commit -m "Prepare new upstream release 3.8.1-PROMOS.1-SNAPSHOT"
git push --force-with-lease origin promos/patch
git push --set-upstream origin promos/3.8.1
```

## Build

To build the XChart core library with PROMOS customizations just run maven on the specific version branch in __xchart__ directory:
```
git switch promos/3.8.0
cd xchart
mvn clean install
```

## Release

To create and publish a release please use this process:

Switch to the version branch you want to release (e.g. for 3.8.0)
```
git switch promos/3.8.0
```

Remove SNAPSHOT-Suffix from version element in xchart/pom.xml. The result
should look something like this:
```
<version>3.8.0-PROMOS.1</version>
```

Commit this change:
```
git add xchart/pom.xml
git commit -m "Prepare release version 3.8.0-PROMOS.1"
git push origin promos/3.8.0
```

Build and deploy artifacts to maven repository. The repo ID and URL can be found in the internal project documentation.
```
cd xchart
mvn clean install org.apache.maven.plugins:maven-deploy-plugin:deploy -Drepo.id=<...> -Drepo.url=<...>
```

> Simply using _mvn deploy_ does not work, because this is bound to nexus-staging-maven-plugin in the parent pom.

Determine the next development version:
- The version begins with the upstream version corresponding to the version branch that should be released (e.g. _3.8.0_).
- The development version should always end in _-SNAPSHOT_.
- The custom version for PROMOS contains a qualifier string _PROMOS.<custom_version>_ (e.g. _PROMOS.1_)
  where _custom_version_ should be incremented for every release based on the same upstream version.
- So if you release version _3.8.0-PROMOS.1_ the next development version should be _3.8.0-PROMOS.2-SNAPSHOT_

Set this development version in the version element in xchart/pom.xml. The result
should look like this:
```
<version>3.8.0-PROMOS.2-SNAPSHOT</version>
```

Commit and push changes to origin:
```
git add xchart/pom.xml
git commit -m "Prepare development version 3.8.0-PROMOS.2-SNAPSHOT"
git push origin promos/3.8.0
```
