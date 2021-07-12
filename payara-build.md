In order to release a patched version follow these steps:

## Build and test locally

```
mvn clean install
```

Update property `jakarta.el.version` in Payara's root POM.
Test the changes locally.
Commit the changes

## Prepare release version

```
NEW_RELEASE="3.0.3-payara-p6"
mvn versions:set -DnewRelease=$NEW_RELEASE -DgenerateBackupPoms=false
cd api
mvn versions:set -DnewRelease=$NEW_RELEASE -DgenerateBackupPoms=false
cd ../impl
mvn versions:set -DnewRelease=$NEW_RELEASE -DgenerateBackupPoms=false
```

Deploy patch to payara-artifacts, assuming your local profile to override deployment repo is `payara-artifacts`.

```
mvn -Ppayara-artifacts clean deploy
cd api
mvn -Ppayara-artifacts clean deploy
```

## Tag release

Commit changed pom files

```
git tag $NEW_RELEASE
git push origin $NEW_RELEASE
```

## Upgrade to snapshot
```
NEW_RELEASE="3.0.3-payara-p7-SNAPSHOT"
mvn versions:set -DnewRelease=$NEW_RELEASE -DgenerateBackupPoms=false
cd api
mvn versions:set -DnewRelease=$NEW_RELEASE -DgenerateBackupPoms=false
cd ../impl
mvn versions:set -DnewRelease=$NEW_RELEASE -DgenerateBackupPoms=false
```

Commit and push PR.

## Update Server with new release

Adjust both `jakarta.el.version` and `jakarta.el-api.version` to new versions in server now.