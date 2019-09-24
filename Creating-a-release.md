# Step 1: Update the `master` branch

## Automated


## Automated

### One time setup

1. Create a suite.sh file with the following contents:

```bash
#!/bin/bash

# Allow using globs for the file move later on
shopt -s extglob

# Do everything in a tmp directory
mkdir ./tmp
cd tmp

# Make a fresh copy of the api
git clone -b build git@github.com:directus/api.git
git clone -b build git@github.com:directus/app.git
git clone git@github.com:directus/directus.git

cd directus
rm -vr !(".git")
cd ..

rm -rf api/.git

cp -r api/* directus/

mkdir directus/public/admin

rm -rf app/.git

cp -r app/. directus/public/admin

cp directus/public/admin/config_example.js directus/public/admin/config.js

cd directus

# Get the version number from the user
echo "What version?"
read version

# Commit the changes
git add -A
git commit -m "$version"
git push origin master
cd ..
cd ..
rm -rf tmp
echo ""
echo "✨ All done!"
```

2. Add execute permissions to this file

```bash
$ chmod +x ./suite.sh
```

### Create the build and update the master branch

```bash
$ bash ./suite.sh
```

## Manually

1. Clone the directus/directus repo

```bash
$ git clone git@github.com:directus/directus.git
```

2. Delete everything in it except the `.git` and `.github` folders

3. Clone the api build

```bash
$ git clone -b build git@github.com:directus/api.git api-build
```

4. Remove the `.git` folder from the `api-build` folder

```bash
$ rm -rf api-build/.git
```

5. Copy everything in the api-build folder to the main directus/directus repo

```bash
$ cp -r api-build/ directus
```

6. Clone the app build

```bash
$ git clone -b build git@github.com:directus/app.git app-build
```

7. Make the public/admin directory in directus/directus

```bash
$ mkdir directus/public/admin
```

8. Delete the `.git` folder from the app-build

```bash
$ rm -rf app-build/.git
```

9. Copy everything from app-build to directus/public/admin

```bash
$ cp -r app-build/ directus/public/admin
```

10. Duplicate the `directus/public/admin/config_example.js` file to `directus/public/admin/config.js`

```bash
$ cp directus/public/admin/config_example.js directus/public/admin/config.js
```

11. Change the `directus/public/admin/config.js` file to point to the relative API

```js
api: {
  "../_/": "Directus API"
}
```

12. Delete all .DS_Store files

```bash
$ cd directus
$ find . -name '.DS_Store' -type f -delete
```

13. Add-commit-push

```bash
$ git add .
$ git commit -m "<VERSION NUMBER>"
$ git push origin master
```

```bash
$ cd ..
$ rm -rf directus
$ rm -rf api-build
$ rm -rf app-build
```

# Step 3: Creating the release on GitHub

1. On the releases page, draft a new release
2. **Make sure to use `master` as Target**
3. Publish the release!

