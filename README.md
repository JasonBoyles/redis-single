Description
===========

This is a template to deploy a [Redis](http://redis.io/) server with [OpenStack
Heat](https://wiki.openstack.org/wiki/Heat) on the [Rackspace
Cloud](http://www.rackspace.com/cloud/). This template configures the server
using [chef-solo](http://docs.opscode.com/chef_solo.html).

Requirements
============
* A Heat provider that supports the Rackspace `OS::Heat::ChefSolo` plugin.
* An OpenStack username, password, and tenant id.
* [python-heatclient](https://github.com/openstack/python-heatclient)
`>= v0.2.8`:

```bash
pip install python-heatclient
```

We recommend installing the client within a [Python virtual
environment](http://www.virtualenv.org/).

Example Usage
=============
Here is an example of how to deploy this template using the
[python-heatclient](https://github.com/openstack/python-heatclient):

```
heat --os-username <OS-USERNAME> --os-password <OS-PASSWORD> --os-tenant-id \
  <TENANT-ID> --os-auth-url https://identity.api.rackspacecloud.com/v2.0/ \
  stack-create redis-server -f redis-single.yaml -P redis-version=v2.8.9
```

* For UK customers, use `https://lon.identity.api.rackspacecloud.com/v2.0/` as
the `--os-auth-url`.

Optionally, set environmental variables to avoid needing to provide these
values every time a call is made:

```
export OS_USERNAME=<USERNAME>
export OS_PASSWORD=<PASSWORD>
export OS_TENANT_ID=<TENANT-ID>
export OS_AUTH_URL=<AUTH-URL>
```

Parameters
==========
Parameters can be replaced with your own values when standing up a stack. Use
the `-P` flag to specify a custom parameter.

* `server_flavor`: Sets the flavor of the created servers. (Default: 1 GB Performance)
* `image`: Operating system to install (Default: Ubuntu 12.04 LTS (Precise
  Pangolin))
* `redis_version`: version of redis to install (Default: 2.8.9)
* `redis_hostname`: Hostname for initial master server. (Default: redis)
* `ssh_keypair_name`: The nova keypair name to create (Default: redis)
* `redis_port`: port the redis server process listens on (Default: 6379)


Outputs
=======
Once a stack comes online, use `heat output-list` to see all available outputs.
Use `heat output-show <OUTPUT NAME>` to get the value for a specific output.

* `private_key`: SSH private key that can be used to login as root
* `redis_password`: Password for redis access
* `redis_server_ip`: Public IP address of the initial redis master server
* `redis_port` : List of public IP addresses for the initial slave servers

For multi-line values, the response will come in an escaped form. To get rid of
the escapes, use `echo -e '<STRING>' > file.txt`. For vim users, a substitution
can be done within a file using `%s/\\n/\r/g`.

Stack Details
=============
By default redis runs on port 6379 and is configured to require password
authentication. The password is available in the `redis_password` output.

Contributing
============
There are substantial changes still happening within the [OpenStack
Heat](https://wiki.openstack.org/wiki/Heat) project. Template contribution
guidelines will be drafted in the near future.

License
=======
```
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
