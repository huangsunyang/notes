#### 标准光照模型

##### Phong模型

- 环境光
- 自发光
- 漫反射光
  - Lambert定律 $c_{diffuse}=c_{light}*m_{diffuse}*max(0,n·l)$
  - $m_{diffuse}$漫反射颜色
  - 反射光线强度与光源方向`l`和法线方向`n`的夹角成正比，即用点积计算

- 高光反射
  - $c_{specular}=c_{light}*m_{specular}*max(0,v·r)^{m_{gloss}}$
  - `v`为反射光方向，`r`为视角方向，$m_{gloss}$为光泽度

##### Blinn-Phong模型的优化

- 对高光反射的优化，避免计算`r`，而使用`normalize(v + l)`

##### 逐像素还是逐顶点

- 逐像素 `Phong shading`计算量更大
- 逐顶点 `Gouraud shading`对于非线性情况模拟不好

- 实践结果逐像素效果更高