### yy33/h52m dump_gui
#### 概览
- yy33的合图主要功能包括
  - 将目录中的散图合为合图，即生成plist和png文件
  - 将csd中引用的图合为合图
    - 独立界面与非独立界面
    - 复用的图片与非复用图片，按照使用次数区分，次数多的单独合图，次数少的按照csd文件合图
  - 将csd中引用的图片名称矫正，多语言涉及
  - 使用Cocos.Tool将csd发布为csb
  - 生成common_plists文件以及json文件
- 使用Chef将图片资源载入引擎Package内
#### 配置项mdir
- mdir，子目录都使用同样配置，但是注意只有一层子目录，合图名会加上子目录名称

>- icon
>  - skills
>    - s1001.png
>    - 1002
>      - s1002.png
>  - buffs
>    - b1001.png

##### 不启用mdir

以上如果将目录设置为icon，不使用mdir设置，相当于直接将icon路径传递给了TexturePacker，由于其本身也会递归遍历文件夹，所以内部的图片也是能够进合图的，合图后的文件如下：

>- icon0.plist
>  - <key>skills/s1001.png</key>
>  - <key>skills/1002/s1002.png</key>
>  - <key>buffs/b1001.png</key>

相当于所有的路径都从icons为相对路径起点

##### 启用mdir

>- icon_skills0.plist
>  - <key>s1001.png</key>
>  - <key>1002/s1002.png</key>
>- icon_buffs0.plist
>  - <key>b1001.png</key>

其实就是把icons每个子文件夹传到TexturePacker里了

#### 配置项mlang

- mlang多语言设置，根据传入的参数（common/chs/eng/...）只打包部分子文件夹中的散图

- 多语言生成的plist中还是会带有eng/chs/common前缀，在language_files_map.json中会有语言->plist/png的记录，暂时没理解怎么处理。

  - 我期望的结果，对于以下的文件结构loadTexture(a.png)能够在不同语言包下返回不同的图片，但是生成的plist文件中带有文件夹前缀，和不用mlang选项时的导出也一直，怀疑时通过json文件做后处理吗？

  >- icon
  >    - chs
  >      - a.png
  >    - eng
  >      - a.png
  >    - common
  >      - common.png
  >

#### 配置项method

- 包含merge/single/copy三种
  - merge将文件下的所有图片合图
  - single对文件夹下的每张图片合图
  - copy原样复制文件夹下的内容