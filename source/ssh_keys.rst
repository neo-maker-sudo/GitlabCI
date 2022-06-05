SSH KEYS
========

1. 使用 ssh-keygen 在本地端新增 SSH Key
2. 將私鑰存在 project 的 Variable
3. 執行 job 期間跑 ssh-agent 加載私鑰 
4. 複製公鑰到 cloud server（usually 放到 ~/.ssh/authorized_keys），或是如果你要使用 private GitLab repository 可以加入到 project 的 deploy key

**Docker executor**
*******************

當 CI/CD 的 job 執行在 docker 而你也想要部屬 source code 到 private cloud server，就必須使用 ssh key pair 方式

1. 新增 SSH Key pair，新增時密鑰不鑰使用 passphrase，不然在執行時會跳出輸入 passphrase 的 input

2. 新增 CI/CD Variable，key 值給 SSH_PRIVATE_KEY，value 值則是剛剛新增 SSH KEY pair 的私鑰

3. 修改 .gitlab-ci.yml 檔案 before_script 的內容

4. 確認 SSH host keys 已經被驗證過

5. 新增公鑰的 remote server，如果使用 private GitLab repository 要到 project 的 deploy key

以下為 Giltab 範例：

.. code-block:: yaml

    before_script:
    ##
    ## Install ssh-agent if not already installed, it is required by Docker.
    ## (change apt-get to yum if you use an RPM-based image)
    ##
    - 'command -v ssh-agent >/dev/null || ( apt-get update -y && apt-get install openssh-client -y )'

    ##
    ## Run ssh-agent (inside the build environment)
    ##
    - eval $(ssh-agent -s)

    ##
    ## Add the SSH key stored in SSH_PRIVATE_KEY variable to the agent store
    ## We're using tr to fix line endings which makes ed25519 keys work
    ## without extra base64 encoding.
    ## https://gitlab.com/gitlab-examples/ssh-private-key/issues/1#note_48526556
    ##
    - echo "$SSH_PRIVATE_KEY" | tr -d '\r' | ssh-add -

    ##
    ## Create the SSH directory and give it the right permissions
    ##
    - mkdir -p ~/.ssh
    - chmod 700 ~/.ssh

    ##
    ## Optionally, if you will be using any Git commands, set the user name and
    ## and email.
    ##
    # - git config --global user.email "user@example.com"
    # - git config --global user.name "User name"


**Shell executor**
******************

如果是單純使用 Shell executor，設定 SSH Key 相對比較簡單，從安裝好 Gitlab Runner 的 cloud server 上新增 SSH Key，使用 SSH Key 在所有的 projects 上

1. 連線到 cloud server 執行 job

.. code-block:: shell

    ssh root@<domain-name>

2. 切換 gitlab-runner 身分

.. code-block:: shell

    sudo su - gitlab-runner

3. 新增 SSH Key pair，不要加 passphrase

4. 新增公鑰的 remote server，如果使用 private GitLab repository 要到 project 的 deploy key
