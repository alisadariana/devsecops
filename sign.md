# Signature Guide

## Step 1: Check if you have an SSH key

```bash
ls -la ~/.ssh/
```

Look for files like `id_ed25519` and `id_ed25519.pub` (or `id_rsa` and `id_rsa.pub`).

## Step 2: If you don't have an SSH key, create one

```bash
ssh-keygen -t ed25519 -C "your-email@example.com"
```

Press Enter to accept the default location, and optionally set a passphrase.

## Step 3: Configure Git to use SSH signing

```bash
# Set your signing format to SSH
git config --global gpg.format ssh

# Tell Git which SSH key to use for signing
git config --global user.signingkey ~/.ssh/id_ed25519.pub

# Enable commit signing
git config --global commit.gpgsign true
```

## Step 4: Create the allowed signers file

```bash
# Create the file
touch ~/.ssh/allowed_signers

# Add your public key to it (this must be one line)
echo "$(git config user.email) $(cat ~/.ssh/id_ed25519.pub)" > ~/.ssh/allowed_signers
```

## Step 5: Tell Git where to find the allowed signers file

```bash
git config --global gpg.ssh.allowedSignersFile ~/.ssh/allowed_signers
```

## Step 6: Verify everything is configured correctly

```bash
# Check your Git config
git config --global --list | grep -E 'user.email|user.signingkey|gpg|commit.gpgsign'

# Check the allowed_signers file
cat ~/.ssh/allowed_signers
```

## Step 7: Test with a commit

```bash
# Make a test commit
git commit --allow-empty -m "Test signed commit"

# Verify the signature
git log --show-signature -1
```
