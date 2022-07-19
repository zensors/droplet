# Image Annotation

An **Image Annotation** is a reference to an image along with collected annotations about that image.  Right now, only object-detection-style annotations are supported, though others (such as image-level labels or relationship labels) will be added in the future.

## Droplets

### Image

An `Image` is a single picture taken by a camera.  An image is defined by one or more URIs (paths) at which it may be located.  When loading an image, implementers should use the first path that is able to be loaded successfully.  (Earlier paths may fail due to access controls or due to an unacceptable protocol.)

```ts
interface Image {
  paths: string[];
}
```

### Image Annotation

An `ImageAnnotation` is a collection of annotations on a given image. See [Object Detection](../common/object-detection.md) for a description of the `ClassAnnotation` type.

```ts
interface ImageAnnotation {
  kind: "ImageAnnotation";
  image: Image;
  classes: Record<string, ClassAnnotation>;
}
```

## Templates

### Image Annotation Template

An `ImageAnnotationTemplate` describes what fields should be present in an `ImageAnnotation`.  It simply consists of a mapping of classes to `ClassAnnotationTemplate`s.  (Note that the `image` field is not included in the template since it is always required.)

```ts
interface ImageAnnotationTemplate {
  classes: Record<string, ClassAnnotationTemplate>;
}
```

For `ImageAnnotationTemplate`, A ≤ B if and only if:

- For each class `C` in `A.classes`, `C` is present in `B.classes` and `A.classes[C]` ≤ `B.classes[C]`
