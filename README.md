# audio2srt
use baidu voice-api to add subtitle to a vedio<br>
利用百度语音api和pydub实现自动生成字幕的功能<br>
因为该程序粗糙请阅读以下注意点。<br>
直接使用注意点：<br>
1.注册百度开发者之后创建语音app，得到appid apikey secretkey（免费使用）。<br>
2.需要用到pydub，百度aip模块<br>
3.将视频转成音频后注意保存为wav格式。<br>
4.音频可以预处理一下去噪（后续我应该也不会加这个）看一下底噪大概范围，填入silence_thresh。<br>
5.没有设置超过60s音频再分割，如果讲话时间较长可能会得到的错误结果较多（后续应该会改进）<br>
6.时间部分只写到分（m），有需要的朋友可以自己写到小时，不过不建议。<br>
7.需要改动pydub里的silence.py里的split_on_silence(可以直接改或者另存为:)<br>

```python
def split_on_silence(audio_segment, min_silence_len=1000, silence_thresh=-16, keep_silence=100,
                     seek_step=1):
    not_silence_ranges = detect_nonsilent(audio_segment, min_silence_len, silence_thresh, seek_step)

    chunks = []
    starttime=[]
    endtime=[]
    for start_i, end_i in not_silence_ranges:
        start_i = max(0, start_i - keep_silence)
        end_i += keep_silence

        chunks.append(audio_segment[start_i:end_i])
        starttime.append(start_i)
        endtime.append(end_i)

    return chunks,starttime,endtime
```
