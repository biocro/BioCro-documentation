# Light Absorption by a Thick Layer

## 1 Overview

This vignette explains my understanding of the formula used in the
`thick_layer_absorption` function found in the C++ source code file
`src/module_library/sunML.cpp`. Although it has been used in BioCro,
this equation seems to be rarely discussed in the plant biology
literature. As far as I can tell, it is ultimately based on Equation (1)
from Saeki ([1960](#ref-saeki_interrelationships_1960)). However, the
description in that paper is short and very sparse regarding details.
Here I have tried to add some explanatory comments and fill in a few
math steps that were skipped. I have tried to follow the “spirit” of
that paper, although I have made a few notational changes. See Section
[4](#sec:caveat) for more information about tracking this equation
through various references.

## 2 Absorption by a Generic Material

Suppose light of intensity \\I_0\\ (representing a flux density of
photons or energy, expressed in units of photons per area per time, or
energy per area per time) is incident on the perfectly flat surface of
an infinitely thick piece of material that reflects, absorbs, and
transmits light. Assume that the optical properties and density of the
material are constant along any plane parallel to the material’s
surface, although they may vary with depth within the material. In this
situation, the light intensity at any point within the material will
only depend on the distance from the material’s surface, so we can
reduce the number of relevant dimensions from three to one, an important
simplification.

As the light passes through the material, its intensity will gradually
diminish until it eventually reaches zero. We can express this
mathematically via a one-dimensional expression \\\begin{equation} I(x)
= I_0 \cdot f(x), \end{equation}\\ where \\x\\ is a coordinate that
represents the amount of material the light has passed through, and
\\f(x)\\ is the fraction of the original light received by the material
at \\x\\. Note that if the material does not have uniform density, \\x\\
is not a spatial coordinate, but instead will have a non-linear
dependence on distance. Although we do not know the particular form of
\\f(x)\\, we can safely assume that:

- \\f\\ is monotonic.

- \\f(0) = 1\\ (so the incident light at the material’s surface is
  \\I_0\\).

- \\f(x)\\ approaches 0 as \\x\\ approaches infinity (so the light
  intensity is fully diminished deep within the material).

Now consider two points \\x\\ and \\x + \Delta x\\ within the material,
separated by an amount of material \\\Delta x\\. The change in light
intensity per unit material between these two points, \\\Delta I/\Delta
x\\, can be expressed as \\\begin{equation} \frac{\Delta I}{\Delta x} =
\frac{I(x + \Delta x) - I(x)}{\Delta x}. \end{equation}\\ Rewriting this
using \\f(x)\\, we have \\\begin{equation} \frac{\Delta I}{\Delta x} =
I_0 \cdot \frac{f(x + \Delta x) - f(x)}{\Delta x}. \end{equation}\\ If
we assume that the change in intensity is due to the absorption and
reflection of light as we pass from point \\x\\ to point \\x + \Delta
x\\, we can also write \\\begin{equation} \frac{\Delta I}{\Delta x}
\approx - I(x) \cdot \left( A(x) + R(x) \right), \end{equation}\\ where
\\A(x)\\ and \\R(x)\\ are the fractions of incident light absorbed and
reflected by a thin layer of the material at \\x\\ and the negative sign
indicates that the overall light intensity *decreases* due to absorption
and reflection. Thus, we can equate the two expressions for \\\Delta I /
\Delta x\\ to find that \\\begin{equation} I_0 \cdot \frac{f(x + \Delta
x) - f(x)}{\Delta x} \approx - I_0 \cdot f(x) \cdot \left( A(x) + R(x)
\right) \end{equation}\\ or, equivalently, \\\begin{equation}
\frac{f(x + \Delta x) - f(x)}{\Delta x} \approx - f(x) \cdot \left(
A(x) + R(x) \right) \end{equation}\\ As \\\Delta x\\ approaches 0, the
approximation becomes exact and we can recognize the left-hand side of
this equation as the derivative of \\f(x)\\ with respect to \\x\\:
\\\begin{equation} \frac{df}{dx}(x) = - f(x) \cdot \left( A(x) + R(x)
\right). \end{equation}\\ Rearranging, we can express \\f(x)\\ in terms
of its derivative and the material’s optical properties:
\\\begin{equation} f(x) = - \frac{\frac{df}{dx}(x)}{A(x) + R(x)}.
\end{equation}\\ Noting that all light received by a thin layer of the
material must be reflected, absorbed, or transmitted, we can use
\\A(x) + R(x) + T(x) = 1\\ to rewrite this equation in terms of the
fraction of transmitted light \\T(x)\\: \\\begin{equation} f(x) = -
\frac{\frac{df}{dx}(x)}{1 - T(x)} \tag{2.1} \end{equation}\\

### 2.1 Additional Considerations (Especially for Plant Biology)

This simple model for calculating light intensities within a material
was first applied in the context of plant biology by Monsi and Saeki
([1953](#ref-monsi_uber_1953)), where it is used to calculate light
levels within a plant canopy. This application is the main reason for
why we describe \\x\\ as an amount of material rather than a depth; in
plant canopies, the leaves are not uniformly distributed with depth, so
the cumulative leaf area index is a better independent variable for
light absorption.

Monsi and Saeki ([1953](#ref-monsi_uber_1953)) is difficult to find and
was written in German, but parts of it have been reproduced in English
and are easier to access ([Saeki
1960](#ref-saeki_interrelationships_1960),
[1963](#ref-saeki_chapter_1963); [Hirose
2004](#ref-hirose_development_2004)). Saeki
([1960](#ref-saeki_interrelationships_1960)) notes the following about
this equation:

> It must be noted that in these equations \\m\\ includes not only the
> fraction resulting from light transmitted through the leaf blades but
> also the fraction reflected downward from inclined leaves. This \\m\\
> is not constant but increases with the depth of foliage, because light
> of particular wavelengths is more liable to be reflected and
> transmitted, and increases in proportion at deeper positions.

(In the original notation, \\m\\ was used in place of the \\T(x)\\ we
use here.) Thus, \\T\\, \\R\\, and \\A\\ are not exactly the same as the
corresponding optical properties of an isolated layer of the material
(such as a leaf).

## 3 Total Absorption

If we consider light from a sufficiently narrow wavelength band, then it
may be reasonable to suppose that \\T\\, \\R\\, and \\A\\ are constant
throughout the material. In this case, it is possible to estimate the
total amount of light absorbed by the material. To do this, we first
calculate the absorbed light at depth \\x\\ per unit material (\\d
I\_\text{abs}(x) / dx\\), using \\d I\_\text{abs}(x) / dx = I(x) \cdot
A\\. Substituting in Equation [(2.1)](#eq:deriv) and recalling that \\A
= 1 - T - R\\, we have \\\begin{equation} \frac{d I\_\text{abs}}{dx}(x)
= - I_0 \cdot \frac{1 - R - T}{1 - T} \cdot \frac{df}{dx}(x).
\end{equation}\\ Now we can integrate this across the entire range of
\\x\\ (0 to \\\infty\\) to find the total amount of light absorbed by
the material (\\I\_\text{abs (total)}\\): \\\begin{equation}
I\_\text{abs (total)} = I_0 \cdot \frac{1 - R - T}{1 - T} \cdot (f(0) -
f(\infty)). \end{equation}\\ By assumption, \\f(0) = 1\\ and \\f(\infty)
= 0\\, so this evaluates to \\\begin{equation} I\_\text{abs (total)} =
I_0 \cdot \frac{1 - R - T}{1 - T} \tag{3.1} \end{equation}\\

Note that Equation [(3.1)](#eq:total) agrees with intuition in two
extreme situations:

- If the material does not reflect any light (\\R = 0\\), then Equation
  [(3.1)](#eq:total) reduces to \\I\_\text{abs (total)} = I_0\\. This
  makes sense because even if thin layers of the material transmit
  light, there is no way for any light to avoid being absorbed by an
  infinitely thick layer if there is no reflection.

- The other situation is where the material does not transmit any light
  (\\T = 0\\). In this case, Equation [(3.1)](#eq:total) reduces to
  \\I\_\text{abs (total)} = I_0 \cdot (1 - R)\\. This makes sense
  because the optical properties of a material with no transmission
  would be determined only by its surface; the surface would have the
  same optical properties as any thin layer, reflecting a fraction \\R\\
  of the light and absorbing the rest.

### 3.1 Additional Considerations (Especially for Plant Biology)

Although Equation [(2.1)](#eq:deriv) was originally developed for plant
canopies, it does not rely on any specific properties of canopies and
can in principle apply to any material. (In fact, we have written this
derivation in a material-agnostic way to emphasize this.) Thus, Equation
[(3.1)](#eq:total) can also apply to a wide variety of materials. The
absorption and reflection of light by soil is another situation where
Equation [(3.1)](#eq:total) may be useful, as the assumption of a thick
layer that does not transmit any light through it is certainly justified
in that scenario.

Because it assumes a thick layer of a homogeneous light-absorbing
material, Equation [(3.1)](#eq:total) is not appropriate for use in a
layered canopy model or one that makes distinctions between different
leaf classes (such as sunlit and shaded). It is best used for situations
such as estimating whole-canopy transpiration or soil evaporation, where
it is useful to know the total solar energy absorbed by a thick layer of
leaves or soil. Care must be taken even in this case, however, since
this equation would still not be appropriate for the small canopies of
young plants, which certainly transmit a significant fraction of the
incident light.

## 4 Caveats From the Author

Although Equation [(2.1)](#eq:deriv) can be found in Monsi and Saeki
([1953](#ref-monsi_uber_1953)), Saeki
([1960](#ref-saeki_interrelationships_1960)), and Saeki
([1963](#ref-saeki_chapter_1963)), Equation [(3.1)](#eq:total) is not
included in those references. So, although this derivation makes sense
to me, there is still a chance that it might not be correct. Equation
[(3.1)](#eq:total) can be found in Humphries
([2002](#ref-humphries_will_2002)), the WIMOVAC code, and the BioCro
code. In these places, it is variously attributed to John H. M. Thornley
and Johnson ([1990](#ref-thornley_plant_1990)) or Monteith and Unsworth
([1990](#ref-monteith1990principles)). Unfortunately, these textbooks
are not available in electronic form. I have looked in a library copy of
Monteith and Unsworth ([1990](#ref-monteith1990principles)) and in
electronic versions of newer editions, but have not been able to find
this equation. I have attempted to find an explanation for Equation
[(3.1)](#eq:total) elsewhere but have not been successful so far. J. H.
M. Thornley ([2002](#ref-thornley_instantaneous_2002)) discusses
Equation [(2.1)](#eq:deriv), but ultimately just references the
(currently inaccessible) textbook John H. M. Thornley and Johnson
([1990](#ref-thornley_plant_1990)).

## References

Hirose, Tadaki. 2004. “Development of the Monsi–Saeki Theory on Canopy
Structure and Function.” *Annals of Botany* 95 (3): 483–94.
<https://doi.org/10.1093/aob/mci047>.

Humphries, SW. 2002. “Will Mechanistically Rich Models Provide Us with
New Insights into the Response of Plant Production to Climate Change? :
Development and Experiments with WIMOVAC : (Windows Intuitive Model of
Vegetation Response to Atmosphere & Climate Change).” PhD Thesis,
University of Essex.
<https://ethos.bl.uk/OrderDetails.do?uin=uk.bl.ethos.268715>.

Monsi, Masami, and Toshiro Saeki. 1953. “Über Den Lichtfaktor in Den
Pflanzengesellschaften Und Seine Bedeutung Für Die Stoffproduktion.”
*Japanese Journal of Botany* 14: 2252.

Monteith, John Lennox, and MH Unsworth. 1990. “Principles of
Environmental Physics.”

Saeki, Toshiro. 1960. “Interrelationships Between Leaf Amount, Light
Distribution and Total Photosynthesis in a Plant Community.”
*Shokubutsugaku Zasshi* 73 (860): 55–63.
<https://doi.org/10.15281/jplantres1887.73.55>.

Saeki, Toshiro. 1963. “CHAPTER 6 - Light Relations In Plant
Communities.” In *Environmental Control of Plant Growth*, edited by L.
T. Evans, 79–94. Academic Press.
<https://doi.org/10.1016/B978-0-12-244350-3.50010-0>.

Thornley, J. H. M. 2002. “Instantaneous Canopy Photosynthesis:
Analytical Expressions for Sun and Shade Leaves Based on Exponential
Light Decay Down the Canopy and an Acclimated Non‐rectangular Hyperbola
for Leaf Photosynthesis.” *Annals of Botany* 89 (4): 451–58.
<https://doi.org/10.1093/aob/mcf071>.

Thornley, John H. M., and I. R. Johnson. 1990. *Plant and Crop
Modelling: A Mathematical Approach to Plant and Crop Physiology*.
