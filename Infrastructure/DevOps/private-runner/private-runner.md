# Github Private Runner Setting

Source:

[Hosting your own runners](https://docs.github.com/en/actions/hosting-your-own-runners)

[Adding self-hosted runners](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/adding-self-hosted-runners)

## User setting

private sunner mostly configured with ahell script, the script **CANNOT** run wuth sudo, so we must create a runner user for it.

### Create runner-user

```cmd
# create user
sudo useradd -m github_runner
sudo passwd github_runner

# setup folder
sudo mkdir actions-runner
sudo chown github_runner:github_runner /data/actions-runner

su - github_runner
```

## Install

### Download

```bash
# Create a folder
mkdir actions-runner && cd actions-runner

# Download the latest runner package
curl -o actions-runner-linux-x64-2.321.0.tar.gz -L https://github.com/actions/runner/releases/download/v2.321.0/actions-runner-linux-x64-2.321.0.tar.gz

# Optional: Validate the hash
echo "ba46ba7ce3a4d7236b16fbe44419fb453bc08f866b24f04d549ec89f1722a29e  actions-runner-linux-x64-2.321.0.tar.gz" | shasum -a 256 -c

# Extract the installer
tar xzf ./actions-runner-linux-x64-2.321.0.tar.gz
```

### Configure

```bash
# Create the runner and start the configuration experience
# 這個 token 就在 github page 上
./config.sh --url https://github.com/Rough-for-Rough --token {token}
# Last step, run it!
./run.sh
```
