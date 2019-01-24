
dried-utils
====

useful pure data abstractions
----

This is my personal repository of pd abstractions, a little bag-o-tricks. Feel free to use these, though they might change. If the documentation isn't clear, check out the abstraction patch itself.

I mostly use purr-data, so vanilla pd compatibility is questionable.

# The abstractions

- **adsr~** - a simple adsr~ envelope for cases where bundled adsr~ is inadequate. Send it a list of attack, decay, sustain, release (all ms).
- **cc** - simply capture and use a MIDI continuous controller. Open the toggle to begin capturing a controller, and close to select the last used controller. Output is scaled from zero - first-argument. Second argment can be used to set the initial controller number. Right output sends controller number whenever toggle is switched off. _(Setting the initial CC number to 0 doesn't work, but any other number does.)_
- **cc-b** - like `cc` but sends a bang for values above zero. Useful for buttons. Argument can be used to set initial controller number.
- **cc-t** - like `cc-b` but alternately sends `1` and `0`. Useful for buttons masquerading as toggles. Argument can be used to set initial controller number.
- **some-harmonic** - outputs a new frequency which is a random integer multiple of the input frequency (or the last output frequency), wrapped to be lower than a given frequency. `$1`: wrap point and start frequency if none specified. `$2`: lower harmonic limit. `$3`: upper harmonic limit.
- **line-eased~** - generates a ramp with a sinusoidal ease-in-out curve. Same API as `line~` but ignores `stop` messages.
- **pan~** - drop-in replacement for `pan` but using signal as right input for smoother panning changes
- **pop~** - makes an electronic-ey pop sound.
- **rand-range** - outputs a random int between two argument ints. Bang for a new number.
- **some-subharmonic** - like `some-harmonic` but with subharmonics.
