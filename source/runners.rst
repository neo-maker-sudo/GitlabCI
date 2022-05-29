Runners
=======

scope
-----

  .. |scope| raw:: html

    <br />

Gitlab CI/CD 的 runners 主要有三種分別是 ``Shared runners`` , ``Group runners``, ``Specific runners``

- Shared runners 適用於 Gitlab 裡面全部的 groups, projects
- Group runners 適用於 Gitlab 裡面全部的 projects 以及 group 底下的 subgroup
- Specific runners 適用於 Gitlab 裡面特定的 project, 一般來說是針對單一個 project。

**Shared runners**
******************

Shared runners 主要用於如果今天有多個專案所執行的 jobs 是雷同，以及相似的需求，就可以使用 Shared runners 去做 CI/CD 的任務執行，與其使用多個 runner 空閒在多個專案之間，使用這樣的方式去做執行也是不錯的選擇。

- 如果使用 Gitlab 自架服務

  * 管理者可以到 Project 的 `Settings` -> `CI/CD` -> `Runners` 安裝以及註冊 shared runners，點擊 ``Show runner installation instructions`` 可以看到相關系統安裝指令。

  .. image:: _static/share_runner_1.png

  .. rst-class:: image-source

  Photo by `fuguFish Creation Gitlab Account`

  .. image:: _static/share_runner_2.png

  .. rst-class:: image-source

  Photo by `Neo Change Gitlab Account`

  * 管理者也可以知道針對於每一個 group 的 `shared runners CI/CD 的上限時間 <https://docs.gitlab.com/ee/ci/pipelines/cicd_minutes.html#set-the-quota-of-cicd-minutes-for-a-specific-namespace>`_

- 如果使用 Gitlab.com 服務

  * You can select from a list of shared runners that GitLab maintains.

  * The shared runners consume the CI/CD minutes included with your account.


**Enable for a project**
########################

如果使用 Gitlab.com 服務的話 shared runners 在所有的 projects 預設都是 enabled

如果使用 Gitlab 自架服務，管理者可以自行決定對於所有的 projects 是否要 enabled shared runners

- 可以到以下範例圖片查看設定

  .. image:: _static/share_runner_for_project_1.png

  .. rst-class:: image-source

  Photo by `fuguFish Creation Gitlab Account`


  .. image:: _static/share_runner_for_project_2.png

  .. rst-class:: image-source

  Photo by `Neo Change Gitlab Account`

- 如果針對 project 做 enabled 的話，到 `Settings` -> `CI/CD` -> `Runners`，選擇 ``Enable shared runners for this project``

  .. image:: _static/share_runner_for_project_3.png

  .. rst-class:: image-source

  Photo by `fuguFish Creation Gitlab Account`

**Enable for a group**
######################

- 到 group 的 `Settings` -> `CI/CD` -> `Runners`，選擇 ``Enable shared runners``

.. image:: _static/share_runner_for_project_4.png

.. rst-class:: image-source

Photo by `fuguFish Creation Gitlab Group`

**Disable for a project**
#########################

-  到 project 的 `Settings` -> `CI/CD` -> `Runners`，選擇 ``Enable shared runners for this project``

.. image:: _static/share_runner_for_project_5.png

.. rst-class:: image-source

Photo by `fuguFish Creation Gitlab Project`

**Disable for a group**
#######################

- 到 group 的 `Settings` -> `CI/CD` -> `Runners`，選擇 ``Enable shared runners``

.. image:: _static/share_runner_for_project_6.png

.. rst-class:: image-source

Photo by `Neo Change Gitlab Account`

**Group runners**
*****************

**Create a group runner**
#########################

1. `安裝 Gitlab Runner <https://docs.gitlab.com/runner/install/linux-manually.html>`_
2. 選擇要執行的 group
3. Sidebar 點選 `CI/CD` -> `Runners`
4. 記住 URL 以及 token，如果是使用 Gitlab.com 的服務 URL 會是 ``https://gitlab.com/``，自架服務則是看設定的 Domain

.. Tip::
  # 官方說明
  When registering a runner on GitLab.com, the gitlab-ci coordinator URL is https://gitlab.com.

.. image:: _static/group_runner_1.png

.. rst-class:: image-source

Photo by `Neo Change Gitlab Account`

5. `註冊 runner <https://docs.gitlab.com/runner/register/>`_

**View and manage**
###################

1. 選擇要查看的 group
2. Sidebar 點選 `CI/CD` -> `Runners`

.. image:: _static/group_runner_2.png

.. rst-class:: image-source

Photo by `fuguFish Creation Gitlab Group`

**Specific runners**
********************

**Create a specific runner**
############################

1. `安裝 Gitlab Runner <https://docs.gitlab.com/runner/install/linux-manually.html>`_
2. 選擇要執行的 project
3. Sidebar 點選 `Settings` -> `CI/CD` -> `Runners`
4. 記住 URL 以及 token，如果是使用 Gitlab.com 的服務 URL 會是 ``https://gitlab.com/``，自架服務則是看設定的 Domain

.. Tip::
  # 官方說明
  When registering a runner on GitLab.com, the gitlab-ci coordinator URL is https://gitlab.com.

.. image:: _static/specific_runner_1.png

.. rst-class:: image-source

Photo by `Neo Change Gitlab Account`


5. `註冊 runner <https://docs.gitlab.com/runner/register/>`_

register a runner
-----------------

runner executors
----------------
