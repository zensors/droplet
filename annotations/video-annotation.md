# Video Annotation

A **Video Annotation** is a reference to a video along with collected annotations about that video.  Right now, only object-detection-style annotations are supported, though others (such as video-segment labels or relationship labels) will be added in the future.

## Droplets

### FrameAnnotation

A `FrameAnnotation` is a single frame of a video.  It is identical to an `ImageAnnotation`, except that it does not define the `image` and `kind` fields.

```ts
interface FrameAnnotation {
	classes?: Record<string, ClassAnnotation>;
}
```

### Video

A `Video` is a reference to a video.  A frame is defined by one or more URIs (paths) at which it may be located.  When loading a video, implementers should use the first path that is able to be loaded successfully.  (Earlier paths may fail due to access controls or due to an unacceptable protocol.)

```ts
interface Video {
	paths: string[];
}
```

### Video Annotation

A `VideoAnnotation` is a collection of annotations on a given video.

```ts
interface VideoAnnotation {
	kind: "VideoAnnotation";
	video: Video;
	frames: FrameAnnotation[];
}
```

## Templates

### Frame Annotation Template

A `FrameAnnotationTemplate` describes what fields should be present in a `FrameAnnotation`.  It simply consists of a mapping of classes to `ClassAnnotationTemplate`s.  Note that a `FrameAnnotationTemplate` is directly compatible with `ImageAnnotationTemplate`.

```ts
interface FrameAnnotationTemplate {
	classes?: Record<string, ClassAnnotationTemplate>;
}
```

For `FrameAnnotationTemplate`, A ≤ B if and only if:

- For each class `C` in `A.classes`, `C` is present in `B.classes` and `A.classes[C]` ≤ `B.classes[C]`

### Video Annotation Template

A `VideoAnnotationTemplate` describes what fields should be present in a `VideoAnnotation`.  It currently only consists of a `FrameAnnotationTemplate`, which describes the structure of each frame.  (Note that the `video` field is not included in the template since it is always required.)

```ts
interface VideoAnnotationTemplate {
	frames: FrameAnnotationTemplate;
}
```
