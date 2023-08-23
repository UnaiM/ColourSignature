ColourSignature (Draft)
=======================

**A framework to handle image colour spaces, independently of colour management systems.**


Motivation
----------

In [VFX][1], we normally use [OCIO][2] to (in layman’s terms) tell the computer how to transform colour between so-called ‘colour spaces’, to account for different image encoding and viewing scenarios. Simply put, OCIO gets a **context** and a colour, and returns a (normally different) colour that suits the requested context; usually over and over until every pixel of an image gets defined.

But what do I mean “context”? For example, at one point the artist might want to view their work as the client-side Editorial fepartment will be grading it; or with very “flat” colours to tech-check the dark areas ([Log][3] space)… Those are valid contexts, but the one I’m interested in for this repo, is _figuring out what the camera means_ when it stores each colour that forms an image.



1. OCIO is focused on **implementations** of colur transforms, i.e. one of its goals is for the computer to be able to transform colours quickly. This means that, as technology evolves, the preferred way of encoding the same colour space might change.
2. OCIO is **project-centric:** colour space definitions in an OCIO config should serve the project they’re being used on. This doesn’t just have an impact on naming (e.g. `Cam. B Plate` instead of `Input - ARRI - Linear - Alexa Wide Gamut`), but also on implementation again— handling [ARRI][4] footage on an [ACES][5] project could very well involve matrix transforms, while in an ARRI project it wouldn’t.


[1]: https://en.wikipedia.org/wiki/Visual_effects 'Visual effects'
[2]: https://opencolorio.org/ 'OpenColorIO'
[3]: https://en.m.wikipedia.org/wiki/Log_profile 'Log profile'
[4]: https://www.arri.com 'ARRI'
[5]: https://www.oscars.org/science-technology/sci-tech-projects/aces 'Academy Color Encoding System'
