# CI Retry

Retry failed jobs when a GitHub Action of GitLab pipeline failed.

## Setup

Please store your `github_token` and `gitlab_token` in a Ansible (vault) file:

You can also pass these values using the `--extra-vars` option.

You can store the password to the Ansible Vault file locally:

```shell
echo "MyPaSsWoRd" > /tmp/vault-pass
```

## Usage

You can retry a single role:

```shell
./github.yml --ask-vault-pass -e github_repo="ansible-role-foo"
./gitlab.yml --ask-vault-pass -e gitlab_project="ansible-role-foo"
```

To loop over all roles:

```shell
test -f /tmp/vault-pass || exit 1
for role in ../ansible-role-* ; do
  basename "${role}"
done | while read role ; do
  echo "${role}"
  ./github.yml --vault-password-file=/tmp/vault-pass -e github_repo="${role}"
  ./gitlab.yml --vault-password-file=/tmp/vault-pass -e gitlab_project="${role}"
  sleep 3
done
```
