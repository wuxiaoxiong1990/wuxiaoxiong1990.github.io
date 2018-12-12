---
layout:     post
title:      Ctypes调用FORTRAN DLL示例
subtitle:   实现python调用fortran函数
date:       2018-12-12
author:     蘑菇熊
header-img: img/post-bg-code.jpg
catalog: true
tags:
    - python
    - Fortran
    - Ctypes
    - DLL
---

# 1. FORTRAN函数 示例my_exam.f90

```
module example
    use iso_c_binding
    implicit none
contains
    subroutine my_exam(m,n,val) BIND(C,name='my_exam')
    !DEC$ ATTRIBUTES DLLEXPORT :: my_exam 
    integer,intent(in) :: m,n
    real(8),dimension(m,n),intent(inout) :: val
    integer :: i,j
    open(1,file = 'fortran_output.dat')
    write(1,'(a)') 'input arrary'
    write(1,100) ((val(i,j),j=1,n),i = 1,m)
    do i = 1,m
        do j = 1,n
            val(i,j) = val(i,j) * (i+j)
        end do
    end do
    write(1,'(a)') 'output arrary'
    write(1,100) ((val(i,j),j=1,n),i = 1,m)
    close(1)
100 format(<n>F10.3)    
    end subroutine
end module
```
# 2. ifort生成DLL
打开命令行形式的intel 64 visual studio 2012 mode
`>ifort /dll my_exam.f90`
# 3. my_exam.py
通过ctypes模块调用，注意ndarray数组按照FORTRAN列主序方式储存
```
import ctypes as ct
import numpy as np

#导入dll
flib = ct.CDLL('my_exam.dll')

#创建矩阵
m = 10
n = 5
rng = np.random.RandomState()
val = rng.random_sample(size=(m, n)) * 10
f = open('expample_output.dat','w')
f.write('input array\n')
for line in val:
    for ele in line:
        f.write("{0:10.3f}".format(ele))
    f.write( '\n')

# 方式1：
# m_p = ct.pointer(ct.c_int(m))
# n_p = ct.pointer(ct.c_int(n))
# val = np.asfortranarray(val).T  #若不转置将面临fortran和C数组在内存中储存顺序不同的问题
# result = flib.my_exam(m_p, n_p, np.ctypeslib.as_ctypes(val))
# val = val.T
# print(val)


# 方式2：
def numpy_pointer(array):
    return array.ctypes.data_as(ct.POINTER(ct.c_double))
def my_exam(m, n, val):
    return flib.my_exam(ct.byref(ct.c_int(m)), ct.byref(ct.c_int(n)), numpy_pointer(val))
val = np.asfortranarray(val)   #按照Fortran方式储存(fortran列主序，C行主序)
result = my_exam(m, n, val)


f.write('output array\n')
for line in val:
    for ele in line:
        f.write("{0:10.3f}".format(ele))
    f.write( '\n')
f.close()
```
# 4. 结果
若example_output.dat与fortran_output.dat数组不同，且前者input array行主序正好与后者的列主序相同，则说明数组按C语言格式传递至子程序中。

> example_output.dat
```
input array
     8.341     9.982     7.849     8.082     0.940
     2.061     0.185     9.257     5.831     5.491
     1.552     4.727     5.735     7.451     5.760
     3.789     6.828     8.692     2.715     5.318
     3.828     4.005     0.733     6.159     8.166
     0.123     7.252     9.402     0.256     2.003
     1.227     9.711     9.069     7.987     3.315
     5.775     0.614     9.754     4.249     4.201
     5.845     0.219     0.986     2.124     9.224
     9.579     2.791     1.573     4.914     8.677
output array
    16.682    29.947    31.394    40.408     5.639
     6.182     0.740    46.284    34.988    38.434
     6.209    23.635    34.408    52.155    46.080
    18.946    40.970    60.842    21.724    47.864
    22.969    28.036     5.865    55.427    81.663
     0.858    58.014    84.620     2.555    22.028
     9.815    87.398    90.688    87.858    39.778
    51.975     6.142   107.290    50.993    54.614
    58.450     2.409    11.833    27.614   129.131
   105.364    33.494    20.448    68.790   130.148
```
> fortran_output.dat
```
input arrary
     8.341     9.982     7.849     8.082     0.940
     2.061     0.185     9.257     5.831     5.491
     1.552     4.727     5.735     7.451     5.760
     3.789     6.828     8.692     2.715     5.318
     3.828     4.005     0.733     6.159     8.166
     0.123     7.252     9.402     0.256     2.003
     1.227     9.711     9.069     7.987     3.315
     5.775     0.614     9.754     4.249     4.201
     5.845     0.219     0.986     2.124     9.224
     9.579     2.791     1.573     4.914     8.677
output arrary
    16.682    29.947    31.394    40.408     5.639
     6.182     0.740    46.284    34.988    38.434
     6.209    23.635    34.408    52.155    46.080
    18.946    40.970    60.842    21.724    47.864
    22.969    28.036     5.865    55.427    81.663
     0.858    58.014    84.620     2.555    22.028
     9.815    87.398    90.688    87.858    39.778
    51.975     6.142   107.290    50.993    54.614
    58.450     2.409    11.833    27.614   129.131
   105.364    33.494    20.448    68.790   130.148
```
