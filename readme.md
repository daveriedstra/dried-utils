
dried-utils
====

useful pure data abstractions
----

This is my personal repository of pd abstractions, a little bag-o-tricks. Feel free to use these, though they might change. If the documentation isn't clear, check out the abstraction patch itself.

I mostly use purr-data, so vanilla pd compatibility is questionable but probable: I am trying to improve compatibility.

### The abstractions

- **adsr~** - a simple adsr~ envelope for cases where bundled adsr~ is inadequate. Send it a list of attack, decay, sustain, release (all ms).
- **auto-limiter~** - simple self-limiter, useful for feedback rejection. Scales up quiet signals and scales down loud ones.
- **cc** - MIDI continuous controller input with CC number and channel learn. Open the toggle to begin learn, and close to select the last used controller. Output is scaled from zero - first-argument. Second argment can be used to set the initial controller number, third is initial channel (if unset all channels are used). Right outputs send CC number and channel number when exiting learn mode.
- **cc-b** - like `cc` but sends a bang for values above zero. Useful for buttons. Arguments can be used to set initial controller number and channel.
- **cc-t** - like `cc-b` but alternately sends `1` and `0`. Useful for buttons masquerading as toggles. Argument can be used to set initial controller number and channel.
- **circ-buff~** - A circular buffer. Continuously records to the table specified in the argument and reports the record head position (minus one block). Block size, buffer size, and additional delay are configurable to the right input (`block-size $1`, `buffer-size $1`, and `delay $1` respectively, all values in samples). Default 1sec buffer. Send `1`/`start` or `0`/`stop` to the right input to start or stop the buffer.
- **count-to** - Counts to the number past to it.
- **cubic-bezier~** / **quadratic-bezier~** / **quintic-bezier~** - generate one-dimensional bezier output for the given progress (signal inlet, 0-1) and control values.
- **delta** - outputs the difference between the present and the previous number.
- **detente** - snap the incoming numbers on either side of the first argument to the first argument (margin is second argument)
- **fold~** - simple wave folder. Folds amplitudes greater than 1.
- **ftomb** - input a frequency, outputs midi note and bend values. `$1`: bend width in semitones (default: 12)
- **gs~** & **gs2~** - graphical gainstages / volume controls.
- **humanize** - add a bit of timing variation to incoming messages (lists of 1-5 floats). `$1`: window (ms), default: 50ms. `$2`: distribution (-1 to 1), default 0.5. Distribution of 0 is totally even / random, 0.5 is mostly on beat, 1 is only on beat, negatives avoid the beat. Note that `humanize` considers the beat to be half the window duration after the message is received, so on-beat messages with a window of 100ms will happen 50ms after they are received in order to enable the possibility of being ahead-of-the-beat.
- **intuos-pen** - abstraction for simpler use of Wacom Intuos Pen Small (model CTH-480). Uses [hid] under the hood. Does things like normalize output values to 0-1 and simplify the messages (check the patch for details). Only handles pen, not touch or tablet buttons. Won't work with anything but the exact model, modify for your device if you like.
- **lfo~** - a quick-and-dirty oscillator between two values. Oscillates between `$2` and `$3` at `$1`hz.
- **line-eased~** - generates a ramp with a sinusoidal ease-in-out curve. Same API as `line~` but ignores `stop` messages.
- **line-bezier~** - like `line-eased~`, generates an eased-in-out ramp using the cubic bezier function, but tries to smooth out corners when the ramp is interrupted.
- **list-remove** - remove the element at `$1` from the list.
- **loudness~** - outputs the approximate LUFS loudness of the incoming signal in dBfs.
- **midi-panic** - outputs note-offs for all 127 midi notes with 1-ms separation.
- **miniseq** - simple graphical 8-step sequencer. Left outlet: selected steps with index; right outlet: all steps with index. Inlet 1 & 2 go straight to the driving `[metro]`, except you can send a `bang` for a single iteration. Inlet 3 controls how many steps in the sequence (negative for reverse), inlet 4 controls the start index.
- **ms-to-samp** - convert milliseconds to samples at the current sample rate. `$1`: milliseconds in.
- **nano-seq** - use your nanoKontrol as a 16-step sequencer with up to 16 layers. Inlet 1: toggle to play; inlet 2: tempo (bpm); outlet 1: layer-index-value triplets; outlet 2: sync bangs.
- **nano-strip** - simple gui for a single control strip on the Korg nanoKontrol 2. First argument is the lane number from 0 - 7, second argument is 0 for button mode (default) or 1 for toggle mode (affects buttons only). Slider and knob are scaled to 0 - 1, buttons output bangs in button mode or 1 / 0 in toggle mode.
- **nano-transport** - simple gui for the transport, track, & marker controls on the Korg nanoKontrol 2.
- **noteon-to-note** - converts pitch-value-duration triplets into the appropriate note-on / note-off sequences (for use with `[poly]`)
- **noteon-to-mono** - quick-n-dirty mono converter (for those times when you can't be bothered to send note-offs). Send pitch/value pairs and have them converted to the appropriate note-on/-off sequences. Notes sustain until the next note-on or the specified max duration (`$1`, ms).
- **oscin** - receive and parse OSC messages over UDP on localhost on port `$1`.
- **pan~** - drop-in replacement for `pan` but using signal as right input for smoother panning changes
- **partial-counter** - start at 1, bang left for up, mid for down, right to reset, below zero is a fraction.
- **partial-poly** - allows simple access to just tuned partials with midi keyboard. Held pitch is root, pitches above or below will sound the partial of their semitone difference (eg, an F4 above C4 will sound the 5th partial, ie, E). Use like poly.
- **poly** - just like vanilla `[poly]` but corrects the index to work neatly with `[clone]`.
- **pool** - pure data implementation of [Pool Time's](https://gitlab.com/pool-time/pool-time) Pool class. Sends between `$3` and `$4` (or just `$3` if `$4` is omitted) bangs randomly during a pool of time which lasts between `$1` and `$2`ms. Send `bang`, "start" or "1" to start, "stop" or "0" to stop, "set loop" or "set noloop" to control whether it loops (noloop by default).
- **pop~** - makes an electronic-ey pop sound.
- **ps~** - simple panning stage for graphical patches.
- **pwm~** - pulse-width modulated ramp. Send a `[phasor~]` 0-1 through this and ratio terms to the two right inlets to get a pwm-scaled output of the ramp (default 1:1, eg, sending `2` to the middle inlet produces a ratio of 2:1). Useful for driving `[sin~]` or some other oscillator or wavetable.
- **quiet-param~** - temporarily mute a signal to quietly change a parameter which can't be interpolated at signal rate.
- **rand-range** - outputs a random int between two argument ints. Bang for a new number.
- **round-clip~** - tanh clipping with variable corner roundness (1 - n).
- **samp-to-ms** - convert samples to milliseconds at the current sample rate. `$1`: samples in.
- **stob~** - converts a schmitt trigger (short impulse in silence) to a bang. Similar to `edge~` but more consistent and vanilla-friendly.
- **scope~** - a scope that animates like a traditional scope app. Needs refinement but useful for many applications. Some zoom settings might affect performance.
- **some-edo** - outputs a new frequency which is a random interval above or below the previous freq using a given Equal Division of the Octave and octave size.
- **some-harmonic** - outputs a new frequency which is a random integer multiple of the input frequency (or the last output frequency), wrapped to be lower than a given frequency. `$1`: wrap point and start frequency if none specified. `$2`: lower harmonic limit. `$3`: upper harmonic limit.
- **some-subharmonic** - like `some-harmonic` but with subharmonics.
- **spigot~** - a spigot~ for audio that opens when sent its creation argument, closes for anything else.
- **sr** - output samplerate on load, on DSP engage, or on bang.
- **ss-saw~** - a sine-summed sawtooth wave using an arbitrary number of harmonics. `$1`: frequency. `$2`: number of harmonics. Shorthand for the technique described [here](http://write.flossmanuals.net/pure-data/generating-waveforms/).
- **ss-squ~** - a sine-summed square wave using an arbitrary number of harmonics. `$1`: frequency. `$2`: number of harmonics. Shorthand for the technique described [here](http://write.flossmanuals.net/pure-data/generating-waveforms/).
- **ss-tri~** - a sine-summed triangle wave using an arbitrary number of harmonics. `$1`: frequency. `$2`: number of harmonics. Shorthand for the technique described [here](http://write.flossmanuals.net/pure-data/generating-waveforms/).
- **sustain** - shorthand for `cyclone/sustain`; put immediately after `notein` to use sustain on the specified midi channel (or all if none specified).
- **throttle** - rate-limit messages, outputting the last received after `$1` ms. _caveat: trims list selector._
- **trig~** - bang to write a single, full-amplitude sample to the audio stream.
- **widen~** - set-and-forget stereo widening using the technique described [here](https://www.reddit.com/r/audioengineering/comments/ba338a/heres_a_mixing_trick_stereo_widening_using_phase/). `$1`: "width" of the effect (0 - 1). _NB: Output can be a little louder than 1._
- **xfade~** - equal-power crossfade between two signals at audio rate. Control inlet (far right) is from -1 (only left input) to +1 (only right input).
