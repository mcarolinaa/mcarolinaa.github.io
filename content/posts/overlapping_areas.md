---
title: "Overlapping areas - QGIS"
date: 2025-09-10
description: ""
tags: ["gis", "qgis", "shapefile"]
type: post
weight: 25
---


# Checking Out Area Overlap in QGIS

&nbsp;

**Data source:** [https://basedosdados.org/](https://basedosdados.org/)  
**Data's from:** Brazilian Geographic Data (geobr)  
**Focus:** Biomes and the Legal Amazon

&nbsp;

Questions about areas overlapping each other pop up all the time.

* Do these areas actually overlap?
* How big is the overlap area?
* What are the details on that overlap?

&nbsp;

Those are the kinds of questions we constantly face when working with spatial data. We need super accurate analysis to answer all those doubts.

In my day-to-day work, these areas are usually farms, land parcels, or even way smaller territories. In this example, I'm using two big areas, but the idea is the same: one polygon with info on the Brazilian biomes, and another polygon with the national Legal Amazon area.


![Overlapping areas](/posts/pics/overlay_layers.png)

&nbsp;

The QGIS processing tools (in the Toolbox) have a bunch of options for these operations, but they're all a bit different. So, it's key to really get what your problem is, and then check the documentation to figure out which tool does the trick.

For instance, I checked here ([https://docs.qgis.org/3.40/en/docs/user_manual/processing_algs/qgis/vectoroverlay.html#intersection-multiple](https://docs.qgis.org/3.40/en/docs/user_manual/processing_algs/qgis/vectoroverlay.html#intersection-multiple)) and the **Intersection (multiple)** tool looks like it does exactly what I need.

**Check out this description for the operation:**

> *This operation modifies only the features geometry. The attribute values of the features are not modified, although properties such as area or length of the features will be modified by the overlay operation. If such properties are stored as attributes, those attributes will have to be manually updated.*

In my case, I *only* want the geometry changed (the polygon that comes out of the Legal Amazon and biomes intersection). I don't want the attributes—just the biome classification—to change. Plus, I'll need to calculate the intersection areas between the Legal Amazon and each biome anyway. So, this tool is a perfect fit!

&nbsp;

### Do These Areas Overlap?


![Overlapping areas](/posts/pics/overlay_calculation.png)

&nbsp;

So, after running the tool, I can see (or really, confirm) the Legal Amazon's huge area overlapping the Brazilian biomes. The map it spits out tells me right away which biomes have the overlap: **Amazônia (Amazon), Cerrado, and Pantanal**, and it clearly shows the specific part of each.


![Informações do arquivo raster](/posts/pics/overlay_layers_results.png)

&nbsp;

### How Big is the Overlap, and What Are the Details?

But hey, I still need more details on this overlap, like the actual area.

**Super important point here:** the area will be calculated using the layer's projection units. Since my original layers were in **EPSG:4674 - SIRGAS 2000 (geographic coordinates)**, I gotta change the intersection layer to UTM first. I went with the **SIRGAS 2000 / Brazil Polyconic projection (EPSG:5880)**, but you can pick another one if you like. Get more info on this projection right here ([https://epsg.io/5880](https://epsg.io/5880)).

To reproject it, I just export the layer, making sure to specify the correct EPSG this time. Now that I have the overlap result layer and the biomes in the right projection, I can finally calculate the areas.

You've got a few ways to calculate polygon areas and their attributes. For example, you could use a toolbox feature called the **Field Calculator**. Or, you can just jump into the polygon's attribute table, open up the Field Calculator from there, type in the area expression, and boom—you're done.

For this, I already put together the expression to show the area in **km²** and created a brand new field for it.


![Overlapping areas](/posts/pics/overlay_calc_areas.png)

&nbsp;

Now I have an extra attribute in my Legal Amazon/Biomes overlap layer, which shows the area for each biome in the intersection.

If you look at the **Amazon biome**, pretty much *all* of it is in the overlap. For the other biomes, only a portion of them falls within the big Legal Amazon area.

You can check with a reliable source and see that there are, in fact, around 4 million square kilometers of the Amazon biome in Brazil.


![Overlapping areas](/posts/pics/overlay_calc_results.png)

