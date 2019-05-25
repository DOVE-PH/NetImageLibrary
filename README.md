# .Net Image Library

.NET library for easy image handling like:
  * Filters
  * Cropping
  * Thumbnail creation
  * Saving as JPG/GIF/PNG files or streams
  * Opening images from streams or URLs

You'll find information on how to get started at http://kaliko.com/image-library/get-started/ and full API documentation at http://kaliko.com/image-library/api/

<a href="https://www.nuget.org/packages/ImageLibrary/">Get .Net Image Library from NuGet</a>

Using only safe code making this library possible to use on web hosts with medium trust.

When having access to full trust make sure to use ImageLibrary.FastFilters to greatly improve performance!

Current build contains the following filters:
  * Gaussian blur filter
  * Unsharpen mask filter
  * Chroma key filter
  * Contrast filter
  * Brightness filter
  * Invert filter
  * Desaturnation filter

If you plan using this library with WPF or simular, read this post on <a href="http://labs.kaliko.com/2011/03/convert-to-bitmapimage.html">how to convert an KalikoImage object to System.Windows.Media.Imaging.BitmapImage and System.Windows.Controls.Image</a>.

## Examples

### Crop scaling
Scale by cropping the image to cover the requested width and height:
```csharp
    using (var image = new KalikoImage(ImageFilePath)) {
        image
            .Scale(new CropScaling(500, 500))
            .SaveJpg(OutputFilePath, 80);
    }
```

### Fit scaling
Scale by fitting the image inside the requested width and height:
```csharp
    using (var image = new KalikoImage(ImageFilePath)) {
        image
            .Scale(new FitScaling(500, 500))
            .SaveJpg(OutputFilePath, 80);
    }
```

### Pad scaling
Scale by fitting the image inside the requested width and height and then pad to the width and height using colour:
```csharp
    using (var image = new KalikoImage(ImageFilePath)) {
        image
            .Scale(new PadScaling(500, 500, Color.Crimson))
            .SaveJpg(OutputFilePath, 80);
    }
```

### Focal point scaling
Similar to the crop scaling but takes a focal point as parameter to determine the point of interest when cropping the image:
```csharp
    using (var image = new KalikoImage(ImageFilePath)) {
        image
            .Scale(new FocalPointScaling(500, 500, 0.75, 1))
            .SaveJpg(OutputFilePath, 80);
    }
```

### Brightness filter
```csharp
    using (var image = new KalikoImage(ImagePath)) {
        image.ApplyFilter(new FastBrightnessFilter(50));
        image.SaveJpg(OutputPath, 80);
    }
```

### Contrast filter
```csharp
    using (var image = new KalikoImage(ImagePath)) {
        image.ApplyFilter(new FastContrastFilter(50));
        image.SaveJpg(OutputPath, 80);
    }
```

### Chroma key filter (green screen)
```csharp
    using (var image = new KalikoImage("c:\\images\\greenscreen.jpg"))) {
        // Make green screen parts transparent
        image.ApplyFilter(new FastChromaKeyFilter(Color.FromArgb(13, 161, 37), 40, 0.5f, 0.75f));

        using (var background = new KalikoImage("c:\\images\\backdrop.jpg")) {
            // Apply cleaned green screen image over backdrop
            background.BlitImage(image);
            background.SaveJpg(OutputPath, 80);
        }
    }
```

### Desaturate filter
```csharp
    using (var image = new KalikoImage(ImagePath)) {
        image.ApplyFilter(new FastDesaturationFilter());
        image.SaveJpg(OutputPath, 80);
    }
```

### Gaussian blur filter
```csharp
    using (var image = new KalikoImage(ImagePath)) {
        image.ApplyFilter(new FastGaussianBlurFilter(1.1f));
        image.SaveJpg(OutputPath, 80);
    }
```

### Invert filter
```csharp
    using (var image = new KalikoImage(ImagePath)) {
        image.ApplyFilter(new FastInvertFilter());
        image.SaveJpg(OutputPath, 80);
    }
```

### Unsharp mask filter
```csharp
    using (var image = new KalikoImage(ImagePath)) {
        image.ApplyFilter(new FastUnsharpMaskFilter(1.1f, 0.44f, 0));
        image.SaveJpg(OutputPath, 80);
    }
```

### Draw text
```csharp
    using (var image = new KalikoImage(ImagePath)) {
        var text = new TextField("Lorem ipsum") {
            Alignment = StringAlignment.Center,
            VerticalAlignment = StringAlignment.Center,
            Outline = 5,
            OutlineColor = Color.Red,
            Font = new Font("Arial", 60),
            Rotation = 30f,
            TextColor = Color.DarkOrange,
            TextShadow = new TextShadow(Color.FromArgb(128, 0, 0, 0), 4, 4)
        };
        image.DrawText(text);
        image.SaveJpg(OutputPath, 80);
    }
```

### Gradient fill
```csharp
    using (var image = new KalikoImage(ImagePath))
    {
        image.GradientFill(Color.FromArgb(128, 255, 200, 90), Color.FromArgb(128, 255, 64, 0));
        image.SaveJpg(OutputPath, 80);
    }
```

### Watermark
```csharp
    using (var image = new KalikoImage(ImagePath))
    {
        image.BlitFill("c:\\images\\semi-transparent-watermark.png"));
        image.SaveJpg(OutputPath, 80);
    }
```


## History

**4.0.0**
* Migrated projects to be compatible with .NET 4.x and Core
* Added FocalPointScaling as an alternative to crop scaling but that allows to define the point of interest

**3.0.0**
* Added faster filter alternatives for full trust environmnets
* Added SetResolution functions

**2.0.6**
* Added support to store original or set new resolution (DPI) 
* Added RotateFlip method
* Added access to bitmap data

**2.0.5**
* Rewritten file loader to prevent file locks
* Fixed image loading to ignore pixel-per-inch resolutions of original images
* Fixed constructor using System.Drawing.Image to support indexed palettes

**2.0.4**
* Added new TextField class for better text support
* Fixed scaling bug and updated test program 

**2.0.0**
  * Replaced Gaussian blur filter with better implementation (affects unsharpen masks)
  * Added chroma key filter
  * Rewritten API for Scaling
  * Added color space handling

**1.2.4**
  * Updated to Visual Studio 2010.
  * Code clean-up.<br>
  * Unwanted-border-artifact-problem fixed (thanks Richard!)<br>
  * IDisponable has been implemented.<br>

**1.2.3**
  * Minor changes.
  * First API documentation uploaded. Still missing a whole lot, but it's a start :)

**1.2.2**
  * Minor changes

**1.2.1**
  * Bug in thumbnail function fixed.
  * Code cleaned up.
