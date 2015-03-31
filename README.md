# Continuous Integration of Documentation [![Build Status](https://travis-ci.org/kunaaljain/test-centos-docs.svg?branch=master)](https://travis-ci.org/kunaaljain/test-centos-docs)

This is example of a part of documentation toolchain, where documents are written in markdown, and once changes are committed to master, a web request is sent to a bot which generates the website and deploys it to the hosting platform.

Site: [http://clown-olga-13325.bitballoon.com/](http://clown-olga-13325.bitballoon.com/)

## Specification
* Since I don't have access to a server or hosting platform, I am using free hosting platform on [BitBalloon](http://bitballoon.com) and using Travis CI for bot.
* I am currently using mkdocs for static website generation with the default theme. This is not a good choice, however for testing purposes it is easier to implement for documentation. Another great option is jekyll.
* Can be easily customized for themes, others static website generators and deploy platform

## How to contribute
* Feel free to fork and make changes
* Make a pull request
* If travis CI builds it successfully, it can be merged upon which it will be deployed.

### Author: Kunal Jain
### For CentOS 
