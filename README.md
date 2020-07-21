<h1 align="center">
  Polylidar3D
  <br>
</h1>

<h4 align="center">Polygon Extraction from 2D Point Sets, Unorganized/Organized 3D Point Clouds, and Triangular Meshes</h4>

<p align="center">
  <a href="#key-features">Key Features</a> •
  <a href="#documentation">Documentation</a> •
  <a href="#polylidar-use-cases">Use Cases</a> •
  <a href="#credits">Credits</a> •
  <a href="#related-methods">Related</a> •
  <a href="#citation">Citations</a> •
  <a href="#license">License</a>
</p>

<p align="middle">
  <img src="https://raw.githubusercontent.com/JeremyBYU/polylidar/major_changes_refactor/assets/polylidar_3D_architecture.jpg" height="100%" />
  <!-- <img src="https://raw.githubusercontent.com/JeremyBYU/polylidarv2/master/assets/polylidar_3D_architecture.png" height="100%" />  -->
</p>

[![Docs](https://img.shields.io/badge/API-docs-blue)](https://jeremybyu.github.io/Polylidar3D/)
[![Cite](https://img.shields.io/badge/cite-%2010.1109--LRA.2020.3002212-red)](https://ieeexplore.ieee.org/document/9117017)

## Key Features

* Fast (Multi)Polygon Extraction from multiple sources of 3D Data
  * Written in C++ for portability
  * Extremely fast single-threaded but includes CPU multi-threading using data and task-based parallelism
  * Polygons with holes may be returned
* Python3 bindings using PyBind11
  * Low overhead for calling python/cpp interface (no copying of point cloud data)
* Python and C++ Examples
  * Examples from 2D Point Sets, Unorganized 3D point clouds, Organized 3D point clouds (i.e., range images), and user provided meshes
* Cross platform
  * Windows and Linux ready.

Polylidar3D is a non-convex polygon extraction algorithm which takes as input either unorganized 2D point sets, unorganized 3D point clouds (e.g., airborne LiDAR point clouds), organized point clouds (e.g., range images), or user provided meshes. The non-convex polygons extracted represent flat surfaces in an environment, while interior holes represent obstacles on said surfaces. The picture above provides an overview of Polylidar3D's data input, frontend, backend, and output. The frontend transforms input data into a *half-edge* triangular mesh.  This representation provides a common level of abstraction such that the the back-end core algorithms may efficiently operate on. The back-end is composed of four core algorithms: mesh smoothing, dominant plane normal estimation, planar segment extraction, and finally polygon extraction.  Polylidar3D outputs *planar triangular segments*, sets of flat connected triangles, and their polygonal representations. Polylidar3D is extremely fast, taking as little as a few milliseconds and makes use of CPU multi-threading and GPU acceleration when available.

Currently this repo (named Polylidar3D) has all the front-end modules and the plane/polygon extraction of the back-end core algorithms. The GPU accelerated mesh smoothing procedures for organized points clouds are found in a separate repo titled [OrganizedPointFilters](https://github.com/JeremyBYU/OrganizedPointFilters). This must be installed if you desire fast mesh smoothing for organized point clouds (i.e., denoising). The dominant plane normal estimation procedure is general and implemented in a separate repo titled [Fast Gaussian Accumulator (FastGA)](https://github.com/JeremyBYU/FastGaussianAccumulator). This must be installed if you don't know the dominant plane normals in your data input (very likely for organized point clouds and meshes). These modules themselves are written in C++ as well with Python bindings; see the respective repos for installation instructions. One day I will try to ease installation burden and automatically pull these dependencies into the build process.

## Branches and Documentation

Please see [documentation](https://jeremybyu.github.io/Polylidar3D/) for installation, api, and examples. Note that Polylidar went though major changes in July 2020 for 3D work, now called `Polylidar3D`. The old repository for 2D work (and some *basic* 3D) is found in the branch [polylidar2D](https://github.com/JeremyBYU/polylidar/tree/polylidar2d) and is connected to the this [paper](https://ieeexplore.ieee.org/document/9117017). `Polylidar3D` can still handle 2D point sets but the API is different and not the focus of this repo. For papers referencing Polylidar2D and Polylidar3D please see <a href="#citation">Citations</a>. 

*Eventually* I am going to make a standalone cpp/header file for 2D point set -> polygon extraction for those that don't need any of the features of `Polylidar3D`. 

## Polylidar Use Cases

* [Polylidar-RealSense](https://github.com/JeremyBYU/polylidar-realsense) - Live ground floor detection with Intel RealSense camera using Polylidar
* [Polylidar-KITTI](https://github.com/JeremyBYU/polylidar-kitti) - Street surface and obstacle detection from autonomous driving platform.
* [PolylidarWeb](https://github.com/JeremyBYU/polylidarweb). An very old Typescript (javascript) version with live demos of Polylidar2D.
* [Concave-Evaluation](https://github.com/JeremyBYU/concavehull-evaluation) - Evaluates and benchmarks several competing concavehull algorithms.

## Credits

This software is only possible because of the great work from the following open source packages:

* [Delaunator](https://github.com/mapbox/delaunator) - Original triangulation library 
* [DelaunatorCPP](https://github.com/delfrrr/delaunator-cpp) - Delaunator ported to C++ (used)
* [parallel-hashmap](https://github.com/greg7mdp/parallel-hashmap) - Very fast hashmap library (used)
* [PyBind11](https://github.com/pybind/pybind11) - Python C++ Binding (used)
* [Robust Geometric Predicates](https://www.cs.cmu.edu/~quake/robust.html) - Original Robust Geometric predicates
* [Updated Predicates](https://github.com/danshapero/predicates) -Updated geometric predicate library (used)

## Related Methods

### 2D ConcaveHull Extraction

* [CGAL Alpha Shapes](https://doc.cgal.org/latest/Alpha_shapes_2/index.html) - MultiPolygon with holes.
* [PostGIS ConcaveHull](http://postgis.net/docs/ST_ConcaveHull.html) - Single Polygon with holes.
* [Spatialite ConcaveHull](https://www.gaia-gis.it/fossil/libspatialite/wiki?name=tesselations-4.0) - MultiPolygon with holes.
* [Concaveman](https://github.com/mapbox/concaveman) - A 2D concave hull extraction algorithm for 2D point sets.


## Contributing

Any help or suggestions would be appreciated!

## Citation

If are using Polylidar for 2D work please cite: 

```
@ARTICLE{9117017,
  author={J. {Castagno} and E. {Atkins}},
  journal={IEEE Robotics and Automation Letters}, 
  title={Polylidar - Polygons From Triangular Meshes}, 
  year={2020},
  volume={5},
  number={3},
  pages={4634-4641}
}
```

If you are using Polylidar3D for 3D work please cite:

```
TBA
```

## License

MIT

---

> GitHub [@jeremybyu](https://github.com/JeremyBYU)

