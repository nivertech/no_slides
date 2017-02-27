# Simple distributed command
Clear ring_data
```shell
$ rm -rf ring_data_dir_*
```
Run two node and on second node:
``` elixir
:riak_core.join(‘gpad_1@127.0.0.1’)
NoSlides.Service.ping
```
See the log on firs node

# Distributed command

On node 1
```elixir
NoSlides.Service.put(:gpad, 42)
NoSlides.Service.put(:joe, 1)
NoSlides.Service.put(:asdf, 45)
NoSlides.Service.put(:gino, 666)
NoSlides.Service.get(:gpad)
NoSlides.Service.get(:joe)
```
On node 2:
```elixir
NoSlides.Service.get(:gpad)
NoSlides.Service.get(:joe)
Remove node … :riak_core.leave
```

On node 1:
```elixir
NoSlides.Service.get(:gpad)
```

# Handoff
Clear ring data
```shell
$ rm -rf ring_data_dir_*
```

Join node
```elixir
:riak_core.join('gpad_1@127.0.0.1')
```

On node 1
```elixir
NoSlides.Service.put(:gpad, 42)
NoSlides.Service.put(:joe, 1)
NoSlides.Service.put(:asdf, 666)
```

On node 2:
Remove node …
```elixir
:riak_core.leave
```

On node 1:
```elixir
NoSlides.Service.get(:gpad)
```

Rejoin …
```elixir
:riak_core.join(‘gpad_1@127.0.0.1’)
```

Kill node 2 -> Data disappear!!!

# Coverage - DEMO
Start 3 nodes
```elixir
NoSlides.Service.put(:gpad, 42)
NoSlides.Service.put(:gino, 123)
NoSlides.Service.put(:pino, 321)
NoSlides.Service.put(:joe, 1)
NoSlides.Service.keys
```

# Replication - DEMO

Start 5 nodes and show the ring_status
```elixir
NoSlides.Service.ring_status
```
Execute this command:
```elixir
#From Node 4
NoSlides.Service.ft_put(:gpad, 666)
NoSlides.Service.ft_get(:gpad)

#Kill node 2
NoSlides.Service.ft_get(:gpad) # {:ok, [666, nil]}{:ok, [666, nil]}
NoSlides.Service.ft_put(:gpad, 1234)
NoSlides.Service.ft_get(:gpad) # {:ok, [1234]}

# Restart node 2 -> start and handoff
NoSlides.Service.ft_get(:gpad) # {:ok, [1234]} or {:ok, [1234, nil]}

```
