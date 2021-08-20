<div
  align="center"
>

![](https://github.com/blaylockbk/goes2go/blob/master/docs/_static/goes2go_logo_100dpi.png?raw=true)


# Download and plot GOES-East and GOES-West data

<!-- Badges -->
[![](https://img.shields.io/pypi/v/goes2go)](https://pypi.python.org/pypi/goes2go/)
![](https://img.shields.io/github/license/blaylockbk/goes2go)
[![DOI](https://zenodo.org/badge/296737878.svg)](https://zenodo.org/badge/latestdoi/296737878)
<!--(Badges)-->

</div>

**Brian Blaylock**  
[🌐 Personal Webpage](http://home.chpc.utah.edu/~u0553130/Brian_Blaylock/home.html)  
[🌐 University of Utah HRRR archive page](http://hrrr.chpc.utah.edu/)



---

<br>

# 📔 [GOES-2-go Documentation](https://blaylockbk.github.io/goes2go/_build/html/)

<br>

---

Download and read files from the NOAA GOES archive on AWS.http://home.chpc.utah.edu/~u0553130/Brian_Blaylock/hrrr_FAQ.html

Read doc strings for functions in `goes2go/` folder for full usage description.

>### Some Useful Links
>- [📔 GOES-R Series Data Book](https://www.goes-r.gov/downloads/resources/documents/GOES-RSeriesDataBook.pdf)
>- [🎠 Beginner's Guide](https://www.goes-r.gov/downloads/resources/documents/Beginners_Guide_to_GOES-R_Series_Data.pdf)
>- [🖥 Rammb Slider GOES Viewer](https://rammb-slider.cira.colostate.edu)
>- [💾 GOES on AWS](https://registry.opendata.aws/noaa-goes/)
>- [🐍 Unidata Plot GOES Data](https://unidata.github.io/python-training/gallery/mapping_goes16_truecolor/)
>- [🗺 Plotting tips form geonetcast blog](https://geonetcast.wordpress.com/2019/08/02/plot-0-5-km-goes-r-full-disk-regions/)
>- [🐍 `glmtools`](https://github.com/deeplycloudy/glmtools/)

---

> ### What if I don't like the goes2go package?
> As an alternative you can use [rclone](https://rclone.org/) to download GOES files from AWS. I quite like rclone. Here is a [short rclone tutorial](https://github.com/blaylockbk/pyBKB_v3/blob/master/rclone_howto.md).

# Download Data
Download GOES 16 ABI (this example downloads the multichannel fixed grid product for CONUS) and read it with xarray.

```python
from goes2go.data import goes_latest, goes_nearesttime

# Get latest data
G1 = goes_latest(satellite='G16', product='ABI')

# Get data for a specific time
G2 = goes_nearesttime(datetime(2020,10,1), satellite='G16', product='GLM')
```

# RGB Recipes
For a GOES ABI multichannel xarray.Dataset, return an RGB array for an RGB product. See [DEMO](https://blaylockbk.github.io/goes2go/_build/html/user_guide/notebooks/DEMO_rgb_recipes.html#) for more examples of RGB products.

```python
from goes2go.data import goes_latest
import matplotlib.pyplot as plt
G = goes_latest()
ax = plt.subplot(projection=G.rgb.crs)
ax.imshow(G.rgb.TrueColor(), **G.rgb.imshow_kwargs)
ax.coastlines()
```

![](./images/TrueColor.png)


# Field of View

Create shapely.Polygons for ABI and GLM field of view. See notebooks for [GLM](https://blaylockbk.github.io/goes2go/_build/html/user_guide/notebooks/field-of-view_GLM.html) and [ABI](https://blaylockbk.github.io/goes2go/_build/html/user_guide/notebooks/field-of-view_ABI.html) field of view.

```python
from goes2go.data import goes_latest
G = goes_latest()

# Get polygons of the full disk or ABI domain field of view.
G.FOV.full_disk
G.FOV.domain

# Get Cartopy coordinate reference system
G.FOV.crs
```

GOES-West is centered over -137 W and GOES-East is centered over -75 W. When GOES was being tested, it was in a "central" position, outlined in the dashed black line. Below is the ABI field of view for the full disk:
![field of view image](./images/ABI_field-of-view.png)

The GLM field of view is slightly smaller and limited by a bounding box. Below is the GLM field of view:
![field of view image](./images/GLM_field-of-view.png)
