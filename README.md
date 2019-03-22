# EMEPLEIER
Send OSC messages to MPlayer

In SuperDirt

```
~oscaddr = NetAddr.new("127.0.0.1", 5005);    // create the NetAddr
(
~video = "";
~dirt.receiveAction = { |event|
	var dur, nchan, vel, note, pchan;

	if(event['video']!=nil)
	{
		if(event.video!=~video)
		{
			event.postln;
			~oscaddr.sendMsg("/loadfile",event.video);
			~video = event.video;
		}
	};

	if(event['depth']!=nil)
	{
		SystemClock.sched(event.latency,{
			~oscaddr.sendMsg("/seek",(event.n+1)*event.depth + event.pos);
		});
	}
};
```

In tidal

```
let (pos, pos_p) = pF "pos" (Just 0.0)
    (depth, depth_p) = pF "depth" (Just 0.0)
    (video, video_p) = pS "video" (Just "0001.mp4")
    
d1 $ n "0 1 2 1" # s "in" # depth 1.2 # pos "40 50" # video "0012.mp4" # gain 1.60
```

