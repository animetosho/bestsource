This is a fork of the [‘BestSource’ VapourSynth plugin](https://github.com/vapoursynth/bestsource), designed for use in the [Anime Tosho frame server](https://github.com/animetosho/frame-server). Please see [the original README](https://github.com/vapoursynth/bestsource/blob/master/README.md) for more info.

Modifications made in this fork to the `VideoSource` function:

* ability to [accept custom options to libavformat/libavcodec](https://github.com/vapoursynth/bestsource/pull/67)
* avoid decoding the first frame twice, whilst avoiding the unnecessary instantiation of a second decoder
  * also avoid decoding the frame for a third time if track indexing, and re-use the existing decoder there
* cache first few decoded frames during indexing (dependent on `cachesize`) instead of discarding all of them
* add a `seeksearch` parameter. BestSource uses a hard-coded value of '100' frames when trying to determine seek locations, and the number of frames from the start to force linear decoding. This parameter allows one to alter this arbitrary threshold to suit one's needs
* add parameter `index_limit` which limits the number of frames when constructing the index. If used, the cache file will never be written (hence cannot be used with `cachemode=2` or `4`), but will be ignored if a cache file is loaded. If this option is applied, the returned clip will have no more than the frame limit.
* *libbestsource* is statically linked by default

## Image Benchmark

This fork significantly improves performance of loading images or single frame videos. The following script was used for testing the performance of such:

```python
import vapoursynth as vs
import time

file = './src-2560x1440.png'

times = []
for j in range(0,5):
	start = time.time_ns()
	for i in range(0,5):
		clip = vs.core.bs.VideoSource(file, cachemode=0)
		#clip = vs.core.imwri.Read(file)
		#clip = vs.core.ffms2.Source(file, cache=False)
		#clip = vs.core.lsmas.LWLibavSource(file, cache=0)
		with clip.get_frame(0) as f:
			pass
	taken = time.time_ns() - start
	times.append(taken /1000000.0)


print("time taken (ms):", times)
print("fastest    (ms):", min(times))
```

The ‘fastest’ time as obtained on a Pentium J5005 is listed below:

| Source                                                       | Time (ms) |
| ------------------------------------------------------------ | --------: |
| [imwri R2](https://github.com/vapoursynth/vs-imwri/releases/tag/R2) |    2299.9 |
| [BestSource R8](https://github.com/vapoursynth/bestsource/releases/tag/R8) |    2491.2 |
| **BestSource R8 AnimeTosho**                                 | **866.0** |
| [ffms2 5.0](https://github.com/FFMS/ffms2/releases/tag/5.0)  |    1786.7 |
| [L-SMASH LWLibavSource 1194](https://github.com/HomeOfAviSynthPlusEvolution/L-SMASH-Works/releases/tag/1194.0.0.0) |    1294.6 |

