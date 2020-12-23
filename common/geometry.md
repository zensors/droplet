# Geometry

In order to represent labels on parts of images, the Droplet format defines a number of types for representing geometry.  In droplet, the coordinate space for an image is **always** 0-to-1 on both axes.  This was chosen so that stored geometries are independent of the resolution of the image.

Since Droplet is a JSON-based serialization format, here we provide TypeScript-style type definitions for all of the serialized types.  There are also language-specific bindings for operating on droplets; see the language-specific API documentation for more information.

## Point

A `Point` is a 2-dimensional point, generally representing a position on an image.  A `Point` is considered _invalid_ if either of its coordinates are outside the inclusive range `[0, 1]`.

```ts
type Point = [x: number, y: number];
```

## Rectangle

A `Rectangle` is a 2-dimensional axis-aligned rectangle, defined by its top-left and bottom-right points.  A `Rectangle` is considered _invalid_ if either of its points are invalid or if its top-left point is not both strictly above and strictly to the left of its bottom-right point.  (Zero-area rectangles are not considered valid.)

```ts
type Rectangle = [topLeft: Point, bottomRight: Point];
```

## Polygon

A `Polygon` is a 2-dimensional simple polygon, defined as a list of its vertices.  A `Polygon` is considered _invalid_ if it has fewer than three points, if any of its points are invalid, or if it is self-intersecting.

```ts
type Polygon = Point[];
```

## Mask

A `Mask` is any 2-dimensional region.  It may have holes and may be discontiguous.  A `Mask` is defined by a collection of polygons, and inclusion is determined via the [even-odd rule](https://en.wikipedia.org/wiki/Even%E2%80%93odd_rule).  A `Mask` is considered invalid if it doesn't have at least one polygon, if any of its constituent polygons are invalid, or if any pair of its constituent polygons intersect each other.

```ts
type Mask = Polygon[];
```
