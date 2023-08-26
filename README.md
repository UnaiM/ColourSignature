ColourSignature (Draft)
=======================

**A hashing algorithm for colour spaces, independent of their implementation.**


Motivation
----------

In VFX, we normally use [OCIO][1] for colour management; when visual media enters the pipeline, we correlate it with the name of a colour space defined in an OCIO config. I think ideally **this isn’t enough,** because:

1. OCIO is focused on **implementations** of colour transforms, i.e. one of its goals is for the computer to be able to transform colours quickly. This means that, as technology evolves, the preferred way of encoding the same colour space might change.
2. OCIO is **project-centric:** colour space definitions in an OCIO config should serve the project they’re being used on. This doesn’t just have an impact on naming (e.g. `Cam. B Plate` instead of `Input - ARRI - Linear - Alexa Wide Gamut`), but also on implementation again— handling [ARRI][2] footage on an [ACES][3] project could very well involve matrix transforms, while in an ARRI project it wouldn’t.
3. If some media is to be used across projects, this usually implies maintaining a studio-wide OCIO config, and generating potentially **lossy colour-transformed copies** of the media.

Colour transforms and their implementations aside, conceptually **there’s only one correct input colour space** for each piece of visual media. Therefore, there _has_ to be a more **unambiguous, OCIO config-agnostic** way of leveraging this.


Objective
---------

Contrary to what the last paragraph could imply, **this isn’t about encoding colour spaces** perfectly with all their intricacies— we can already achieve this using [CTL][4], and OCIO’s restrictions (it doesn’t process CTL, it needs baked LUTs) expertly respond to current technology limitations. **ColourSignature doesn’t replace CTL or OCIO _at all._**

Instead, this is about identifying a colour space’s distinctive characteristics, in a more robust and interchangeable way, than just referring to a name that an OCIO config also happens to use. **ColourSignature is a [hashing algorithm][5]** yet to be implemented, and the idea is for it to be the _best_ at hashing colour spaces:

1. It should produce **the same hash for the same colour space** no matter how it’s encoded, e.g.:
   * Specifying a transform _from_ ACES2065-1 **vs** specifying a transform _to_ [CIE 1931 XYZ][6] ([D65][7]).
   * With a [matrix][8] and a [logarithmic function][9] **vs** with a [3D LUT][10].
2. It should produce **different hashes for different colour spaces** even between very similar pairs, e.g.:
   * ARRI LogC3 ([EI][11]640) Wide Gamut 3 **vs** ARRI LogC3 (EI800) Wide Gamut 3.
   * ACEScc **vs** ACESproxy.
3. It should produce relatively **short hashes,** so that (at least) once generated, using them is fast.


Applications
------------

Here’s a list of what ColourSignature could be used for:

* **Cataloguing media** in a way that’s ressilient to different colour management strategies, provided there’s an OCIO config that correctly defines that media’s colour space, to generate the hash.
* **Automating colour space selection** when loading media; again, this will still work if the OCIO config needs to change mid-production for some reason.
* **Sanity-checking OCIO configs,** in cases where a colour space is defined both from and to the reference space— if both directions yield the same hash, there will be no surprises.
* **Finding duplicate colour spaces,** helping studios simplify their OCIO configs.


[1]: https://opencolorio.org 'OpenColorIO'
[2]: https://arri.com/en/learn-help/learn-help-camera-system/image-science 'ARRI Image Science'
[3]: https://oscars.org/science-technology/sci-tech-projects/aces 'Academy Color Encoding System'
[4]: https://oscars.org/science-technology/sci-tech-projects/color-transformation-language 'Color Transformation Language'
[5]: https://en.wikipedia.org/wiki/Hash_function 'Hash function'
[6]: https://en.wikipedia.org/wiki/CIE_1931_color_space 'CIE 1931 color space'
[7]: https://en.wikipedia.org/wiki/Illuminant_D65 'Illuminant D65'
[8]: https://filmmakingelements.com/what-is-a-3x3-matrix-in-color-grading 'What Is A 3×3 Matrix In Color Grading'
[9]: https://en.wikipedia.org/wiki/Log_profile 'Log profile'
[10]: https://en.wikipedia.org/wiki/3D_lookup_table '3D lookup table'
[11]: https://en.wikipedia.org/wiki/Film_speed#EI 'Film speed'
