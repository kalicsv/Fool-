Installation
The simplest way to install is downloading from PyPI:


$ pip install -U iptvtools

If you prefer the latest master branch or want to make some code changes, you can download and install with the following command:



$ git clone https://github.com/huxuan/iptvtools.git

$ cd iptvtools

$ pip install .



usage: iptv-filter [-h] [--min-height MIN_HEIGHT] [-c CONFIG] [-i INPUT]
                   [-I INTERVAL] [-o OUTPUT] [-r] [-t TEMPLATE] [-T TIMEOUT]
                   [-u UDPXY] [-v]

optional arguments:
  -h, --help            show this help message and exit
  --min-height MIN_HEIGHT
                        Minimal acceptable height/resolution, defaults to
                        0which means 0P. Set 0 to disable the filtering.
  -c CONFIG, --config CONFIG
                        Configuration file to unify title and id information,
                        defaults to `config.json`
  -i INPUT, --input INPUT
                        Input playlist file/url, defaults to `[https://iptv-
                        org.github.io/iptv/index.m3u]`. Note that multiple
                        input files/urls are supported.
  -I INTERVAL, --interval INTERVAL
                        Interval in seconds between fetching each url,
                        defaults to 1.
  -o OUTPUT, --output OUTPUT
                        Output playlist file name, defaults to
                        `iptvtools.m3u`.
  -r, --replace-group-by-source
                        Flag to indicate whether to replace the group title
                        with source name, where source name is the basename of
                        input files/urls without extension.
  -t TEMPLATE, --template TEMPLATE
                        Template playlist file/url which have well-maintained
                        channel information to cooperate with EPG and will
                        replace the corresponding entry (except url) if
                        matched , defaults to `[]`. Note that multiple
                        template files/urls are supported.
  -T TIMEOUT, --timeout TIMEOUT
                        Acceptable timeout when retrieving stream information,
                        defaults to 10.
  -u UDPXY, --udpxy UDPXY
                        UDP Proxy which will convert the url automatically,
                        defaults to `None`.
  -v, --version        
 show program's version number and exit

Example
There is a well-maintained IPTV list only

 for Beijing Unicom and a well-maintained

 templates & EPG mainly for China. So fo
r me:


$ iptv-filter \

-i https://gist.githubusercontent.com/sdhzdmzzl/93cf74947770066743fff7c7f4fc5820/raw/0be4160f4024320f23daad65bce79e606da47995/bj-unicom-iptv.m3u \
-t http://epg.51zmt.top:8000/test.m3u


With UDPXY, it becomes:

iptv-filter \

-i https://gist.githubusercontent.com/sdhzdmzzl/93cf74947770066743fff7c7f4fc5820/raw/0be4160f4024320f23daad65bce79e606da47995/bj-unicom-iptv.m3u \

-t http://epg.51zmt.top:8000/test.m3u \

-u http://192.168.0.1:8888


、，Just replace http://192.168.0.1:8888 with corresponding UDPXY prefix should be OK









Selected Parameters
Here is some further explanation for those not so obvious parameters.

MIN_HEIGHT
HEIGHT is a dominant factor of stream quality, where 1080 in height means 1080p. It is necessary to set this filter if the stream is supposed to be shown on high resolution screens, e.g., a 4K TV.

CONFIG
CONFIGis a customized configuration to unifytitleandid. titleis the exact title which will be shown and theidis used for potential match with the template. A general idea is to make theidas simple as possible so they will have a high possibility to match, though there might be some false positive cases. So,id_unifierscan be treated as a further simplification oftitile_unifier.

For example, entry"-": ""will convertCCTV-1toCCTV1, entry"＋": "+"will convertCCTV-5＋toCCTV-5+. A whole replacement is also possible, as"BTV冬奥纪实": "北京纪实"will match the whole ofBTV冬奥纪实and replace it with北京纪实.

Please be caution about using too many of them since this simplified strategy is just for some basic requirement. Some entries may lead to some unexpected changes. For example, entry"CCTV-1": "中央1套"will convertCCTV-11to中央1套1. So, in generally, only keep those necessary entries and keep it as simple as possible.

TEMPLATE
A m3u playlist with well-maintained information to cooperate with EPG. Please refer toWell‐maintained templates & EPGs.

BTW, there is also a listWell‐maintained playlists.

TIMEOUT
TIMEOUT is used to check the connectivity. Direct check which only fecth the response header tends to be fast. But it usually takes seconds to probe stream information depends on your network (bandwidth and latency). For me, it is about 3 to 5 seconds.

UDPXY
If the IPTV streams is forwarded by UPDXY, setting it will convert all the urls automatically. For examples, with UDPXYhttp://192.168.0.1:8888/, rtp://123.45.67.89:1234will be converted tohttp://192.168.0.1:8888/rtp/123.45.67.89:1234.





