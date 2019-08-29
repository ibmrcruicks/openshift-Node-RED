# Background information for Node in OpenShift

The OpenShift `nodejs` cartridge documentation can be found at:

http://openshift.github.io/documentation/oo_cartridge_guide.html#nodejs

# get minishift running
- `minishift --vm-driver=hyperkit start` (or equivalent)
- `eval $(minishift oc-env)`
- `oc login -u developer`

# openshift-Node-RED

## starter repo
git clone the [starter repo](https://github.com/emarilly/openshift-node-red)
- `git clone https://github.com/emarilly/openshift-node-red`

change directory into the openshift-noe-red folder

## update package.json
edit `package.json`:
- update the node.js engine to [current LTS](https://nodejs.org/en/download/) (`>=10.0.0`)
- update the npm engine to be inline with node LTS (`>= 6.0.0`)
- update node-red dependency to current levels (`>= 0.20.0`)
- add a scripts section
  ```
  "scripts": {
    "build": "true",
    "start": "node server.js"
  },
  ```

## update server.js
edit `server.js`:
- comment out basic authentication setup
  ```
  //setup basic authentication
  //var basicAuth = require('basic-auth-connect');
  //self.app.use(basicAuth(function(user, pass) {
  //    return user === 'test' && pass === atob('dGVzdA==');
  //}));
  ```
- update local port to `8080`
  ```
  self.port      = process.env.OPENSHIFT_NODEJS_PORT || 8080;
  ```
- update default ipaddress to "0.0.0.0"
  ```
  self.ipaddress = process.env.OPENSHIFT_NODEJS_IP || "0.0.0.0";
  ```
- npm install

# push node-red into OpenShift
- `oc new-app nodejs~./ --name=myfirst-node-red`
- `oc start-build myfirst-node-red --from-dir=./`
- run `oc status` until the build is complete
- `oc get service myfirst-node-red`
- `oc expose service myfirst-node-red`

use the response from
- `oc get route/myfirst-node-red`

to identify the URL for you first Node-RED application in Openshift
