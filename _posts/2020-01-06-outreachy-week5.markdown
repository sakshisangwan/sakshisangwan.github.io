---
title: "Outreachy Week 5: Packaging timeago.js"
layout: post
date: 2020-01-06 02:48
# image: /assets/images/markdown.jpg
headerImage: false
tag:
- outreachy
- debian
category: blog
author: sakshisangwan
description: This has been a lot of fun.
---

# Packaging node-timeago.js

## Task
> Package the module timeago.js to the latest version. Currently the package at upstream has version v4.0.2 whereas in Debian we have v3.0.2+dfsg-5. 


## Journey (still continues..)
**DETAILS:**
* Instead of packaging to v4.0.2 directly, we are packaging v4.0.0 first, since it is a major version update (3.x to 4.x) and also it would be easier to update to v4.0.2 from v4.0.0
* In the previous versions, webpack was used to bundle files, it has been replaced with rollup in the newer ones.

**WHAT HAS BEEN DONE:**
* There was a patch to update webpack files, since webpack wasn't required anymore, I removed the patch.
* The debian/watch file fetched the tarballs from npm registry which contained generated files and thus was causing build issues, so I replaced it to fetch from the github release tag for v4.0.0.
* The package.json file contains the build commands to build and run the  module.
	*Build commands as per package.json:*
	> `rimraf ./dist && rollup -c && cp -rf dist/ gh-pages && npm run size`
	`rimraf ./lib && tsc --module commonjs --outDir lib`
	`rimraf ./esm && tsc --module ESNext --outDir esm`

* `rimraf` acts like `rm`, it is used to remove files,  `tsc` is for typescript and  `outDir` specifies the resultant directory.
* To run  the commands when the package is built the same are added to debian/nodejs/build file, so the build file looks like: 
	
	> `tsc --module commonjs --outDir lib`
`tsc --module ESNext --outDir esm`
`rollup -c`

* Everything inside the dist directory should be installed in /usr/share/javascript/ ; to do the same debian/nodejs/links file is created.
	*In this case two files were generated inside the dist directory, so it looks like:*
	> `timeago.js/dist/timeago.full.min.js /usr/share/javascript/timeago.full.min.js`
`timeago.js/dist/timeago.js /usr/share/javascript/timeago.js`

* The package depends on few other packages, they should be installed when timeago.js is being installed. Such packages are added to the d/control file.
* All the generated files should be removed in debclean and hence should be mentioned in the debian/clean file, here the lib/ and esm/ directories were being generated and thus added to d/clean.
* The rollup-plugin-uglify was causing issues and thus had to be removed, to do the same I created a patch.
	> **Creating a patch**:
		* To create a patch, modify the file as per the changes required. 
		*  Run `dpkg-source --commit`, this will ask for a patch name, provide the same in accordance to the patch.
		* Use `quilt refresh` and `quilt pop -a` to remove all the applied patches as they will be re-applied while trying to build the package.
	
* The package build failed with the rollup v0.68.2 which is available in unstable, so tried to build it with the updated rollup v1.10.1 in experimental.
* I forgot to remove webpack as a dependency from debian/control and thus the package required rollup as well as webpack. The two for now cannot be installed at the same time in experimental (that's a WIP). This was causing issues, removing webpack as a dependency resolved the issue.
* Finally, was able to successfully build the package, but `lintian` threw a lot many errors and warnings. 
* Earlier I had made mistakes in the links file which caused broken symlinks, usage of wildcard characters and creation of a * file (Literally a file named `*`, not even kidding :P ) . Correcting the links file removed half the issues.
* `node-timeago.js source: repackaged-source-not-advertised` this was caused because in d/ch, dist directory was mentioned in Files-Excluded. 
File-Excluded is used to exclude files from upstream source packages.
Since this was not being repacked i.e. did not exist in the upstream and was being cleaned using debclean hence removing that from d/ch solved this.
* I am still stuck with few lintian errors and warnings, hope to have them solved by tomorrow.

That's all for now, will write more as I complete this package. :)

