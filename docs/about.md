## Aim

Initial Idea : http://wiki.centos.org/GSoC/2015/Ideas#docs-toolchain

The CentOS Project needs more short-form contributions of content that focuses on how-to do things on top of a CentOS Linux installation, with a method to push changes in to appropriate and related upstream open source projects.

There is a lot of content scattered across the Internet on how to do things with CentOS Linux. The goal of this toolchain is to make it easy for people to contribute new, short-form content articles to the Project with an ability to push them outward to relevant upstream projects.

## Workflow 

1. The user authors content in markdown format and creates a pull request on Github.
2. The backend service mirrors the pull request to pagure and creates a issue.
3. The doc is built and a link is provided to preview the doc.
5. Staff reviews the docs, gives comments either on pagure or github (two way synced)
4. Once the staff approves the doc is built and doc site updated.

## Infra

1. A backend server listing to Github and Pagure webhooks and mirroring content.
2. A http site displaying the temporary docs.
3. A pagure instance


## Testing

1. Fork the https://github.com/kunaaljain/test-centos-docs
2. Add new content to docs folder in mardown language.
3. Update the mkdocs.yml file and add the new file you just created with appropriate heading.
4. Create a pull request.
5. Within 15-20 seconds, a comment will be made on pull request, with the pagure link, which displaying various metadata.
6. Once the staff approves, PR is merged and site updated.
