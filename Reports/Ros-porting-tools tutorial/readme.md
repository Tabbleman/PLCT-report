# ros-porting-tools

Start
-----

ros porting tools本仓库用于自动引入ros到openEuler上。

项目结构

```text-plain
[root@localhost ros-porting-tools]# tree -L 1
.
├── base.sh
├── bb_fix
├── change-obs-packages-to-gitee.sh
├── check-build-status-in-obs.sh
├── clone-projects-from-gitee.sh
├── config
├── create-ebs-projects.sh
├── create-graph-deps-for-check-list.sh
├── create-pr-in-gitee.sh
├── create-release-management.sh
├── create-report.sh
├── create-ros-3rdparty-in-obs.sh
├── create-ros-projects-in-gitee.sh
├── create-ros-projects-in-obs.sh
├── create-ros-yaml-for-gitee.sh
├── doc
├── download-obs-projects.sh
├── gen-pkg-bb.sh
├── gen-pkg-ext-bb.sh
├── gen-pkg-spec.sh
├── get-deps-src.sh
├── get-deps-src-yum.sh
├── get-epol.sh
├── get-license.py
├── get-package-xml.py
├── get-pkg-deps.sh
├── get-pkg-src.sh
├── get-ref-count.sh
├── get-repo-list.sh
├── get-src-from-github.sh
├── get-src-from-ubuntu.sh
├── LICENSE
├── output
├── package_fix
├── push-projects-to-gitee.sh
├── push-projects-to-obs.sh
├── README.en.md
├── README.md
├── ros
├── spec_fix
├── template
└── trigger-projects-build-in-obs.sh
```

Usage
-----

### 基本

```text-plain
[root@localhost ros-porting-tools]# ./get-repo-list.sh  获得列表
Tue Aug 20 06:50:54 AM UTC 2024 [Info ] find /root/ros-porting-tools/ros/humble/ros-projects.list
.......Tue Aug 20 06:50:57 AM UTC 2024 [Info ] affordance-primitives is unknown or disabled, ignore
........
```

执行完之后会在output出一个

```text-plain
[root@localhost ros-porting-tools]# head output/ros.repos 
repositories:
  acado_vendor:
    type: git
    url: https://gitlab.com/autowarefoundation/autoware.auto/acado_vendor.git
    version: main
  ackermann_msgs:
    type: git
    url: https://github.com/ros-drivers/ackermann_msgs.git
    version: ros2
 ...
```

之后在`ros/humble/ros-projects.list`里面，获得要修的包的url。

```text-plain
[root@localhost ros-porting-tools]# ./get-repo-list.sh ros/humble/ros-projects.list 
Tue Aug 20 06:58:44 AM UTC 2024 [Info ] find /root/ros-porting-tools/ros/humble/ros-projects.list
.......Tue Aug 20 06:58:45 AM UTC 2024 [Info ] affordance-primitives is unknown or disabled, ignore
..................................................................................................................Tue Aug 20 06:59:51 AM UTC 2024 [Info ] irobot-create-msgs is unknown or disabled, ignore
..........
```

然后下载vcs tool`pip install vcstool`，使用`./get-src-from-github.sh output/ros.repos` 下载仓库到本地。

```text-plain
[root@localhost ros-porting-tools]# ./get-src-from-github.sh output/ros.repos 
Tue Aug 20 08:10:23 AM UTC 2024 [Info ] Start to download ros-pkg.
```

然后可以通过`gen-pkg-spec.sh output/xxx` （xxx是包名）

生成对应的放在eulermaker ci/cd的spec文件。

\[ref\]: [ros-porting-tools](https://gitee.com/openeuler/ros-porting-tools/tree/humble/#https://gitee.com/link?target=http%3A%2F%2Frepo.ros2.org%2Fstatus_page%2Fros_humble_default.html%25E9%25A1%25B5%25E9%259D%25A2%25E8%258E%25B7%25E5%258F%2596%25E8%25BD%25AF%25E4%25BB%25B6%25E5%258C%2585%25E5%2590%258D%25E3%2580%2581%25E4%25BB%2593%25E5%25BA%2593%25E5%259C%25B0%25E5%259D%2580%25E3%2580%2581%25E8%25BD%25AF%25E4%25BB%25B6%25E5%258C%2585%25E7%258A%25B6%25E6%2580%2581%25E3%2580%2581%25E7%2589%2588%25E6%259C%25AC%25E5%258F%25B7)