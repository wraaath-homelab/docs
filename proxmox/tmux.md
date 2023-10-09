Proxmox seems to just reset terminal sessions, so here is how to keep a session with Tmux:
```bash
tmux new -s xmrig
tmux attach -t xmrig
```
> Note:
>> In my case I used it for XMRig, where you need a terminal output thingy running.