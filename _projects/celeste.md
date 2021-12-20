---
layout: post
title: Celeste Crater Counting
description: Celestial image and impact crater viewer. 
---
CELESTE is a program to inspect large planetary images and interact with impact crater data. The crater data can be overlayed onto the images and edited to correct for projection issues. Slices of the original images can be exported together with planetary metadata.

![Celeste logo](../../assets/images/celeste_logo_1.png)

CELESTE is one piece of a bigger effort to learn from existing crater data and apply Machine Learning techniques to generate new, more accurate and complete data. The tool itself is published under an open-source license [here](https://gitlab.com/ruffson/celeste) and other components for the ML side will be published soon.

Recognizing features from images with ML is relatively straight-forward in today's world. The main concern of this project is dealing with large amounts of image data (e.g. the entire Mars surface at 5px/meter) as well as parsing and using relevant geo data in the process.

The goal for this project is to publish all the necessary tools to generate new data as well as the data and models themselves. 

Much more information can be found in the project's [repository](https://gitlab.com/ruffson/celeste).

## Implementation & Open-source contributions

As an opportunity to learn a new language and to get a break from C++, I used _Rust_ which meant that some frameworks lacked features I needed. What a great reason to contribute back to open-source software. Besides learning a lot about the TIFF format and especially about the specifics of [GeoTIFF](http://geotiff.maptools.org/spec/geotiffhome.html) and relevant practices of the geo community, these were the biggest obstacles I had to overcome besides the main program itself:

- Because the `TIFF` crate did not support _modern_ JPEG compression I contributed that to the repository [here](https://github.com/image-rs/image-tiff/pull/95). I needed JPEG compression within the TIFF format for the planetary data because it would not have been practical to work with the data otherwise. The native JPEG compression of TIFF is suboptimal (and not used by GeoTIFFs anyway).

- Due to the  `TIFF` crate not supporting readout of GEO tags from GeoTIFF images, I added this in a Pull Request [here](https://github.com/image-rs/image-tiff/pull/100). This only reads the tags and makes them available for other frameworks as discussed [here](https://github.com/image-rs/image-tiff/issues/98#issuecomment-723858446). I wrote a small library to parse the tags into semantic entities [here](https://gitlab.com/ruffson/geo).
- The UI toolkit [druid](https://github.com/linebender/druid) did not support gray-scale images initially. Since the relevant images for Mars are all gray-scale and rather large, it was important for me to save space and make gray-scale images possible. I started a discussion about this [here](https://github.com/linebender/piet/issues/313) and implemented a solution [here](https://github.com/linebender/piet/pull/350). Due to the way the Linux graphics backend _cairo_ supports gray-scale images, the solution for Linux I implemented cannot be used directly for other platforms. Which is why they implemented a less efficient but general solution and I maintain a fork for Linux. 

If you have any suggestions or questions please open an issue in the [repository](https://gitlab.com/ruffson/celeste) or reach out via [linkedin](https://www.linkedin.com/in/raphael-tessmer).
