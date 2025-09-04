
Android devices come with a wide range of screen **sizes** and **densities (DPI)**. To ensure images look sharp and consistent, Android uses **density buckets** and **resource qualifiers**.

---
## **Key Concepts**

- **Pixel (px):** Smallest unit of the screen. Device-dependent.
- **Density-independent Pixel (dp / dip):** Virtual pixel unit. Scales automatically with screen density.
    - **1 dp ≈ 1 px at 160 dpi** (baseline = mdpi).
- **Scale-independent Pixel (sp):** Same as dp, but also scales with user’s **font size preference**. Used for **text**.

---
## **Density Buckets**

|**Qualifier**|**DPI (dots per inch)**|**Scale factor**|
|---|---|---|
|ldpi|~120 dpi|0.75x|
|mdpi|~160 dpi (baseline)|1.0x|
|hdpi|~240 dpi|1.5x|
|xhdpi|~320 dpi|2.0x|
|xxhdpi|~480 dpi|3.0x|
|xxxhdpi|~640 dpi|4.0x|
|**nodpi**|—|no scaling|

---
## What is nodpi ?

- The nodpi qualifier means the resource is used **without any scaling** regardless of screen density.
- Commonly used for:
    - **Nine-patch drawables** (.9.png)
    - **Backgrounds, gradients, or images** where scaling would distort the appearance
    - **Assets meant to be pixel-perfect** (e.g., pre-scaled bitmaps, special cases like QR codes or barcodes)    

- Placed inside:

```
res/drawable-nodpi/
```

> **NOTE:** Be careful: nodpi images will look **physically smaller or larger** depending on the device density, since Android won’t scale them.

---
## **Image Placement**

- Place density-specific bitmaps in folders:
    - `res/drawable-mdpi/`
    - `res/drawable-hdpi/`
    - `res/drawable-xhdpi/` … etc.
- Place non-scaled images in:
    - `res/drawable-nodpi/`
- Android automatically picks the closest density asset, except for nodpi which is always used as-is.

---
## **Vector Drawables (Preferred)**

- Use **vector images (XML)** whenever possible → one file scales across all densities.
- Great for icons and simple graphics.
- Fallback to bitmaps only for complex images/photos.

---
## **Best Practices**

- Always design in **dp** (not px) for layouts.
- Use **sp** for text sizes.
- Prefer **vector drawables** over bitmap density sets.
- Use nodpi only when scaling would break the image.
- Test on different density emulators in Android Studio.

---
