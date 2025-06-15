# GTK

The UI component library used by Xfce.

## History

Started as GIMP Toolkit, an outgrowth of the UI layer for GIMP extracted to a
library. Then took life of its own, among the three popular (but not exclusive)
libraries people used for writing GUI code:

* Motif
* GTK
* Qt

Motif was a proprietary thing that ultimately faded away. GTK was inspired, the
API was, from Motif, since the dudes who wrote GIMP (and eventually wrote
Cockroach DB, what an idiotic name) wanted to use Motif but couldn't for
licensing reasons I presume so they retained-ish the API but wrote the
implementation from scratch.

Qt was (and I think still is somewhat?) proprietary (made by a Norwegian company
called, you guessed it, Trolltech, au contraire what a delightful name), but
with enough sprinkling of open sourcedness for it to not meet Motif's fate.

These were not the only GUI component libraries (e.g. Xfce initially used
something called X Forms, that's where the now a historical artifact name comes
from - **X** **F**orms **C**omponents **E**nvironment), but with the fadeout of
Motif, the GTK/Qt duopoly became de facto.

You could write UI components directly using the X API (and my word, maybe one
day I should!). But I presume it (a) was too low level, and (b) each app looked
like a thing of its own. Charming no doubt, but no good if you're trying to do
the "year of the linux desktop" by providing a consistent "desktop environment".

> Now there is one more reason. X is not the only display server in town.

So the lines were drawn as the two major (but not the only! I'm writing on a
this on a third one, Xfce) attempts at providing consistent desktop environments
settled on their UI component library of choice.

GTK was the choice for GNOME, and Qt for KDE.

Beyond keeping the G in GTK relevant, the GNOME association played out in a more
important way. The GNOME folks were maintaining GTK, and were by far the biggest
user of GTK, so over time they started to make it more and more bespoke to their
requirement.

This didn't play out well with the rest of the application developers who were
independently using GTK, and that play has not ended yet as I write. The major
and painful breaking changes that come with GTK major versions induce further
teeth grashing.

While all this is happening (over decades!), Xfce needs to settle on a UI
component library. GTK is painful and heavy, but Qt is ugly (and heavy), so they
settle on GTK.

> GTK had several names and versions. GTK 1/2/3/4, and there was a GTK+ at some
> point. None of that is relevant. As of 2025, there is only GTK 3, and GTK 4,
> and both are used.

Currently, Xfce uses GTK 3. GTK has itself moved on to GTK 4. Xfce used to use
GTK 2, and since they took the desert walk from 2 to 3 I presume they'll also go
along with the 3 to 4 joyride at some point.
