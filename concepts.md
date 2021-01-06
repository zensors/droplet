# Concepts

The primary goal of the Droplet format is to facilitate data interoperability.  To this end, Droplet defines a number of common data types such that the same code that can be used to operate on one kind of data can be used to operate on any other compatible data as well.

## Droplets and Templates

At the center of the design are _droplets_, which hold ML data, and _templates_, which describe what ML data is present.  For example, consider a case where we have an image, and a collection of bounding boxes on that image that tell us where the people are in that image.  In this case, the template would be a representation of the statement "there is an image, and a set of bounding boxes representing the people in that image", while the droplet would be the actual image alongside the actual bounding boxes for the people in the image.

This is analogous to having a value of a given type in most programming languages; here, the type is the template (which describes the form of the available data), and the value is the droplet (which actually holds that data).

More concretely, here is a specific example of what that would look like in the Droplet format:

#### Template
```json
{
  "kind": "ImageAnnotation",
  "classes": {
    "person": {
      "instances": {
        "boundingBox": true
      }
    }
  }
}
```

_This template says that we have an image annotation for which we annotate a class called `person`, and for that class we annotate instances, and for those instances we provide a bounding box._

#### Droplet
```json
{
  "kind": "ImageAnnotation",
  "image:": {
    "paths": ["http://example.com/path/to/image"]
  },
  "classes": {
    "person": {
      "instances": [
        {
          "boundingBox": {
            "rectangle": [[0.2, 0.4], [0.3, 0.6]]
          }
        },
        {
          "boundingBox": {
            "rectangle": [[0.6, 0.3], [0.8, 0.9]]
          }
        }
      ]
    }
  }
}
```

_This droplet actually provides the content of the annotation specified in the droplet: an image on which we have annotated the `person` class, for which we have annotated instances, for which we have provided bounding boxes._

## Template Bounding

A common operation with droplets and templates is to answer whether or not a particular droplet matches a given template; in other words, for field that the template says should be present, is that field actually present in the droplet?

This concept, which we call **lower-bounding**, is key to data interoperability.  By querying which droplets are _lower-bounded_ by a given template, we can find all of the available data that can be used for training models on a particular task!

The opposite of this, called **upper-bounding**, is less common but just as important.  An example of when upper-bounding is useful is when providing machine-generated pre-labels for data collection.  Here, rather than the droplet specifying a _superset_ of the fields specified in the template, we want to ensure that a droplet specifies a _subset_ of the fields in the template.  For example, if we want to collect bounding boxes and segmentations on instances of "car" in an image, then it's ok if we have instances that have no bounding boxes but which do have segmentations, but it's _not_ ok if we have excess information, such as a "front left tire" keypoint.

The actual mechanics of bounding are actually always carried out between pairs of templates.  This is because droplets are always stored with a reference to the template that they match, which sometimes contains information not present in the droplet itself (such as the possible values for a particular instance attribute).

If you're familiar with TypeScript (or any other language with structural typing), then this will likely seem familiar to you; whether or not a template A upper bounds a template B is essentially the same question as "is a value of type A assignable to type B."

Whenever we define a template type, we provide rules for a relation "A ≤ B", which denotes that "A lower-bounds B," in order to fully define the template bounding rules for that particular template type.  Note that it is posible for "A ≤ B" and "B ≤ A" to be false statements simultaenously; this is because two templates may be mutually incompatible.  (If you have a set theory background, you might say that templates are a [poset](https://en.wikipedia.org/wiki/Partially_ordered_set).)
