# MCME-ChunkAnalysis
Preforms block analysis for large data sets. There are two main functions:

```
/block analyse <Block ID>[:data] -t [json tags] -p [pack] -s
```
This will count the number of blocks in a given section of the map that match the provided block id and data. A JSON tag can be added for block entities using `-t`. If `-p` is provided with a pack parameter, then the whole region for that pack will be scanned. If the `-s` is provided, the player's world edit selection is analysed.

```
/block replace <Find ID>[:find data] -t [find tags] <Replace ID>[:replace data] -t [replace tags] -s -p [pack] -d
```
This extends on the analyse function except it attempts to replace the blocks located by the search with the provided id, data, and potential JSON tags. An additional `-d` parameter is provided if you would like both forward and backward find and replace, so that two blocks are effectively swapped.

The interesting part of this implementation is not the practical uses, but rather the multi threading to allow such high volumes of block to be scanned. To do this, the chunks must be collapsed into finalized instances which bukkit labels snapshots. In addition, these snapshots cannot be modified in any way, so to preform a replace function, a queue of blocks waiting to be updated is formed and the modifications and completed synchronously with the world tick. This limits the amount of lag that takes place in the main thread while allowing high volumes of blocks to be scanned at once.
