Jobs
====

**name limitations**
********************

job 的名稱命名有一些限制，主要是 Gitlab 的 keyword 名稱，字元必須小於 255 characters，如果 job 的名稱有重複的話，只會挑選其中一個做執行，沒有辦法去預測 Pipeline 會去選擇哪個做執行

無法命名的 keyword：
  - image
  - services
  - stages
  - types
  - before_script
  - after_script
  - variables
  - cache
  - include
  - true
  - false
  - nil

**Hide jobs**
*************

如果想要暫時停止 job 但是要保留內容在 configuration 裡面有兩個方式

- Comment out jobs

.. code-block:: yaml

  # hidden_job:
  #   script:
  #     - run test

- 將 job 的名稱以 dot(.) 為開頭，Gitlab CI/CD 則不會去執行它

.. code-block:: yaml

  .hidden_job:
    script:
      - run test

- 如果是更進階的使用，可以使用 extends keyword 去操作，類似 inheritance 的概念，如以下

.. code-block:: yaml

  .test:
    script: echo 'test'
    stage: test
    only:
      refs:
        - branches

  build:
    extends: .test
    script: echo 'build extend .test job'
    only:
     variables:
      - $BUILD

翻譯結果為以下:

.. code-block:: yaml

  build:
    stage: test
    script: echo 'build extend .test job'
    only:
      refs:
        - branches
      variables:
        - $BUILD

**inheritance Part1**
*********************

可以透過 inherit keyword 讓 job 去繼承 default keyword 或是變數

  .. code-block:: yaml

    default:
      image: 'ruby:2.4'
      before_script:
        - echo Hello World

    variables:
      DOMAIN: example.com
      WEBHOOK_URL: https://my-webhook.example.com

    test1:
      inherit:
        default: false
        variables: false
      script: bundle exec rubocop

    test2:
      inherit:
        default: [image]
        variables: [WEBHOOK_URL]
      script: bundle exec rspec

    test3:
      inherit:
        variables: false
      script: bundle exec capybara

    test4:
      inherit:
        default: true
        variables: [DOMAIN]
      script: karma

- test1:

  * 沒有繼承任何內容，因為 default, variables 都是 false

- test2:

  * 繼承 default:image, variables:WEBHOOK_URL 兩個內容

- test3:

  * 繼承 default:before_script, default:image

- test4:

  * 繼承 default:before_script, default:image, variables:DOMAIN


.. note::

  透過上述官方 `範例Part1 <https://docs.gitlab.com/ee/ci/jobs/#control-the-inheritance-of-default-keywords-and-global-variables>`_ 內容可以得知，如果 job 裡面有描述到 inherit keyword 而同時如果 configuration 有 default keyword, variables content 內容，則會繼承相關內容，透過 test3 job 觀察，它並沒有特別 ``default :true`` 但同樣還是會繼承 default keyword

**inheritance Part2**
*********************

如果今天不是使用 inherit keyword 而是單純設定 default keyword 的方式會如何呢

.. code-block:: yaml

  default:
    before_script:
      - echo "Execute this `before_script` in all jobs by default."
    after_script:
      - echo "Execute this `after_script` in all jobs by default."

  job1:
    script:
      - echo "These script commands execute after the default `before_script`,"
      - echo "and before the default `after_script`."

  job2:
    before_script:
      - echo "Execute this script instead of the default `before_script`."
    script:
      - echo "This script executes after the job's `before_script`,"
      - echo "but the job does not use the default `after_script`."
    after_script: []

.. note::

  透過上述官方 `範例Part2 <https://docs.gitlab.com/ee/ci/yaml/script.html#set-a-default-before_script-or-after_script-for-all-jobs>`_，before_script 會在 job1 的 script 前先執行，after_script 則是 script 之後，如果要覆蓋掉 default 的話可以像 job2 額外再寫一次 before_script 去執行，而如果只是單純想要 job 忽略掉 default keyword 的內容可以給空 list，如同 job2 的 ``after_script: []``
