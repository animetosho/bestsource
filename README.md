This is a fork of the [‘BestSource’ VapourSynth plugin](https://github.com/vapoursynth/bestsource), designed for use in the [Anime Tosho frame server](https://github.com/animetosho/frame-server). Please see [the original README](https://github.com/vapoursynth/bestsource/blob/master/README.md) for more info.

Modifications made in this fork to the `VideoSource` function:

* ability to [accept custom options to libavformat/libavcodec](https://github.com/vapoursynth/bestsource/pull/67)
* avoid decoding the first frame twice, whilst avoiding the unnecessary instantiation of a second decoder
  * also avoid decoding the frame for a third time if track indexing, and re-use the existing decoder there
* cache first few decoded frames during indexing (dependent on `cachesize`) instead of discarding all of them
* add a `seeksearch` parameter. BestSource uses a hard-coded value of '100' frames when trying to determine seek locations, and the number of frames from the start to force linear decoding. This parameter allows one to alter this arbitrary threshold to suit one's needs
* add parameter `index_limit` which limits the number of frames when constructing the index. If used, the cache file will never be written (hence cannot be used with `cachemode=2`), but will be ignored if a cache file is loaded. If this option is applied, the returned clip will have no more than the frame limit.
* *libbestsource* is statically linked by default