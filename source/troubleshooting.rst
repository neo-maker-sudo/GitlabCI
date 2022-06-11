TroubleShooting
===============

- 如果 pipeline 裡面出現 timeout 或是 job stuck 基本上多種情況，但大致的問題主因會是 Gitlab 沒有辦法抓到相對應的 Runner，或是抓到了 Runner 但無法執行，以下列舉，目前遇到之情況

  1. pipeline 有設定 tags 標籤去抓特定的 runner 執行 project CI/CD，但是因 configuration 沒有寫 tags keywords 所以無法執行。

  Problem: pipeline 顯示畫面
    .. image:: _static/troubleshooting_01.png

    .. rst-class:: image-source

    Photo by `Neo Change Gitlab Account`

  Solution: 把 tags 加到 configuration 裡面即可

    .. image:: _static/troubleshooting_02.png

    .. rst-class:: image-source

    Photo by `Neo Change Gitlab Account`

  2. remote server 的 gitlab-runner 並沒有啟動，導致 pipeline 直接 failed

  Problem: pipeline 顯示畫面
    .. image:: _static/troubleshooting_03.png

    .. rst-class:: image-source

    Photo by `Neo Change Gitlab Account`

  Solution: 連線到 remote server，執行以下指令確認 gitlab-runner 是否啟用

  .. code-block:: console

    sudo systemctl status gitlab-runner

  如果不是顯示綠色的 Active 字眼代表服務並未啟用，如下圖

  .. image:: _static/troubleshooting_04.png

  .. rst-class:: image-source

  Photo by `Neo Change Gitlab Account`

  接下來有兩個方式，前景或是背景執行

  前景執行

  .. code-block:: console

    sudo gitlab-runner run

  .. image:: _static/troubleshooting_05.png

  .. rst-class:: image-source

  Photo by `Neo Change Gitlab Account`

  背景執行

  .. code-block:: console

    sudo systemctl start gitlab-runner

    # 如果想要之後開機自動啟用 gitlab-runner 的話，可以執行這個指令
    sudo systemctl enable gitlab-runner

    sudo systemctl restart gitlab-runner

  .. Tip::
    如果是使用 windows 的 wsl 的話，輸入 systemctl 指令可能會出現相關錯誤訊息，意思是代表 Linux 沒有使用 systemd，可以 `參考 <https://ithelp.ithome.com.tw/articles/10255920>`_ 去排除問題

    .. code-block:: console

        System has not been booted with systemd as init system (PID 1). Can't operate.
        Failed to connect to bus: Host is down

- Gitlab run 的時候沒有辦法印出 log，確認使用的 gitlab runner 的 version，差距過大會有問題，需要把 server 上的 gitlab runner package delete 掉，然後再使用 curl 下載最新版本的 package
