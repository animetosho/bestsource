This is a fork of the [‘BestSource’ VapourSynth plugin](https://github.com/vapoursynth/bestsource), designed for use in the [Anime Tosho frame server](https://github.com/animetosho/frame-server). Please see [the original README](https://github.com/vapoursynth/bestsource/blob/master/README.md) for more info.

Modifications made in this fork:

* ability to [accept custom options to libavformat/libavcodec](https://github.com/vapoursynth/bestsource/pull/67)
* avoid decoding the first frame twice, whilst avoiding the unnecessary instantiation of a second decoder
* add a `seeksearch` parameter to `VideoSource`
* *libbestsource* is statically linked by default