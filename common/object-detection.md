# Object Detection

The Droplet format defines a number of types for representing objects detected in visual media.  Object detection annotations are organized around on _classes_.  Each class is a single category of object that you wish to detect; for example, `person` or `car`.  For each class being annotated, an image annotation may provide a list of _instances_, or singlular example of that object class in the image, and/or a list of _multi-instances_ or groups of that object in the image that were labeled once as an entire group.  Each instance and multi-instance may have associated geometric information, and instances may additionally have deeper _attribute_ information.

## Droplets

### Bounding Box

A `BoundingBox` is a tight axis-aligned rectangle containing a given detection.  It may also have an assigned confidence, which would primarily be used to represent model output.

```ts
interface BoundingBox {
  rectangle: Rectangle;
  confidence?: number;
}
```

### Segmentation

A `Segmentation` is a tight mask containing a given detection.  It may also have an assigned confidence, which would primarily be used to represent model output.

```ts
interface Segmentation {
  mask: Mask;
  confidence?: number;
}
```

### Keypoint

A `Keypoint` is an appearance of a point of interest for a particular class.  For example, an annotation might provide the position of the "left shoulder" keypoint for a "person," or the "back right tire" of a "car."  It may also have an assigned confidence, which would primarily be used to represent model output.

```ts
interface Keypoint {
  point: Point;
  occluded?: boolean;
  confidence?: number;
}
```

### Instance

An `Instance` is an appearance of a single example of a particular class within an image.  An instance may have any subset of the following fields:

- `id`: a unique id for the instance; generally only useful in scenarios where the same logical instance can appear in multiple places in the media, such as in a video
- `boundingBox`: a bounding box for the instance
- `segmentation`: a segmentation for the instance
- `keypoints`: a mapping from keypoint node names to their positions in the image (or `null` if the keypoint is not in-frame for this instance)
- `attributes`: a mapping from attribute names to their values

```ts
interface Instance {
  id?: string;
  boundingBox?: BoundingBox;
  segmentation?: Segmentation;
  keypoints?: Record<string, Keypoint | null>;
  attributes?: Record<string, string>;
}
```

### Multi-Instance

A `MultiInstance` is an appearance of a group of examples of a particular class within an image.  (Some datasets, such as COCO, call this a _crowd_.)  Note that there is not a strict definition for when a group of examples transitions from being many instances to being a single multi-instance.  It is only recommended that this be used for operating on premade datasets.

A multi-instance may have any subset of the following fields:

- `boundingBox`: a bounding box for the multi-instance
- `segmentation`: a segmentation for the multi-instance
- `count`: the number of true instances in the multi-instance

```ts
interface MultiInstance {
  boundingBox?: BoundingBox;
  segmentation?: Segmentation;
  count?: number;
}
```

### Class Annotation

A `ClassAnnotation` is the collection of all of the instances and multi-instances of a particular class within an image.

```ts
interface ClassAnnotation {
  instances?: Instance[];
  multiInstances?: MultiInstance[];
}
```

## Templates

### Instance Template

An `InstanceTemplate` describes what fields should be present for instances of a particular class.  This may consist of any subset of the following:

- `boundingBox`: if true, then instance annotations must provide bounding boxes; if omitted, treated as `false`
- `segmentation`: if true, then instance annotations must provide segmentations; if omitted, treated as `false`
- `keypoints`: if present, then instance annotations must provide a keypoint annotation for every listed keypoint; if omitted, treated as `[]` (no keypoints)
- `attributes`: if present, then instance annotations must provide an attribute annotation for every listed attribute, and it must come from the given set of values; if omitted, treated as `{}` (no attributes)

```ts
interface InstanceTemplate {
  id?: boolean;
  boundingBox?: boolean;
  segmentation?: boolean;
  keypoints?: string[];
  attributes?: Record<string, string[]>;
}
```

For `InstanceTemplate`, A ≤ B if and only if:

- `A.id` ⇒ `B.id`
- `A.boundingBox` ⇒ `B.boundingBox`
- `A.segmentation` ⇒ `B.segmentation`
- `A.keypoints` is a subset of `B.keypoints`
- For each attribute in `A.attributes`, the same attribute with exactly the same set of values is present in `B.attributes`

### Multi-Instance Template

A `MultiInstanceTemplate` describes what fields should be present for multi-instances of a particular class.  This may consist of any subset of the following:

- `boundingBox`: if true, then instance annotations must provide bounding boxes; if omitted, treated as `false`
- `segmentation`: if true, then instance annotations must provide segmentations; if omitted, treated as `false`
- `count`: if true, then multi-instance annotations must provide a count; if omitted, treated as `false`

```ts
interface MultiInstanceTemplate {
  boundingBox?: boolean;
  segmentation?: boolean;
  count?: boolean;
}
```

For `MultiInstanceTemplate`, A ≤ B if and only if:

- `A.boundingBox` ⇒ `B.boundingBox`
- `A.segmentation` ⇒ `B.segmentation`
- `A.count` ⇒ `B.count`

### Class Annotation Template

A `ClassAnnotationTemplate` describes what fields should be present on annotations of a particular class.  This may consist of any subset of the following:

- `instances`: an instance template; if omitted, treated as the most permissive template (i.e., the `InstanceTemplate` with all fields omitted)
- `multiInstances`: a multi-instance template; if omitted, treated as the most permissive template (i.e., the `MultiInstanceTemplate` with all fields omitted)

```ts
interface ClassAnnotationTemplate {
  instances?: InstanceTemplate;
  multiInstances?: MultiInstanceTemplate;
}
```

For `ClassAnnotationTemplate`, A ≤ B if and only if:

- `A.instances` ≤ `B.instances`
- `A.multiInstances` ≤ `B.multiInstances`