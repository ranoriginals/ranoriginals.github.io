# SEM3D使用记录


## SPECFEM3D 使用记录

以SPECFEM3D内置的mesh生成程序xmeshfem3D为例

示例路径：`SPECFEM3D/EXAMPLES/meshfem3d_examples/cavity/DATA/meshfem3D_files`


#### Mesh_Par_file的具体参数

```bash
LATITUDE/LONGITUDE = # 如果是地理坐标系，则要以十进制的小数给出经纬度，不应使用度分秒的形式
                     # 如果是笛卡尔坐标系，则是给定X、Y方向的长度，单位是米
SUPPRESS_UTM_PROJECTION = .true.  # 模型建立在笛卡尔坐标系下，longtitude对应x方向，latitude对应y方向
                        = .false. # 模型建立在地理坐标系下
DEPTH_BLOCK_KM          =         # 单位是KM，注意与SPECFEM2D不同的是，这里建立模型使用的是海拔，即地 
                                  # 表为0，一定深度为负值，写interfaces.dat要注意这一点。
USE_REGULAR_MESH        = .false. # 模型内的网格大小不同，存在doublings
                        = .true.  # 模型内的网格大小一致，不存在doublings
INTERFACES_FILE         =  interfaces.dat # 界面文件
```

#### intefaces.dat

```bash
# intefaces.dat写法，与SPECFEM2D不同的是：SPECFEM2D定义4层需要给出5个界面，假设模型Z方向有10km，则10km深处为Z值为0，地表的Z值为10000；SPECFEM3D定义4层给出4个界面文件就可以，最底的界面不需要给出，而且地表的Z值为0，一定深度的界面Z值为负值。
e.g:
# interface number 1
# SUPPRESS_UTM_PROJECTION  NXI  NETA LONG_MIN   LAT_MIN    SPACING_XI SPACING_ETA
  .true.                    2    2     0.d0       0.d0        288.d0     144.d0
  interface1.dat

# inteface1/2/3/4.dat写法：在intefaces.dat文件内会给出如上所述的NXI与NETA的值，比如上方值都为2，则控制一个界面的点为2*2=4，在inteface1.dat内需要给出4个点的Z值
e.g:
-34.5
-34.5
-34.5
-34.5

# 给出所有的界面文件之后，再给出每层的Z方向element的个数即可，这部分与SPECFEM2D类似
```

#### Par_file的具体参数

```bash
# 对于常规的正演,下面两项不需要调整
SIMULATION_TYPE  = 1
SAVE_FORWARD     = .false.
UTM_PROJECTION_ZONE = 11 # 只有在SUPPRESS_UTM_PROJECTION设置为.false.的时候才会起作用
NGNOD            = 8 # 对于xmeshfem3D,只能设置为8


```

#### STATIONS的具体参数

```bash
# STATIONS文件的格式通常如下
# Station Network Latitude(degrees) Longitude(degrees) Elevation(m) burial(m)
# 台站名  台网名   维度(或是y坐标)    经度(或是x坐标)     海拔         埋深
```



