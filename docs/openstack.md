# Installing OpenStack with RDO Manager

**Note:** This is just testing content that I copied from the [RDO-Manager]
documentation.

RDO-Manager is an OpenStack Deployment & Management tool for RDO. It is based
on OpenStack TripleO project and its philosophy is inspired by SpinalStack.

Useful links:
- RDO-Manager Home Page
- RDO-Manager Repositories
- TripleO Documentation

## Architecture

With RDO-Manager, you start by creating an undercloud (an actual operator
facing deployment cloud) that will contain the necessary OpenStack components
to deploy and manage an overcloud (an actual tenant facing workload cloud).
The overcloud is the deployed solution and can represent a cloud for any
purpose (e.g. production, staging, test, etc). The operator can choose any of
available Overcloud Roles (OpenStack components) they want to deploy to his
environment.

Go to RDO-Manager Architecture to learn more.


## Components

RDO-Manager is composed of set of official OpenStack components accompanied by
few other open sourced plugins which are increasing RDO-Manager capabilities.

Go to RDO-Manager Components to learn more.

[RDO-Manager]: https://repos.fedorapeople.org/repos/openstack-m/docs/master/index.html
