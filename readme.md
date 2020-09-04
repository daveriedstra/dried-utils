
dried-utils
====

useful pure data abstractions
----

This is my personal repository of pd abstractions, a little bag-o-tricks. Feel free to use these, though they might change. If the documentation isn't clear, check out the abstraction patch itself.

I mostly use purr-data, so vanilla pd compatibility is questionable but probable: I am trying to improve compatibility.

# The abstractions

- **adsr~** - a simple adsr~ envelope for cases where bundled adsr~ is inadequate. Send it a list of attack, decay, sustain, release (all ms).
- **cc** - MIDI continuous controller input with CC number and channel learn. Open the toggle to begin learn, and close to select the last used controller. Output is scaled from zero - first-argument. Second argment can be used to set the initial controller number, third is initial channel (if unset all channels are used). Right outputs send CC number and channel number when exiting learn mode.
- **cc-b** - like `cc` but sends a bang for values above zero. Useful for buttons. Arguments can be used to set initial controller number and channel.
- **cc-t** - like `cc-b` but alternately sends `1` and `0`. Useful for buttons masquerading as toggles. Argument can be used to set initial controller number and channel.
- **cubic-bezier~** / **quadratic-bezier~** / **quintic-bezier~** - generate one-dimensional bezier output for the given progress (signal inlet, 0-1) and control values.
- **fold~** - simple wave folder. Folds amplitudes greater than 1.
- **intuos-pen** - abstraction for simpler use of Wacom Intuos Pen Small (model CTH-480). Uses [hid] under the hood. Does things like normalize output values to 0-1 and simplify the messages (check the patch for details). Only handles pen, not touch or tablet buttons. Won't work with anything but the exact model, modify for your device if you like.
- **lfo~** - a quick-and-dirty oscillator between two values. Oscillates between `$2` and `$3` at `$1`hz.
- **line-eased~** - generates a ramp with a sinusoidal ease-in-out curve. Same API as `line~` but ignores `stop` messages.
- **line-bezier~** - like `line-eased~`, generates an eased-in-out ramp using the cubic bezier function, but tries to smooth out corners when the ramp is interrupted.
- **miniseq** - simple graphical 8-step sequencer. Left outlet: selected steps with index; right outlet: all steps with index. Inlet 1 & 2 go straight to the driving `[metro]`, except you can send a `bang` for a single iteration. Inlet 3 controls how many steps in the sequence (negative for reverse), inlet 4 controls the start index.
- **nano-seq** - use your nanoKontrol as a 16-step sequencer with up to 16 layers. Inlet 1: toggle to play; inlet 2: tempo (bpm); outlet 1: layer-index-value triplets; outlet 2: sync bangs.
- **nano-strip** - simple gui for a single control strip on the Korg nanoKontrol 2. First argument is the lane number from 0 - 7, second argument is 0 for button mode (default) or 1 for toggle mode (affects buttons only). Slider and knob are scaled to 0 - 1, buttons output bangs in button mode or 1 / 0 in toggle mode.
- **nano-transport** - simple gui for the transport, track, & marker controls on the Korg nanoKontrol 2.
- **noteon-to-note** - converts pitch-value-duration triplets into the appropriate note-on / note-off sequences (for use with `[poly]`)
- **pan~** - drop-in replacement for `pan` but using signal as right input for smoother panning changes
- **partial-counter** - start at 1, bang left for up, mid for down, right to reset, below zero is a fraction.
- **partial-poly** - allows simple access to just tuned partials with midi keyboard. Held pitch is root, pitches above or below will sound the partial of their semitone difference (eg, an F4 above C4 will sound the 5th partial, ie, E). Use like poly.
- **poly** - just like vanilla `[poly]` but corrects the index to work neatly with `[clone]`.
- **pool** - pure data implementation of [Pool Time's](https://gitlab.com/pool-time/pool-time) Pool class. _(TODO - documentation)_
- **pop~** - makes an electronic-ey pop sound.
- **pwm~** - pulse-width modulated ramp. Send a `[phasor~]` 0-1 through this and ratio terms to the two right inlets to get a pwm-scaled output of the ramp (default 1:1, eg, sending `2` to the middle inlet produces a ratio of 2:1). Useful for driving `[sin~]` or some other oscillator or wavetable.
- **quiet-param~** - temporarily mute a signal to quietly change a parameter which can't be interpolated at signal rate.
- **rand-range** - outputs a random int between two argument ints. Bang for a new number.
- **round-clip~** - tanh clipping with variable corner roundness (1 - n).
- **scope~** - a scope that animates like a traditional scope app. Needs refinement but useful for many applications. Some zoom settings might affect performance.
- **some-edo** - outputs a new frequency which is a random interval above or below the previous freq using a given Equal Division of the Octave and octave size.
- **some-harmonic** - outputs a new frequency which is a random integer multiple of the input frequency (or the last output frequency), wrapped to be lower than a given frequency. `$1`: wrap point and start frequency if none specified. `$2`: lower harmonic limit. `$3`: upper harmonic limit.
- **some-subharmonic** - like `some-harmonic` but with subharmonics.
- **spigot~** - a spigot~ for audio that opens when sent its creation argument, closes for anything else.
- **ss-saw~** - a sine-summed sawtooth wave using an arbitrary number of harmonics. `$1`: frequency. `$2`: number of harmonics. Shorthand for the technique described [here](http://write.flossmanuals.net/pure-data/generating-waveforms/).
- **ss-squ~** - a sine-summed square wave using an arbitrary number of harmonics. `$1`: frequency. `$2`: number of harmonics. Shorthand for the technique described [here](http://write.flossmanuals.net/pure-data/generating-waveforms/).
- **ss-tri~** - a sine-summed triangle wave using an arbitrary number of harmonics. `$1`: frequency. `$2`: number of harmonics. Shorthand for the technique described [here](http://write.flossmanuals.net/pure-data/generating-waveforms/).
- **throttle** - rate-limit messages, outputting the last received after `$1` ms. _caveat: trims list selector._
- **widen~** - set-and-forget stereo widening using the technique described [here](https://www.reddit.com/r/audioengineering/comments/ba338a/heres_a_mixing_trick_stereo_widening_using_phase/). `$1`: "width" of the effect (0 - 1). _NB: Output can be a little louder than 1._
