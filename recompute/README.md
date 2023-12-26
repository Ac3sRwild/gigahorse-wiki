### Remote Compute

It's possible to move the compute task to another machine or machines, in order to avoid having to install a GPU or powerful CPU in every harvester:

![Remote_Compute_Drawings drawio](https://github.com/madMAx43v3r/chia-gigahorse/assets/951738/9bb8d9b7-6a15-4b4a-82aa-6ab72471d5e5)

To use the remote compute feature:
- Start `chia_recompute_server` on the machine that is doing the compute (included in release).
- `export CHIAPOS_RECOMPUTE_HOST=...` on the harvester (replace `...` with the IP address or host name of the compute machine, and make sure to restart via `chia stop all -d` or `stop_all.cmd` on windows)
- On Windows you need to set `CHIAPOS_RECOMPUTE_HOST` variable via system settings.
- `CHIAPOS_RECOMPUTE_HOST` can be a list of recompute servers, such as `CHIAPOS_RECOMPUTE_HOST=192.168.0.11,192.168.0.12`. A non-standard port can be specified via `HOST:PORT` syntax, such as `localhost:12345`. Multiple servers are load balanced in a fault tolerant way.
- `CHIAPOS_RECOMPUTE_PORT` can be set to specify a custom default port for `chia_recompute_server` (default = 11989).
- See `chia_recompute_server --help` for available options.

To use the remote compute proxy:
- Start `chia_recompute_proxy -n B -n C ...` on a machine `A`. (`B`, `C`, etc are running `chia_recompute_server`)
- Set `CHIAPOS_RECOMPUTE_HOST` on your harvester(s) to machine A.
- `chia_recompute_proxy` can be run on a central machine, or on each harvester itself, in which case `A = localhost`.
- See `chia_recompute_proxy --help` for available options.

When using `CHIAPOS_RECOMPUTE_HOST`, the local CPU and GPUs are not used, unless you run a local `chia_recompute_server` and `CHIAPOS_RECOMPUTE_HOST` includes the local machine.

#### CPU based Compute Servers
For CPU based compute it's important to increase `CHIAPOS_MAX_CORES` on the harvesters to achieve full CPU utilization on compute servers.
Because `CHIAPOS_MAX_CORES` is the maximum parallel requests made from a harvester to recompute servers, and a request is processed on a single CPU core only.
By default `CHIAPOS_MAX_CORES` is the number of phsical CPU cores on the harvester.

For example if you have a single compute server with 32 CPU cores, you should set `CHIAPOS_MAX_CORES` on the harvesters to 32.
The sum of `CHIAPOS_MAX_CORES` accross all harvesters should be greater or equal to the sum of CPU cores on all compute servers.
In case of low number of harvesters (ie. 1-3) you should set `CHIAPOS_MAX_CORES` to the number of CPU cores on your compute server.
