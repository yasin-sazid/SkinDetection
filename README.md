# RGB-Based Skin Detection

A Java image-classification application that learns skin-color statistics from image/mask pairs and applies them to new images. The project implements the training and pixel-classification workflow directly with Java image APIs and a three-dimensional RGB lookup table.

## Learning approach

For each RGB value, training accumulates separate counts for skin and non-skin pixels. `ImageProcessor` normalizes both distributions and calculates a skin-to-non-skin likelihood ratio. The resulting `256 × 256 × 256` table provides a direct lookup for classification.

This explicit statistical representation makes the transformation from labeled pixels to an image-processing model visible in the code.

## Workflow

```text
Training images + reference masks
                |
               RGB
                |
      Skin / non-skin histograms
                |
         ImageProcessor
                |
      Serialized Database object
                |
          Result → Output image
```

## Components

- `mainPackage.Main`: interactive **Train** and **Test** menu.
- `FolderToFiles`: image discovery.
- `RGB`: image/mask reading and pixel-count accumulation.
- `ImageProcessor`: class normalization and likelihood-ratio calculation.
- `Database`: serializable trained-data container.
- `Result`: pixel classification and output-image generation.

## Run

Use a JDK and open `noobML/` as the project directory:

```bash
cd noobML
javac -d /tmp/skin-classes src/mainPackage/*.java src/package123/*.java
java -cp /tmp/skin-classes mainPackage.Main
```

Provide the training image/mask folder at the prompt. The trainer associates BMP/JPG files by their identifying filename content. Prepare a writable `Database/` directory in the working directory for `database.ser` and generated images. The test workflow loads that serialized table and writes images using the `Database/output` filename prefix.

The `photos/` directory contains sample imagery. Java ImageIO, buffered images, collections, and object serialization provide the application's supporting infrastructure.
