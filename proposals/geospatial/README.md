# Geospatial Support for OpenUSD

EARLY DRAFT - everything can change at any time


## Preamble

### Mission Statement

OpenUSD has become increasingly popular outside of VFX and Animation in recent years due to its robust, extensible, and interoperable framework for describing complex 3D scenes and assets. Its adoption across industries such as visual effects, animation, gaming, and industrial design has enabled collaborative workflows and large-scale data exchange. As digital content creation expands into domains like architecture, engineering, construction, and digital twins, the need for precise geospatial location support becomes critical. Integrating geospatial context into OpenUSD allows for accurate placement, analysis, and visualization of assets in real-world environments, unlocking new possibilities for simulation, planning, and cross-domain collaboration.

This proposal adds geospatial support to OpenUSD with the explicit goals of being able to
- ... _optionally_ store geospatial context on any prim derived from `UsdGeomImageable`.
- ... load prims with different geospatial contexts into the same stage.
- ... render prims with different geospatial context in the same Hydra session.
- ... transforming/reprojecting geometry when exporting layers.

(TODO: these goals might still change based on the discussion items at the end)

TODO: also list non-goals once we are more clear on the actual goals

### Definitions and Terms

#### Geospatial Context

The term "geospatial" refers to data or information that is associated with a specific location on the surface of a celestial body. Geospatial data typically includes coordinates, addresses, or other identifiers that allow mapping, analysis, and visualization of features or phenomena in relation to their geographic position.

#### Datum

A datum is a reference point or surface against which position measurements are made, and it defines the origin and orientation of a coordinate system. In geospatial contexts, a geodetic datum specifies the size and shape of the Earth (or other celestial body) and the location of the origin used for mapping and surveying.

#### Ellipsoid

An ellipsoid is a mathematically defined, smooth, three-dimensional surface that approximates the shape of the Earth or other celestial body. It is defined by its semi-major and semi-minor axes and is used in geodesy to provide a simplified model for calculations of position and distance.

#### Prime Meridian

The prime meridian is the zero-degree longitude line from which all other longitudes are measured. It serves as the reference for the geographic coordinate system's longitudinal measurements. The most widely recognized prime meridian passes through Greenwich, England.

#### Coordinate Reference System

A Coordinate Reference System (CRS) is a framework that defines how the two-dimensional, three-dimensional, or higher-dimensional coordinates of spatial data relate to real-world locations. A CRS specifies the origin, orientation, units of measurement, and mathematical rules for mapping coordinates to positions on the Earth's surface or other celestial bodies. Common examples include WGS84 (used by GPS), NAD83, and various projected coordinate systems.

#### Geographic Coordinate System

A Geographic Coordinate System (GCS) is a type of coordinate reference system that uses latitude and longitude to define locations on the surface of a sphere or ellipsoid, such as the Earth. GCSs are based on a mathematical model of the Earth and typically include a datum, an ellipsoid, and a prime meridian. Examples include WGS84 and NAD83.

#### Projected Coordinate System

A Projected Coordinate System (PCS) is a type of coordinate reference system that transforms geographic coordinates (latitude and longitude) from a geographic coordinate system into a flat, two-dimensional surface using mathematical projections. PCS allows for accurate measurement and mapping over localized areas and is commonly used in cartography and GIS applications. Examples include UTM (Universal Transverse Mercator) and State Plane Coordinate System.

#### Vertical Coordinate System

A Vertical Coordinate System (VCS) is a reference system used to measure and define elevations or depths relative to a specific surface, such as mean sea level, a geoid, or an ellipsoid. VCSs specify the origin, units (such as meters or feet), and direction (upward or downward) for vertical measurements. They are essential for representing height, altitude, or depth in geospatial data, especially in applications involving terrain, buildings, or subsurface features.

### Related Work

TODO: discuss the relationship and impact of below items with and on this proposal

- Open Geospatial Consortium Standard "Well-known text representation of coordinate reference systems": https://docs.ogc.org/is/18-010r7/18-010r7.html
- JSON representation of the above: https://proj.org/en/stable/specifications/projjson.html
- UsdShadeCoordSysAPI (implemented in OpenUSD main): https://openusd.org/release/api/class_usd_shade_coord_sys_a_p_i.html#details
- Hydra Scene Index support for geo-spatial data (Nvidia plugin): https://github.com/NVIDIA-Omniverse/OpenUSD-plugin-samples/tree/main/src/hydra-plugins#creating-a-custom-hydra-20-scene-index-for-geospatially-aware-transforms
- Layer Metadata vs Applied Schemas (published): https://github.com/PixarAnimationStudios/OpenUSD-proposals/blob/main/proposals/revise_use_of_layer_metadata/README.md
- OpenExec efforts (proposal): https://github.com/FlorianZ/OpenUSD-proposals-openexec/tree/main/proposals/openexec#readme


## Proposal

### Discussion Items

This is a list of items we need to complete and decide on before we can start to draft the actual proposal:
1. Decision on using OGC "Well-known text representation of coordinate reference systems" as basis
1. Decision on scope: do we limit ourselves on PCS or do we add GCS (and even VCS) from the start?
1. Decision on where the geospatial context is stored (Layer Metadata vs Prim Schemas)
1. Do we include Hydra in the proposal, i.e. the computational part?
1. Related to above: How to resolve conflicts during composition? What computational features (e.g. reprojection) do we require from an implementation?
1. Decision on data representation, i.e. how to encode the geospatial context? Full set of attributes or just the WKID? Individual attributes or a JSON blob?
1. How to tackle a reference implementation?
