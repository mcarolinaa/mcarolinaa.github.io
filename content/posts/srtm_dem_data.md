---
title: "SRTM, DEM, what are those?"
date: 2025-09-25
description: ""
tags: ["gis", "qgis", "elevation"]
type: post
weight: 25
---


# SRTM, DEM, and Others... What Do They Even Mean, and What's the Difference?

&nbsp;

Maybe when you first had to start dealing with **altitude data**, you heard these terms thrown around and wondered what they meant or if they were the same thing.

First off, it's good to know that we can describe the Earth's terrain—its shape and structure—using something called **geomorphometric data**. A classic example of this is the **contour maps** we can create for a specific area.

This kind of information is super important for things like agriculture. You can get an exact understanding of the land's relief, which helps you plan the best way to manage the soil, combat erosion, and take other conservation steps.

In this context, you’ve probably run into **DEM** (short for: Digital Elevation Model) and **SRTM** (Shuttle Radar Topography Mission), and maybe you're confused about what each one means (I know I was!).

### What is SRTM?

**SRTM** was a mission back in the 2000s that aimed to get high-resolution elevation data for basically the whole planet. The goal was to become a complete, global source for this type of data.

It's provided by the **USGS (United States Geological Survey)**. Here in Brazil, we can find this data compiled in the **Topodata project** from **INPE**, which brings together geomorphometric data for Brazil with some enhancements and corrections.

The Topodata initiative makes grabbing this kind of data much easier. You can get more info here: [TOPODATA - Banco de Dados Geomorfométricos do Brasil](https://www.google.com/search?q=http://www.webmapit.com.br/inpe/topodata/)

![Topodata](/posts/pics/tela_topodata.png)
&nbsp;

### What is DEM?

The **Digital Elevation Model (DEM)** is a model that represents the surface using $x$, $y$, and $z$—with $z$ representing the altitude.

Since a DEM is a *type* of representation, it can come from various data sources, like **LiDAR, SRTM**, and others.

And that's where the link between SRTM and DEM comes in: the **DEM is like the final product**, and while it *can* come from SRTM data, it doesn't have to. **SRTM is a type of data**, while **DEM is a model** developed using that kind of data (but it can use other sources too).

### How to Get SRTM Data

After landing on the Topodata page, you can head straight for the geomorphometric data. Just go to **Acesso (Access)** --> **GOOGLE – TOPODATA**.

![Topodata](/posts/pics/tela_google_topodata.png)
&nbsp;

There, you'll see the entire Brazilian territory divided into grids. You can search for your region(s) of interest. When you click a grid, you’ll see the available data types on the left side of the screen. The first on the list is altitude, so you can download that data in a .zip file.

### If SRTM is a data source for a DEM, are there newer sources?

Yep. I talked about SRTM first because it was a popular source for a long time (and I think it still is today!). But yes, we have newer sources now.

Some of them include:

  * **Coopernicus DEM**
    This data represents the Earth's surface and comes in three scenarios: EEA-10, GLO-30, and GLO-90. The numbers are the spatial resolutions (the 10-meter one is only for Europe and has limited access). The mission that provided this data was **TanDEM-X** between 2011 and 2015, and the data became available in 2019. It’s set to be maintained until 2026.

  * **ALOS World 3D - 30m**
    This is Japanese data from the **ALOS satellite** mission (2006-2011). The data has the same spatial resolution as the previous one, at 30 meters.

  * **National Sources (Brazil): ANADEM (ANA/UFRGS)**
    **ANADEM** also has a 30-meter resolution and was developed using the Copernicus DEM GLO-30. What's cool here is that it includes an interesting removal of the bias caused by vegetation. This model covers all of South America and is also available for free via Google Earth Engine. It was developed through a partnership between UFRGS and ANA. Find more details here: [https://www.ufrgs.br/hge/anadem-modelo-digital-de-terreno-mdt/](https://www.ufrgs.br/hge/anadem-modelo-digital-de-terreno-mdt/)

Of course, these aren't the only choices—there's way more you can use. You can find more info in places like this: [https://opentopography.org/](https://opentopography.org/)

Currently, the **ANADEM** data is probably the best bet for Brazil, but remember, everything depends on what you need to use it for!






