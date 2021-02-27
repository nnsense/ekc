# ekc: what is

This is a very small script that scans all the AWS regions into the current account or another one (using `-p`) for EKS clusters and creates the right command line to configure `~/.kube/config`.

It can also list (`-l` or `--list`) the clusters it founds with more details.

It requires `aws cli` to be configured and working.

## Arguments

`-r` or `--region` if you want to spped up the scan targeting only one region
`-p` or `--profile` to use a different `aws cli` profile.
`-l` or `--list` to list details about the cluster(s) (instead of running the kube update command)
`-t` or `--tag` can optionally be used to add one tag to the details you get from `--list`

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

That's all ;)
