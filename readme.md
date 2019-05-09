
dried-utils
====

useful pure data abstractions
----

This is my personal repository of pd abstractions, a little bag-o-tricks. Feel free to use these, though they might change. If the documentation isn't clear, check out the abstraction patch itself.

I mostly use purr-data, so vanilla pd compatibility is questionable.

# The abstractions

- **adsr~** - a simple adsr~ envelope for cases where bundled adsr~ is inadequate. Send it a list of attack, decay, sustain, release (all ms).
- **cc** - simply capture and use a MIDI continuous controller. Open the toggle to begin capturing a controller, and close to select the last used controller. Output is scaled from zero - first-argument. Second argment can be used to set the initial controller number. Right output sends controller number whenever toggle is switched off. _(~~Setting the initial CC number to 0 doesn't work, but any other number does.~~ 0 is now allowed, but passes through all incoming values until a controller is set.)_
- **cc-b** - like `cc` but sends a bang for values above zero. Useful for buttons. Argument can be used to set initial controller number.
- **cc-t** - like `cc-b` but alternately sends `1` and `0`. Useful for buttons masquerading as toggles. Argument can be used to set initial controller number.
- **some-harmonic** - outputs a new frequency which is a random integer multiple of the input frequency (or the last output frequency), wrapped to be lower than a given frequency. `$1`: wrap point and start frequency if none specified. `$2`: lower harmonic limit. `$3`: upper harmonic limit.
- **lfo~** - a quick-and-dirty oscillator between two values. Oscillates between `$2` and `$3` at `$1`hz.
- **line-eased~** - generates a ramp with a sinusoidal ease-in-out curve. Same API as `line~` but ignores `stop` messages.
- **pan~** - drop-in replacement for `pan` but using signal as right input for smoother panning changes
- **pop~** - makes an electronic-ey pop sound.
- **rand-range** - outputs a random int between two argument ints. Bang for a new number.
- **some-subharmonic** - like `some-harmonic` but with subharmonics.
- **spigot~** - a spigot~ for audio that opens when sent its creation argument, closes for anything else.
- **ss-saw~** - a sine-summed sawtooth wave using an arbitrary number of harmonics. `$1`: frequency. `$2`: number of harmonics. Shorthand for the technique described [here](http://write.flossmanuals.net/pure-data/generating-waveforms/).
- **ss-squ~** - a sine-summed square wave using an arbitrary number of harmonics. `$1`: frequency. `$2`: number of harmonics. Shorthand for the technique described [here](http://write.flossmanuals.net/pure-data/generating-waveforms/).
- **ss-tri~** - a sine-summed triangle wave using an arbitrary number of harmonics. `$1`: frequency. `$2`: number of harmonics. Shorthand for the technique described [here](http://write.flossmanuals.net/pure-data/generating-waveforms/).
- **widen~** - set-and-forget stereo widening using the technique described [here](https://www.reddit.com/r/audioengineering/comments/ba338a/heres_a_mixing_trick_stereo_widening_using_phase/). `$1`: "width" of the effect (0 - 1). _NB: Output can be a little louder than 1._
