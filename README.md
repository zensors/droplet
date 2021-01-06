<p align="center">
<img src="./assets/logo.png" width="400">
</p>

<p align="center" style="font-style: italic">
A JSON-based format for working with machine learning data, with a focus on data interoperability.

<br />
<br />

<a href="https://www.gnu.org/licenses/gpl-3.0">
  <img src="https://img.shields.io/badge/License-GPLv3-blue.svg" />
</a>
</p>

---

## About

**Droplet** is a JSON-based format for serializing and operating on machine learning annotations.  The primary goal of the format is to provide a structured way to specify an annotation to facilitate data interoperability and code reuse.

Though developed by [Zensors](https://zensors.com) for [dataTap](https://datatap.dev), Droplet is an open format and free for anyone to use.

## Why Droplet?

**Right now, every dataset and machine learning project uses its own data format.**  This drastically slows down the advancement of the field as more and more time is spent converting data between formats rather than working on algorithm improvements that drive real growth.  Moreover, with today's fragmented world of machine learning data formats, it's impossible to bring together data from multiple sources without spending time porting over the data.  Further, machine learning tools need to either be aware of all of these formats, or be rewritten from the ground up every time a new dataset emerges.

**Droplet is a standard format that solves this problem.**  By normalizing to a single format, we can:

- **Pull in data from multiple sources** – since every dataset is in the same format, you can mix and match annotations from different data sources with no effort.

- **Have a standardized set of tools** – code written for one project will work for every project.

- **Spend more time focusing on algorithms** – no more time needs to be spent on writing yet another set of one-off conversion scripts.

## Example

<table class="preview-table">
<tr>
<th style="width: 50%">Droplet</th>
<th style="width: 50%">Preview</th>
</tr>
<tr>
<td>

<details><summary>Show droplet</summary>

```json
{
  "image": {
    "paths": [
      "http://images.cocodataset.org/train2017/000000541901.jpg",
      "http://farm6.staticflickr.com/5310/5619616662_c1e5b34bd3_z.jpg"
    ]
  },
  "classes": {
    "car": {
      "instances": [
        {
          "boundingBox": {
            "rectangle": [[0.1778,0.0741],[0.4367,0.1315]]
          },
          "segmentation": {
            "mask": [
              [
                [0.1801,0.1047],
                [0.1881,0.0868],
                [0.2129,0.0875],
                [0.2478,0.0741],
                [0.3313,0.0823],
                [0.3751,0.1062],
                [0.4129,0.1129],
                [0.4307,0.1241],
                [0.4367,0.1315],
                [0.3989,0.1286],
                [0.1778,0.1069],
                [0.1856,0.1023]
              ]
            ]
          }
        },
        {
          "boundingBox": {
            "rectangle": [[0.8004,0.1322],[0.9991,0.1779]]
          },
          "segmentation": {
            "mask": [
              [
                [0.8187,0.1648],
                [0.9991,0.1779],
                [0.9967,0.1569],
                [0.9613,0.1386],
                [0.8662,0.1322],
                [0.8382,0.1423],
                [0.8040,0.1395],
                [0.8004,0.1569]
              ]
            ]
          }
        },
        {
          "boundingBox": {
            "rectangle": [[0.4938,0.0892],[0.7335,0.1539]]
          },
          "segmentation": {
            "mask": [
              [
                [0.5872,0.1420],
                [0.4938,0.1324],
                [0.4995,0.1087],
                [0.5168,0.0892],
                [0.5571,0.0926],
                [0.5979,0.0959]
              ],
              [
                [0.6120,0.0985],
                [0.6391,0.1011],
                [0.6463,0.1063],
                [0.6671,0.1226],
                [0.6873,0.1268],
                [0.6844,0.1493],
                [0.6053,0.1437]
              ],
              [
                [0.6957,0.1300],
                [0.7140,0.1333],
                [0.7291,0.1374],
                [0.7335,0.1400],
                [0.7311,0.1539],
                [0.6942,0.1509]
              ]
            ]
          }
        },
        {
          "boundingBox": {
            "rectangle": [[0.0000,0.0598],[0.0648,0.0936]]
          },
          "segmentation": {
            "mask": [
              [
                [0.0000,0.0598],
                [0.0235,0.0748],
                [0.0611,0.0832],
                [0.0648,0.0926],
                [0.0623,0.0936],
                [0.0348,0.0926],
                [0.0035,0.0917],
                [0.0000,0.0607]
              ]
            ]
          }
        }
      ]
    },
    "person": {
      "instances": [
        {
          "boundingBox": {
            "rectangle": [[0.1411,0.3198],[0.7087,0.9617]]
          },
          "segmentation": {
            "mask": [
              [
                [0.4444,0.3198],
                [0.4835,0.3221],
                [0.5195,0.3311],
                [0.5405,0.3401],
                [0.5495,0.3649],
                [0.5495,0.4009],
                [0.5465,0.4234],
                [0.5676,0.4392],
                [0.5826,0.4527],
                [0.5405,0.4595],
                [0.5285,0.4640],
                [0.5195,0.4887],
                [0.5495,0.5315],
                [0.5766,0.5946],
                [0.6486,0.6734],
                [0.6787,0.6779],
                [0.7087,0.6847],
                [0.6937,0.7005],
                [0.6937,0.7433],
                [0.6637,0.7613],
                [0.6066,0.7838],
                [0.5676,0.7883],
                [0.5556,0.7815],
                [0.4414,0.6554],
                [0.3393,0.6599],
                [0.3784,0.7050],
                [0.4024,0.7523],
                [0.4324,0.8063],
                [0.4835,0.8649],
                [0.4955,0.9077],
                [0.5345,0.9234],
                [0.5285,0.9505],
                [0.5135,0.9617],
                [0.4745,0.9617],
                [0.4054,0.9414],
                [0.3484,0.8739],
                [0.2763,0.7387],
                [0.1832,0.6802],
                [0.1411,0.6441],
                [0.1592,0.5495],
                [0.2222,0.4572],
                [0.3153,0.4009],
                [0.3514,0.3851],
                [0.3874,0.3716]
              ]
            ]
          }
        },
        {
          "boundingBox": {
            "rectangle": [[0.4468,0.0199],[0.4716,0.0353]]
          },
          "segmentation": {
            "mask": [
              [
                [0.4468,0.0305],
                [0.4501,0.0265],
                [0.4526,0.0219],
                [0.4549,0.0199],
                [0.4601,0.0199],
                [0.4641,0.0216],
                [0.4660,0.0240],
                [0.4687,0.0282],
                [0.4712,0.0318],
                [0.4716,0.0341],
                [0.4658,0.0353],
                [0.4528,0.0344],
                [0.4489,0.0344],
                [0.4476,0.0327],
                [0.4506,0.0318],
                [0.4487,0.0307]
              ]
            ]
          }
        }
      ]
    }
  }
}
```

</details>

</code></pre>
</td>
<td>
<img src="./assets/example.png" />

_Note: this image and annotation are reproduced from the [COCO dataset](https://cocodataset.org/)._
</td>
</tr>
</table>

## Specification

The Droplet specification has a few pieces that describe both the format and some features of its usage.  These are split into the following categories:

- [Concepts](./concepts.md)

- Common Types
  - [Geometry](./common/geometry.md)

- Annotation Types
  - [Image Annotation](./annotations/image-annotation.md)

## Using Droplet

In order to use droplet, you'll probably want to use bindings for whatever langauge you're using.  Right now, bindings for the following languages exist:

- Python: [`datatap-python`](https://github.com/zensors/datatap-python)

## Contributing

The current version of Droplet only has specific facilities for detection-style annotations on individual images, though other varieties of annotations (e.g., textual descriptions, relationships) and annotations on other subjects (e.g, video) will be added in the future.

Have an idea for how to improve Droplet?  We're looking for community contributions to help the format succeed!  Just [open an issue](https://github.com/zensors/droplet/issues) with your idea!
