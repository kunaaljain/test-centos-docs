# Continuous Integration of Documentation [![Build Status](https://travis-ci.org/kunaaljain/test-centos-docs.svg?branch=master)](https://travis-ci.org/kunaaljain/test-centos-docs)

This is front end part of documentation toolchain, where documents are written in markdown, and once changes are committed to master, a web request is sent to a bot which generates the website and deploys it to the hosting platform.

The Backend service is server which mirrors the Github Pull request to the Pagure issues, so that the toolchain has no dependency on Github. The two way sync is achieved with the help of webhooks and API of Github and Pagure.
Another service builds the documents in PR so that it can be viewed before merging.

Site: [http://clown-olga-13325.bitballoon.com/](http://clown-olga-13325.bitballoon.com/)

## How to contribute
* Feel free to fork and make changes
* Content is to be written in .md file extension in docs.
* Add the files you created here https://github.com/kunaaljain/test-centos-docs/blob/master/mkdocs.yml so that the site naviagtion bar links to your content.
* Make a pull request
* A corresponding pagure issue will be created in few seconds, and a comment will be made on the PR with the issue link.
* The issue link contains the various data including the link to built document for preview.
* If travis CI builds it successfully, it can be merged upon which it will be deployed.

### Author: Kunal Jain and Lei Yang
### For CentOS
