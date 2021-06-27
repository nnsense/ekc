## ekc: what is

This is a very small script that scans all the AWS regions into the current account or another one (using `-p`) for EKS clusters and creates the right command line to configure `~/.kube/config`.

It can also list (`-l` or `--list`) the clusters it founds with more details.

It requires `aws cli` to be configured and working.


## Arguments

`-r` or `--region` if you want to spped up the scan targeting only one region
`-p` or `--profile` to use a different `aws cli` profile.
`-l` or `--list` to list details about the cluster(s) (instead of running the kube update command)
`-t` or `--tag` can optionally be used to add one tag to the details you get from `--list`
`--role-arn` is required when using `--profile` to create the kubectl command line including the `--role-arn` bit
`--kubeconfig` allows to set a custom kube config file, by default the tool configures the user's `~/.kube/config`


## Example

Run `ekc` without any argument to scan all the regions of the currently `aws cli` configured account and output the right commands to update `~/.kube/config`

Run `ekc -r eu-west-1` to scan the EKS clusters into the Ireland region or `ekc -r eu-west-1 -p engineering` to scan clusters in Ireland using the `aws cli` profile "engineering".

Run `ekc -r eu-west-1 -l` to list the clusters along some details, eg:

```
+----------------------------------+-----------+----------------------+-------------+
| Name                             | Region    | CreationDate         | K8S Version |
+----------------------------------+-----------+----------------------+-------------+
| dev-mycluster-380245-cluster     | eu-west-1 | 2021-02-20, 16:29:06 | 1.19        |
| dev-mycluster-558933-cluster     | eu-west-1 | 2021-02-23, 10:28:22 | 1.19        |
| dev-mycluster-412415-cluster     | eu-west-1 | 2021-02-16, 13:25:01 | 1.19        |
| dev-mycluster-089125-cluster     | eu-west-1 | 2021-02-27, 10:52:29 | 1.19        |
| dev-mycluster-270438-cluster     | eu-west-1 | 2021-02-24, 15:03:49 | 1.19        |
| dev-mycluster-128141-cluster     | eu-west-1 | 2021-02-23, 14:24:47 | 1.19        |
+----------------------------------+-----------+----------------------+-------------+
```

Run `ekc -r eu-west-1 -lt MyTag` to get the same details as above along the cluster's tag having the `key` "MyTag", eg:

```
+----------------------------------+-----------+----------------------+-------------+-------------------+
| Name                             | Region    | CreationDate         | K8S Version | MyTag             |
+----------------------------------+-----------+----------------------+-------------+-------------------+
| dev-mycluster-380245-cluster     | eu-west-1 | 2021-02-20, 16:29:06 | 1.19        | True              |
| dev-mycluster-558933-cluster     | eu-west-1 | 2021-02-23, 10:28:22 | 1.19        | True              |
| dev-mycluster-412415-cluster     | eu-west-1 | 2021-02-16, 13:25:01 | 1.19        | False             |
| dev-mycluster-089125-cluster     | eu-west-1 | 2021-02-27, 10:52:29 | 1.19        | False             |
| dev-mycluster-270438-cluster     | eu-west-1 | 2021-02-24, 15:03:49 | 1.19        | True              |
| dev-mycluster-128141-cluster     | eu-west-1 | 2021-02-23, 14:24:47 | 1.19        | False             |
+----------------------------------+-----------+----------------------+-------------+-------------------+
```


# ekc-cleanup

This is pretty close to the ekc tool, but it search for a string into each kuberentes deployment and reports back a list of the clusters found along the presence of the string among the pods.
Its purpose is to keep the deployments under control, to see how many have a specific deployment in place (say you have multiple clusters but some have left without anything running and you want to know what can be deleted.

It has the same `-r` switch for the region to search and `-p` to use a different aws profile as the ekc script, and it requires `-s` for the string to search into the output of a `kubectl get pods` command. The output is to stdout by default, or to a file by setting `--file somefile.log`.

Example:
```
> ekc-cleanup -s myapp
eu-west-1/first-cluster (Created by Godzilla on 2021-04-16, 15:31:35) - myapp found
us-east-1/second-cluster (Created by Pluto on 2021-04-16, 13:06:09) - myapp pound
```
