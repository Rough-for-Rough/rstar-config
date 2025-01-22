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

## Test

```yml
name: Maven Verify on Commit

on:
  push:
    branches:
      - main # 或指定其他分支

jobs:
  maven-verify:
    runs-on: [self-hosted, rstar] # 替換為你的 Private Runner 標籤

    steps:
      # 檢出專案程式碼
      - name: Checkout code
        uses: actions/checkout@v3

      # 設定 Java 環境（替換成你的 Java 版本）
      - name: Set up JDK 17
        uses: actions/setup-java@v3
        with:
          java-version: "17"
          distribution: "temurin" # 指定 JDK 分發版本（Adoptium 提供的 OpenJDK）
          cache: "maven" # 啟用 Maven 依賴快取

      # 執行 Maven Verify 和 Package
      - name: Build and Package JAR
        run: |
          mvn verify
          mvn package

      # 移動 JAR 檔案到目標資料夾
      - name: Move JAR to Target Folder
        run: |
          mkdir -p /data/runner-data/rstar
          mv target/*.jar /data/runner-data/rstar/
```

## SonarQube

在首頁點選 Add Project，選擇 Manual。

制定 Project Key 和 Token，後續將生成的 Script 加到 pipeline 內。

```yml
# 透過 Maven 執行 SonarQube 分析
- name: Run SonarQube analysis
  run: |
    mvn verify sonar:sonar \
      -Dsonar.projectKey=pipeline-test \
      -Dsonar.host.url=http://192.168.1.200:9000 \
      -Dsonar.login={token}
```
