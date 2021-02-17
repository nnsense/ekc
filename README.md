# ekc: what is

This is a very small script that scans all the AWS regions, into the current account or another one for EKS clusters and creates the right command line to configure `~/.kube/config`.

It requires `aws cli` to be configured and working.

It accept two arguments:

`-r` or `--region` if you want to spped up the scan targeting only one region
`-p` or `--profile` to use a different `aws cli` profile.

## Example

Run `ekc` without any argument to scan all the regions of the currently configured account.

Run `ekc -r eu-west-1` to list the EKS clusters into the Ireland region.

Run `ekc -r eu-west-1 -p engineering` to list only clusters into Ireland region using the profile "engineering"

That's all ;)
