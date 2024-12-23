This should be a **signed** commit using GPG.
This is commited but not signed.
This is a signed commit using GPG.
Configured git to sign all commits by default using the command below
```bash
git config --global commit.gpgsign true
```
All commits from here on are signed commits.

**Now...** changed commit signing mode to ssh from gpg
Also change git default commit sign from true (or automatic) to manual using the command below:
```bash
git config --global --unset commit.gpgsign
```

This commit is set to ssh signing but not signed.
This commit is now signed with ssh.
**Now...** configured git to sign all commits by default.

**Now...** I have changed commit signing format back to gpg using the following commands:
```bash
git config --global --unset gpg.format
gpg --list-secret-keys --keyid-format=LONG
# copy the secret primary key of the gpg key-pair and use it to change the signing key
git config --global user.signingkey [secret-primary-key]
```
This commit is signed using gpg.