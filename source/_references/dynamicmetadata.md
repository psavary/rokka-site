---
title: Dynamic metadata
use: [references]
---

## Intro

Dynamic metadata is metadata added to a source image that changes the image identifying hash. Currently the only dynamic metadata supported is the [subject area](#subject-area).

## Add dynamic metadata to a source image

To add dynamic metadata to a source image, you need to provide the organization, the identifying hash of the image, the name of the dynamic metadata and the values to add. Do this by making a POST request to `https://api.rokka.io/sourceimages/{organization}/{hash}/meta/dynamic/{name}`

Rokka will generate a new identifying hash for the image and delete the old identifying hash. The new location of the image will be returned in the `Location` header of the response. 

In the following example, we are adding a subject area to an image.

```bash
curl -H 'Content-Type: application/json' -X PUT 'https://api.rokka.io/sourceimages/testorganization/0dcabb778d58d07ccd48b5ff291de05ba4374fb9/meta/dynamic/subjectarea' -d '[
    {
        "width": 20, 
        "height": 20, 
        "x": 0, 
        "y": 0
    }
]'
```


```php
use Rokka\Client\Core\DynamicMetadata\SubjectArea;

$client = \Rokka\Client\Factory::getImageClient('testorganization', 'apiKey', 'apiSecret');

$hash = '0dcabb778d58d07ccd48b5ff291de05ba4374fb9';

$dynamicMetadata = new SubjectArea(0, 0, 30, 230);

$newHash = $client->setDynamicMetadata($dynamicMetadata, $hash);

echo 'Updated subject area. New image hash: ' . $newHash . PHP_EOL;

```

## Delete dynamic metadata from a source image

To delete dynamic metadata from a source image, you need to provide the organization, the identifying hash of the image and the name of the dynamic metadata. Do this by making a DELETE request to `https://api.rokka.io/sourceimages/{organization}/{hash}/meta/dynamic/{name}`

Rokka will generate a new identifying hash for the image and delete the old identifying hash. The new location of the image will be returned in the `Location` header of the response. 

```bash
curl -H 'Content-Type: application/json' -X DELETE 'https://api.rokka.io/sourceimages/testorganization/0dcabb778d58d07ccd48b5ff291de05ba4374fb9/meta/dynamic/subjectarea'
```

## [Subject area](#subject-area)

The subject area of an image is a box defined by its width, height and the coordinates of its starting point (top-left corner). Setting the subject area of an image allows you to use the Crop operation with the `auto` setting, in which case the image will be cropped to only the subject area.

You can use this to specify the most important part of the image, the part that should be retained at any size.

### Properties

- `width` (required): Integer. The width of the subject area.
- `height` (required): Integer. The height of the subject area.
- `x`: Integer. The x-offset of the starting point, in pixels. The default value is 0.
- `y`: Integer. The y-offset of the starting point, in pixels. The default value is 0.
