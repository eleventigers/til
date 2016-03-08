# Panorama to Skybox

While making custom Unity skybox materials I learned that 360 degree panorama images are actually maps in [equirectangular projection](https://en.wikipedia.org/wiki/Equirectangular_projection). Search using the `equirectangular` term led me to find some awesome resources which I used to build cubemaps with less pain than expected:

- [NoEmotion HDRs](http://noemotionhdrs.net) - amazing quality nature and urban equirectangular HDR maps under permissive CC license.
- [LuminanceHDR](https://github.com/LuminanceHDR/LuminanceHDR) - great open source application for HDR image editing. I used it to tonemap and export NoEmotion panoramas as Low Dynamic Range (LDR) images.
- [Cubemap](https://github.com/seiferteric/cubemap) - effective little script to slice panoramas into 6 face image sets (cubemaps).
- [Panorama to Cubemap](https://www.assetstore.unity3d.com/en/#!/content/13616) - Unity editor script to convert singular equirectangular map textures into cubemaps. Not too slow, works reliably and most importantly free.
