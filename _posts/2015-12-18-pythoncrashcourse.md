---
layout: post
title:  "Python Crash Course"
date:   2015-12-18 13:02:19
categories: education course python climate
permalink: /posts/crash-course
image: "https://github.com/oliverangelil/oliverangelil.github.io/blob/master/photos/blog4_pythonbanner.jpg?raw=true"
---

A crash course for data scientists / climate scientists new to Python. With the help of example code, learn how to: read in netCDF files; generate maps; calculate area-weighted averages; plot time series; write list-comprehensions (i.e. more compact loops); and use CDO within Python using the CDO Python module.


## 1 Setup
### 1.1 Download and Install Python
Download the miniconda distribution of Python 2.7 from [here](http://conda.pydata.org/miniconda.html).
Navigate to the file in bash, and type: *bash Miniconda-latest-MacOSX-x86_64.sh*  
(If you’re not on a Mac, replace Miniconda-latest-MacOSX-x86_64.sh with the relevant file name you just downloaded.)  
Follow the on-screen instructions. Once you’re done, a 'miniconda' directory should appear in your home directory, and a line in your .bash_profile should declare 'miniconda' as a path.

### 1.2 Download Packages
Miniconda comes with a package manager called 'conda'. There are thousands of python packages available, but the most useful packages for a climate scientist are:
* netcdf4 (read & write NetCDF files)
* numpy (manipulation of arrays and maths functions to operate on them)
* matplotlib (plotting)
* basemap (plotting data on map projections)  

Download them with: *conda install netcdf4 numpy matplotlib basemap*  
List the installed packages in your conda environment: *conda list*

## 2 Reading in a NetCDF file
First download an example NetCDF file from Oliver’s website: wget http://kingklip.ddns.net/~oliver/share_files/precip.nc -P ~/ 

In case wget doesn’t exist on your laptop, you can install it with: *sudo port install wget*
once you have installed MacPorts. Alternatively, pasting the link above into your browser should automatically start the download of the NetCDF file.  

Create a new file called ‘first script.py’ and add these lines to it:

** Load modules **


```python
from netCDF4 import Dataset  # to work with NetCDF files
import numpy as np
import matplotlib.pyplot as plt  # to generate plots
from mpl_toolkits.basemap import Basemap  # plot on map projections
from os.path import expanduser
home = expanduser("~")  # Get users home directory
import os  # operating system interface
```

** Input NetCDF file info**


```python
file = home + '/Downloads/precip.nc'  # Adjust if necessary
variable = 'precip'
```

** Extract the variables**


```python
fh = Dataset(file, mode='r')  # file handle, open in read only mode
lon = fh.variables['lon'][:]
lat = fh.variables['lat'][:]
pr = fh.variables[variable][:]
fh.close()  # close the file
```

Execute the file in a new terminal tab as an interactive session with: *python -i first_script.py*  
To exit interactive mode, type ctrl+d.  
While in the interactive session, type the following to see the dimensions of the variables:


```python
pr.shape  # Results in nmonths x nlat x nlon (811, 72, 144)
len(lat)  # Length of lat is 72
print 'The variable contains ' + str(pr.shape[0]) + ' timesteps.'  # Print statement to the screen
```

    The variable contains 811 timesteps.


## 3 Accessing the data

** Outputting all the latitude values **


```python
lat  # prints a list to the screen with values from 88.75 to -88.75
```




    array([ 88.75,  86.25,  83.75,  81.25,  78.75,  76.25,  73.75,  71.25,
            68.75,  66.25,  63.75,  61.25,  58.75,  56.25,  53.75,  51.25,
            48.75,  46.25,  43.75,  41.25,  38.75,  36.25,  33.75,  31.25,
            28.75,  26.25,  23.75,  21.25,  18.75,  16.25,  13.75,  11.25,
             8.75,   6.25,   3.75,   1.25,  -1.25,  -3.75,  -6.25,  -8.75,
           -11.25, -13.75, -16.25, -18.75, -21.25, -23.75, -26.25, -28.75,
           -31.25, -33.75, -36.25, -38.75, -41.25, -43.75, -46.25, -48.75,
           -51.25, -53.75, -56.25, -58.75, -61.25, -63.75, -66.25, -68.75,
           -71.25, -73.75, -76.25, -78.75, -81.25, -83.75, -86.25, -88.75], dtype=float32)



** Accessing the first and the last value of lat **


```python
lat[0]  # Caution! Python’s indexing starts with zero
lat[-1]  # gives the last value of the vector
```




    -88.75



** Getting the precipitation field of the first month **


```python
pr[0,:,:]  # or simply pr[0]
pr[0].shape  # (72, 144)
```




    (72, 144)



## 4 Getting help
If you want to learn more about what a specific function does, you can use the help function:


```python
help(np.argmin)
```

    Help on function argmin in module numpy.core.fromnumeric:
    
    argmin(a, axis=None, out=None)
        Returns the indices of the minimum values along an axis.
        
        Parameters
        ----------
        a : array_like
            Input array.
        axis : int, optional
            By default, the index is into the flattened array, otherwise
            along the specified axis.
        out : array, optional
            If provided, the result will be inserted into this array. It should
            be of the appropriate shape and dtype.
        
        Returns
        -------
        index_array : ndarray of ints
            Array of indices into the array. It has the same shape as `a.shape`
            with the dimension along `axis` removed.
        
        See Also
        --------
        ndarray.argmin, argmax
        amin : The minimum value along a given axis.
        unravel_index : Convert a flat index into an index tuple.
        
        Notes
        -----
        In case of multiple occurrences of the minimum values, the indices
        corresponding to the first occurrence are returned.
        
        Examples
        --------
        >>> a = np.arange(6).reshape(2,3)
        >>> a
        array([[0, 1, 2],
               [3, 4, 5]])
        >>> np.argmin(a)
        0
        >>> np.argmin(a, axis=0)
        array([0, 0, 0])
        >>> np.argmin(a, axis=1)
        array([0, 0])
        
        >>> b = np.arange(6)
        >>> b[4] = 0
        >>> b
        array([0, 1, 2, 3, 0, 5])
        >>> np.argmin(b) # Only the first occurrence is returned.
        0
    


Exit the help page by pressing 'q'. You can also apply the help function on an object (variable, list, ...) to learn more about what you could do with it.


```python
b = ['a', 334, 'abc']
help(b)
```

    Help on list object:
    
    class list(object)
     |  list() -> new empty list
     |  list(iterable) -> new list initialized from iterable's items
     |  
     |  Methods defined here:
     |  
     |  __add__(...)
     |      x.__add__(y) <==> x+y
     |  
     |  __contains__(...)
     |      x.__contains__(y) <==> y in x
     |  
     |  __delitem__(...)
     |      x.__delitem__(y) <==> del x[y]
     |  
     |  __delslice__(...)
     |      x.__delslice__(i, j) <==> del x[i:j]
     |      
     |      Use of negative indices is not supported.
     |  
     |  __eq__(...)
     |      x.__eq__(y) <==> x==y
     |  
     |  __ge__(...)
     |      x.__ge__(y) <==> x>=y
     |  
     |  __getattribute__(...)
     |      x.__getattribute__('name') <==> x.name
     |  
     |  __getitem__(...)
     |      x.__getitem__(y) <==> x[y]
     |  
     |  __getslice__(...)
     |      x.__getslice__(i, j) <==> x[i:j]
     |      
     |      Use of negative indices is not supported.
     |  
     |  __gt__(...)
     |      x.__gt__(y) <==> x>y
     |  
     |  __iadd__(...)
     |      x.__iadd__(y) <==> x+=y
     |  
     |  __imul__(...)
     |      x.__imul__(y) <==> x*=y
     |  
     |  __init__(...)
     |      x.__init__(...) initializes x; see help(type(x)) for signature
     |  
     |  __iter__(...)
     |      x.__iter__() <==> iter(x)
     |  
     |  __le__(...)
     |      x.__le__(y) <==> x<=y
     |  
     |  __len__(...)
     |      x.__len__() <==> len(x)
     |  
     |  __lt__(...)
     |      x.__lt__(y) <==> x<y
     |  
     |  __mul__(...)
     |      x.__mul__(n) <==> x*n
     |  
     |  __ne__(...)
     |      x.__ne__(y) <==> x!=y
     |  
     |  __repr__(...)
     |      x.__repr__() <==> repr(x)
     |  
     |  __reversed__(...)
     |      L.__reversed__() -- return a reverse iterator over the list
     |  
     |  __rmul__(...)
     |      x.__rmul__(n) <==> n*x
     |  
     |  __setitem__(...)
     |      x.__setitem__(i, y) <==> x[i]=y
     |  
     |  __setslice__(...)
     |      x.__setslice__(i, j, y) <==> x[i:j]=y
     |      
     |      Use  of negative indices is not supported.
     |  
     |  __sizeof__(...)
     |      L.__sizeof__() -- size of L in memory, in bytes
     |  
     |  append(...)
     |      L.append(object) -- append object to end
     |  
     |  count(...)
     |      L.count(value) -> integer -- return number of occurrences of value
     |  
     |  extend(...)
     |      L.extend(iterable) -- extend list by appending elements from the iterable
     |  
     |  index(...)
     |      L.index(value, [start, [stop]]) -> integer -- return first index of value.
     |      Raises ValueError if the value is not present.
     |  
     |  insert(...)
     |      L.insert(index, object) -- insert object before index
     |  
     |  pop(...)
     |      L.pop([index]) -> item -- remove and return item at index (default last).
     |      Raises IndexError if list is empty or index is out of range.
     |  
     |  remove(...)
     |      L.remove(value) -- remove first occurrence of value.
     |      Raises ValueError if the value is not present.
     |  
     |  reverse(...)
     |      L.reverse() -- reverse *IN PLACE*
     |  
     |  sort(...)
     |      L.sort(cmp=None, key=None, reverse=False) -- stable sort *IN PLACE*;
     |      cmp(x, y) -> -1, 0, 1
     |  
     |  ----------------------------------------------------------------------
     |  Data and other attributes defined here:
     |  
     |  __hash__ = None
     |  
     |  __new__ = <built-in method __new__ of type object>
     |      T.__new__(S, ...) -> a new object with type S, a subtype of T
    


You can use the 'dir' function on a function if you are interested in the “sub-functions” available in the master-function.


```python
dir(np.sum)
```




    ['__call__',
     '__class__',
     '__closure__',
     '__code__',
     '__defaults__',
     '__delattr__',
     '__dict__',
     '__doc__',
     '__format__',
     '__get__',
     '__getattribute__',
     '__globals__',
     '__hash__',
     '__init__',
     '__module__',
     '__name__',
     '__new__',
     '__reduce__',
     '__reduce_ex__',
     '__repr__',
     '__setattr__',
     '__sizeof__',
     '__str__',
     '__subclasshook__',
     'func_closure',
     'func_code',
     'func_defaults',
     'func_dict',
     'func_doc',
     'func_globals',
     'func_name']



You can also try to find solutions to any problem in the python documentation: https://www.python.org/doc/versions/

## 5 Visualising the Data
### 5.1 Quick and Dirty Visualisation


```python
plt.imshow(pr[0])  # data for the first time-step (January 1948)
plt.show()
```


![png](images/output_27_0.png)


### 5.2 Visualising on a [Map Projection](http://matplotlib.org/basemap/users/examples.html)


```python
lons, lats = np.meshgrid(lon,lat)
m = Basemap(projection='kav7', lon_0=0)
m.drawmapboundary(fill_color='Gray', zorder=0)
m.drawparallels(np.arange(-90.,99.,30.), labels=[1,0,0,0])
m.drawmeridians(np.arange(-180.,180.,60.), labels=[0,0,0,1])
h=m.pcolormesh(lons,lats,pr[0], shading='flat', latlon=True)
m.colorbar(h, location='bottom', pad='15%', label='Rainfall [mm/day]')
plt.ion(); plt.show()
# Uncomment this line to save the figure to your home directory:
# plt.savefig(home+’/basemap_figure.png’)
```


![png](images/output_29_0.png)


## 6 Working with masked arrays
In the previous section we noticed that our precipitation data-set only has values over land. This is where masked arrays come into play.

** Have a look at pr **


```python
pr  # Python automatically created a masked array with the data and the mask
```




    masked_array(data =
     [[[-- -- -- ..., -- -- --]
      [-- -- -- ..., -- -- --]
      [-- -- -- ..., -- -- --]
      ..., 
      [0.12399999797344208 0.12600000202655792 0.1289999932050705 ...,
       0.12399999797344208 0.12300000339746475 0.12300000339746475]
      [0.2019999921321869 0.2019999921321869 0.2019999921321869 ...,
       0.20600000023841858 0.20399999618530273 0.2029999941587448]
      [0.2759999930858612 0.2750000059604645 0.27399998903274536 ...,
       0.2800000011920929 0.2789999842643738 0.27799999713897705]]
    
     [[-- -- -- ..., -- -- --]
      [-- -- -- ..., -- -- --]
      [-- -- -- ..., -- -- --]
      ..., 
      [0.2919999957084656 0.2880000174045563 0.28299999237060547 ...,
       0.30799999833106995 0.30300000309944153 0.296999990940094]
      [0.22599999606609344 0.22100000083446503 0.2160000056028366 ...,
       0.25200000405311584 0.242000013589859 0.2329999953508377]
      [0.29600000381469727 0.2919999957084656 0.289000004529953 ...,
       0.3100000023841858 0.3050000071525574 0.30000001192092896]]
    
     [[-- -- -- ..., -- -- --]
      [-- -- -- ..., -- -- --]
      [-- -- -- ..., -- -- --]
      ..., 
      [0.44899997115135193 0.4449999928474426 0.4399999976158142 ...,
       0.46399998664855957 0.4599999785423279 0.45399999618530273]
      [0.3100000023841858 0.3050000071525574 0.29899999499320984 ...,
       0.3349999785423279 0.32499998807907104 0.3160000145435333]
      [0.3240000009536743 0.32100000977516174 0.31800001859664917 ...,
       0.335999995470047 0.3319999873638153 0.328000009059906]]
    
     ..., 
     [[-- -- -- ..., -- -- --]
      [-- -- -- ..., -- -- --]
      [-- -- -- ..., -- -- --]
      ..., 
      [1.7590000629425049 1.722999930381775 1.684000015258789 ...,
       1.8200000524520874 1.805999994277954 1.787000060081482]
      [0.8130000233650208 0.7940000295639038 0.7730000019073486 ...,
       0.8799999952316284 0.8579999804496765 0.8330000042915344]
      [0.421999990940094 0.4169999957084656 0.41200000047683716 ...,
       0.4449999928474426 0.43699997663497925 0.4300000071525574]]
    
     [[-- -- -- ..., -- -- --]
      [-- -- -- ..., -- -- --]
      [-- -- -- ..., -- -- --]
      ..., 
      [1.431999921798706 1.3980000019073486 1.3619999885559082 ...,
       1.4909999370574951 1.4760000705718994 1.4570000171661377]
      [0.8069999814033508 0.7879999876022339 0.7689999938011169 ...,
       0.8810000419616699 0.8550000190734863 0.8279999494552612]
      [0.6660000085830688 0.6570000052452087 0.6510000228881836 ...,
       0.6980000138282776 0.6869999766349792 0.6759999990463257]]
    
     [[-- -- -- ..., -- -- --]
      [-- -- -- ..., -- -- --]
      [-- -- -- ..., -- -- --]
      ..., 
      [0.8579999804496765 0.8449999690055847 0.8300000429153442 ...,
       0.8840000033378601 0.8779999613761902 0.8689999580383301]
      [0.39800000190734863 0.39000001549720764 0.38099998235702515 ...,
       0.42899999022483826 0.4189999997615814 0.40799999237060547]
      [0.2029999941587448 0.20000000298023224 0.1979999989271164 ...,
       0.21300001442432404 0.20899999141693115 0.20600000023841858]]],
                 mask =
     [[[ True  True  True ...,  True  True  True]
      [ True  True  True ...,  True  True  True]
      [ True  True  True ...,  True  True  True]
      ..., 
      [False False False ..., False False False]
      [False False False ..., False False False]
      [False False False ..., False False False]]
    
     [[ True  True  True ...,  True  True  True]
      [ True  True  True ...,  True  True  True]
      [ True  True  True ...,  True  True  True]
      ..., 
      [False False False ..., False False False]
      [False False False ..., False False False]
      [False False False ..., False False False]]
    
     [[ True  True  True ...,  True  True  True]
      [ True  True  True ...,  True  True  True]
      [ True  True  True ...,  True  True  True]
      ..., 
      [False False False ..., False False False]
      [False False False ..., False False False]
      [False False False ..., False False False]]
    
     ..., 
     [[ True  True  True ...,  True  True  True]
      [ True  True  True ...,  True  True  True]
      [ True  True  True ...,  True  True  True]
      ..., 
      [False False False ..., False False False]
      [False False False ..., False False False]
      [False False False ..., False False False]]
    
     [[ True  True  True ...,  True  True  True]
      [ True  True  True ...,  True  True  True]
      [ True  True  True ...,  True  True  True]
      ..., 
      [False False False ..., False False False]
      [False False False ..., False False False]
      [False False False ..., False False False]]
    
     [[ True  True  True ...,  True  True  True]
      [ True  True  True ...,  True  True  True]
      [ True  True  True ...,  True  True  True]
      ..., 
      [False False False ..., False False False]
      [False False False ..., False False False]
      [False False False ..., False False False]]],
           fill_value = -9.96921e+36)



** Have a look at the data and the mask separately **


```python
pr.data  # gives you the data array only
pr.mask  # gives you True where the data is masked (over the ocean) and False where data is available (over land)
```




    array([[[ True,  True,  True, ...,  True,  True,  True],
            [ True,  True,  True, ...,  True,  True,  True],
            [ True,  True,  True, ...,  True,  True,  True],
            ..., 
            [False, False, False, ..., False, False, False],
            [False, False, False, ..., False, False, False],
            [False, False, False, ..., False, False, False]],
    
           [[ True,  True,  True, ...,  True,  True,  True],
            [ True,  True,  True, ...,  True,  True,  True],
            [ True,  True,  True, ...,  True,  True,  True],
            ..., 
            [False, False, False, ..., False, False, False],
            [False, False, False, ..., False, False, False],
            [False, False, False, ..., False, False, False]],
    
           [[ True,  True,  True, ...,  True,  True,  True],
            [ True,  True,  True, ...,  True,  True,  True],
            [ True,  True,  True, ...,  True,  True,  True],
            ..., 
            [False, False, False, ..., False, False, False],
            [False, False, False, ..., False, False, False],
            [False, False, False, ..., False, False, False]],
    
           ..., 
           [[ True,  True,  True, ...,  True,  True,  True],
            [ True,  True,  True, ...,  True,  True,  True],
            [ True,  True,  True, ...,  True,  True,  True],
            ..., 
            [False, False, False, ..., False, False, False],
            [False, False, False, ..., False, False, False],
            [False, False, False, ..., False, False, False]],
    
           [[ True,  True,  True, ...,  True,  True,  True],
            [ True,  True,  True, ...,  True,  True,  True],
            [ True,  True,  True, ...,  True,  True,  True],
            ..., 
            [False, False, False, ..., False, False, False],
            [False, False, False, ..., False, False, False],
            [False, False, False, ..., False, False, False]],
    
           [[ True,  True,  True, ...,  True,  True,  True],
            [ True,  True,  True, ...,  True,  True,  True],
            [ True,  True,  True, ...,  True,  True,  True],
            ..., 
            [False, False, False, ..., False, False, False],
            [False, False, False, ..., False, False, False],
            [False, False, False, ..., False, False, False]]], dtype=bool)



** Create a vector of precipitation data where values are not masked **


```python
pr.data[~pr.mask]  # the tilde inverts the mask
```




    array([ 0.17300001,  0.178     ,  0.184     , ...,  0.21300001,
            0.20899999,  0.206     ], dtype=float32)



** Create a climatology field from the precipitation data **


```python
clim = np.mean(pr, axis=0)  # because pr is a masked array, all masked values are ignored in the calculation.
#‘clim’ can be plotted onto a map projection the same way you plotted ‘pr[0]’ above.
```

## 7 Calculate and plot the global monthly mean precipitation rate
** Define weighting matrix**


```python
wgtmat = np.cos(np.tile(abs(lat[:,None])*np.pi/180, (1, len(lon))))  # Dimension: 72x144
```

** Calculate the global monthly mean precipitation rate **


```python
mean_pr = np.zeros(pr.shape[0])  # Preallocation
for i in range(pr.shape[0]):  # Don’t forget the ‘:’
    mean_pr[i] = np.sum(pr[i] * wgtmat * ~pr.mask[i])/np.sum(wgtmat * ~pr.mask[i])
```

** Closer look at the range function in the loop **


```python
range(10)  # vector from 0 to 9
```




    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]



** Example of a simple if-statement **


```python
if variable == 'precip':
    unit = 'mm/day'
elif variable == 'tas':
    unit = 'degC'
else:
    print 'Unknown variable.'
```

** Plot a time series of monthly rainfall **


```python
fig=plt.figure(figsize=(10, 6))
ax = fig.add_subplot(111)
plt.plot(range(1,pr.shape[0]+1), mean_pr, 'k-', label='precip', linewidth=2)
plt.xlabel('Month')
plt.ylabel('Precipitation Rate [' + unit + ']')
plt.axis([1,pr.shape[0]+1, np.min(mean_pr)-0.05, np.max(mean_pr)+0.05])
plt.title('Monthly Mean Precipitation Rate Over Land', fontsize=12)
plt.legend(loc=2, borderaxespad=1., frameon=False)
plt.ion(); plt.show()
# plt.savefig(home+'/monthly_global_precip.png')
```


![png](images/output_48_0.png)


** Calculate annual mean precipitation rates **


```python
mean_pr_reshaped = mean_pr[0:804].reshape((67,12))
mean_pr_yr = np.mean(mean_pr_reshaped,axis=1)
mean_pr_yr  # print the results to the screen
```




    array([ 2.34122654,  2.39769661,  2.44706527,  2.33105606,  2.37154988,
            2.37479375,  2.44355377,  2.45796706,  2.46120614,  2.35362403,
            2.33199054,  2.38930704,  2.39497503,  2.40873591,  2.38752621,
            2.35254451,  2.41257926,  2.27560623,  2.37640593,  2.3712407 ,
            2.36476765,  2.34906387,  2.37150542,  2.37820148,  2.26528269,
            2.47499136,  2.43855641,  2.44839001,  2.34376893,  2.38182722,
            2.37328668,  2.33829389,  2.34705887,  2.37406431,  2.267096  ,
            2.32070233,  2.35867178,  2.33199493,  2.29706605,  2.25080488,
            2.42721782,  2.34744161,  2.31406722,  2.26004589,  2.22112421,
            2.25623566,  2.28570306,  2.30157791,  2.34726441,  2.2564742 ,
            2.38044012,  2.387748  ,  2.38468824,  2.30812182,  2.24653872,
            2.2931487 ,  2.33201704,  2.32289978,  2.36968599,  2.40534721,
            2.44495052,  2.41782222,  2.52221994,  2.44827882,  2.36710785,
            2.42469235,  2.36887914])



## 8 Saving numpy arrays into a single file
** Saving **


```python
np.savez(home + '/outfile.npz', lat=lat, mean_pr_yr=mean_pr_yr)
```

** Loading **


```python
npzfile = np.load(home + '/outfile.npz')
npzfile.files  # results in [’lat’, ’mean_pr_yr’]
npzfile['lat']
```




    array([ 88.75,  86.25,  83.75,  81.25,  78.75,  76.25,  73.75,  71.25,
            68.75,  66.25,  63.75,  61.25,  58.75,  56.25,  53.75,  51.25,
            48.75,  46.25,  43.75,  41.25,  38.75,  36.25,  33.75,  31.25,
            28.75,  26.25,  23.75,  21.25,  18.75,  16.25,  13.75,  11.25,
             8.75,   6.25,   3.75,   1.25,  -1.25,  -3.75,  -6.25,  -8.75,
           -11.25, -13.75, -16.25, -18.75, -21.25, -23.75, -26.25, -28.75,
           -31.25, -33.75, -36.25, -38.75, -41.25, -43.75, -46.25, -48.75,
           -51.25, -53.75, -56.25, -58.75, -61.25, -63.75, -66.25, -68.75,
           -71.25, -73.75, -76.25, -78.75, -81.25, -83.75, -86.25, -88.75], dtype=float32)



## 9 List comprehension
Often, list comprehensions can be used instead of for-loops. A simple example is shown below:


```python
words = ['one','two','three','four']
uppercase_words = [words[i].upper() for i in range(len(words))]
# the same can be done with the following:
uppercase_words = [i.upper() for i in words]
```

## 10 Using CDO in Python (the CDO module)

There is also the possibility to work with the CDO module within Python. Many Python distributions (including anaconda) come with a command line tool called pip. Install the CDO module by typing the following in bash: *pip install --user cdo*  
Import the module into Python with:


```python
from cdo import *
cdo = Cdo()
```

** Compute the global mean monthly precipitation (as in section 7) and return it as a numpy array: **


```python
mean_pr = np.squeeze(cdo.fldmean(input=file, returnArray='precip'))
```

** Use more than one CDO command**  
Compute global precipitation anomalies relative to the 1971-2000 mean global precipitation:


```python
global_pr_anomalies = np.squeeze(cdo.sub(input = '-fldmean ' + file + ' -timmean -selyear,1971/2000 -fldmean ' + file, returnArray='precip', options = '-L'))
# Sometimes CDO throws an error ("segmentation error"). Using the "-L" option can prevent this.
```

** Read in all years in the netCDF file: **


```python
years = cdo.showyear(input=file)
# this is one long string, so split each year into individual elements:
years = np.array(years[0].split(), dtype=np.float)
print years
```

    [ 1948.  1949.  1950.  1951.  1952.  1953.  1954.  1955.  1956.  1957.
      1958.  1959.  1960.  1961.  1962.  1963.  1964.  1965.  1966.  1967.
      1968.  1969.  1970.  1971.  1972.  1973.  1974.  1975.  1976.  1977.
      1978.  1979.  1980.  1981.  1982.  1983.  1984.  1985.  1986.  1987.
      1988.  1989.  1990.  1991.  1992.  1993.  1994.  1995.  1996.  1997.
      1998.  1999.  2000.  2001.  2002.  2003.  2004.  2005.  2006.  2007.
      2008.  2009.  2010.  2011.  2012.  2013.  2014.  2015.]


Detailed information on using this module can be found [here](https://code.zmaw.de/projects/cdo/wiki/Cdo%7Brbpy%7D). A useful reference card with important CDO commands can be found [here](http://www.nco.ncep.noaa.gov/pmb/codes/nwprod/sorc/rtofs_cdo-1.4.0.1.fd/cdo-1.5.0/doc/cdo_refcard.pdf). See some specific questions I asked in the [CDO forums](https://code.zmaw.de/boards/1/topics/4024) about using this Python
module. For example how to use operators that are followed by a comma and a value, like the “runmean”, “remapcon”, and “selyear” operators, and how to use any of the operators beginning with “show”.


